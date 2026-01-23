+++
title = '[Swift] weak self 🖋️'
date = 2026-01-01T00:00:00+09:00
draft = false
tags =  ["Apple", "Swift", "iOS"]
categories = ["Apple Platforms"]
summary = ""
+++

## Motivation
- 클로저 안의 weak self

## Overview
- self: 클래스 인스턴스 참조
- 약한 참조
- ARC

## Note
- 일반적으로 클로저는 클로저 내부에서 변수를 사용할 때 암시적으로 변수를 캡처하지만, 이 경우에는 명시적으로 작성해야 합니다.
- `self`를 캡처하려면, 사용할 때 명시적으로 `self`를 작성하거나, 클로저의 캡처 리스트에 `self`를 포함합니다. 
- `self`를 명시적으로 작성하는 것은 의도를 분명하게 표현하고, 참조 순환이 없음을 확인하도록 유도하는 역할도 합니다

## Capture Values
- 클로저는 정의된 주변 컨텍스트로부터 상수와 변수를 캡처할 수 있음.
- 상수와 변수를 정의한 원래 범위가 더이상 존재하지 않더라도 본문 내에서 상수와 변수의 값을 참조하고 수정할 수 있음.
- 값을 캡처할 수 있는 가장 간단한 클로저 형태는 다른 함수의 본문 내에 작성하는 중첩 함수
	- 중첩 함수는 바깥 함수의 어떠한 인자도 캡처할 수 있고 바깥 함수 내에 정의된 상수와 변수를 캡처할 수도 있습니다.
### References 👀
- [Swift Programming Language - 탈출 클로저](https://bbiguduk.github.io/swift-book-korean/documentation/the-swift-programming-language-korean/closures/#탈출-클로저-Escaping-Closures)