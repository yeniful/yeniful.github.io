+++
title = '[Apple Platforms] Testing 🖋️'
date = 2026-01-23T00:00:00+09:00
draft = false
tags =  ["Apple Platforms", "iOS"]
categories = ["Apple"]
summary = ""
+++

## Motivation

### 기능을 완전하게 구현하려면?
- 최근 과제 전형을 마치고 제출했던 코드를 다시 살펴보았다. 
- 요구사항에 따라 동작하게 만들었지만 에러 핸들링, 예외 처리, 효율 측면에서 놓친 부분이 많았다는 걸 깨달았다. 
- 단순히 기능을 구현하는 것과, 예상하지 못한 동작과 상태를 대비해서 잘 만드는 건 전혀 다른 문제였다.
- 과제에서 크게 신경을 쓰지 못한 Testing 영역이 눈에 들어왔고 '코딩과 테스팅의 순서를 바꿨었더라면?'이라는 생각이 들었다.

### Code First
> 요구사항 확인 → 기능 개발 → 테스트 코드 작성 → 테스트 실행
- 기존 개발 플로우대로라면 보통 요구사항 확인 → 기능 개발 → 테스트 코드 작성 순서로 진행할 것 같다.
	- (사실 프로젝트를 진행하면서 테스트 코드를 작성한 경험이 없다.)
- 이 방식은 극단적으로 말하면 “내가 만든 기능이 맞다고 믿는 범위” 안에서만 테스트를 짤 위험이 있다.
	- 내가 구현한 형태에 맞춰 테스트를 설계하게 되면, 확증 편향처럼 **요구사항을 검증한다기보다 구현을 정당화하는 테스트**가 될 수 있겠다는 생각이 들었다.

### Test First
> 요구사항 확인 → 테스트 코드 작성 (+ 예상 출력 수동 입력) → 기능 개발 -> 테스트 실행 (실제 출력과 비교)
- 그때 떠오른 방법이 “시나리오 기반 테스트를 먼저 만들고 개발을 시작하는 방식”이었다. 
- 순서를 바꿔서, 요구사항 확인 → 테스트 코드 작성 → 개발의 이터레이션을 돌려보자는 생각을 했다.
	- 특히 요구사항을 **유저 시나리오**로 풀어 테스트를 먼저 설계하면, “정상 입력만 들어온다”는 가정에서 벗어나 더 넓은 시야로 문제를 볼 수 있을 것 같았다. 
	- 예를 들어 실패 케이스(잘못된 입력, 빈 값, 중복 요청, 타임아웃 등)를 자연스럽게 떠올리게 되고, 그 과정에서 예외 처리와 에러 핸들링의 품질도 함께 올라갈 가능성이 크다.

### TDD와 BDD
- 테스트 코드에 대해 알아보면서, 내가 느낀 문제의식이 **TDD**와 닮아 있다는 것도 알게 됐다.
	- TDD는 **"테스트 먼저, 구현 나중"** 원칙으로, Red(실패하는 테스트 작성) → Green(테스트 통과하는 최소 코드 작성) → Refactor(코드 개선) 사이클을 반복하며 개발하는 방법론이다.
- 동시에 내가 생각했던 "유저 시나리오 기반 테스트"를 더 확장한 개념이 **BDD**와 유사하다는 점도 흥미로웠다.
	- BDD는 TDD에서 발전한 개념으로, **사용자 행동(Behavior)** 관점에서 Given-When-Then 형식으로 테스트를 작성한다.
- 둘의 핵심 차이는 테스트 범위가 아니라 **관점**이다.
	- TDD가 "이 함수가 올바르게 동작하는가?", "성능은 어떠한가?"를 묻는다면, BDD는 "사용자가 X를 했을 때 Y가 일어나는가?"를 묻는다.
	- TDD는 개발자 관점에서 코드의 정확성과 성능을, BDD는 사용자/비즈니스 관점에서 요구사항 충족을 검증한다.
- 둘 다 함께 쓰는 방법도 생각했는데, 검증하려는 영역이 여러 테스트 코드에 걸쳐 중복되거나 테스트 코드 작성에만 시간이 과도하게 쏟아질 위험(본말전도)도 있어 균형이 필요하겠다는 고민도 남았다.

## AI 시대의 테스팅
- AI 시대에 테스트 코드가 어떤 의미있는지 궁금해져서 영상을 찾아보다가 2025년 6월 켄트백의 TDD 토크를 발견했다.
- [TDD, AI agent and coding with Kent Beck](https://www.youtube.com/watch?v=aSXaxOdVtAQ&t) 내용을 짧게 요약하자면 다음과 같다.
- AI 시대의 사고방식:
	- **비전과 설계 능력**이 핵심 스킬로 부상 (**비전 설정**, **이정표 관리**, **복잡성 제어**)
	- **언어 세부사항은 더 이상 중요하지 않음**
	- **실험의 양적 증가**가 경쟁 우위
		- 코드 양이 증가함에 따라 **탐색된 아이디어의 양**이 중요 
		- **"모든 것을 실험"** 해봐야 하는 시대
![[Screenshot 2026-01-23 at 19.45.17.png]]
- [켄트 백의 증강형 코딩 후기](https://www.stdy.blog/warning-signs-for-off-track-ai-and-tdd-system-prompts-by-kent-beck/) 번역도 재미있게 읽었다. (+ TDD를 위한 프롬프트)
	- 야크털 깎기(yak shaving)라는 표현 재밌다.

### 아무튼 테스팅
- 이번 과제를 통해 하나의 수단이자 도구로서의 테스트 코드의 필요성을 체감했다.
- 개인 iOS 앱 프로젝트에서서 테스트 코드를 작성하며 배운 것들을 이 문서에 차근차근 정리해보려고 한다.

## Overview
- 테스트 코드의 목적
- Testing의 종류
- Apple Platforms 개발에서의 testing frameworks
	- Swift Testing
	- XCTest
- 좋은 테스트 작성 팁





## References
[Apple Developer Testing](https://developer.apple.com/documentation/xcode/testing)
[TDD, AI Agents and Coding - 켄트백 원문](https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent)
[TDD, AI Agents and Coding - 켄트백 번역 및 요약 (Geeknews)](https://news.hada.io/topic?id=21446)
[iOS 개발자의 쉽게 쓰는 테스트코드, TDD의 생활화(1)-구조와 방식](https://medium.com/bejewel/ios-개발자의-쉽게-쓰는-테스트코드-tdd의-생활화-1-구조와-방식-c5609aa8b886)
[뱅크샐러드 iOS팀이 숨쉬듯이 테스트코드 짜는 방식 1편 - 통합 UI테스트](https://blog.banksalad.com/tech/test-in-banksalad-ios-1/)
[실전에서 TDD하기](https://tech.kakaopay.com/post/implementing-tdd-in-practical-applications/)
[가독성 좋은 테스트 코드를 작성하는 방법](https://yozm.wishket.com/magazine/detail/2435/)