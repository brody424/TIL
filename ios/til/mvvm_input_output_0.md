# MVVM + RxSwift를 편하게 사용하기 위해서 Input, Output 패턴을 사용하는게 무조건 좋을까?

지금까지 나는 View에서 ViewModel을 갖고있고 View에서 ViewModel의 함수를 직접 호출하였다.  
예를 들면 로그인 버튼을 눌렀다면 아래의 코드처럼 작성했다. 
``` swift
viewModel.loginButtonTapped()
```

그런데 MVVM패턴과 RxSwift를 같이사용하는 방법중 Input, Output을 정의해서 사용하는 방법이 있다는것을 알았다.
``` swift
protocol BaseViewModel {
    associatedtype Input
    associatedtype Output
    
    var disposeBag: DisposeBag { get set }
    
    func transform(input: Input) -> Output
}

class LoginViewModel {
    struct Input {
        let kakaoLoginTapped: Observable<Void>
    }

    struct Output {
        let labelText: Driver<String>
    }
    ...
}
```
위의 코드처럼 Input, Output 처럼 정의하고 개발하는 것이다.

위의처럼 장점은 Input, Output을 직관적으로 볼 수 있다는 점이다. 

직관적이고 편한건 OK...

그런데 MVVM은 1:N의 관계를 가진다. 때로는 N:N의 관계를 가질 수도있다고 한다.

만약 1:N의 관계일 때 Input, Output를 어떻게 정의하는게 좋을까?

짧게 생각한 방법으로는   
1. Input, Output에 여러 화면에서 사용되는 Input, Output을 정의해놓는다
2. 각 화면마다 Input, Output을 따로 만든다.

2가지 방법 모두 마음에 들지 않는다.

만약 QuizListVC, QuizDetailVC라는 화면이 있고 QuizViewModel에서 관리한다고 생각해보면 Input, Output을 사용해서 코드가 깔끔해지는 방법이 당장은 생각이 나지 않는다.

그래서 ViewModel과 View의 관계가 1:1로 사용이 가능하면 Input, Output을 사용하고
1:N, N:N의 관계에서는 예전처럼 직접 호출하고 View에서 ViewModel을 바인딩해서 사용하면서 좋은 방법을 찾아보려고 한다.



