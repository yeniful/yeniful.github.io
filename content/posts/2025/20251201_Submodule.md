+++
title = 'Git Submodule로 멀티플랫폼 프로젝트 데이터 관리하기'
date = 2025-12-01T00:00:00+09:00
draft = true
tags = ["Git", "Submodule", "iOS"]
categories = []
summary = "iOS 앱에서 시작해 웹까지 확장하면서, Submodule을 활용해 공유 데이터를 관리한 경험을 공유합니다."
+++

## 프로젝트 소개

7월부터 느린 호흡으로 Blendio라는 카페 레시피 학습 앱을 개발하고 있습니다.
iOS 앱으로 시작했지만, 최근에는 다른 OS 유저와 관리자 페이지 구현을 위해 웹도 개발하고 있습니다.
데이터 관리를 위해 Submodule을 활용한 경험을 간단한 기록으로 남겨보려고 합니다.

---
## 데이터 관리 방식 변화

### 1단계: Swift 파일로 하드코딩

처음에는 iOS 앱만 개발하려고 했고, MVP를 빠르게 구현하는 게 목표였습니다. 
그래서 레시피 데이터를 Swift 파일에 직접 하드코딩을 하면서 프로젝트 구조를 잡아갔습니다.
레시피 데이터가 민감 데이터다 보니 `.gitignore`로 버전 관리에서 제외했습니다.

```swift
struct Recipe {
    static let drinks = [
        Drink(name: "아메리카노", shots: 2, water: 200),
        Drink(name: "카페라떼", shots: 2, milk: 150),
        // ...
    ]
}
```

### 2단계: Submodule로 분리

`.gitignore`로 관리하고 있지만, 파일 추적을 막는 것을 깜빡하고 프로젝트 레포를 공개하면 데이터도 함께 노출되는 구조였기에 데이터를 별도로 관리해야겠다는 판단을 했습니다.
또한 인터널 테스터(카페 알바생)와 외부 테스터를 분리해야 했고, public 레포에 공개된 프로젝트 파일이 빌드가 된 채로 유지를 하고싶었습니다. 이러한 이유로 테스트 데이터는 프로젝트 안에 두고 실제 레시피 데이터는 별도 private 레포로 분리하여 submodule로 사용했습니다.

### 3단계: JSON 변환

웹 개발이 확정되면서 Swift 파일을 JSON으로 변환했습니다. 
데이터 수정 코스트를 줄이고 앱과 웹에서 같은 데이터를 사용할 수 있게 되었습니다.

---

## Submodule이란?

Submodule은 원래 **외부 라이브러리나 공통 코드를 여러 프로젝트에서 공유**할 때 쓰는 기능입니다. 오픈소스 라이브러리를 프로젝트에 포함하거나, 여러 서비스에서 공통 모듈을 참조할 때 주로 사용합니다.

저는 Submodule을 **private 데이터 분리 및 저장**하는 용도로 활용했는데요. 
정석적인 목적은 아니지만, 현재 프로젝트 상황에 부합해서 선택하게 되었습니다.

---

## 왜 Submodule?

처음엔 서버를 구축할까 고민을 하다가, 서버 구현 없이도 MVP 구현이 되겠다는 판단이 들었습니다.

SPM도 고려했는데, 웹에서는 사용하기 어려워 제외했습니다. 결국 "Git만으로 데이터 분리하고, iOS와 웹에서 둘 다 쓸 수 있는 방법"을 찾다 보니 Submodule이 적절하다고 생각했습니다.

데이터 레포를 따로 두고 메인 프로젝트에서 참조만 하면 되고, 데이터 레포는 private으로 유지하면서 앱 코드는 public으로 공개할 수 있는 점도 좋았습니다.

---

## 서브모듈 구조

서브모듈 구조는 다음과 같습니다.

```
Blendio/
├── iOS/
├── web/
└── 데이터_서브모듈/      # Submodule (private repo)
    ├── Data/
    │   └── recipes.json
    ├── Models/
    └── Assets.xcassets/
```

`.gitmodules` 파일은 아래와 같이 구성됩니다.

```
[submodule "데이터_서브모듈"]
    path = "데이터_서브모듈"
    url = https://github.com/유저네임/데이터_서브모듈.git
    branch = main
```

---

## Submodule 명령어 정리

### 초기 설정

```bash
# Submodule 추가
git submodule add <repository-url> [path]

# 예시
git submodule add https://github.com/유저네임/데이터_서브모듈.git 데이터_서브모듈
```

### 프로젝트 클론

```bash
# 방법 1: 클론할 때 submodule까지 함께 가져오기
git clone --recurse-submodules <repository-url>

# 방법 2: 이미 클론한 후 submodule 초기화
git submodule init
git submodule update
```

### Submodule 업데이트

```bash
# 원격 저장소의 최신 커밋으로 업데이트
git submodule update --remote

# 특정 submodule만 업데이트
git submodule update --remote 데이터_서브모듈
```

### Submodule 내부 작업 흐름

```bash
# 1. Submodule 디렉토리로 이동
cd 데이터_서브모듈

# 2. 브랜치 확인 (Detached HEAD 상태일 수 있음)
git checkout main

# 3. 변경사항 커밋 & 푸시
git add .
git commit -m "feat: add new recipe"
git push

# 4. 메인 프로젝트로 돌아와서 참조 업데이트
cd ..
git add 데이터_서브모듈
git commit -m "update: update recipe data"
git push
```

### 유용한 명령어

```bash
# 모든 submodule 상태 확인
git submodule status

# submodule 요약 정보 (변경된 커밋 수 등)
git submodule summary

# 모든 submodule에서 명령어 실행
git submodule foreach 'git pull origin main'
```

---

## 주의할 점

### 1. 푸시 순서

**반드시 Submodule을 먼저 푸시**한 후에 메인 프로젝트를 푸시해야 합니다.

```
1. Submodule 푸시
2. 메인 프로젝트 푸시
```

순서가 바뀌면 메인 프로젝트는 존재하지 않는 커밋을 참조하게 됩니다.

### 2. 클론 후 초기화

첫 클론시 submodule 폴더가 비어있을 수 있습니다. `--recurse-submodules` 옵션을 쓰거나, 클론 후 초기화 명령을 실행해야 합니다.

```bash
# 권장
git clone --recurse-submodules <repository-url>

# 또는
git clone <repository-url>
git submodule init
git submodule update
```

---

## 마무리

Submodule의 원래 목적과는 조금 다르게 활용하고 있지만, 
현재 프로젝트 규모와 상황에서는 충분히 잘 동작하고 있어서 만족하며 쓰고 있습니다.

**현재 느끼는 장점은 다음과 같습니다:**
- 데이터 레포를 private으로 유지하면서 프로젝트 코드는 공개 가능
- iOS와 웹에서 동일한 JSON 데이터를 참조
- 서버 없이도 데이터 버전 관리가 가능

**"Submodule도 하나의 독립된 Git 저장소다."** 라는 개념만 이해하면 데이터 동기화를 쉽게 할 수 있기 때문에 저와 비슷한 조건에서 고민을 하고 계신다면, Submodule 사용을 고려해보셔도 좋을 것 같습니다.

글에 잘못된 내용이나 더 좋은 아이디어가 있다면 댓글로 알려주세요!

---


