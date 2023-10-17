---
layout: post
title: (WWDC23) Embed the Photos Picker in your app
description: This is a collection of short CSS snippets I thought might be useful for beginners
summary: This is a collection of short CSS snippets I thought might be useful for beginners.
tags: [WWDC, WWDC23]
---

### Intro
- iOS 14에서 재디자인된 Picker UI 신기능: search, zoomable grid
- 해당 세션에서는 앱에 시스템 피커를 임베드할 수 있는 API들에 대해 소개할 것
1. Embedded picker
2. Options menu
3. HDR and Cinematic

### Embedded picker
- 앱에서 실행되는 것 같지만, 다른 프로세스에서 실행됨. 앱의 top 에서 렌더링된것
    - picker에 직접적으로 접근할 수 없음.
    - Picker 내용도 스크린샷 할 수 없음

### Options menu

- picker style
- `.presentation`: 기본
- `.inline`: accessories가 사라진
- `.compact`: 하단의 scrollable picker

### HDR and Cinematic

