+++
title = "프로그래머스 Greedy 유형"
date = 2025-06-01T00:00:00+09:00
draft = false
tag = ["프로그래머스", "알고리즘", "그리디", "greedy", "algorithm"]
categories = ["알고리즘", "algorithm"]
summary = ""
+++

## 42862 체육복
[🔗 문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

### 내 풀이
> 배열의 크기를 `n + 2`로 만들어줘서 1번 째 학생과 n번 째 학생이 앞뒤 학생에게 체육복을 빌릴 수 있는지를 판별할 때 out of range를 방지할 수 있다.
```
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int answer = 0;
        int[] states = new int[n + 2];
        states[0] = -1;
        states[n + 1] = -1;
        for(int i = 1 ; i <= n ; i++) states[i] = 1;
        for(int num : lost) states[num] -= 1;
        for(int num : reserve) states[num] += 1;
        
        int i = 1;
        do {
            if (states[i - 1] == 2 && states[i] == 0){
                states[i] += 1;
                states[i - 1] -= 1;
            } else if (states[i + 1] == 2 && states[i] == 0) {
                states[i] += 1;
                states[i + 1] -= 1;
            }
            i++;
        } while (i <= n);
        
        for (int state : states) if (state >= 1) answer++;
            
        return answer;
    }
}
```

### 다른 사람 풀이
> 조건문 안에서 `++`, `--` 연산을 해도 총 인원에 영향을 주지 않기 때문에 `answer`를 미리 `n`으로 설정하고, 못빌리는 것이 확정일 때 `n--` 해주는 접근이 좋았다. 시간 복잡도가 상수배 이라서 큰 영향은 없을 수 있지만, states 전체를 순회하면서 카운팅하는 부분을 제외하면 루프 한 번을 덜 돌아도 됐었다.
그리고 배열의 크기를 n + 2로 지정해줬기 때문에 굳이 do-while을 쓰지 않아도 됐었다.
```
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int[] people = new int[n];
        int answer = n;

        for (int l : lost) 
            people[l-1]--;
        for (int r : reserve) 
            people[r-1]++;

        for (int i = 0; i < people.length; i++) {
            if(people[i] == -1) {
                if(i-1>=0 && people[i-1] == 1) {
                    people[i]++;
                    people[i-1]--;
                }else if(i+1< people.length && people[i+1] == 1) {
                    people[i]++;
                    people[i+1]--;
                }else 
                    answer--;
            }
        }
        return answer;
    }
}
```

### 💡 노트
> 배열을 선언할 때 기본타입(Primitive type)의 배열인 경우 원소들은 초기 값을 가지고 있는 반면에(int = 0) 참조타입(Reference type)의 배열을 선언했을 경우 배열 원소의 초기값이 null임을 주의해야 한다.
```
int[] arr = new int[3]; // 3의 크기를 가지고 초기값 0으로 채워진 배열 생성
// arr[0] >> null
// arr[1] >> null
// arr[2] >> null

// Class 타입(참조 타입)의 인스턴스를 원소로 가지는 배열을 생성했을 때. 
Student[] students = new Student[5];     //Student Class의 인스턴스 최대 5개 할당할 수 있는 배열
// students[0] >> null
// students[1] >> null
// students[2] >> null
// students[3] >> null
// students[4] >> null
```
[👀 참고 링크](https://ifuwanna.tistory.com/231)

## 42862 체육복
[🔗 문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862)