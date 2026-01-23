+++
title = '[Swift] 딕셔너리 (Dictionary)  🖋️'
date = 2026-01-01T00:00:00+09:00
draft = false
tags =  ["Collection", "Swift", "Apple Platforms"]
categories = ["Apple Platforms"]
summary = ""
+++

## 설명
- Key-Value 저장하는 컬렉션 타입
- 해시 테이블로 구현되어있음
	- 빠른 속도로 접근, 수정, 추가, 키 존재 여부 확인 가능 O(1)
```
let scores = ["roy": 95, "gucci": 82, "yeni": 90]

// 모두 O(1) 시간
let royScore = scores["roy"]           // 95
let gucciScore = scores["gucci"]       // 82
scores["terry"] = 75                    // 추가
```


## 유의
### 제거시 O(n)의 가능성
- 값 제거 (removeValue(forKey: key))의 경우 기술적으로는 O(1)로 구현되어 있음 
- 하지만 Copy-On-Write 최적화 때문에 O(n) 복잡도를 가질 수 있음.
```
// 첫 번째 Dictionary
var dict1 = ["a": 1, "b": 2, "c": 3]

// dict2는 dict1과 메모리를 공유 (Copy-on-Write)
var dict2 = dict1

// 이 제거 연산은 dict1을 수정하기 전에 전체 복사가 필요할 수 있음 (O(n))
dict1.removeValue(forKey: "a")
```


## 활용
### Key 존재 확인
```
// 👍 O(1) : 직접 접근해서 확인
if dict["key"] != nil { ... }

// 😱 O(n) : 배열로 변환 후 순회하기 때문에 비효율적이다.
if dict.keys.contains("yeni") { ... }
```
### Value 조회
```
// 👍 O(1) : 해시 기반 직접 조회
let value = scores["yeni"]

// 🤩 값이 없을 수도 있으므로 Optional 처리가 필요하다.
if let score = scores["yeni"] {
    print("Alice's score: \(score)")
}
```
