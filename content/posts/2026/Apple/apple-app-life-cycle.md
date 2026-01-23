+++
title = '[Apple Platforms] 앱 생명 주기 (App Life Cycle)  🖋️'
date = 2026-01-01T00:00:00+09:00
draft = false
tags =  ["iOS", "Apple Platforms"]
categories = ["Apple Platforms"]
summary = ""
+++

## Overview
- 앱 생명주기: 앱이 실행되는 시점부터 종료되는 시점까지 거치는 여러 상태의 순환 과정


## iOS 앱의 5가지 상태
1. Not Running (실행되지 않음)
2. Inactive (비활성)
3. Active (활성)
4. Background (백그라운드)
5. Suspended (일시 중지)


## SwiftUI에서의 앱 생명주기
- SwiftUI에서 앱은 `@main` attribute로 표시된 구조체에서 시작.
- `UIApplicationMain`의 대체
``` Swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```