# TCA Scope가 있는데 ifLet을 왜 사용하는 건데?

## ifLet 사용하는건데..?
둘다 Embeds a child reducer in a parent domain 라고 하는데 
ifLEt은 옵셔널이 가미되었다고 함.


그러면 ifLet을 사용할 필요가 없는거 아닌가?   
왜냐하면 scope의 state도 optional을 사용해서 옵셔널로 사용하고 있음..ㅋㅋ

ifLet을 사용해도 scope로 열고 있는데 ifLet 왜사용하는거지?  


## 자식 Reducer를 처리하고 싶을 때 사용하면 좋을듯
```Swift
        .ifLet(\.$loginState, action: \.loginAction) {
            LoginStore()
        }

        .fullScreenCover(store: store.scope(state: \.$loginState, action: \.loginAction), content: { store in
            LoginView(store: store)
        })
```

이렇게 사용하고 있는데 ifLet이 없으면 LoginStore에서 리듀서에 잡히질 않음.
LoginStore를 만든 부모 Store에서 모든것을 처리해줘야 하는데 절레절레임
왜냐하면 로그인View의 Action을 LoginView를 오픈한 ScreenView에서 처리할 수가 없음.

결국은 부모에서 자식을 오픈하는데 scope는 무조건 사용하고
부모 + 자식에서 액션을 처리하고 싶으면 ifLet 사용하고 
부모에서만 액션을 처리할 수 있다면 scope만 사용하면 될듯.


## ifLetStore랑 엮으면 뷰를 보여주고 싶을 때만 보여줄수 있다.
