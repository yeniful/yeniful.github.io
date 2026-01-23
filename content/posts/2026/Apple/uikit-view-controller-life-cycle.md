+++
title = '[UIKit] ViewController 생명주기 (View Controller Life Cycle)'
date = 2026-01-17T00:00:00+09:00
draft = false
tags =  ["UIKit", "Apple Platforms", "iOS"]
categories = ["Apple Platforms"]
summary = "iOS ViewController의 생명주기 메서드와 Push/Pop, Present/Dismiss 상황별 호출 순서를 정리합니다."
+++

## ViewController의 생명 주기란?
ViewController의 생명 주기(Life Cycle)란 화면이 생성되고 사라지기까지의 과정을 말합니다. iOS는 이 과정의 특정 시점에 메서드를 호출하여 개발자가 적절한 작업을 수행할 수 있도록 합니다.

## 생명 주기 메서드
- `viewDidLoad` : 뷰가 메모리에 로드될 때 (1회)
- `viewWillAppear` : 화면에 나타나기 직전 (매번)
- `viewDidAppear` : 화면에 나타난 후 (매번)
- `viewWillDisappear` : 화면에서 사라지기 직전 (매번)
- `viewDidDisappear` : 화면에서 사라진 후 (매번)

## 사용 예시
```swift
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
- viewDidLoad를 제외한 메서드들.
- A->B 이동을 위해 B를 푸시할 때 A는 화면에서는 사라지지만, Navigation Stack에 남아 A가 여전히 존재하기 때문입니다.

**Push 과정**
1. A: viewWillDisappear
2. B: viewDidLoad (최초 1회)
3. B: viewWillAppear
4. (애니메이션 진행)
5. B: viewDidAppear
6. A: viewDidDisappear

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
- 화면이 완전히 가려지는 경우 호출되고, 뒤에 일부가 보이는 경우 호출되지 않습니다.
- iOS 13 이후 `modalPresentationStyle`의 기본값이 `.automatic`(대부분 `.pageSheet`)으로 변경되어, 별도 설정 없이 present하면 `viewWillAppear`가 호출되지 않습니다.

> 🆗 호출되는 경우:
> - `.fullScreen`

**Present 과정**
1. A: viewWillDisappear
2. B: viewDidLoad (최초 1회)
3. B: viewWillAppear
4. B: viewDidAppear
5. A: viewDidDisappear

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
- `NotificationCenter` 사용
- Closure (Completion Handler) 사용

## 활용
- `viewDidLoad`에서 초기 설정(UI 구성, constraint 설정)을 하고, `viewWillAppear`에서 최신 데이터를 가져오는 로직을 다룹니다.
- `viewWillAppear`에서 무거운 작업 시 버벅임이 발생할 수 있으므로 비동기 처리를 해야 합니다.
```swift
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