+++
title = '[UIKit] ViewController 생명주기'
date = ==2026-01-17==T00:00:00+09:00
draft = ==true==
tags =  ["UIKit", "Apple", "iOS"]
categories = ["Apple Platforms"]
summary = ""
+++

## ViewController의 생명 주기란?
## 생명 주기 메서드
- `viewDidLoad` : 뷰가 메모리에 로드될 때 (1회)
- `viewWillAppear` : 화면에 나타나기 직전 (매번)
- `viewDidAppar` : 화면에 나타난 후 (매번)
- `viewWillDisappear` : 화면에서 사라지기 직전 (매번)
- `viewDidDisappear` : 화면에서 사라진 후 (매번)

## 사용 예시
```
override func viewDidLoad() { 
	super.viewDidLoad() 
	fetchUserData() // 이 곳에서 API 호출시 최초 1번만 데이터를 가지고 옵니다.
}

override func viewWillAppear(_ animated: Bool) {
	super.viewWillAppear(animated) 
	fetchUserData() // 이 곳에서 API 호출시 화면 돌아올 때마다 최신 데이터로 새로고침 합니다.
}
```

## Cases
### Case 1 : Push & Pop
**질문**
A화면에서 B화면을 `Push`했다가 `Pop`해서 다시 A로 돌아올 때 A에 대해 어떤 생명 주기 메서드들이 호출될까요?
**답변**
> viewDidLoad를 제외한 메서드들.
> A->B 이동을 위해 B를 푸시할 때 A는 화면에서는 사라지지만, Navigation Stack에 남아 A가 여전히 존재하기 때문입니다.

**Push 과정**
1. A: viewWillDisappear
2. B: viewWillAppear
3. (애니메이션 진행)
4. B: viewDidAppear
5. A: viewDidDisappear

**Pop 과정**
1. B: viewWillDisappear
2. A: viewWillAppear
3. (애니메이션 진행)
4. A: viewDidAppear
5. B: viewDidDisappear

### Case 2 : Present (Modal) & Dismiss
**질문**
A화면에서 B화면을 present했다가 dismiss한 경우 A 화면의 `viewWillAppear`가 호출될까요?

**답변**
> 화면이 완전히 가려지는 경우 호출되고, 뒤에 일부가 보이는 경우 호출되지 않습니다.
> 
> 🆗 호출되는 경우:
> - `.fullScreen`

**Present 과정**
1. A: viewWillDisappear
2. B: viewWillAppear
3. B: viewDidAppear
4. A: viewDidDisappear

**Dismiss 과정**
1. B: viewWillDisappear
2. A: viewWillAppear
3. A: viewDidAppear
4. B: viewDidDisappear


> ❌ 호출되지 않는 경우:
> - `.pageSheet` (기존의 `.automatic`, 거의 `.fullScreen`과 유사하지만 상단 살짝 보임)
> - `.formSheet` (iPhone에서는 `.pageSheet`과 동일하게 보이지만, iPad에서는 더 작게 보임)

**Present 과정**
1. A: viewWillDisappear ❌ 호출 안 됨 
2. A: viewDidDisappear ❌ 호출 안 됨 
3. B: viewWillAppear ✅ 
4. B: viewDidAppear ✅

**Dismiss 과정**
1. B: viewWillDisappear ✅ 
2. B: viewDidDisappear ✅ 
3. A: viewWillAppear ❌ 호출 안 됨 
4. A: viewDidAppear ❌ 호출 안 됨

### Case 3 : `.pageSheet`가 적용된 B를 `dismiss`하고나서 A화면의 데이터를 새로고침 하는 방법들
- `.fullScreen` 사용
- `Delegate`로 전달
- `NotifivationCenter` 사용
- Closure (Completion Handler) 사용

## 결론
- `viewDidLoad`에서 초기 설정(UI 구성, constraint 설정)을 하고, `viewWillAppear`에서 최신 데이터를 가져오는 로직을 다룹니다.
- 다만 `viewWillAppear`에서 무거운 작업시 버벅일 수 있기 때문에 비동기 처리를 해야합니다

```
// ❌ 지양
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    let data = fetchHugeDataSynchronously()  // 동기 작업 → 버벅임
    tableView.reloadData()
}

// ✅ 권장
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    Task {
        let data = await fetchData()  // 비동기 처리
        tableView.reloadData()
    }
}
```