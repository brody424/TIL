# ViewController에서 제공하는 self.view를 변경해보자.

개발을 할 때 self.view를 기본제공이 아닌 다른 view로 변경해야 할 일이 가아아아아아끔 있다.  

그럴 때 사용하는 방법은 loadView에서 사용할 view를 생성해주는것이다.

```Swift
    override func loadView() {
        self.view = CustomView()
    }
```
위에처럼 사용하면 된다.  

절대로 super.loadView()를 사용하면 안된다고 한다.   

