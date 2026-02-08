+++
title = '[Apple Platforms] scenePhase'
date = 2026-02-08T00:00:00+09:00
draft = false
tags =  ["iOS", "Apple Platforms"]
categories = ["Apple Platforms"]
summary = ""
+++

## Motivation
- 앱 개발 중 네트워크 상태 확인이나 인증 토큰 갱신 같은 로직을 포그라운드 복귀 시점에 실행해야하는 경우
	- UIKit에서는 `SceneDelegate`의 `sceneWillEnterForeground`, `sceneDidBecomeActive`, `sceneDidEnterBackground` 등 구현 필요 
	- SwiftUI에서는 `\.scenePhase`라는 Environment 값으로 간단하게 처리할 수 있음

## Overview


## scenePhase란?
SwiftUI가 제공하는 `Environment` 값으로, 현재 Scene의 상태를 나타낸다.
- `.active` 앱이 포그라운드에 있고 사용자와 상호작용 가능
- `.inactive` 앱이 포그라운드에 있지만 상호작용 불가 (예: 멀티태스킹 전환 중)
- `.background` 앱이 백그라운드로 이동한 상태


## 사용
```swift
@Environment(\.scenePhase) var scenePhase

var body: some View {
    ContentView()
        .onChange(of: scenePhase) { oldPhase, newPhase in
            switch newPhase {
            case .active:
                print("앱 활성화")
            case .inactive:
                print("앱 비활성화")
            case .background:
                print("백그라운드로 이동")
            @unknown default:
                break
            }
        }
}
```
> 참고
> `onChange(of:)` 클로저에서 `oldPhase`, `newPhase` 두 파라미터를 받는 형태는 **iOS 17+** 문법이다. iOS 16 이하를 지원해야 한다면 단일 파라미터 형태(`{ phase in ... }`)를 사용해야 한다.

## `.onAppear` vs `scenePhase` pros and cons

`onAppear`
- Pros
	- 특정 뷰가 화면에 나타날 때만 실행되므로 뷰 단위 제어에 적합
	- 탭 전환이나 네비게이션 push/pop 시에도 호출
	- 뷰마다 서로 다른 로직을 적용할 수 있다.

- Cons
	- 백그라운드 → 포그라운드 복귀 시에는 호출되지 않음.
	- 같은 뷰가 여러 번 나타나면 중복 호출
	- 앱 전체 상태 관리 용도로는 부적합

`scenePhase`
- Pros
	- 앱 전체 생명주기 변화를 한 곳에서 관리할 수 있음
	- 앱 최초 실행과 포그라운드 복귀를 동일한 코드로 처리 가능
	- 상태 전환당 한 번만 호출되므로 중복 호출 걱정 없음

- Cons
	- 뷰 단위 세밀한 제어 불가
	- 탭 전환 같은 앱 내부 이동은 감지 못함
	- 루트 뷰에서만 사용하는 게 일반적

사용 예시
- 상황추천네트워크 연결/인증 상태 확인 → `scenePhase`
- 특정 화면 진입 시 데이터 로드 → `onAppear` / `task`
- 앱 전체 상태 동기화 → `scenePhase`
- 화면별 애널리틱스 트래킹 → `onAppear`

요약
- 앱 레벨 → `scenePhase`
- 뷰 레벨 → `onAppear`

## task vs onAppear

- Apple은 iOS 15부터 `task` modifier를 도입
	- 비동기 작업이 필요한 경우 `onAppear` 안에서 `Task { ... }`를 직접 생성하는 것보다 `task`를 사용하는 편이 낫다
	- `task`의 가장 큰 장점인 뷰의 생명주기와 작업의 생명주기의 자동 연동
	- 뷰가 사라지면 진행 중인 비동기 작업이 자동으로 취소되므로, 화면을 빠르게 이탈했을 때 불필요한 네트워크 요청이 계속되는 문제 방지

```swift
// task 사용 (iOS 15+)
struct ContentView: View {
    var body: some View {
        List(items) { item in
            Text(item.name)
        }
        .task {
            await loadData()
        }
    }
}
```

## Apple에서 권장하는 방식?
위에서 "권장"이라고 표현한 부분들은, Apple이 공식 문서에서 "반드시 이렇게 해야 한다"고 명시한 것은 아님
- `scenePhase` 문서는 _"You can observe the current scene phase in your app's App instance."_ 라고 사용 방법을 설명할 뿐.
- `task` 문서도 _"Use this modifier to perform an asynchronous task with a lifetime that matches the lifetime of the modified view."_ 라고 적합성을 설명.
- WWDC 세션들 역시 "이렇게 쓸 수 있다"는 예시를 보여주는 것이지 규범적 표현은 아님.

**공식 문서와 WWDC 예제 코드에서 사용하는 패턴이자, 커뮤니티에서 통용되는 베스트 프랙티스**로 이해


### References 👀
- [scenePhase](https://developer.apple.com/documentation/swiftui/scenephase)
- [task modifier](https://developer.apple.com/documentation/swiftui/view/task(priority:_:))
- [WWDC20 - App Essentials in SwiftUI](https://developer.apple.com/videos/play/wwdc2020/10037/)
- [WWDC21 - Discover concurrency in SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10019/)
- [WWDC22 - The SwiftUI cookbook for navigation](https://developer.apple.com/videos/play/wwdc2022/10054/)
- [Fruta](https://developer.apple.com/documentation/swiftui/fruta_building_a_feature-rich_app_with_swiftui)