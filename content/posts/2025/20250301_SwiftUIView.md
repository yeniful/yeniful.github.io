+++
title = '[SwiftUI] struct View vs @ViewBuilder 함수, 뭐가 다를까?'
date = 2025-03-01T00:00:00+09:00
draft = false
tags = ["SwiftUI", "iOS"]
categories = ["iOS"]
summary = ""
+++

## 모티베이션

> "재사용되는 View를 구조체로 빼야 하나, 아니면 그냥 함수로 만들어도 될까?"

 View를 만들 때 특별한 기준 없이 View를 만들 때 습관적으로 구조체를 만들거나, 간단한 건 함수로 처리하곤 했는데요. 앱의 구조와 성능, 유지보수 방식에 꽤 중요한 영향을 준다는 것을 알게 되었고, 간단한 내용이지만 한 번 정리해보고 싶었습니다.

그래서 이번 글에서는 Apple 공식 문서와 WWDC 세션에서 강조한 원리를 바탕으로 **struct View vs. @ViewBuilder 함수**의 차이를 정리해보려고 합니다.

---

## 두 가지 방식 살펴보기

먼저 동일한 UI를 두 가지 방식으로 만들어봤습니다.

**구조체 View로 분리**
```swift
struct ProfileCard: View {
    let name: String
    let role: String

    var body: some View {
        VStack(alignment: .leading, spacing: 8) {
            Text(name)
                .font(.headline)
            Text(role)
                .font(.subheadline)
                .foregroundColor(.secondary)
        }
        .padding()
        .background(Color(.systemBackground))
        .cornerRadius(12)
    }
}

// 사용
ProfileCard(name: "예니", role: "iOS Developer")
```

**함수로 분리**
```swift
@ViewBuilder
func profileCard(name: String, role: String) -> some View {
    VStack(alignment: .leading, spacing: 8) {
        Text(name)
            .font(.headline)
        Text(role)
            .font(.subheadline)
            .foregroundColor(.secondary)
    }
    .padding()
    .background(Color(.systemBackground))
    .cornerRadius(12)
}

// 사용
profileCard(name: "예니", role: "iOS Developer")
```

겉보기에 유사해 보이지만, SwiftUI 내부에서는 이 둘을 다르게 처리합니다.

---

## 6가지 차이

### 1. 상태(State)를 가질 수 있는가

SwiftUI에서 상태(@State, @Binding, @StateObject)는
**View의 identity와 lifetime에 연결되는 값**입니다.

그리고 SwiftUI는 **구조체로 만든 View에만** 이러한 상태 저장소를 생성합니다.
즉, 상태는 **View 구조체 내부에서만 선언할 수 있습니다.**

```swift
struct ExpandableCard: View {
    let title: String
    @State private var isExpanded = false  // ✅ 자체 상태 보유
    var body: some View {
        VStack {
            Button(title) {
                isExpanded.toggle()
            }
            if isExpanded {
                Text("상세 내용입니다")
            }
        }
    }
}
```

반면 **함수**는 identity가 없으므로 자체 상태 저장소를 가질 수 없습니다.
상태가 필요하다면 부모 View에서 관리하고 **Binding**으로 넘겨받아야 합니다.
(identity에 대해서는 다음 항목에서 설명하겠습니다.)

```swift
// 함수는 자체 @State를 가질 수 없음
@ViewBuilder
func expandableCard(title: String, isExpanded: Binding<Bool>) -> some View {
    VStack {
        Button(title) {
            isExpanded.wrappedValue.toggle()
        }

        if isExpanded.wrappedValue {
            Text("상세 내용입니다")
        }
    }
}
```

즉, struct View는 SwiftUI가 관리하는 state storage를 갖고 있지만,
함수 기반 View는 갖고 있지 않아서 state를 가질 수 없습니다.

### 2. View Identity

WWDC21의 [Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022/) 에서 SwiftUI의 핵심 원리로 세 가지를 소개합니다
- ***Identity***
- ***Lifetime***
- ***Dependencies***

여기서 Identity는 다음을 의미합니다:
> "Identity is how SwiftUI recognizes elements as the same or distinct across multiple updates of your app."

즉, Identity란 SwiftUI가 앱의 여러 업데이트에서 **요소를 같은 것 또는 다른 것으로 인식하는 방식**입니다.

SwiftUI는 두 가지 방식으로 View를 식별합니다.
- ***Explicit Identity*** : 개발자가 직접 제공하는 식별자 (예: `ForEach`의 `id`, `.id()` modifier)
- ***Structural Identity*** : **View의 타입**과 계층 구조 내 위치로 암묵적으로 부여되는 식별자

**구조체**로 만든 View는 고유한 타입이 있기 때문에 identity가 있습니다.
즉, SwiftUI는 구조체로 만든 View에 명확한 structural identity를 부여합니다.

따라서 SwiftUI는 "이 ProfileCard는 이전에 있던 같은 ProfileCard다."를 인식합니다.

```swift
struct ParentView: View {
    @State private var count = 0
    var body: some View {
        VStack {
            Text("Count: \(count)")
            Button("증가") { count += 1 }
            // 고유 identity를 가지는 ProfileCard
            ProfileCard(name: "김개발", role: "iOS")
        }
    }
}
```

하지만 **함수**로 만든 View는 매 렌더링마다 "새로 구성된 View"로 취급됩니다.


Identity는 직전에 설명드린 State와도 관련이 있습니다. 
WWDC21 세션에서는 State와 Identity의 관계를 명확히 설명합니다:

SwiftUI가 View를 보고 State나 StateObject를 발견하면, 해당 데이터를 View의 lifetime 동안 유지해야 한다는 것을 인지합니다.
> "When SwiftUI is looking at your view and sees a State or a StateObject, it knows that it needs to persist that piece of data throughout the view's lifetime."

 State의 지속성(Persistence)은 View의 lifetime에 연결되어 있습니다.
> "The persistence of your state is tied to the lifetime of your views."

구조체로 만든 View와 달리 함수로 만든 View는 단순히 View를 반환하는 코드일 뿐, 
별도의 identity를 형성하지 않습니다.
함수를 호출하는 부모 View가 다시 그려질 때마다 함수도 재호출됩니다.

### 3. @ViewBuilder의 역할

[ViewBuilder 문서](https://developer.apple.com/documentation/swiftui/viewbuilder)에 따르면, ViewBuilder는 "클로저로부터 View를 구성하는 커스텀 파라미터 속성"입니다.
WWDC 세션에서는 함수에 `@ViewBuilder`를 적용하는 방법도 소개합니다:

> "Swift does not infer helper functions to be view builders by default, but we can opt into that by manually applying the ViewBuilder attribute ourselves."

하지만 `@ViewBuilder`를 함수에 적용해도 해당 함수가 구조체 View와 동일한 identity를 갖게 되는 것은 아닙니다. ViewBuilder는 단지 여러 View를 조합할 수 있게 해주는 문법적 편의 기능일 뿐입니다.

### 4. AnyView 회피와 성능

WWDC21 세션에서는 `AnyView`의 과도한 사용을 피하라고 권고합니다

> "In general, we recommend avoiding AnyViews whenever possible. Having too many AnyViews will often make code harder to read and understand... And because AnyView hides static type information from the compiler, it can sometimes prevent helpful diagnostic errors and warnings from being surfaced in your code. Finally, keep in mind that using AnyView when you don't need to can result in worse performance."

함수에서 여러 타입의 View를 반환하려면 `AnyView`로 감싸거나 `@ViewBuilder`를 사용해야 합니다. 구조체를 사용하면 각 View가 명확한 타입을 가지므로 이러한 문제를 피할 수 있습니다.

### 5. View Life Cycle

함수로 반환한 View에도 `onAppear`, `onDisappear`, `task` 같은 생명주기 modifier를 붙일 수는 있습니다. 하지만 함수는 독립적인 identity가 없으므로, 해당 modifier는 부모 View의 생명주기를 따르게 됩니다.

반면 **구조체**는 자체적인 identity와 lifetime을 가지므로, 생명주기 modifier가 해당 View의 등장/퇴장 시점에 정확히 동작합니다.

```swift
struct DataFetchingView: View {
    @State private var data: [Item] = []

    var body: some View {
        List(data) { item in
            Text(item.name)
        }
        .onAppear {
            // View가 나타날 때 데이터 로드
        }
        .task {
            // 비동기 작업 수행
        }
    }
}
```

---

## 정리: 언제 무엇을 선택할까

| 상황 | 선택 | 근거 |
|------|------|------|
| 자체 상태(`@State` 등)가 필요할 때 | **구조체** | 함수는 상태를 가질 수 없음 |
| 여러 화면에서 재사용할 컴포넌트 | **구조체** | 독립적인 identity와 타입으로 관리 가능 |
| `onAppear`, `task` 등 생명주기 활용 | **구조체** | 의미 있는 lifetime 보장 |
| 복잡한 로직이 포함된 View | **구조체** | 명확한 identity로 성능 최적화 |
| 같은 View 내에서 단순 중복 제거 | **함수** | 빠르고 간편함 |
| 상태 없는 아주 작은 UI 조각 | **함수** | 오버헤드 없이 간단히 처리 |

---

## 결론

**기본적으로 구조체 친화적인 SwiftUI는 View를 만들 때 구조체 사용을 권장합니다.**
그리고 상황적으로 유연하게 함수로 View를 그리는 것이 가능합니다.
- **재사용·확장성·상태·생명주기가 필요하다 → struct**
- **한 화면 안에서의 작은 UI 조각 → 함수**

SwiftUI는 Identity, Lifetime, Dependencies라는 원리를 기반으로 작동하는데요,
이 관점에서 보면 struct View는 SwiftUI가 기대하는 방식에 자연스럽게 맞춰져 있고,
함수 기반 View는 역할이 제한적입니다.

함수는 "같은 View 안에서 반복되는 작은 UI 조각을 빠르게 추출"하는 용도로 제한적으로 사용하면 좋습니다. 
만약 함수가 점점 복잡해지거나, 상태가 필요해지거나, 다른 곳에서도 쓰이게 된다면 그때 구조체로 승격시키면 됩니다.

---

## 참고 자료
- [WWDC21: Demystify SwiftUI](https://developer.apple.com/videos/play/wwdc2021/10022/) 
- [Apple Developer Documentation: View](https://developer.apple.com/documentation/swiftui/view)
- [Apple Developer Documentation: State](https://developer.apple.com/documentation/swiftui/state)
- [Apple Developer Documentation: ViewBuilder](https://developer.apple.com/documentation/swiftui/viewbuilder)
- [WWDC24: SwiftUI essentials](https://developer.apple.com/videos/play/wwdc2024/10150/)
