# DispatchQueue 정리하기.

https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-grand-dispatch-queue-1-397db16d0305 

를 보고 알았던 내용, 헷갈렸던 내용, 몰랐던 내용을 간단하게 정리!


# 알았던 것  
- Sync 와 Async
    - sync 와 async는 작업을 보내는 시점에서 기다릴지 말지 여부를 정한다.
    - async로 작업을 보내면 작업이 끝날 때 까지 기다리지 않고 다음일 하러 간다.
    -
- Concurrent Queue 와 Serial Queue
    - Concurrent Queue는 작업을 여러개의 Thread로 배분하고 Serial Queue는 하나의 Thread로 작업을 보낸다.
    - Serial Queue는 작업의 순서가 중요한 작업에서 사용하면 좋다.

# 헷갈렸던 것
- Custom DispatchQueue의 default는 Serial Queue이다.
- Custom DispatchQueue attributes를 사용하여 Concurrent Queue로 동작할 수 있다.

# 몰랐던 점
- Global Queue에서 sync로 작업을 보내면 Main Thread에서 작업을 한다.
- qos 를 넣지 않으면 OS에서 추론해준다. 

# sync 사용시 주의 사항
1. main queue에서 다른 queue로 작업을 보낼 때 sync를 사용하면 안된다.
    - 작업이 끝날때까지 기다려야 하는데 Main Thread에서는 UI 업데이트를 하고 있기 때문에 UI 업데이트가 지연되면 사용자 편의성에 좋지 않기 때문이다.
2. 동일한 Queue에서 sync 작업을 보내면 안된다.
    - 작업이 끝날 때까지 기다려야 하는데 작업이 시작될 수 없음. 데드락 발생할 가능성이 있다.
3. Main Thread에서 DispatchQueue.main.sync를 사용하면 안된다
    - 빌드에러 발생하고, 데드락 발생

# DispatchGroup 
- async에서만 사용이 가능하다.
- 하나의 group에 여러개의 qos 사용이 가능하다. 
- group이 비어있다면 바로 notify가 실행된다.
- wait을 사용하면 실행시킨 thread를 block할 수 있다. (다음 라인으로 넘어가지 않고 기다리는 느낌)
- wait은 main thread에서 호출하지 말자.(UI 버벅임 발생)
- wait은 데드락이 발생할 수 있다.
- task가 비동기적이라면 group의 종료시점을 잡기 쉽지 않다. (그래서 leave(), enter() 함수 존재. 명시적으로 시작과 끝점을 알려중어 DispatchGroup의 notify를 호출할 수 있다.)

# DispatchGroupItem
- 취소기능, 순서기능 제공함
- 재사용 가능

# DispatchSemaphore
- 한번에 수행할 수 있는 작업의 수를 제한할 수 있다.
- let semaphore = DispatchSemaphore(value: 2) 처럼 사용
- 임계구역 들어갈 때 wait() 사용하면 세마포어 카운트 감소
- 사용이 끝났을 때 signal() 사용하면 세마포어 카운트 증가
- 세마포어는 wait, signal이 한쌍이 아니여도 상관없음 signal -> wait 을 사용해도 됨.
- 이렇게 하면 순서를 보장할 수 있음.
    - 초기값을 0으로 설정하고 signal을 주면 세마포어가 1이 되고 wait 하고 있는 부분 작업이 시작이됨.


# 동시성에서 발생하는 문제들
- 하나의 공유자원에 동시에 접근한다면 문제가 발생할 수 있음
- Race Condition(경쟁상태), Deadlock(교착상태), Priority Inversion(우선순위 뒤바뀜) 등이 있음
1. Race Condition  
한번에 여러개의 쓰레드에서 공유자원에 접근하면 값이 예상과 다르게 변경될 수 있음.   
해결 방법 : Thread Safe하게 만들자.  
- Serial Queue와 sync를 사용하여 순차적으로 처리하면 동시에 값을 접근하지 못함.  
- Dispatch Barrier 를 사용하자. (Concurrent Queue + Dispatch Barrier)  
여러개의 Thread중에서 Barrier를 사용한 스레드를 제외하고 모든 스레드에서 사용을 막음.    
READ작업은 일반적인 task WRITE 작업은 Barrier에 넣으면 문제를 해결할 수 있음.   
Serial Queue보다 조금더 효율적임  
2. Deadlock  
Serial Queue를 사용해서 순서를 보장하자.  
3. Priority Inversion(우선순위 뒤바뀜)  
우선순위가 낮은데 먼저 작업을 요청해서 먼저 작업이 끝날 수 있음.
