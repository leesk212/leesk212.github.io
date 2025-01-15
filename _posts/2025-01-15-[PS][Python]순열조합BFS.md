---
toc: True
tags: PS PYTHON
---

# Python Library 활용

```python

test_list = [1,2,3,4,5]

from itertools import permutations
nPr = list(permutations(test_list,3))
print(nPr)
# [(1, 2, 3), (1, 2, 4), (1, 2, 5), (1, 3, 2), (1, 3, 4), (1, 3, 5), (1, 4, 2), (1, 4, 3), (1, 4, 5), (1, 5, 2), (1, 5, 3), (1, 5, 4), (2, 1, 3), (2, 1, 4), (2, 1, 5), (2, 3, 1), (2, 3, 4), (2, 3, 5), (2, 4, 1), (2, 4, 3), (2, 4, 5), (2, 5, 1), (2, 5, 3), (2, 5, 4), (3, 1, 2), (3, 1, 4), (3, 1, 5), (3, 2, 1), (3, 2, 4), (3, 2, 5), (3, 4, 1), (3, 4, 2), (3, 4, 5), (3, 5, 1), (3, 5, 2), (3, 5, 4), (4, 1, 2), (4, 1, 3), (4, 1, 5), (4, 2, 1), (4, 2, 3), (4, 2, 5), (4, 3, 1), (4, 3, 2), (4, 3, 5), (4, 5, 1), (4, 5, 2), (4, 5, 3), (5, 1, 2), (5, 1, 3), (5, 1, 4), (5, 2, 1), (5, 2, 3), (5, 2, 4), (5, 3, 1), (5, 3, 2), (5, 3, 4), (5, 4, 1), (5, 4, 2), (5, 4, 3)]

from itertools import combinations
nCr = list(combinations(test_list, 3))
print(nCr)
# [(1, 2, 3), (1, 2, 4), (1, 2, 5), (1, 3, 4), (1, 3, 5), (1, 4, 5), (2, 3, 4), (2, 3, 5), (2, 4, 5), (3, 4, 5)]

#중복순열
from itertools import product
# 중복 가능한 객체를 계속 추가 시켜줘야함
nPr_dup = list(product(test_list, test_list))
print(nPr_dup)
# [(1, 1), (1, 2), (1, 3), (1, 4), (1, 5), (2, 1), (2, 2), (2, 3), (2, 4), (2, 5), (3, 1), (3, 2), (3, 3), (3, 4), (3, 5), (4, 1), (4, 2), (4, 3), (4, 4), (4, 5), (5, 1), (5, 2), (5, 3), (5, 4), (5, 5)]

nPr_dup = list(product(test_list, test_list, test_list))
print(nPr_dup)
# [(1, 1, 1), (1, 1, 2), (1, 1, 3), (1, 1, 4), (1, 1, 5), (1, 2, 1), (1, 2, 2), (1, 2, 3), (1, 2, 4), (1, 2, 5), (1, 3, 1), (1, 3, 2), (1, 3, 3), (1, 3, 4), (1, 3, 5), (1, 4, 1), (1, 4, 2), (1, 4, 3), (1, 4, 4), (1, 4, 5), (1, 5, 1), (1, 5, 2), (1, 5, 3), (1, 5, 4), (1, 5, 5), (2, 1, 1), (2, 1, 2), (2, 1, 3), (2, 1, 4), (2, 1, 5), (2, 2, 1), (2, 2, 2), (2, 2, 3), (2, 2, 4), (2, 2, 5), (2, 3, 1), (2, 3, 2), (2, 3, 3), (2, 3, 4), (2, 3, 5), (2, 4, 1), (2, 4, 2), (2, 4, 3), (2, 4, 4), (2, 4, 5), (2, 5, 1), (2, 5, 2), (2, 5, 3), (2, 5, 4), (2, 5, 5), (3, 1, 1), (3, 1, 2), (3, 1, 3), (3, 1, 4), (3, 1, 5), (3, 2, 1), (3, 2, 2), (3, 2, 3), (3, 2, 4), (3, 2, 5), (3, 3, 1), (3, 3, 2), (3, 3, 3), (3, 3, 4), (3, 3, 5), (3, 4, 1), (3, 4, 2), (3, 4, 3), (3, 4, 4), (3, 4, 5), (3, 5, 1), (3, 5, 2), (3, 5, 3), (3, 5, 4), (3, 5, 5), (4, 1, 1), (4, 1, 2), (4, 1, 3), (4, 1, 4), (4, 1, 5), (4, 2, 1), (4, 2, 2), (4, 2, 3), (4, 2, 4), (4, 2, 5), (4, 3, 1), (4, 3, 2), (4, 3, 3), (4, 3, 4), (4, 3, 5), (4, 4, 1), (4, 4, 2), (4, 4, 3), (4, 4, 4), (4, 4, 5), (4, 5, 1), (4, 5, 2), (4, 5, 3), (4, 5, 4), (4, 5, 5), (5, 1, 1), (5, 1, 2), (5, 1, 3), (5, 1, 4), (5, 1, 5), (5, 2, 1), (5, 2, 2), (5, 2, 3), (5, 2, 4), (5, 2, 5), (5, 3, 1), (5, 3, 2), (5, 3, 3), (5, 3, 4), (5, 3, 5), (5, 4, 1), (5, 4, 2), (5, 4, 3), (5, 4, 4), (5, 4, 5), (5, 5, 1), (5, 5, 2), (5, 5, 3), (5, 5, 4), (5, 5, 5)]

#중복조합
from itertools import combinations_with_replacement

# 중복 가능한 객체를 계속 추가 시켜줘야함
nCr_dup = list(combinations_with_replacement(test_list,3))
print(nCr_dup)
# [(1, 1, 1), (1, 1, 2), (1, 1, 3), (1, 1, 4), (1, 1, 5), (1, 2, 2), (1, 2, 3), (1, 2, 4), (1, 2, 5), (1, 3, 3), (1, 3, 4), (1, 3, 5), (1, 4, 4), (1, 4, 5), (1, 5, 5), (2, 2, 2), (2, 2, 3), (2, 2, 4), (2, 2, 5), (2, 3, 3), (2, 3, 4), (2, 3, 5), (2, 4, 4), (2, 4, 5), (2, 5, 5), (3, 3, 3), (3, 3, 4), (3, 3, 5), (3, 4, 4), (3, 4, 5), (3, 5, 5), (4, 4, 4), (4, 4, 5), (4, 5, 5), (5, 5, 5)]

nCr_dup = list(combinations_with_replacement(test_list,2))
print(nCr_dup)
# [(1, 1), (1, 2), (1, 3), (1, 4), (1, 5), (2, 2), (2, 3), (2, 4), (2, 5), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5), (5, 5)]

```

# DFS 활용 구현

## 순열, 중복순열

```python
test_list = [1,2,3,4,5]

N, r = 5,3
selected = [ 0 for _ in range(5)]

# 순열
def nPr(selected, collected, cnt):
    if cnt == r:
        print(collected)
        return
    else:
        for cur in range(0, N):
            if selected[cur] != 0:
                continue
            selected[cur] = 1
            collected.append(test_list[cur])
            nPr(selected, collected, cnt+1)
            collected.pop()
            selected[cur] = 0

nPr(selected,[],0)

# 중복순열
def nPr_dup(collected, cnt):
    if cnt == r:
        print(collected)
        return
    else:
        for cur in range(0, N):
            collected.append(test_list[cur])
            nPr_dup(collected, cnt+1)
            collected.pop()

nPr_dup([],0)


```

## 조합, 중복조합

```python


```


# BFS Sample

```python
import sys
from collections import deque

sys.stdin = open('./sample.txt')
input = sys.stdin.readline

TC = int(input())


def find_target(start, end, value, N):
    visited = [ 0 for _ in range(N+1) ]

    queue = deque()
    queue.append((start, value))

    while queue:
        cur,cur_value = queue.popleft()
        if cur == end:
            value = cur_value
            break

        for key,value in graph[cur]:
            next_ = key
            next_value = value
            if visited[next_] !=0:
                continue
            visited[next_] = 1
            queue.append((next_,next_value+cur_value))

    if value == 0:
        return 0
    else:
        return value


for tc in range(1, TC+1):
    N, M = map(int, input().split())
    B = int(input())
    answer = 0

    graph = {}
    for _ in range(M):
        data_temp = list(map(int, input().split()))
        if data_temp[0] not in graph:
            graph[data_temp[0]] = [(data_temp[1],data_temp[2])]
        else:
            graph[data_temp[0]].append((data_temp[1],data_temp[2]))

    print(graph)

    # 1. Let's find bomul
    First_result = find_target(1,B,0, N )

    if First_result != 0:
        # Let's find start
        Second_result = find_target(B,1,0, N)
        if Second_result != 0:
            answer = First_result+Second_result

    if answer == 0:
        print(f'#{tc} NO')
    else:
        print(f'#{tc} YES {answer}')
```

