---
toc: True
tags: PS PYTHON prepare_coding_test
---

# 2차원 배열 입력 받기

```python
arr=[]
for i in range(B):
	arr.append(list(map(int, input().split())))
```

# 정렬

## List 정렬

```python
>>> a = [1, 10, 5, 7, 6]
>>> a.sort()
>>> a
[1, 5, 6, 7, 10]
>>> a = [1, 10, 5, 7, 6]
>>> a.sort(reverse=True)
>>> a
[10, 7, 6, 5, 1]
```

## Dictionary 정렬


```python

# make a dictionary
pgm_lang = {    "java": 20,     "javascript": 8,     "c": 7,      "r": 4,     "python": 28 } 


# (1) 키를 기준으로 오름차순 정렬 (sort by key in ascending order): sorted()

sorted(pgm_lang.keys())
#['c', 'java', 'javascript', 'python', 'r']

sorted(pgm_lang.items())
#[('c', 7), ('java', 20), ('javascript', 8), ('python', 28), ('r', 4)]


#  (2) 키를 기준으로 내림차순 정렬 (sort by key in descending order): reverse=True
# sorting in reverse order
sorted(pgm_lang.keys(), reverse=True)
['r', 'python', 'javascript', 'java', 'c']
for (key, value) in sorted(pgm_lang.items(), reverse=True):
    print(key, ":", value)

r : 4
python : 28
javascript : 8
java : 20
c : 7

# (3) 값을 기준으로 오름차순 정렬 (sort by value in ascending order)
sorted(pgm_lang.items(), key = lambda item: item[1]) # value: [1]
# [('r', 4), ('c', 7), ('javascript', 8), ('java', 20), ('python', 28)]

```
