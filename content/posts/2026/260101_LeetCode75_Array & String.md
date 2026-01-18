+++
title = '[LeetCode 75] Array / String'
date = 2026-01-01T00:00:00+09:00
draft = true
tags =  ["Java", "PS", "LeetCode", "LeetCode75", "Array", "String"]
categories = ["PS"]
summary = ""
+++

## 풀이
##### [1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/?envType=study-plan-v2&envId=leetcode-75)
- 비고 : LC75_1
- 탐색 : 원 포인터
- 설계 : 
- 구현1
```
class Solution {
    public String mergeAlternately(String word1, String word2) {
        int length1 = word1.length();
        int length2 = word2.length();
        StringBuilder sb = new StringBuilder();

        for(int i = 0; i < Math.max(length1, length2); i++){

            if (i < length1) sb.append(word1.charAt(i));

            if (i < length2) sb.append(word2.charAt(i));

        }

        return sb.toString();

    }

}

```
- 구현2
```
class Solution {
    public String mergeAlternately(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        StringBuilder result = new StringBuilder();
        int i = 0, j = 0;

        while (i < m || j < n) {
            if (i < m) {
                result.append(word1.charAt(i++));
            }
            if (j < n) {
                result.append(word2.charAt(j++));
            }
        }

        return result.toString();
    }
}
```

##### [1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)
- 비고 : LC75_1
- 탐색:
- 설계:
- 구현:
```

```

##### [1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
- 비고 : LC75_2
- 탐색:
- 설계:
- 구현:
```

```

##### [1431. Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)
- 비고 : LC75_3
- 탐색:
- 설계:
- 구현:
```

```

##### [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)
- 비고 : LC75_4
- 탐색:
- 설계:
- 구현:
```

```

##### [345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)
- 비고 : LC75_5
- 탐색:
- 설계:
- 구현:
```

```

##### [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
- 비고 : LC75_6
- 탐색:
- 설계:
- 구현:
```

```


##### [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
- 비고 : LC75_7
- 탐색:
- 설계:
- 구현:
```

```


##### [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
- 비고 : LC75_8
- 탐색:
- 설계:
- 구현:
```

```


##### [443. String Compression](https://leetcode.com/problems/string-compression/)
- 비고 : LC75_9
- 탐색:
- 설계:
- 구현:
```

```

## 배움
### Java Syntax
- 
### Techniques
- 
