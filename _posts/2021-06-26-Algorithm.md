---
layout: post
title:  "Programmers 모의고사"
date:   2021-06-26 +0900
categories: Algorithm Python Programmers
---

__임시저장__

```python
def solution(answers):
    result = []
    one_counter = 0
    two_counter = 0
    three_counter = 0
    one_people = [1,2,3,4,5]
    two_people = [2,1,2,3,2,4,2,5]
    three_people = [3,3,1,1,2,2,4,4,5,5]
    
    for index,value in enumerate(answers):
        if value == one_people[index%5]: one_counter +=1
        if value == two_people[index%8]: two_counter +=1
        if value == three_people[index%10]: three_counter +=1

    count_dict = {1:one_counter,2:two_counter,3:three_counter}
    top_score = max(count_dict.values())
    result = [index for index,value in count_dict.items() if top_score == value]

    return result
```