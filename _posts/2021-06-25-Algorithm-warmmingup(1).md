---
layout: post
title:  "Programmers 소수만들기"
date:   2021-06-25 +0900
categories: Algorithm Python Programmers
---

## 소수의 정의
우선 문제를 해결하기 위해서는 소수라는 것이 무엇인지 알 필요가 있다.</br>
소수란 간단히 설명하자면 1과 해당 수로만 나누어 질 수 있는 수를 의미한다.</br>

## 문제해결
그렇다면 문제를 보자면 리스트의 3가지의 수를 더해서 소수가 되는 것을 찾아야 하는데,  
가장 간단히 생각해보면 입력받는 리스트의 세개의 요소를 사용하므로 3중루프를 이용해서 하나씩 탐색해보는 방법이 있다.
하나씩 검사하는데 소수를 판별해야하므로 1부터 해당 숫자까지 전부 나눠서 나머지가 0이 되는 순간이 딱 두번만 이루어진다면 해당 수는 소수로 판별한다.

# 소스코드
```python
def solution(nums):
    counter = 0
    answer = 0
    for i in range(len(nums)):
        for j in range(i+1,len(nums)):
            for k in range(j+1,len(nums)):
                temp = nums[i] + nums[j] + nums[k]
                for q in range(1,temp+1):
                    if temp%q == 0:
                        counter = counter + 1
                if counter == 2:
                    answer = answer + 1
                counter = 0
    return answer
```