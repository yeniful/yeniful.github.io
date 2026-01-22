+++
title = '[UIKit] Scene Delegate'
date = 2026-01-01T00:00:00+09:00
draft = false
tags =  ["UIKit", "Apple", "iOS"]
categories = ["Apple Platforms"]
summary = "iOS 13부터 도입된 SceneDelegate의 역할, 도입 배경, 각 메서드의 동작 원리 알아보기."
+++

## Motivation
- iOS 13+ 씬 기반 UIKit 프로젝트는 `AppDelegate`와 `SceneDelegate`가 함께 생성된다.
- Storyboard를 쓰는 기본 템플릿에서는 시스템이 `UIWindow`를 생성/연결한다.
- Programmatic UI를 쓰면 `scene(_:willConnectTo:)`에서 `UIWindow`를 직접 구성해야 한다.
- SceneDelegate의 역할은 무엇일까?

## Overview
- `AppDelegate`는 **프로세스 수준 이벤트**와 **Scene session 생성/폐기**를 관리한다.
- `SceneDelegate`는 **각 Scene의 UI 생명주기와 UI 설정**을 관리한다.
- iOS 12까지 `AppDelegate`가 UI 생명주기까지 담당했지만, iOS 13부터 하나의 앱이 여러 UI 인스턴스(Scene)를 가질 수 있게 되면서 역할이 분리되었다.
- 씬 기반 라이프사이클을 채택하면 UIApplicationDelegate의 UI 관련 콜백은 호출되지 않고, SceneDelegate 메서드가 1:1로 대응된다.
- 이 글에서는 `SceneDelegate`의 역할과 각 메서드의 동작 원리를 알아본다.

## SceneDelegate란?
- iOS 13부터 도입된 앱의 화면(UI) 생명주기를 관리하는 객체.
- AppDelegate에서 UI 상태 관리 역할이 분리되어 나왔다.

## 도입 배경
- **WWDC 2019**에서 iOS 13 / iPadOS 13의 **Scene 기반 라이프사이클**과 iPad 멀티 윈도우가 소개되었다.
- 멀티 윈도우를 지원하려면 각 화면 인스턴스(Scene)마다 독립적인 UI 생명주기 관리가 필요했다.

#### 기존 AppDelegate 구조의 한계 (WWDC19 Session 258)
> *"In iOS 12 and earlier, your application had **one process** and also just **one user interface instance** to match it."*
> *"Applications now still share **one process** but may have **multiple user interface instances or scene sessions**."*

- iOS 12 이전: AppDelegate의 `didEnterForeground`, `willResignActive` 등의 메서드는 **앱 전체에 대해 한 번만 호출**됨
- iOS 13 이후: 같은 앱의 창이 여러 개 열려있으면, 각 창이 **독립적으로** foreground/background 전환을 해야 함
- **한계**: AppDelegate 하나로는 "어떤 UI가 foreground로 왔는지" 구분할 수 없음
- **해결**: UI 생명주기 관리를 Scene 단위로 분리 → SceneDelegate 도입

|               | iOS 12 이전                | iOS 13 이후                            |
| ------------- | ------------------------ | ------------------------------------ |
| 구조            | 1 App = 1 Process = 1 UI | 1 App = 1 Process = **여러 UI(Scene)** |
| AppDelegate   | 프로세스 생명주기, UI 상태 모두 관리   | 프로세스 생명주기 + **Scene session 생성/폐기** 관리 |
| SceneDelegate | 없음                       | 각 Scene의 UI 생명주기 + UI 설정/복원          |

> 참고: iOS 9의 Split View는 **다른 앱**을 나란히 보는 것이고, iOS 13의 멀티 윈도우는 **같은 앱의 여러 인스턴스**를 동시에 보는 것이다.

## Scene 관련 클래스 구조

```
UISceneSession ──────── UIScene ◀── UIWindowScene
       │                                   │
       │ .scene 프로퍼티로 참조                │ .windows 프로퍼티로 관리
       │                                   ▼
       │                              UIWindow
       │                                   │
       └─ 메타정보/상태 관리                    ▼
          (state restoration 등)     rootViewController
```

| 클래스 | 역할 |
|---|---|
| **UISceneSession** | Scene의 세션 메타정보를 담고, 시스템이 세션을 추적/유지한다. |
| **UIScene** | 앱 UI의 하나의 인스턴스를 나타내는 추상 클래스. |
| **UIWindowScene** | UIScene의 구체적인 서브클래스. UIWindow를 표시할 환경을 제공한다. |
| **UIWindow** | 앱의 UI를 담는 컨테이너. rootViewController를 통해 화면에 뷰를 표시한다. |

> SceneDelegate는 `UIWindowSceneDelegate` 프로토콜을 채택하여 UIWindowScene의 생명주기 이벤트를 처리한다.

#### Scene ≠ Window
- **Scene과 Window는 다른 개념이다.**
- **Scene**은 앱 UI 인스턴스(세션)이고, **Window**는 화면에 그려지는 컨테이너다.
- 멀티 윈도우(iPadOS)는 같은 앱의 여러 `Scene`을 동시에 보여주는 것에 가깝다.
- 보통 1 `Scene` = 1 `UIWindow 구성으로 사용하는 경우가 많다.

## Scene Configuration (Info.plist / 코드)
- 시스템은 새 Scene을 만들기 전에 `application(_:configurationForConnecting:options:)`를 호출해 어떤 Scene 구성을 쓸지 묻는다.
- 이 구성에는 **SceneDelegate 클래스**, **Storyboard 이름**, (선택적으로) **Scene 서브클래스**가 포함된다.
- 구성은 Info.plist에 **정적으로 선언**하거나 런타임에 **동적으로 생성**할 수 있다.
- `options`에는 user activity, URL context 등이 포함되어 어떤 Scene을 열지 결정하는 힌트를 제공한다.

## SceneDelegate 메서드
- 프로젝트를 생성했을 때 `SceneDelegate.swift `파일 구성은 다음과 같다.
```
import UIKit

class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    
    var window: UIWindow?
	
	func scene(_ scene: UIScene, 
				willConnectTo session: UISceneSession, 
				options connectionOptions: UIScene.ConnectionOptions) {
	
		guard let _ = (scene as? UIWindowScene) else { return }

    }

    func sceneDidDisconnect(_ scene: UIScene) { ... }
    func sceneDidBecomeActive(_ scene: UIScene) { ... }
    func sceneWillResignActive(_ scene: UIScene) { ... }
    func sceneWillEnterForeground(_ scene: UIScene) { ... }
    func sceneDidEnterBackground(_ scene: UIScene) { ... }

}
```

- 각 메서드의 기본 주석들을 함께 읽어보자
##### func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) { ... }

```
- Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
- If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
- This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
```
- `scene()` 메서드를 통해 `UIWindow`를 설정하고 `UIWindowScene`에 연결하세요.
- Storyboard를 사용하면 `window` 프로퍼티가 자동으로 초기화되고 `scene`에 연결됩니다.
- 이 delegate가 호출된다고 해서 연결되는 scene이나 session이 새로운 것은 아닙니다.

> scene이 앱에 **연결될 때** 호출된다. Programmatic UI를 사용하면 이 메서드에서 UIWindow를 직접 생성하고 rootViewController를 설정해야 한다.

##### func sceneDidDisconnect(_ scene: UIScene) { ... }
```
- Called as the scene is being released by the system.
- This occurs shortly after the scene enters the background, or when its session is discarded.
- Release any resources associated with this scene that can be re-created the next time the scene connects.
- The scene may re-connect later, as its session was not necessarily discarded (see `application:didDiscardSceneSessions` instead).
```
- `sceneDidDisconnect()`는 시스템이 scene을 **해제할 때** 호출됩니다.
- 백그라운드 진입 직후일 수도 있지만, 시스템이 필요할 때 **언제든** 호출될 수 있습니다.
- 다음에 scene이 다시 연결될 때 재생성할 수 있는 리소스를 해제하세요.
- scene이 나중에 다시 연결될 수 있습니다. 세션이 반드시 삭제되는 것은 아닙니다.

> 시스템이 리소스를 회수하기 위해 scene을 해제할 때 호출될 수 있다. 완전히 종료된 것이 아니므로, 나중에 다시 연결될 수 있다.

##### func sceneDidBecomeActive(_ scene: UIScene) { ... }
```
- Called when the scene has moved from an inactive state to an active state.
- Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
```
- scene이 비활성 상태에서 **활성 상태로 전환될 때** 호출됩니다.
- scene이 비활성 상태일 때 일시 중지되었거나 아직 시작되지 않은 작업을 재시작하세요.

##### func sceneWillResignActive(_ scene: UIScene) { ... }
```
- Called when the scene will move from an active state to an inactive state.
- This may occur due to temporary interruptions (ex. an incoming phone call).
```
- scene이 활성 상태에서 **비활성 상태로 전환될 때** 호출됩니다.
- 일시적인 중단(예: 전화 수신)으로 인해 발생할 수 있습니다.

##### func sceneWillEnterForeground(_ scene: UIScene) { ... }
```
- Called as the scene transitions from the background to the foreground.
- Use this method to undo the changes made on entering the background.
```
- scene이 백그라운드에서 **포그라운드로 전환될 때** 호출됩니다.
- 이 메서드를 통해 백그라운드 진입 시 만든 변경사항을 되돌리세요.

##### sceneDidEnterBackground(_ scene: UIScene) { ... }
```
- Called as the scene transitions from the foreground to the background.
- Use this method to save data, release shared resources, and store enough scene-specific state information to restore the scene back to its current state.
```
- `scene`이 포그라운드에서 **백그라운드로 전환**될 때 호출됩니다.
- 이 메서드를 통해 다음의 작업을 하세요.
	- 데이터 저장
	- 공유 리소스 해제
	- `scene`을 현재 상태로 복구하기에 충분한 씬별 상태 정보를 저장.

## Scene 생명주기 흐름

```
			┌─────────────────────────────────────┐
			│           App Launch                │
			└─────────────────┬───────────────────┘
							  ▼
			┌─────────────────────────────────────┐
			│   scene(_:willConnectTo:options:)   │
			│           Scene 연결                 │
			└─────────────────┬───────────────────┘
							  ▼
			┌─────────────────────────────────────┐
			│     sceneWillEnterForeground(_:)    │
			│         포그라운드 진입 예정             │
			└─────────────────┬───────────────────┘
							  ▼
  ┌─────────────────────────────────────────────────────────┐
  │                  sceneDidBecomeActive(_:)               │
  │                      활성 상태 전환                       │
  └───────────────────────────┬─────────────────────────────┘
							  │
				  ┌───────────┴───────────┐
				  ▼                       ▼
┌──────────────────────────┐   ┌──────────────────────────┐
│ sceneWillResignActive(_:)│   │  (사용자가 앱 사용 중)     │
│    비활성 상태 전환 예정       │   │                          │
└────────────┬─────────────┘   └──────────────────────────┘
			 ▼
┌──────────────────────────┐
│ sceneDidEnterBackground  │
│      백그라운드 진입         │
└────────────┬─────────────┘
			 ▼
┌──────────────────────────┐
│  sceneDidDisconnect(_:)  │
│    시스템이 Scene 해제       │
└──────────────────────────┘
```

> **참고**: `sceneWillEnterForeground`는 정의상 **백그라운드 → 포그라운드 전환** 시 호출된다. 첫 연결 직후 이어지는 흐름에서도 호출될 수 있지만, 공식 의미는 “foreground 진입”이다.  
> **참고**: `sceneDidDisconnect`는 백그라운드 진입 직후뿐 아니라 **언제든 호출될 수 있으며**, 항상 발생하는 단계는 아니다.

## 메서드 활용 예시

| 메서드 | 활용 예시 |
|---|---|
| `scene(_:willConnectTo:options:)` | UIWindow 생성, rootViewController 설정 |
| `sceneDidDisconnect(_:)` | 재생성 가능한 리소스 해제, 캐시 정리 |
| `sceneDidBecomeActive(_:)` | 타이머 재시작, 애니메이션 재개, 데이터 새로고침 |
| `sceneWillResignActive(_:)` | 게임 일시정지, 타이머 중지, 진행 중인 작업 저장 |
| `sceneWillEnterForeground(_:)` | 블러 처리 제거, UI 새로고침 |
| `sceneDidEnterBackground(_:)` | 민감한 정보 블러 처리, 데이터 저장, 상태 정보 저장 |

## Storyboard 대신 Programmatic UI 구현하기
- Storyboard를 사용하지 않고, 코드를 통한 Programmatic UI 를 구현하려면 먼저 Storyboard가 연결된 값들을 없애주어야 한다.
	- Info.plist의 Scene Configuration에서 Storyboard Name을 제거한다. ![[Screenshot 2026-01-22 at 18.11.26.png]]
- SceneDelegate의 가장 첫번 째 메서드인  `func scene()`을 다음과 같이 수정해준다.
```
    var window: UIWindow?
    
	func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {

		guard let windowScene = (scene as? UIWindowScene) else { return }
		let window = UIWindow(windowScene: windowScene) // 객체 생성
		window.rootViewController = ViewController()
		window.makeKeyAndVisible()
		self.window = window // 생성 후나중에 할당
	}
```

- 주로 아래의 방식으로 할당했었는데, 위처럼 객체를 완성하고 나중에 할당하는 패턴이 Swift에서 권장되는 패턴이라고 한다.
```
guard let windowScene = (scene as? UIWindowScene) else { return }
window = UIWindow(windowScene: windowScene)
window?.rootViewController = ViewController()
window?.makeKeyAndVisible()
```

>
추가 설명
> - Scene Configuration에 storyboard가 지정되어 있으면, 시스템이 해당 storyboard로 window를 생성/연결한다.
> - Info.plist / Scene Configuration / Storyboard Name 값에서 시작 스토리보드를 지정해줄 수 있다.
> - 기본 템플릿에서는 ViewController가 초기 루트로 설정되어 있다.


## Q&A

#### Q: iOS 13 미만을 지원하려면 SceneDelegate를 어떻게 처리해야 할까?
- `@available(iOS 13.0, *)` 어노테이션을 SceneDelegate 클래스에 추가한다.
- AppDelegate에서 `window` 프로퍼티를 선언하고, iOS 12 이하에서는 AppDelegate에서 UI를 설정한다.
- 백포트 시 AppDelegate/SceneDelegate를 **둘 다** 두면 UIKit이 런타임에 올바른 쪽을 호출한다.

#### Q: sceneDidDisconnect와 앱 종료는 같은 건가?
- **아니다.** `sceneDidDisconnect`는 시스템이 Scene을 **해제**할 때 호출된다. 백그라운드 직후뿐 아니라 언제든 호출될 수 있다.
- Scene의 세션(UISceneSession)은 유지될 수 있으며, 사용자가 다시 앱으로 돌아오면 Scene이 재연결될 수 있다.
- 세션이 **영구 폐기**되면 AppDelegate의 `application(_:didDiscardSceneSessions:)`에서 정리한다. 이 콜백은 다음 실행 직후 호출될 수도 있다.

#### Q: Foreground/Background와 Active/Inactive의 차이는?
- **Foreground/Background**: 앱이 화면에 보이는지 여부
  - Foreground: 화면에 보임
  - Background: 화면에 안 보임
- **Active/Inactive**: 앱이 이벤트를 받을 수 있는지 여부
  - Active: 터치 이벤트 등을 받을 수 있음
  - Inactive: 화면에 보이지만 이벤트를 받지 못함 (예: 전화 수신, 제어 센터 열기)

#### Q: 왜 scene(_:willConnectTo:)에서 guard let으로 UIWindowScene을 캐스팅할까?
- `scene` 파라미터의 타입은 `UIScene`(추상 클래스)이다.
- UIWindow를 생성하려면 구체적인 서브클래스인 `UIWindowScene`이 필요하다.
- 다운캐스팅에 실패하면 윈도우를 만들 수 없으므로 early return 한다.

#### Q: AppDelegate의 application(_:didFinishLaunchingWithOptions:)는 언제 호출되나?
- iOS 13 이후에도 **여전히 호출된다.**
- 앱 프로세스가 시작될 때 가장 먼저 호출되며, Scene 구성/연결보다 **먼저** 실행된다.
- 앱 전체에서 공유하는 초기 설정(Firebase, 로깅 등)은 여기서 하면 된다.

#### Q: iPhone(iOS)에서는 여러 Scene을 사용하지 않는 건가?
- iOS에서는 **일반적으로 하나의 Scene**을 설계하고, iPadOS/macOS에서는 여러 Scene을 고려한다.
- 하지만 SceneDelegate 구조가 iPhone에서도 기본 템플릿으로 제공되는 이유:
  - iPad와 **코드를 공유**할 때 유리함
  - 향후 확장성과 코드 일관성을 위해

#### Q: SwiftUI는 SceneDelegate를 어떻게 처리할까?
- SwiftUI는 SceneDelegate를 **직접 사용하지 않는다.**
- 대신 `App` 프로토콜과 `Scene` 프로토콜로 추상화되어 있다.
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
- `WindowGroup`은 SwiftUI가 제공하는 대표적인 Scene 컨테이너다.
- 생명주기 이벤트는 `scenePhase` 환경 변수로 감지할 수 있다.
```swift
@Environment(\.scenePhase) var scenePhase

var body: some View {
    ContentView()
        .onChange(of: scenePhase) { phase in
            switch phase {
            case .active: // sceneDidBecomeActive
            case .inactive: // sceneWillResignActive
            case .background: // sceneDidEnterBackground
            }
        }
}
```
- AppDelegate나 SceneDelegate가 필요하면 `@UIApplicationDelegateAdaptor`를 사용할 수 있다.

---

## 참고 (공식)
- WWDC19: Architecting Your App for Multiple Windows (Video)  
  https://developer.apple.com/videos/play/wwdc2019/258/
- WWDC19: Architecting Your App for Multiple Windows (Slides PDF)  
  https://devstreaming-cdn.apple.com/videos/wwdc/2019/258ggtahutefvsda35yt/258/258_architecting_your_app_for_multiple_windows.pdf
- UIApplicationDelegate (Scene configuration / discard sessions)  
  https://developer.apple.com/documentation/uikit/uiapplicationdelegate
- UISceneDelegate (Scene lifecycle)  
  https://developer.apple.com/documentation/uikit/uiscenedelegate
- SwiftUI Tutorial: Responding to events (Scene architecture / scenePhase)  
  https://developer.apple.com/tutorials/app-dev-training/responding-to-events
