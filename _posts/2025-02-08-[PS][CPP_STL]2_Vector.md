---

toc: True
tags: PS CPP CPP_STL

---

# 2. Vector

## 2.1 method 사용 예제

![image 3](https://github.com/user-attachments/assets/ac5c67f4-9981-4239-a262-1f1d3fc53bab)


- 정리
    
    
    | 함수 | 설명 |
    | --- | --- |
    | **push_back(value)** | 벡터의 끝에 요소 추가 |
    | **pop_back()** | 벡터의 마지막 요소 제거 |
    | **size()** | 벡터의 크기 반환 |
    | **resize(n)** | 벡터의 크기를 `n`으로 조정 |
    | **clear()** | 벡터의 모든 요소 삭제 |
    | **empty()** | 벡터가 비어있는지 확인 |
    | **begin()** | 벡터의 첫 번째 원소에 대한 반복자 반환 |
    | **end()** | 벡터의 끝을 넘는 반복자 반환 |
    | **insert(pos, value)** | 특정 위치에 요소 삽입 |
    | **erase(pos)** | 특정 위치의 요소 제거 |

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main() {
    // 벡터 선언
    vector<int> vec;

    // 1. push_back: 요소 추가
    vec.push_back(10);
    vec.push_back(20);
    vec.push_back(30);

    cout << "벡터 원소 추가 후: ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
    // 출력: 벡터 원소 추가 후: 10 20 30

    // 2. pop_back: 마지막 요소 삭제
    vec.pop_back();
    cout << "마지막 원소 제거 후: ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
    // 출력: 마지막 원소 제거 후: 10 20

    // 3. size: 벡터 크기 확인
    cout << "벡터 크기: " << vec.size() << endl;
    // 출력: 벡터 크기: 2

    // 4. resize: 크기 조정 (기존보다 크면 기본값 0 추가)
    vec.resize(5);
    cout << "resize(5) 후: ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
    // 출력: resize(5) 후: 10 20 0 0 0

    // 5. clear: 모든 요소 삭제
    vec.clear();
    cout << "clear() 후 벡터 크기: " << vec.size() << endl;
    // 출력: clear() 후 벡터 크기: 0

    // 6. empty: 벡터가 비어 있는지 확인
    if (vec.empty()) cout << "벡터가 비어 있음" << endl;
    // 출력: 벡터가 비어 있음

    // 7. insert: 특정 위치에 요소 삽입
    vec.insert(vec.begin(), 100);  // 맨 앞에 100 추가
    vec.insert(vec.begin() + 1, 200);  // 두 번째 위치에 200 추가
    cout << "insert 후: ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
    // 출력: insert 후: 100 200

    // 8. erase: 특정 위치의 요소 삭제
    vec.erase(vec.begin()); // 첫 번째 요소 제거
    cout << "erase 후: ";
    for (int i = 0; i < vec.size(); ++i) {
        cout << vec[i] << " ";
    }
    cout << endl;
    // 출력: erase 후: 200

    // 9. begin, end: 반복자를 이용한 순회
    vec.push_back(300);
    vec.push_back(400);
    cout << "벡터 순회(begin ~ end): ";
    for (vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: 벡터 순회(begin ~ end): 200 300 400

    return 0;
}

```

## 2.2 Insight 질문

### 2.2.1 Q. vector와 array의 차이는?

**- 배열 (array)**

- **크기 고정**: 배열은 선언 시 크기가 고정되어, 한 번 설정된 크기를 변경할 수 없음.
- **메모리 연속성**: 배열은 메모리에서 연속적으로 배치됨
- **크기 제한**: 배열의 크기가 고정되므로, 크기를 변경하려면 새로운 배열을 생성해야 함
- **할당 방식**: `std::array`(C++11 이후) 또는 C-style array는 정적 메모리 할당

**- 벡터 (vector)**

- **크기 동적 변화**: 벡터는 크기가 동적으로 변경가능,  원소가 추가되거나 삭제되면 크기가 자동으로 조정
- **메모리 연속성**: 벡터도 연속된 메모리를 사용하지만, 크기를 늘릴 때 메모리를 재할당 필요
- **성능**: 원소 추가가 용이하지만, 크기 재조정 시 비용이 발생 가능
- **할당 방식**: 벡터는 동적 메모리 할당을 사용하며, 필요에 따라 메모리가 확장

| 속성 | 배열 (array) | 벡터 (vector) |
| --- | --- | --- |
| 크기 | 고정 | 동적 (필요시 자동 조정) |
| 크기 변경 가능성 | 불가능 | 가능 (push_back 등) |
| 메모리 할당 | 정적 (스택 또는 전역) | 동적 (힙 메모리) |
| 메모리 연속성 | 연속적 | 연속적 (하지만 재할당 시 복사 발생) |

### 2.2.2 Q. vector는 무슨 기준으로 동적으로 element 추가 가능한건지?

동적 크기 조정

- 벡터는 원소가 추가될 떄, 내부 배열이 꽉차면 크기를 자동으로 증가시킴
- 벡터는 메모리 크기를 대게 2배로 증가시킴 (지수 성장)
- 벡터크기가 1일떄 원소를 추가하면 크기가 2로 증가등

재할당 원리

- 동적 메모리를 활용함으로, 크기가 초과하면 새로운 메모리 공간을 할당하고, 기존 데이터를 새로운 메모리로 복사함
- 이 떄문에 push_back은 평균적으로 O(1)이 걸리지만 worstcase는 O(n)이 발생(메모리 공간 재할당 후 데이터 복사까지)

### 2.2.3 Q.vector에서 중간에 값을 넣거나 삭제할 때 복잡도는 얼마인지?

**값 삽입 (중간에 `insert`)**

- 벡터에서 중간에 값을 삽입하는 경우, **삽입 위치 이후의 모든 원소들을 한 칸씩 뒤로 이동**시켜야함
- **시간 복잡도**: **O(n)**(최악의 경우 삽입 위치가 벡터의 처음에 가까울수록 모든 원소가 이동해야함 O(n))

**값 삭제 (중간에 `erase`)**

- 벡터에서 중간에 값을 삭제하는 경우, **삭제 위치 이후의 모든 원소들을 한 칸씩 앞으로 이동**시켜야함.
- **시간 복잡도**: **O(n)**(마찬가지로, 삭제 위치가 처음에 가까우면 모든 원소가 이동해야 하므로 O(n))

### 2.2.4 Q. vector에 struct를 적용하려면 어떻게 해야 하는지?

```cpp
#include <iostream>
#include <vector>
using namespace std;

// 구조체 정의
struct Person {
    string name;
    int age;
    Person(string n, int a) : name(n), age(a) {}
};

int main() {
    // vector에 구조체 삽입
    vector<Person> people;

    // 구조체 객체 생성 후 삽입
    people.push_back(Person("Alice", 30));
    people.push_back(Person("Bob", 25));
    people.push_back(Person("Charlie", 35));

    // 벡터에서 구조체 요소 출력
    for (int i = 0; i < people.size(); ++i) {
        cout << "Name: " << people[i].name << ", Age: " << people[i].age << endl;
    }

    return 0;
}

```
