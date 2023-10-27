# Concurrent 공부하면서 궁금했던 점.

https://sujinnaljin.medium.com/ios-차근차근-시작하는-gcd-grand-dispatch-queue-1-397db16d0305

오랜만에 다시 GCD 공부를 했다. 위의 naljin님의 블로그와 여러개의 블로그를 참고해서 공부하다 궁금했던점이 생겨서 정리해보려고 한다.  
오피셜한 정보도 아니고 그냥 궁금해서 테스트한 결과 저장해놓는 용도이다.

<br/>

##  1. DispatchQueue.global.async 클로저안에 Custom Dispatch Queue를 사용한다면 쓰레드가 이동될까?
```
class TestGCD {
    var a = 1
    let queue = DispatchQueue(label: "custom.test.queue")
    func update(_ value: Int) {
        queue.async {
            print("Custom : \(Thread.current)")
            self.a = value
        }
    }
}

let gcd = TestGCD()

print(Thread.current)
DispatchQueue.global().async {
    print("Global : \(Thread.current)")
    gcd.update(2)
    print(gcd.a)
}

print(gcd.a)

```

### 실행전 가설

Global 과 Custom 의 Thread는 다를것이다.  
이유는 DispatchQueue.global에서 Thread를 한번 배정해주고 (기존에는 메인쓰레드일거니까)  
Custom Queue를 만나면 한번 더 Thread를 배정해줄거니까.

하지만 아닐수도 있는게 Custom DispatchQueue는 Serial Queue이기때문에 그대로 사용할 수도 있지 않을까?

<br/>

### 결과

```
<_NSMainThread: 0x6000020841c0>{number = 1, name = main}
1
Global : <NSThread: 0x60000208e600>{number = 4, name = (null)}
Custom : <NSThread: 0x600002088680>{number = 5, name = (null)}
2
```

### 결론
쓰레드는 새로 변경된다.   
Custom Dispatch를 concurrent로 해도 바뀜. Serial이면 안바뀌지 않을까? 라는 생각을 잠깐 했는데 잘 바뀐다.

---

## 2. DispatchQueue.global.async 를 중첩해서 사용하면 Thread는 어떻게 변경될까?

```
DispatchQueue.global().async {
    print("Global 1: \(Thread.current)")
    DispatchQueue.global().async {
        print("Global 2: \(Thread.current)")
        DispatchQueue.global().async {
            print("Global 3: \(Thread.current)")
        }
    }
}
```

### 실행전 가설  
음... 아무래도 thread가 3개로 생기지 않을까?  
이유는 global queue는 concurrent queue이기 때문에 다시 배정해줄거 같다.


### 결과

```
Global 1: <NSThread: 0x600003b19e00>{number = 4, name = (null)}
Global 2: <NSThread: 0x600003b19e00>{number = 4, name = (null)}
Global 3: <NSThread: 0x600003b19e00>{number = 4, name = (null)}
```

### 결론
똑같은 qos이면 쓰레드 이동을 안하는것 같다.  

---

## 2-1. 그렇다면 중간에 하나의 qos만 다르면 어떻게 될까?

```
DispatchQueue.global().async {
    print("Global 1: \(Thread.current)")
    DispatchQueue.global(qos: .userInteractive).async {
        print("Global 2: \(Thread.current)")
        DispatchQueue.global().async {
            print("Global 3: \(Thread.current)")
        }
    }
}
```

### 실행전 가설
Global1, Global3은 같은 쓰레드 Global2만 다른쓰레드에 배정되지 않을까?

### 결과

```
Global 1: <NSThread: 0x600002595bc0>{number = 5, name = (null)}
Global 2: <NSThread: 0x60000259a400>{number = 4, name = (null)}
Global 3: <NSThread: 0x60000259a400>{number = 4, name = (null)}
```

### 결론
이건또 뭐임?  
Global1, Global2가 다른건 인정하겠는데 Global3이 Global2랑 같은게 신기하다.  
아마 또 Thread를 변경하는게 자원낭비라고 판단되어서 같은듯!?

---

## 2-2. 그렇다면 모든 qos가 다르다면?

```
DispatchQueue.global(qos: .background).async {
    print("Global 4: \(Thread.current)")
    DispatchQueue.global(qos: .default).async {
        print("Global 5: \(Thread.current)")
        DispatchQueue.global(qos: .userInteractive).async {
            print("Global 6: \(Thread.current)")
        }
    }
}
```

### 실행전 가설
Qos가 다르니까 모든 쓰레드가 다르겠지!!!?

### 결과
```
Global 4: <NSThread: 0x6000014f8780>{number = 4, name = (null)}
Global 5: <NSThread: 0x6000014f8780>{number = 4, name = (null)}
Global 6: <NSThread: 0x6000014f6e40>{number = 6, name = (null)}
```

### 결론
또 실행전 가설이랑 다르네... 쓰레드는 정말 알다가도 모르겠다..


## 2번 결론
궁금해서 여기저기 검색하다가 결국 GPT한테도 물어보고 왔다!  
Concurrent Queue이기때문에 다른쓰레드에서 작업을 할 수도 있는것이고, 무조건 다른쓰레드에서 작업을 하지는 않는다고 한다.  
생각해보니까 그렇지.. 무조건 다른 쓰레드로 이동하면 쓰레드 낭비(?)이긴 하지..ㅋ

---

## 3. 그러면 struct에서는 Race Condition이 발생할까?

```
struct Counter {
    var value: Int = 0
    
    func increment() {
        value += 1
    }
}

let counter = Counter()
let dispatchGroup = DispatchGroup()

for _ in 0..<100 {
    DispatchQueue.global().async(group: dispatchGroup) {
        for _ in 0..<100 {
            counter.increment()
        }
    }
}

dispatchGroup.wait()
print("Counter value: \(counter.value)")
```

###  실행전 가설
흠 아무래도 value type이기 때문에 10000이 결과값으로 나오지 않을까?

### 결과

```
Counter value: 9919
```

### 결론
Struct이라고 Race Condition이 발생하지 않는것은 아니다!

