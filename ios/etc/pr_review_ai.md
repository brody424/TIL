# PR Review AI를 찾아서!!

한국에서 iOS 개발을 할 때 큰팀이 잘 없었다.  

첫회사인 마드라스체크에서는 나를포함하여 3명의 iOS 개발자가 있었으며,  
두번째 회사인 카사코리아에서는 2명, OPGG에서는 결국 iOS개발자가 혼자뿐이였다.  
(결국 혼자가 되어버림 ㅠㅠ)

그리고 개인앱을 진행할 때는 나 혼자 개발을 하기 때문에 PR을 통해 프로젝트의 안정성을 올리고 싶지만 올릴 수 없었다.  

그러던 중 내눈에 들어온 기술이 있으니 AI를 사용한 PR 코드리뷰이다!

나는 지금 코파일럿을 결제하고 있기 때문에 코파일럿에서 PR을 해주는지 확인을 하고 있는데 마땅치 않은것 같다.  

그래서 PR Review를 해주는 AI를 찾아나섰다!

# 어떤것들이 있을까?

1. Copilot에서 기능을 제공하는지 확인하자.
2. GPT 를 사용하여 깃헙액션과 엮으면 PR Review를 해준다고 한다.
 - https://online-unreal.tistory.com/entry/AI%EC%9D%98-%ED%9E%98%EC%9D%84-%EB%B9%8C%EB%A6%B0-%EC%BD%94%EB%93%9C-%ED%92%88%EC%A7%88-%ED%96%A5%EC%83%81-ChatGPT%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Git-PRPull-Request-%EB%A6%AC%EB%B7%B0
  - https://jypark1111.tistory.com/258
3. CodeRabbit을 사용하자. (한달에 12달러 ㅂㄷㅂㄷ)
 - https://coderabbit.ai/#pricing
4. githubnext에서 제공하는 Copilot for Pull Requests 알아보자.
 - https://githubnext.com/projects/copilot-for-pull-requests/#reviewing-pull-requests-with-ai  
5. 깃헙에 있는 pr-agent 를 알아보자.
 - https://pr-agent-docs.codium.ai/


## 1. Copilot에서 기능을 제공하는지 확인하자.
- summary 정도 작성해주는거 밖에 없는듯하다. 
패스~

## 2. GPT 를 사용하여 깃헙액션과 연동하여 PR Review 진행하기.
- OpenAI 키 받아서 깃헙에 연결하면 되는데 사용법이 어려워 보이지 않는다.
- 크레딧이 쓰는만큼 나가는데 얼마나 나갈지 감이 안온다.
- GPT 모델을 고를 수 있다.
  - gpt-4o   
    - $5.00 / 1M input tokens  
    - $15.00 / 1M output tokens  
  - gpt-4o Mini  
    - $0.150 / 1M input tokens  
    - $0.600 / 1M output tokens  
  - gpt-3.5 turbo  
    - $3.000 / 1M input tokens  
    - $6.000 / 1M output tokens  
뭐지..? GPT 3.5가 4o Mini보다 비싸네!   

GPT-4o mini is our most cost-efficient small model that’s smarter and cheaper than GPT-3.5 Turbo, and has vision capabilities. The model has 128K context and an October 2023 knowledge cutoff.  

4o는 3.5터보보다 더 스마트하고 저렴한 가장 비용 효율적인 소형 모델이라고 하니 4o로 테스트를 진행해야 겠다.

### 테스트 시작!
- gpt-4o mini로 테스트 진행
- 시작 크레딧 9.91달러
- 시작 날짜 24.09.19
- 개발하고 있던 MyTimeLite를 전체파일을 PR 돌려보자!

### 테스트 결과
약 5000줄을 추가하고 50줄을 삭제하는 PR 테스트 결과!   
일단 결과는 만족스럽다.  

만족했던 점
- 52개의 코멘트가 달렸는데 퀄리티가 대부분 나쁘지 않았다.
- 비용이 나쁘지 않았다. gpt-4o-mini로 테스트했으며 0.02달러 소모되었다.(토큰은 약 3.8만개)

불만족했던 점
- 이상한 리뷰가 껴있긴 하다.   
  - 리뷰에서 4o-mini 모델을 찾을 수 없다고 하는데 OpenAI 사이트가보면 4o-mini로 잘 작동했었다.  
  - 환경설정에 대한 리뷰도 해주는데 잘 와닿지는 않았다.  
  - 한글로 설정했지만 갑자기 영어로 북치고 장구를 친다.  
- 5000라인에 소요시간 4분 41초라... 애매한 느낌...? (3분 내로 끊어줄 줄 알았다 ㅋㅋ)
- 파일에대해서 리뷰를 해주는거 같은데 코드의 맨 뒤에 몇줄만 보여주는데 고쳐야 될 부분을 명확하게 보여주면 더 좋을거 같다.  

### GPT 결론
가성비 구웃~ 쓸만한대!

## 3. CodeRabbit 패스!
한달에 12달러 내고 리뷰받고 싶진 않아서 일단 패스하고 싶었지만...   
4번과 5번을 테스트할 수 없어서 코드레빗도 테스트를 시작한다!  

일단 pricing을 가보니 1년 결제해야 12달러이고 1달 결제는 15달러라고 한다.  
개인 프로젝트에서 한달에 받을 PR 리뷰의 갯수를 판단하면 매우 비싸다고 느껴졌다.  

카드 필요없이 14일 무료 기간이라고 해서 구글 아이디만 바꾸면 계속 사용할 수 있지 않을까? 라는 꼼수가 생각났지만  
구글 로그인은 없었다 ㅋㅋㅋ

### 테스트 시작
- GPT와 동일하게 개발하고 있는 MyTimeLite 전체 파일을 PR 돌려보자.
- Comment로 리뷰 달아줄줄 알았는데 Summary만 요약해줘서 사용법 더 찾는중!
  - 웹 페이지에 있는 설정 가져와서 yaml파일에 업데이트 했다. 설정이 엄청 많아졌는데 PR 한번 해보자!


### 테스트 결과
음... 오묘하다.... 

만족했던 것   
- 당연하게도 위에 GPT보다는 자세하게 코멘트를 준다. 
  - 해당 파일에 11~13줄에 이거 말고 다른건 어때요? 이런식으로 준다!
- post하지 않은 중요도가 떨어지는 코멘트도 몰아서 준다.
- summary를 써주는게 프로젝트를 파악하고 써주는 느낌이다.
  - 근데 이건 뭐 내가 써도 되니까..;;

불만족했던 것
- 일단 깃헙액션에 실시간으로 안뜨는거 같은데 언제 끝날지를 모른다는 단점(workflow를 넣었는데도 나중에 나온다!!!!)
- 얼마나 걸리는지 모르지만... GPT 보다는 오래걸린다..
- 코멘트 하나에 뭉쳐서 코드리뷰를 해줘서 보기 힘들다(분명히 예시에는 하나씩 나오는거 같은데 내가 설정을 잘못한 모양이다)
- 코멘트를 하나하나씩 달아주는 것도 있고 하나에 뭉쳐서 주는 경우가 있어서 보기가 조금 불편하다.
- 무료라서 그런지 뭐만하면 리밋에 걸린다..ㅠ  20분에 한번씩 해야 되는거 같다...(근데 나 Pro trial인데..?)

### 결론
음... 뭐 코멘트 달면서 얘기하는건 좋은데... 한달에 몇번 안할거 같은 PR에 15달러 쓰기는 좀...
GPT 사용하면 PR만 하면 한번에 0.01달러도 안나올텐데... 굳이... 라는 생각이 든다.   
GPT보다 사용성은 불편하고 느린데 비싸기까지 하니 수고링!

## 4. githubnext에서 제공하는 Copilot for Pull Requests 알아보자
이것만 쓰는건 불가능하고 github 코파일럿 엔터프라이즈를 써야지 쓸 수 있는것 같다.  
엔터프라이즈는 돈을 내야 되는것 같으므로 그냥 패스 했다.  
지금도 코파일럿을 많이 사용하고 있지 않아서 해지할 까 생각하는데 여기에 돈 쓰기는 돈낭비인거 같다.  

## 5. pr-agent알아보기.  
이게 2번에서 사용한 GPT를 사용해서 깃헙액션과 연동하는 것이였다.   


## GPT vs CodeRabbit
- GPT는 50개의 코멘트에 여러개가 적혀있었다면 코드레빗은 130개 정도 고치라고 코멘트줌.
  - 해당 파일을 읽어봤는데! 이거이거 좋고 이거이거이거 고치세요! 
- 코드레빗은 해당 파일 10번째줄 안좋은거 같아요 이걸로 고치세요!


# 결론
음... 
GPT 사용법 압승. 가성비 압승. 속도 압승
CodeRabbit 디테일 살짝 승

둘 다 아직 보완해야 될 부분이 많이 보이지만 내 상황에 맞게 GPT를 사용하자.   
코드레빗은 무료일때는 열심히 사용하다가 탈퇴하는걸로 ㅋㅋ
