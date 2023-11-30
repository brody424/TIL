# UITableViewDiffableDataSource와 RxSwift를 사용하는데 tableView.rx.itemSelected를 사용하면 indexPath의 값이 이상하게 나오는 이슈

테이블뷰의 LoadMore를 구현중에 발생한 이슈로 데이터를 추가로 받아와서 append후 Snapshot의 apply를 하면 cell의 indexPath가 꼬이는 현상이 발생한다.

<img src="https://github.com/brody424/TIL/assets/15370950/8c615f9f-d972-49f2-8a67-110c6e23b136" width="200">

위의 이미지에서 이름과 IndexPath.row를 보여주고 있는데 리로드하는 과정에서 뜬금없이 84번이 88번과 89번에 껴있는데,  
섞이는 애니메이션이 발생하면서 이런현상이 발생한다.  

---

기존에 RxDataSource를 사용했을 때는 아래처럼 개발했다.
```Swift
tableView.rx.modelSelected(GithubUserEntity.self)
    .bind { [weak self] user in
        // 선택한 셀의 User 데이터가 들어옴
    }.disposed(by: disposeBag)        
```

하지만 이제는 RxDataSource가 아닌 UITableViewDiffableDataSource를 사용하고 있는데 modelSelected를 하면 런타임오류(크래시)가 발생한다.  

그래서 아래처럼 itemSelected를 사용하고 있었다.

```Swift
tableView.rx.itemSelected
    .bind { [weak self] indexPath in
        guard let selectedItem = self?.dataSource?.itemIdentifier(for: indexPath) else { return }
        print("IndexPath : \(indexPath.row)")
        self?.viewModel.input.userTappedSubject.onNext(indexPath.row)
    }.disposed(by: disposeBag)
```

그런데 왠걸 테이블뷰를 아래로 스크롤하여 다음 페이지를 받아와서 SNapshot apply를 하면 간헐적으로 itemSelected의 인덱스가 꼬이는 현상이 발생했다.  
애니메이션이 있으면 자주꼬이고 애니메이션을 없애면 간헐적으로 꼬인다.  
방법을 찾아보니 __UITableViewDiffableDataSource__ 에서 itemIdentifier 메소드를 제공하고 있어고 이거를 사용하면 modelSelected처럼 사용할 수 있다!!!!   


## 해결방법
```Swift
tableView.rx.itemSelected
    .bind { [weak self] indexPath in
        guard let selectedItem = self?.dataSource?.itemIdentifier(for: indexPath) else { return }
        switch selectedItem {
        case .item(let user):
            self?.viewModel.input.userTappedSubject.onNext(user.id)
        case .noData:
            break
        }
    }.disposed(by: disposeBag)
```

