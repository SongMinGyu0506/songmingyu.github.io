---
layout: post
title:  "Programmers 음양더하기"
date:   2021-06-25 +0900
categories: Algorithm Python Programmers
---
## 문제해결
sign 길이와 absolutes의 길이가 같다고 한다. 그렇다면 sign을 확인하여 false일때만 음수로 변환하여 최종 변환된 값을 전부 더해주면 해결

## 소스코드
```python
def solution(absolutes, signs):
    answer = 0
    counter = 0
    temp_absolutes = []
    for i in signs:
        if i == True:
            temp_absolutes.append(absolutes[counter])
            counter = counter + 1
        else:
            temp_absolutes.append(absolutes[counter]-(absolutes[counter]*2))
            counter = counter + 1
    for i in temp_absolutes:
        answer = answer + i
    return answer
```