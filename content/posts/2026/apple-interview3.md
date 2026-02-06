+++
title = '[Apple Platforms] iOS 개발자 기술 인터뷰 준비 3'
date = 2026-01-01T00:00:00+09:00
draft = true
tags = ["iOS", "Apple Platforms"]
categories = ["Apple Platforms"]
summary = ""
+++

## Motivation
- iOS 직무의 기술 면접을 보게 되었다. 인터뷰에서 물어볼법한 질문 리스트에 대해 답을 달아보면서 기술 지식들을 정리해보려고 한다.

## Overview
- 컴퓨터 시스템 기초
- 프로세스와 스레드
- 메모리관리
- 네트워크
- 이미지와 파일
- 보안
- 알고리즘과 자료구조
- 운영체제
- 데이터베이스
- 디자인 패턴
- 직렬화
- 동시성
- **Swift 기초** ✅
- **Swift, iOS중급** ✅
- iOS 고급
- iOS 고급+
- 리더십 및 아키텍처

## Q&A + beef up
### Swift 기초

32. 서버 API가 사용자 이름을 때로는 보내고 때로는 보내지 않을 때, 이를 어떻게 처리해야 하나요? 
    32.1. Swift의 옵셔널(Optional) 타입을 사용하여 이 상황을 모델링하는 방법은 무엇인가요? 
    32.2. 강제 언래핑(!)을 사용했을 때 앱이 크래시되는 시나리오와 이를 방지하는 방법은 무엇인가요? 
    32.3. 옵셔널 체이닝을 사용하여 중첩된 JSON 데이터를 안전하게 파싱하는 예시를 들어주세요.

	- 옵셔널 처리: 있을 수도 있고 없을 수도 있다는 타입을 지정해준다.
	1. 모델의 타입 뒤에 ?를 붙여준다.
	2. if let이나 guard let 으로 안전하게 언래핑한다.
	3. ?

34. #꼭보자 iOS 앱의 생명주기(App Life Cycle)에 대해 설명해주세요. 
    34.1. 앱의 각 상태에서 가능한 작업은 무엇인가요? 
    34.2. 상태 변화에 따라 호출되는 AppDelegate 또는 SceneDelegate 메서드는 무엇인가요? 
    34.3. 백그라운드에서 작업을 완료하기 위한 방법은 어떤 것이 있나요?

	- dd
	- dd
	- dd
	- dd
	- 


35. #힘빼자 복잡한 UI에서 Auto Layout 성능이 느려질 때 어떻게 최적화하시겠습니까? 
    35.1. 제약 조건이 너무 많을 때 발생하는 성능 문제와 해결 방법은? 
    35.2. 불필요한 제약 조건을 찾아내는 방법은? 
    35.3. 제약 조건의 우선순위(Priority)를 활용한 최적화 방법은? 
    35.4. 동적으로 변하는 콘텐츠에서 Auto Layout vs Manual Layout의 선택 기준은? 
    35.5. SwiftUI와 UIKit을 혼용할 때 레이아웃 충돌을 해결하는 방법은? 
	    35.5.1. UIViewRepresentable에서 intrinsicContentSize 처리 방법은?

36. #보자 Swift에서 클로저(Closure)란 무엇이며, 어떻게 사용하나요? 
    35.1. 클로저의 캡처(Capture) 기능은 무엇인가요? 
    35.2. @escaping 클로저와 non-escaping 클로저의 차이점은 무엇인가요? 
    35.3. 트레일링 클로저(Trailing Closure) 문법은 어떤 경우에 유용한가요?


37. 테이블뷰의 delegate 메서드가 호출되지 않는 문제를 어떻게 디버깅하시겠습니까? 
    36.1. Delegate 패턴에서 자주 발생하는 실수들과 해결 방법은? 
    36.2. delegate를 weak으로 선언하는 이유와 strong으로 선언했을 때의 문제는? 
    36.3. optional 메서드와 required 메서드의 차이와 사용 시기는? 
    36.4. Delegate vs Closure vs Combine을 선택하는 기준은? 
    36.4.1. 1:1 통신과 1:N 통신에서 각각 어떤 패턴이 적합한가요? 
    36.5. SwiftUI에서 Delegate 패턴을 대체하는 방법은?

38. Swift의 기본 데이터 타입과 컬렉션(Collection) 타입에는 어떤 것들이 있나요? 
    37.1. 값 타입(Value Type)과 참조 타입(Reference Type)의 차이점은 무엇인가요? 
    37.2. 구조체(Struct)와 클래스(Class)의 사용 시기는 어떻게 구분하나요? 
    37.3. 열거형(Enum)의 원시값(Raw Value)과 연관값(Associated Value)은 무엇인가요?


39. Xcode에서 디버깅 시 자주 사용하는 기능은 무엇인가요? 
    38.1. 중단점(Breakpoint)의 종류와 활용 방법을 설명해주세요. 
    38.2. LLDB 콘솔에서 유용한 명령어는 어떤 것이 있나요?

	- ?
	1. ?
	2. po

40. iOS 앱에서 데이터를 저장하는 방법에는 어떤 것들이 있나요? 
	39.1. UserDefaults의 사용 시 주의할 점은 무엇인가요? 
    39.2. Keychain은 어떤 데이터를 저장하기에 적합한가요? 
    39.3. Core Data와 SQLite의 차이점은 무엇이며, 각각 언제 사용하면 좋나요?

	- CoreData, SwiftData, UserDefault, App Storage, Environment, KeyChain ...
	1. 용량 제한이 있다.
	2. 비밀번호. 보안이 필요한 것들
	3. ?? CoreData는 ~~ SQLite는 ~~~

41. Swift에서 프로토콜(Protocol)이란 무엇이며, 어떻게 활용하나요? 
	40.1. 프로토콜의 요구사항은 무엇인가요? 
	40.2. 프로토콜 확장(Protocol Extension)을 사용하는 이유는 무엇인가요? 
	40.3. 프로토콜 지향 프로그래밍(Protocol-Oriented Programming)의 장점은 무엇인가요?

	- 무언가를 구현할 때 꼭 구현해줘야하는 것만 청사진으로 제시. 어떻게 구현하는 지는 구현하는 곳에서 정하도록 하는 것.
	1. ?
	2. 프로토콜을 재사용하기 위해서? 겹치는 요구사항이 있으면 새로 만드는 것 보다 확장해서 만들도록 (class의 상속처럼?)
	3. 누락되는 요구사항 없도록 함. 안전하고 실수를 방지할 수 있다.

42. Swift의 접근 제어자(Access Control Levels)에 대해 설명해주세요. 
    41.1. open과 public의 차이점은 무엇인가요? 
    41.2. internal, fileprivate, private의 사용 시기는 어떻게 결정하나요? 
    41.3. 접근 제어자를 사용하는 이유는 무엇인가요?

	- 접근 범위를 지정해줘서 예상하지 못한 곳에서 사용하거나 접근하는 것을 방지
	1. ? public은 같은 프로젝트 파일 ? open 은 all?
	2. ?
	3. 접근 범위를 지정해줘서 예상하지 못한 곳에서 사용하거나 접근하는 것을 방지

43. API 호출이 실패했을 때 재시도 로직을 구현하려면 어떻게 하시겠습니까? 
    42.1. 네트워크 에러 종류별 대응 전략은? 
    42.2. 지수 백오프(Exponential Backoff) 알고리즘을 구현하는 방법은? 
    42.3. 재시도 횟수와 간격을 어떻게 결정하나요? 
    42.4. URLSession의 캐싱 정책을 활용한 오프라인 대응 방법은? 
    42.5. 백그라운드에서 대용량 파일을 다운로드할 때 고려사항은? 
    42.5.1. Background URLSession 설정과 제약사항은?

44. 의존성 관리 도구(CocoaPods, Carthage, Swift Package Manager)의 종류와 차이점은 무엇인가요? 
    43.1. 각 도구의 사용 방법과 장단점을 설명해주세요. 
    43.2. 의존성 관리를 통해 얻을 수 있는 이점은 무엇인가요?

45. Swift의 고차 함수(Higher-Order Functions)에 대해 설명해주세요. 
    45.1. map과 flatMap의 차이점은 무엇인가요? 
    45.2. filter, reduce 함수는 어떤 경우에 사용하나요? 
    45.3. compactMap은 어떤 역할을 하나요?

	- 🐮 고차함수란? 함수를 파라미터로 받거나 함수를 반환하는 함수
	1. `map` : 각 요소 변환 -> return 같은 개수 배열
		-[[1,2], [3,4]] → [[1,2], [3,4]] (그대로)
	2. `platMap`: 변환 + 평탄화 -> return 1차원 배열 (중첩 배열 펼치기)
		- [[1,2], [3,4]] → [1, 2, 3, 4] (펼침)
	3. `compactMap`은 nil 제거 + 언래핑
```swift
let strings = ["1", "2", "three", "4", "five"]

// map: nil 포함
let mapped = strings.map { Int($0) }
// [Optional(1), Optional(2), nil, Optional(4), nil]

// compactMap: nil 제거 + 언래핑
let compacted = strings.compactMap { Int($0) }
// [1, 2, 4]
```

46. Git에서 브랜치(Branch)를 사용하는 이유와 장점은 무엇인가요? 
    46.1. 브랜치를 병합(Merge)하는 방법에는 어떤 것들이 있나요? 
    46.2. 브랜치 전략에 대해 설명해주세요. 
    46.3. 충돌(Conflict)이 발생했을 때 해결 방법은 무엇인가요?

47. Swift의 에러 처리 방법에 대해 설명해주세요. 
    47.1. throws, try, catch 키워드의 사용 방법은 무엇인가요? 
    47.2. 옵셔널을 사용한 에러 처리와 do-catch를 사용하는 에러 처리의 차이는 무엇인가요? 
    47.3. 에러를 전파하는 방법은 무엇인가요?

48. #보자 메모리 관리에서 강한 참조(Strong Reference)와 약한 참조(Weak Reference)의 차이점은 무엇인가요? 
    48.1. 순환 참조(Retain Cycle)가 발생하는 경우와 해결 방법은 무엇인가요? 
    48.2. 클로저에서 [weak self]와 [unowned self]의 차이는 무엇인가요?
	- 강한 참조 (Strong): 참조 카운트 증가 → 메모리 -  유지 
	- 약한 참조 (Weak): 참조 카운트 증가 안 함 → 메모리 해제 가능
	- 순환참조시 둘 다 RC가 0이 되지 않음 → 영원히 해제 불가 -> 순환참조 방지를 위한 weak 사용

	- weak self: self가 먼저 해제될 수 있을 때
	- unowned self: self가 항상 존재한다고 확신할 때
		- self가 해제된 상태에서 접근하면 크래시
	

50. #꼭보자 iOS 앱에서 Multi-threading을 구현하는 방법은 무엇인가요? 
    49.1. DispatchQueue와 OperationQueue의 차이점은 무엇인가요? 
    49.2. 동시성 프로그래밍에서 Race Condition을 방지하는 방법은 무엇인가요? 
    49.3. 메인 스레드에서 UI 업데이트를 해야 하는 이유는 무엇인가요?
	
51. UIKit에서 TableView와 CollectionView의 차이점은 무엇인가요? 
    50.1. 셀(Cell)의 재사용(Reusability)은 어떻게 구현되나요? 
    50.2. 동적인 셀 높이(Dynamic Cell Height)를 설정하는 방법은 무엇인가요? 
    50.3. CollectionView의 레이아웃을 커스터마이징하는 방법은 무엇인가요?
	
	- ㅇㅇ
	- 
    
52. #보자 ARC(Automatic Reference Counting)의 동작 원리는 무엇인가요? 
    51.1. Retain Cycle이 발생하지 않도록 방지하는 방법은 무엇인가요? 
    51.2. deinit 메서드는 언제 호출되며, 어떤 역할을 하나요?

- 

49. #보자 상속(Inheritance)과 프로토콜(Protocol)의 차이점은 무엇인가요? 
    52.1. 클래스 상속을 사용할 때의 장단점은 무엇인가요? 
    52.2. 다중 상속(Multiple Inheritance)이 불가능한 이유는 무엇인가요? 
    52.3. 프로토콜 준수(Conformance)를 통해 다형성을 구현하는 방법은 무엇인가요?

	- 상속은 Class, 하나의 클래스를 상속할 수 있지만, 프로토콜은 여러개 채택이 가능하다.
	1. 장점? 단점?
	2. ?
	3. ?

50. 사용자 인터페이스(UI) 테스트와 단위(Unit) 테스트의 차이점은 무엇인가요? 
    53.1. XCTest 프레임워크를 사용하여 테스트를 작성하는 방법은 무엇인가요?
    53.2. 테스트 주도 개발(TDD)의 장점은 무엇인가요? 
    53.3. 의존성 주입(Dependency Injection)을 활용하여 테스트 가능한 코드를 작성하는 방법은 무엇인가요?

	- ?
	1. ?
	2. ?
	3. ?

51. #힘빼자 Xcode에서 Instruments를 사용하여 앱의 성능을 분석하는 방법은 무엇인가요? 
    51.1. Time Profiler를 사용하여 성능 이슈를 찾는 방법을 설명해주세요. 
    51.2. Allocations Instrument를 사용하여 메모리 누수를 탐지하는 방법은 무엇인가요? 
    51.3. Leaks Instrument를 사용하여 메모리 누수를 찾는 방법은 무엇인가요?

52. Swift의 제네릭(Generic)에 대해 설명해주세요. 
    52.1. 제네릭을 사용하는 이유는 무엇인가요? 
    52.2. 제네릭 타입 파라미터와 제약 조건을 설정하는 방법은 무엇인가요? 
    52.3. 제네릭을 사용할 때의 장점과 주의할 점은 무엇인가요?

53. Swift의 클로저와 함수의 차이점은 무엇인가요? 
    53.1. 클로저가 일급 객체(First-Class Citizen)인 이유는 무엇인가요? 
    53.2. 함수형 프로그래밍 패러다임에서 클로저가 어떻게 활용되나요?

	- 클로저 안에는 이름 있는 클로저(함수)와 이름 없는 클로저로 이루어짐.
		- **함수는 이름 있는 클로저**이며, 익명 클로저만의 특징은 **주변 값 캡처**와 **인라인 익명 선언**.
	1. 🐮 변수에 할당, 파라미터로 전달, return의 반환값으로 사용할 수 있어서.
	2. 

54. 동시성 프로그래밍에서 동기(Synchronous)와 비동기(Asynchronous)의 차이점은 무엇인가요? 
    57.1. iOS에서 비동기 작업을 처리하는 방법은 무엇인가요? 
    57.2. 세마포어(Semaphore)와 뮤텍스(Mutex)의 차이점은 무엇인가요?

55. GCD(Grand Central Dispatch)의 주요 개념과 사용 방법을 설명해주세요. 
    58.1. 직렬(Serial) 큐와 동시(Concurrent) 큐의 차이는 무엇인가요? 
    58.2. 글로벌 큐(Global Queue)와 메인 큐(Main Queue)는 어떻게 다르나요? 
    58.3. DispatchWorkItem을 사용하는 방법은 무엇인가요?

56. #보자 SwiftUI에서 @State 변수를 변경했는데 화면이 업데이트되지 않는다면 어떤 문제를 의심해야 하나요? 
    59.1. @State, @Binding, @ObservedObject의 차이점과 각각 언제 사용해야 하나요? 
    59.2. View의 body가 다시 그려지는 시점은 언제이며, 성능을 위해 주의할 점은 무엇인가요? 
    59.3. SwiftUI의 데이터 흐름과 UIKit의 MVC 패턴의 차이점은 무엇인가요?

57. 여러 화면에서 동일한 알림 기능을 사용해야 할 때, 싱글톤 패턴 대신 어떤 디자인 패턴을 고려해볼 수 있나요? 
    60.1. Observer 패턴과 NotificationCenter의 관계는 무엇인가요? 
    60.2. Dependency Injection을 사용하면 싱글톤 대비 어떤 이점이 있나요? 
    60.3. Protocol을 활용한 의존성 역전은 테스트 가능성을 어떻게 향상시키나요?

### 중급 Swift 및 iOS

60. #꼭보자 테이블뷰를 빠르게 스크롤할 때 이미지가 잘못된 셀에 표시되는 문제를 어떻게 해결하나요? 
    61.1. 셀 재사용(Cell Reuse) 메커니즘이 이 문제와 어떤 관련이 있나요? 
    61.2. 이미지 다운로드 작업을 취소하는 시점과 방법은 무엇인가요? 
    61.2.1. URLSessionTask를 셀별로 추적하는 자료구조 설계는 어떻게 하나요? 
    61.2.2. prepareForReuse()에서 수행해야 할 작업은 무엇인가요? 
    61.3. 이미지 캐싱을 구현할 때 메모리와 디스크 캐싱의 트레이드오프는 무엇인가요? 
    61.3.1. LRU, LFU 등 캐시 eviction 정책 선택 기준은? 
    61.3.2. NSCache vs Dictionary를 사용한 캐싱의 차이점은?

	- 

61. #꼭보자 객체지향 프로그래밍(OOP)의 주요 개념에 대해 설명해주세요. 
    61.1. 캡슐화(Encapsulation)와 정보 은닉(Information Hiding)의 차이점은 무엇인가요? 
    61.2. 상속(Inheritance)의 장단점은 무엇인가요? 
    61.3. 다형성(Polymorphism)을 활용하는 예시를 들어주세요.

	- 🐮 OOP 4대 원칙 -> 유지보수하기 쉬운 코드를 만드는 패러다임
		- 캡슐화 (Encapsulation) : 데이터와 메서드를 하나로 묶고, 외부 접근 제한
		- 상속 (Inheritance) : 자식은 부모 클래스의 속성과 메서드를 물려받음
		- 다형성 (Polymorphism) : 같은 인터페이스로 다른 동작 수행
		- 추상화 (Abstraction) : 복잡한 내부 구현을 숨기고 필요한 기능만 노출
	1. 
    
63. 프로토콜 지향 프로그래밍(POP)이란 무엇이며, 어떤 장점이 있나요? 63.1. 프로토콜 확장(Protocol Extension)을 사용하는 이유는 무엇인가요? 63.2. 프로토콜 컴포지션(Protocol Composition)은 어떤 경우에 사용하나요? 63.3. 프로토콜과 제네릭(Generic)을 함께 사용하면 어떤 이점이 있나요?
64. iOS 앱의 메모리 관리는 어떻게 이루어지나요? 64.1. ARC(Automatic Reference Counting)의 동작 원리를 설명해주세요. 64.2. 강한 참조(Strong Reference)와 약한 참조(Weak Reference)의 차이점은 무엇인가요? 64.3. 순환 참조(Retain Cycle)가 발생하는 경우와 해결 방법을 설명해주세요. 64.4. 강한 참조, 약한 참조, 미소유 참조의 차이점을 설명해주세요.
65. viewDidLoad에서 네트워크 요청을 하면 어떤 문제가 발생할 수 있나요? 65.1. 뷰 컨트롤러가 메모리에서 해제되었다가 다시 생성될 때 viewDidLoad가 재호출되는 상황을 설명해주세요. 65.2. viewWillAppear와 viewDidAppear 중 어디서 UI 업데이트를 해야 하며, 그 이유는 무엇인가요? 65.3. 화면 전환 시 네트워크 요청이 완료되기 전에 뷰 컨트롤러가 해제되면 어떤 문제가 발생하고, 어떻게 해결할 수 있나요?
66. Swift의 문자열(String) 다루기와 관련된 주요 기능은 무엇이 있나요? 66.1. 서브스트링(Substring)과 문자열의 차이점은 무엇인가요? 66.2. 문자열 보간법(String Interpolation)을 사용하는 방법과 주의 사항을 설명해주세요. 66.3. 정규식(Regular Expression)을 사용하여 문자열을 다루는 방법을 설명해주세요.
67. Codable 프로토콜은 무엇이며, 어떻게 사용하나요? 67.1. Encodable과 Decodable 프로토콜의 역할은 무엇인가요? 67.2. JSON 데이터를 커스텀 객체로 디코딩하는 방법을 설명해주세요. 67.3. Codable 프로토콜을 채택한 타입에서 인코딩/디코딩 키를 커스터마이징하는 방법은 무엇인가요?
68. iOS 앱에서 의존성 주입(Dependency Injection)은 어떤 목적으로 사용되나요? 68.1. 의존성 주입의 세 가지 유형을 설명해주세요. 68.2. 의존성 주입 컨테이너(Dependency Injection Container)란 무엇인가요? 68.3. 의존성 주입을 사용함으로써 얻을 수 있는 이점은 무엇인가요?
69. 델리게이션 패턴(Delegation Pattern)과 클로저의 차이점은 무엇인가요? 69.1. 델리게이션 패턴에서 메모리 누수가 발생할 수 있는 경우와 해결 방법을 설명해주세요. 69.2. 클로저의 캡처 리스트(Capture List)는 어떤 역할을 하나요? 69.3. 델리게이션 패턴과 클로저를 함께 사용하는 경우의 장단점은 무엇인가요?
70. UIKit에서 테이블 뷰(UITableView)와 컬렉션 뷰(UICollectionView)의 차이점은 무엇인가요? 70.1. 테이블 뷰와 컬렉션 뷰에서 셀을 재사용하는 이유와 방법을 설명해주세요. 70.2. 테이블 뷰와 컬렉션 뷰의 데이터 소스와 델리게이트의 역할은 무엇인가요? 70.3. 컬렉션 뷰에서 사용할 수 있는 레이아웃의 종류와 특징을 설명해주세요.
71. 레거시 MVC 프로젝트를 MVVM으로 점진적으로 마이그레이션하려면 어떤 전략을 사용하시겠습니까? 71.1. 어떤 화면부터 마이그레이션을 시작하는 것이 좋을까요? 71.1.1. 복잡도가 높은 화면 vs 단순한 화면, 어떤 기준으로 선택하나요? 71.1.2. 테스트 커버리지가 높은 부분부터 시작하는 이유는? 71.2. MVC와 MVVM이 공존하는 과도기에 발생할 수 있는 문제와 해결책은? 71.2.1. 데이터 흐름의 일관성을 유지하는 방법은? 71.2.2. 팀원들의 혼란을 최소화하는 방법은? 71.3. 아키텍처 변경의 성공 지표는 무엇으로 측정하나요?
72. Swift에서 옵셔널(Optional)을 사용할 때 주의할 점은 무엇인가요? 72.1. 강제 언래핑(Force Unwrapping)을 사용하면 안 되는 이유는 무엇인가요? 72.2. 옵셔널 바인딩과 옵셔널 체이닝의 차이점을 설명해주세요. 72.3. 암시적 언래핑 옵셔널(Implicitly Unwrapped Optional)은 어떤 경우에 사용하나요?
73. iOS 앱에서 코어 애니메이션(Core Animation)을 사용하는 방법은 무엇인가요? 73.1. CALayer의 주요 속성과 메서드를 설명해주세요. 73.2. 애니메이션 그룹은 어떤 경우에 사용하나요? 73.3. 키 프레임 애니메이션과 스프링 애니메이션의 차이점은 무엇인가요?
74. 여러 개의 비슷한 네트워크 API 클라이언트를 프로토콜로 추상화하려면 어떻게 설계하시겠습니까? 74.1. 공통 기능을 프로토콜 확장으로 구현할 때의 이점과 주의점은? 74.1.1. 프로토콜 확장의 메서드 디스패치 방식과 override 불가능한 이유는? 74.2. Associated Type을 사용한 제네릭 프로토콜의 장단점은? 74.3. 테스트를 위한 Mock 객체 생성이 쉬운 프로토콜 설계 방법은? 74.3.1. Dependency Injection과 프로토콜의 관계는? 74.4. SwiftUI의 View 프로토콜처럼 PAT를 활용하는 방법은?
75. iOS 앱에서 네트워크 요청 시 응답 캐싱(Response Caching)을 하는 방법은 무엇인가요? 75.1. URLCache는 어떤 역할을 하나요? 75.2. 응답 캐싱의 장단점은 무엇인가요? 75.3. 응답 캐싱을 커스터마이징하는 방법을 설명해주세요.

76. #보자 Combine 프레임워크란 무엇이며, 어떤 기능을 제공하나요? 
    76.1. Publisher와 Subscriber의 역할은 무엇인가요? 
    76.2. Operator의 종류와 사용 방법을 설명해주세요. 
    76.3. Combine과 RxSwift의 차이점은 무엇인가요?

	- 🐮 **Combine**은 ==Apple의 반응형 프레임워크==로, **Publisher**(발행) → **Operator**(변환) → **Subscriber**(구독) 흐름으로 ==비동기 이벤트를 선언적으로 처리==
		- iOS 13+
		- 핵심 구성 요소
			- Publisher: 데이터 생성
			- Operator: 데이터 변환
			- Subscriber: 데이터 소비
	  1. Publisher: "이런 데이터를 내보낼거야" Subscriber: "데이터 받으면 이렇게 처리할거야"
	  2. Operator의 종류
		  1. Transforming: `map`, `flatMap`, `compactMap`
		  2. Filtering: `.filter`, `.first`,` .removeDuplicates`
		  3. Combining
		  4. 에러 처리
		  5. 스케줄링
	```swift
	[1, 2, 3].publisher
	    .map { $0 * 2 }           // 각 값 변환: 2, 4, 6
	    .flatMap { ... }          // Publisher를 다른 Publisher로 변환
	    .compactMap { Int($0) }   // nil 제거하며 변환
	    
	    
	[1, 2, 3, 4, 5].publisher
	    .filter { $0 > 2 }        // 조건 만족만: 3, 4, 5
	    .first()                  // 첫 번째만
	    .removeDuplicates()       // 중복 제거
	    
	    
	let pub1 = [1, 2].publisher
	let pub2 = ["A", "B"].publisher
	
	pub1.combineLatest(pub2)      // 최신 값 결합
	pub1.merge(with: pub2)        // 두 스트림 합침
	pub1.zip(pub2)                // 쌍으로 결합: (1,"A"), (2,"B")
	
	publisher
    .catch { error in Just("기본값") }    // 에러 시 대체값
    .retry(3)                             // 실패 시 3번 재시도
    .replaceError(with: "에러 발생")       // 에러를 값으로 대체
    
    publisher
    .subscribe(on: DispatchQueue.global())  // 백그라운드에서 구독
    .receive(on: DispatchQueue.main)        // 메인에서 결과 받기
	```

	  3. Combine과 RxSwift 차이점
			![[Pasted image 20260203042901.png]]

77. #힘빼자 Swift의 고급 제네릭 기능과 타입 소거(Type Erasure)에 대해 설명해주세요. 
    77.1. Associated Type과 제네릭의 차이점은 무엇이며, 각각 언제 사용하나요? 
    77.2. 타입 소거(Type Erasure)가 필요한 이유와 구현 방법을 설명해주세요. 
    77.3. where 절을 사용한 제네릭 제약 조건의 활용 예시를 들어주세요.

78. iOS 앱에서 로컬 푸시 알림(Local Push Notification)을 구현하는 방법은 무엇인가요? 
    78.1. 로컬 푸시 알림과 원격 푸시 알림의 차이점은 무엇인가요? 
    78.2. 푸시 알림의 콘텐츠와 트리거는 어떤 역할을 하나요? 
    78.3. 사용자가 푸시 알림을 탭했을 때 앱의 동작을 처리하는 방법을 설명해주세요.

79. iOS 앱에서 SwiftUI와 UIKit을 함께 사용하는 방법은 무엇인가요? 
    79.1. SwiftUI 뷰에서 UIKit 뷰 컨트롤러를 사용하는 방법을 설명해주세요. 
    79.2. UIKit 뷰 컨트롤러에서 SwiftUI 뷰를 호스팅하는 방법은 무엇인가요? 
    79.3. SwiftUI와 UIKit을 함께 사용할 때 주의할 점은 무엇인가요?

80. Swift에서 키 경로(Key Path)란 무엇이며, 어떻게 사용하나요? 
    79.1. 키 경로 표현식의 문법과 사용 예시를 설명해주세요. 
    79.2. 런타임에 키 경로를 사용하여 속성에 접근하는 방법은 무엇인가요? 
    79.3. 키 경로와 KVO의 관계를 설명해주세요.

81. iOS 앱에서 Deep Link와 Universal Link의 차이점은 무엇인가요? 
    80.1. Deep Link를 구현하는 방법과 주의 사항을 설명해주세요. 
    80.2. Universal Link의 동작 원리와 설정 방법은 무엇인가요? 
    80.3. Deep Link와 Universal Link를 함께 사용하는 경우의 장점은 무엇인가요?

82. Swift의 Result 타입과 에러 처리 방식에 대해 설명해주세요. 
    82.1. Result 타입을 사용하는 이유와 장점은 무엇인가요? 
    82.2. 에러 처리 시 do-catch 문과 Result 타입을 함께 사용하는 방법을 설명해주세요.

83. #보자 iOS 앱에서 Thread Sanitizer를 사용하여 동시성 문제를 탐지하고 해결하는 방법을 설명해주세요.

84. Swift의 Sequence와 Collection 프로토콜에 대해 설명해주세요. 
    84.1. Sequence와 Collection 프로토콜의 차이점과 요구 사항을 설명해주세요. 
    84.2. 사용자 정의 Sequence와 Collection을 구현하는 방법과 사용 예시를 들어주세요.

	- Sequence: 연속성 보장 like 배열 Collection: 연속성 보장 x like set

85. #힘빼자 UIKit의 AdaptiveLayout과 Size Classes에 대해 설명해주세요. 
    85.1. AdaptiveLayout의 개념과 사용 목적을 설명해주세요. 
    85.2. Size Classes를 활용하여 다양한 기기에 적응적인 UI를 구현하는 방법을 예시와 함께 설명해주세요.

86. #힘빼자 Swift의 커스텀 연산자(Custom Operator)에 대해 설명해주세요. 
    86.1. 커스텀 연산자를 정의하는 방법과 주의 사항은 무엇인가요? 
    86.2. 커스텀 연산자를 활용한 코드 가독성 향상 방안을 제시해주세요.

87. Swift의 생성자(Initializer)와 관련된 고급 개념에 대해 설명해주세요. 
    86.1. 지정 생성자와 편의 생성자의 차이점은 무엇인가요? 
    86.2. 필수 생성자와 실패 가능한 생성자는 어떤 경우에 사용하나요?

88. Combine 프레임워크에서 Scheduler의 역할과 종류에 대해 설명해주세요. 
    87.1. Scheduler를 사용하여 작업을 특정 큐에서 실행하는 방법을 설명해주세요. 
    87.2. 백그라운드에서 작업을 수행하고 메인 큐에서 UI를 업데이트하는 패턴을 Combine으로 구현하는 방법을 설명해주세요.

89. UIKit의 UIView는 클래스 기반으로 구현되어 있지만, SwiftUI에서 View 프로토콜을 준수하는 타입은 보통 구조체를 사용합니다. 그 이유는 무엇일까요? 
    88.1. View 프로토콜을 준수하는 구조체의 주요 특징은 무엇이며, 이는 어떻게 SwiftUI의 성능 및 사용성에 영향을 미치나요? 
    88.2. SwiftUI의 View가 구조체임에도 불구하고, 상태를 어떻게 관리하고 업데이트하나요? 
    88.3. SwiftUI의 구조체 기반 View 생성과 업데이트 사이클은 어떻게 UIKit의 클래스 기반 UIView와 다른가요?

### References 👀
- [Jercy's Interview Questions for iOS Developers](https://github.com/JeaSungLEE/iOSInterviewquestions?tab=readme-ov-file)
- ref2
- ref3