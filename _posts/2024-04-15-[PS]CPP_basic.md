---
toc: True
tags: Coding_TEST CPP
---

# 배열 초기화
```cpp
#include <stdlib.h>
int visited[10] = { true,};
memset(visited, false, sizeof(visited));
```

# 백터 배열 초기화
```cpp
#include <vector>
vector<int> graph[10];

for(int i=0; i<10; i++){
  graph[i].clear()
}
```
