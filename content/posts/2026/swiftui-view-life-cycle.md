+++
title = '[SwiftUI] View 생명 주기 (View Life Cycle)  🖋️'
date = 2026-01-01T00:00:00+09:00
draft = true
tags =  ["SwiftUI", "Apple Platforms", "iOS"]
categories = ["Apple Platforms"]
summary = ""
+++

## Overview
- SwiftUI는 View modifier 를 통해 생명주기 이벤트를 처리.
	- modifier: View의 외형, 동작, 상태를 변경하는 일종의 메서드
	- UIKit의 ViewController 생명 주기의 메서드 기반 접근과 다름.
- SwiftUI는 3가지 핵심 개념으로 View의 생명 주기를 관리.
	- Indetify(식별)
	- Lifetime(생명주기) - Appearing, Updating, Disappearing
	- Dependencies(의존성)

## Method와 Modifier는 같은가요?
- 구현 수준(기술적 구현)은 '메서드'로 같음.
	- SwiftUI의 모디파이어는 실제로 View 프로토콜의 확장 메서드(extension method)
```
// View 프로토콜의 extension method
extension View {
    func padding(_ length: CGFloat? = nil) -> some View {
        // 구현...
    }
    
    func foregroundColor(_ color: Color?) -> some View {
        // 구현...
    }
}

```
- 사용 방식과 개념에서의 차이
	- SwiftUI에서 확장 메서드를 모디파이어라고 부르는 이유:
		- modifier가 메서드의 일반적인 의미와 달라서.
		- 일반적 의미의 메서드: 객체의 상태를 변경하거나 작업 수행
		- SwiftUI 모디파이어: 함수형 프로그래밍 스타일
- SwiftUI modifier 특징:
	- 순수 함수: 원본을 변경하지 않고 새로운 값을 반환
	- 조합 가능: 메서드 체이닝으로 쉽게 조합
	- 선언적: "how"가 아닌 "what" 표현


## SwiftUI View 생명주기 모디파이어 (Modifier)
- `.onAppear`
	- 여러번 호출 가능
	- 네비게이션으로 다른 화면에 갔다가 돌아오면 다시 호출
- `.onDisppear`
	- View가 화면에서 제거될 때 호출
	- 정리(cleanup) 작업에 사용
- .task



## SwiftUI View의 생명 주기
### 3가지 단계
- Appearing
	- 시점: View가 처음 화면에 나타날 때
	- SwiftUI는 뷰의 타입 인스턴스를 생성하고  body  프로퍼티를 계산
	- 상태(@State ,  @StateObject)는 이 시점에 초기화되어 생명주기와 함께 시작
	- 뷰 계층 구조의 상단에서 하단으로 순서대로 실행
	- 수행하는 작업: 데이터 로드, 초기 설정 등
	- modifier: `.onAppear(perform:)`
		- 뷰가 화면에 나타날 때 실행.
		- 데이터 로드, API 호출 초기화 등에 활용
- Updating
	- 시점: 상태 변경
	- 상태가 변경되면 SwiftUI는 새로운 뷰 값을 생성하고 이전 뷰 값 트리와 비교
	- 변경된 부분만 레이아웃을 다시 계산하고 렌더링
	- 뷰가 상태 변화에 반응하여 UI를 갱신
	- 상태와 연결된 뷰만 다시 렌더링되므로, 상태를 뷰 계층 구조의 아래쪽으로 내려보내면 불필요한 업데이트를 줄일 수 있음
	- modifier: 
- Disappearing
	- 시점: View가 사라질 때
	- 뷰가 더 이상 필요하지 않을 때 렌더링 트리에서 제거
	- onDisappear()  메서드가 호출
	- 수행되는 작업: 진행 중인 작업 취소, 데이터 정리
	- modifier: `.onDisappear(perform:)`



- View 생성 (init)
- body 계산
- 화면에 나타남 (`.onAppear`, `.task`)
- 업데이트 (State/Props 변경)
- body 재계산
- 화면에서 제거됨 (.onDisappear)
- View 소멸