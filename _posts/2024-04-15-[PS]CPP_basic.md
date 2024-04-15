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

# 벡터 배열 최대_소_값, 최대_소_값의 index
```cpp
#include <vector>
#incldue <algorithm>
vector<int> ans;

int max = *max_element(ans.begin(),ans.end());
int max_index = max_element(ans.begin(),ans.end()) - ans.begin();

int min = *min_element(ans.begin(),ans.end());
int min_index = min_element(ans.begin(),ans.end()) - ans.begin();
```

# 벡터 내 특정 값 찾기 index



 
1. find, find_if
 
vector에서 특정 데이터가 존재하는지 확인하고 싶다.

 
그렇다면 algorithm 라이브러리의 find를 사용할 수 있다.

 
find는 반복자를 인자로 갖으면서 배열, vector, deque 처럼 일련의 데이터 구조에서 특정 데이터가 존재하는 확인할 수 있다.

 
또한, find_if를 사용하면 특정 조건에 일치하는 데이터를 탐색할 수 있다.

 

 

 

 

2. 코 드
```cpp
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

bool isOdd(int n) {
    return n % 2 == 1;
}

int main() {
    vector<int> v;
    v.push_back(46);
    v.push_back(67);
    v.push_back(184);
    v.push_back(4);
    v.push_back(17);
    v.push_back(53);
    
    cout << "현재 vector : ";
    for (int i : v) cout << i << " ";
    cout << "\n==============================\n";


    int num = 4;
    auto it = find(v.begin(), v.end(), num);
    if (it == v.end()) {
        cout << num << "은 찾을 수 없습니다.\n";
    }
    else {
        cout << num << "는 존재하며 인덱스는 " << it - v.begin() << " 입니다.\n";
    }
    cout << "\n==============================\n";


    cout << "홀수(Odd)들 \n";
    auto it2 = find_if(v.begin() , v.end(), isOdd);
    while (it2 != v.end()) {
        cout << *it2 << "\n";
        it2 = find_if(it2+1, v.end(), isOdd);
    }

    return 0;
}
```
