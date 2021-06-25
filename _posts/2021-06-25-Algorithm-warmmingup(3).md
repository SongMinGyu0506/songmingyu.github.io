---
layout: post
title:  "Programmers K번째 수"
date:   2021-06-25 +0900
categories: Algorithm Python Programmers
---
## 문제해결
파이썬을 사용할 때, 리스트의 기능과 정렬을 활용하면 10줄 안으로 문제 해결이 가능하다.  
command의 리스트 요소를 활용하는데, 각 첫번째와 두번째 요소로 슬라이싱한 후 정렬  
그리고 세번째 요소로 array의 요소를 answer 리스트에 넣어서 리턴하면 해결

## 소스코드
```python
def solution(array, commands):
    answer = []
    for i in commands:
        temp_list = array[i[0]-1:i[1]]
        temp_list.sort()
        answer.append(temp_list[i[2]-1])
    return answer

solution([1,5,2,6,3,7,4],[[2,5,3],[4,4,1],[1,7,3]])
```