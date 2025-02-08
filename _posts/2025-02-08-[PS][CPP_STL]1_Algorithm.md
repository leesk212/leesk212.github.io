---

toc: True
tags: PS CPP CPP_STL

---

# 1. Algorithm

## 1.1 STL 활용

### 1.1.1 sort

- ref
    1. https://blog.naver.com/ndb796/221227975229

**기본 사용 법 :** 

```cpp
sort(array시작주소, array정렬할끝주소+1, compare)
```

- **기본 오름 차순**
- **내림차순으로 바꾸고싶다면 compare 함수를 직접 짜야함**
    - 구조체에서의 sort compare 함수 ★★★
        
        ```cpp
        bool compare(Student &a, Student &b){
            if(a.name == b.name){   //이름이 같으면, 나이가 적은순
                return a.age < b.age;
            }else{                  //이름 다르면, 이름 사전순
                return a.name < b.name;
            }
        }
        
        int main(void){
            vector<Student> v;
        
            v.push_back(Student("cc", 10));
            v.push_back(Student("ba", 24));
            v.push_back(Student("aa", 11));
            v.push_back(Student("cc", 8));  //cc는 이름이 같으니 나이 기준 오름차순 예시
            v.push_back(Student("bb", 21));
        
            Print(v); //정렬 전 출력
            sort(v.begin(), v.end(), compare); //[begin, end) compare 함수 기준 정렬
            Print(v); //정렬 후 출력
        
            return 0;
        }
        
        ```
        
    - Vector에서의 sort compare 함수
        - ref
            
            https://godog.tistory.com/entry/C-vector-%EC%98%A4%EB%A6%84%EC%B0%A8%EC%88%9C-%EB%82%B4%EB%A6%BC%EC%B0%A8%EC%88%9C-%EC%A0%95%EB%A0%AC
            
        
        ```cpp
        #include <vector>
        #include <algorithm>
        #include <iostream>
        using namespace std;
        
        bool comp(int a, int b) {
            return a > b;
        }
        
        int main() {
            vector<int> v;
            v.push_back(1);
            v.push_back(2);
            v.push_back(3);
            v.push_back(7);
            v.push_back(8);
            v.push_back(1);
            v.push_back(5);
            
            for (int i = 0; i < v.size(); i++) cout << v[i] << " ";
            cout << " 정렬 전\n\n";
        
            sort(v.begin(), v.end());   // 오름차순 정렬
        
            for (int i = 0; i < v.size(); i++) cout << v[i] << " ";
            cout << " 오름차순 정렬 후\n\n";
        
        reverse(v.begin(), v.end());	// 1. 내림차순 정렬 (오름차순정렬 후 같이 사용)
            sort(v.rbegin(), v.rend());		// 2. 내림차순 정렬 ( rbegin()과 rend()를 사용)
            sort(v.begin(), v.end(), comp); // 3. 내림차순 정렬 (비교함수를 사용)
        
            for (int i = 0; i < v.size(); i++) cout << v[i] << " ";
            cout << " 내림차순 정렬 후\n\n";
        
        	return 0;
        }
        ```
        

```cpp
#include <iostream>
#include <algorithm>

using namespace std;

bool compare(int a, int b) {
	return a > b; // 내림차순, 오름차순은 a<b
}

int main() {

	int array_int[10] = { 0, };
	for (int i = 0; i < 10 ; i++) {
		array_int[i] = -1*i;
	}

	cout << "Prev"<<endl;
	for (int i = 0; i < 10; i++) {
		cout << array_int[i] << " ";
	}
  
  /*
   Prev
   0 -1 -2 -3 -4 -5 -6 -7 -8 -9
  */
  
	cout << endl;

	sort(array_int, array_int+3);

	cout << "Post until 3" << endl;
	for (int i = 0; i < 10; i++) {
		cout << array_int[i] << " ";
	}
  
  /*
  Post until 3
	-2 -1 0 -3 -4 -5 -6 -7 -8 -9
  */
    
	cout << endl;

	sort(array_int, array_int+10);

	cout << "Post until 10" << endl;
	for (int i = 0; i < 10; i++) {
		cout << array_int[i] << " ";
	}
  
  /*
  Post until 10
  -9 -8 -7 -6 -5 -4 -3 -2 -1 0
  */
  
	cout << endl;

	sort(array_int, array_int + 10, compare);

	cout << "Post until 10 reverse" << endl;
	for (int i = 0; i < 10; i++) {
		cout << array_int[i] << " ";
	}
	
	/*
	Post until 10 reverse
  0 -1 -2 -3 -4 -5 -6 -7 -8 -9
	*/
}
```

### 1.1.2 lower_bound, upper_bound

```cpp
#include <iostream>
#include <algorithm>
usingnamespace std;

int main() {
	
	vector<int> test = { 1,3,4,5,6,6,10,6,7,8,9 };
	sort(test.begin(), test.end());

	// 응용 1 : 7-4 하면 6의 개수 판단가능	
	cout << upper_bound(test.begin(), test.end(), 6) - test.begin();
	//7
	cout << lower_bound(test.begin(), test.end(), 6) - test.begin();
	//4

	// 응용2 : 특정 범위에 속하는 숫자의 개수 판단가능 (예, 1~6사이수)
	cout << upper_bound(test.begin(), test.end(), 1) - test.begin();
	//1
	cout << lower_bound(test.begin(), test.end(), 6) - test.begin();
	//4

	return 0;
}
```

### 1.1.3 binary_search

```cpp
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> vec = {1, 3, 3, 3, 5, 7, 9};

    int target = 3;

    if (binary_search(vec.begin(), vec.end(), target)) {
        int lower = lower_bound(vec.begin(), vec.end(), target) - vec.begin();
        int upper = upper_bound(vec.begin(), vec.end(), target) - vec.begin() - 1;

        cout << "값 " << target << "은 존재합니다." << endl;
        cout << "첫 번째 등장 위치: " << lower << endl;
        cout << "마지막 등장 위치: " << upper << endl;
    } else {
        cout << "값 " << target << "은 존재하지 않습니다." << endl;
    }

    return 0;
}

값 3은 존재합니다.
첫 번째 등장 위치: 1
마지막 등장 위치: 3
```

### 1.1.4 next_permutaion, pre_permutaion, (을 활용한 combination)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> vec = {1, 2, 3};

    cout << "모든 순열 출력:" << endl;
    do {
        for (int num : vec) cout << num << " ";
        cout << endl;
    } while (next_permutation(vec.begin(), vec.end()));

    return 0;
}
모든 순열 출력:
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    vector<int> vec = {3, 2, 1}; // 내림차순 정렬된 상태로 시작

    cout << "이전 순열 출력:" << endl;
    do {
        for (int num : vec) cout << num << " ";
        cout << endl;
    } while (prev_permutation(vec.begin(), vec.end()));

    return 0;
}
이전 순열 출력:
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n = 5, r = 3;
    vector<int> vec = {1, 2, 3, 4, 5};
    
    // 선택할 원소 개수만큼 1을 채우고 나머지는 0으로 설정
    vector<int> select(n, 0);
    fill(select.begin(), select.begin() + r, 1); // 앞 r개를 1로 설정
    // 11100

    cout << "모든 조합 출력:" << endl;
    do {
        for (int i = 0; i < n; i++) {
            if (select[i]) cout << vec[i] << " ";
        }
        cout << endl;
    } while (prev_permutation(select.begin(), select.end())); // 큰 숫자부터 뽑음

    return 0;
}
모든 조합 출력:
1 2 3
1 2 4
1 2 5
1 3 4
1 3 5
1 4 5
2 3 4
2 3 5
2 4 5
3 4 5

```

## 1.2 생각할 Insight

### 1.2.1 Q. sort는 무슨 알고리즘을 기준으로 구현되어있는가?

- ref
    - https://blockdmask.tistory.com/178
    
- Quick sort (NlogN의 시간 복잡도를 가짐)

![Sorting_quicksort_anim](https://github.com/user-attachments/assets/c2c0508a-6322-4a50-941f-11c6dffb7190)


### 1.2.2 Q. sort를 적용하는게  항상 최선의 정렬방식인가?

아니다, 당연한 얘기지만, quick sort의 단점을 생각해보면 된다. 

- ref
    - https://yabmoons.tistory.com/250
    - https://velog.io/@msung99/Counting-Sort-Radix-Sort-STL-Sort

quick sort의 worst case는 O(n^2)의 시간 복잡도를 가진다.  (pivot이 마지막까지 적절히 발견 안될때)

(기수정렬, 카운팅 정렬 등이 있지만, STL로 구현되어있는가? X, 또한 메모리가 “많이”필요함..)

(결국 돌고돌아 quick sort를 가장 편리하게 쓸 것 이다.-예상)

- 정렬 알고리즘 별 장/단/BigO
  ![image 1](https://github.com/user-attachments/assets/4eb2f803-40e4-422e-bb6d-9febeb8b5c45)

  ![image](https://github.com/user-attachments/assets/ad8e255e-dcc6-408f-b496-bb848120802b)

  

### 1.2.3  Q. lower_bound, upper_bound의 차이는 무엇인가 ?

- 이진 탐색 함수로, 정렬된 범위에서 특정 값의 위치를 찾을 때 사용된다.
- 꼭 오름차순으로 정렬된 자료구조에서 활용해야한다.

### `lower_bound`

- **역할:** 찾고자 하는 `value` 이상(`≥`)의 첫 번째 원소를 가리키는 반복자를 반환합니다.
- **반환값:** `value` 이상의 값이 처음 등장하는 위치를 가리키는 반복자.
- **예제:** `[1, 2, 4, 4, 5, 6]`에서 `lower_bound(4)`는 첫 번째 `4`의 위치를 반환. (3)

### `upper_bound`

- **역할:** 찾고자 하는 `value`보다 큰(`>`) 첫 번째 원소를 가리키는 반복자를 반환합니다.
- **반환값:** `value`보다 큰 값이 처음 등장하는 위치를 가리키는 반복자.
- **예제:** `[1, 2, 4, 4, 5, 6]`에서 `upper_bound(4)`는 `5`의 위치를 반환.

![image 2](https://github.com/user-attachments/assets/0cddebc2-88e1-43e2-97b7-f02eefb6c20d)

### 1.2.4 Q. binary_search를 적용할 때 같은 값이 여러 개 있거나, 타겟 값이 없는 경우, 0번재, 마지막에 타겟이 있는 경우에는 어떻게 적용하는게 좋을까

- 오름차순으로 정렬되어있어야지만 정상 작동 가능하다.
- binary search는 찾고자 하는 자료구조에 해당 값이 있는지 여부를 판단하여 있으면 그 위치를 반환한다.
- 이때,
    - 타겟이 값이 없을 경우, binary search는 False을 반환한다.
    - 타겟이 여러개의 경우, 몇개나 있는지 upper_bound 값 - lower_bound 값을 진행해보면된다.
    - 타겟이 0번쨰의 경우, binary search는 True을 반환하고, lower_bound로 해당 타겟의 index를 찾으면 된다.
        - X **→ 자료 구조형의 가장 첫 인덱스와 마지막 인덱스와의 대소 비교후 진행 (★★★ edge case 염두)**
    - 타겟이 마지막 값의 경우, binary_search는 True를 반환하고, upper_bound로 해당 타겟의 index를 찾으면 된다.
        - **X→ 자료 구조형의 가장 첫 인덱스와 마지막 인덱스와의 대소 비교후 진행 (★★★ edge case 염두)**

### 1.2.5 Q. next, prev permutation은 어떨 때 적용하는게 좋을까. combination은 어떻게 적용해야 할까

1. next, prev permutation의 적용 순간 차이

| 알고리즘 | 순열 생성 방향 | 적용 순간 |
| --- | --- | --- |
| **next_permutation** | 오름차순 → 내림차순 | 모든 순열을 사전식으로 생성, 다음 순열 찾기 |
| **prev_permutation** | 내림차순 → 오름차순 | 모든 순열을 역순으로 생성, 이전 순열 찾기 |

✔ **오름차순 정렬 후 `next_permutation`** → 모든 순열을 생성

✔ **내림차순 정렬 후 `prev_permutation`** → 역순으로 순열을 생성

✔ **특정 순열의 다음/이전 값을 찾을 때 유용**

1. 조합 생성방법 
    
    bitmask를 활용하면 됨 5C3이면, 11100으로 진행해서, 11010 등의 조합을 순열로 찾아내고, 해당 index를 자료구조(vector나 배열)의 index로 넣으면 조합이 됨.
    
    (왜 그럴까? → 어찌보면 당연한것임, 1과 0의 순서는 같은 1끼리, 같은 0끼리는 동일한 순서를 가짐, 즉, 순열에서의 뽑은 순서의 기억을 조합에서는 뽑았다만 있으면 되니, 해당 방법으로 조합을 생성할 수 있게됨)
    

### 1.2.6 Q. 모든 메서드들에 대해서 int, double 같은 기본 타입 말고 struct 를 적용

- 간단함, 아기상어(백준 16236)도 이렇게 풀었음

```cpp
struct information {
	pair<int, int> loc;
	int count_of_eating_feed = 0;
	int time = 0;
};

vector<information>
set<information>
queue<information>
...

함수에서 해당 자료형을 return 시키고 싶다면,

information func(){
...
}
```
