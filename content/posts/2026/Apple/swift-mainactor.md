+++
title = '[Swift] MainActor 🖋️'
date = 2026-01-30T00:00:00+09:00
draft = false
tags =  ["Swift", "Swift Concurrency"]
categories = ["Apple Platforms"]
summary = ""
+++

## Motivation
iOS 개발에서 UI 업데이트는 **반드시 메인 스레드**에서 해야 하지만, Swift Concurrency 도입 후 백그라운드 Task 내에서 UI를 직접 업데이트하는 실수가 쉬워졌다. @MainActor는 컴파일 타임부터 이 문제를 구조적으로 해결해 준다. 이 글에서 MainActor에 대해 다뤄보려고 한다.

``` Swift
Task { // 백그라운드 Task (Global Actor)
    let data = await fetchUser()      
    userNameLabel.text = data.name  // UI 업데이트 (MainActor 필요!)
}
```

## Overview


## 1. MainActor는 언제 필요할까? (문제 상황)
### 1-1. UIKit의 메인 스레드 제약
### 1-2. GCD의 한계 (DispatchQueue.main.async)
### 1-3. Swift Concurrency의 함정 (Task + UI 섞기)


## 2. MainActor 기본 개념
### 2-1. MainActor 알기 전 Actor가 무엇인지
### 2-2. Global Actor와 MainActor
### 2-3. `@MainActor` 속성의 의미


## 3. 사용법
### 3-1. 함수/프로퍼티에 적용
### 3-2. 클래스 전체에 적용 (ViewModel 패턴)
### 3-3. SwiftUI와의 완벽 호환 (@Published)


## 4. Task와 함께 사용
### 4-1. `Task { @MainActor in }`
### 4-2. `await MainActor.run { }`
### 4-3. `Task.detached` + MainActor 전환



## 5. 더 알아보기
### 5-1. `nonisolated` 탈출구
### 5-2. `@Sendable`과 데이터 전달
### 5-3. MainActor 호핑 최적화



## 6. MainActor와 DispatchQueue 비교
### 6-1. `@MainActor` vs `DispatchQueue.main`
### 6-2. 다른 글로벌 액터와 비교




### References 👀
