---

toc: True
tags: PS CPP CPP_STL

---


# 3. List

## 3.1. method 사용 예제

```cpp
#include <iostream>
#include <list>

using namespace std;

int main() {
    // 1️⃣ list 선언
    list<int> lst;

    // 2️⃣ push_back: 뒤에 원소 추가
    lst.push_back(10);
    lst.push_back(20);
    lst.push_back(30);

    cout << "push_back 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: push_back 후: 10 20 30

    // 3️⃣ push_front: 앞에 원소 추가
    lst.push_front(5);
    lst.push_front(2);

    cout << "push_front 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: push_front 후: 2 5 10 20 30

    // 4️⃣ pop_back: 뒤에서 원소 제거
    lst.pop_back();
    cout << "pop_back 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: pop_back 후: 2 5 10 20

    // 5️⃣ pop_front: 앞에서 원소 제거
    lst.pop_front();
    cout << "pop_front 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: pop_front 후: 5 10 20

    // 6️⃣ size: list 크기 확인
    cout << "list 크기: " << lst.size() << endl;
    // 출력: list 크기: 3

    // 7️⃣ clear: list의 모든 원소 제거
    lst.clear();
    cout << "clear() 후 list 크기: " << lst.size() << endl;
    // 출력: clear() 후 list 크기: 0

    // 8️⃣ empty: list가 비어있는지 확인
    if (lst.empty()) cout << "list가 비어 있음" << endl;
    // 출력: list가 비어 있음

    // 9️⃣ insert: 특정 위치에 원소 삽입
    lst.push_back(100);
    lst.push_back(200);
    lst.push_back(300);

    // insert 후
    auto it = lst.begin();
    advance(it, 1);  // 두 번째 원소 위치
    lst.insert(it, 150);  // 150을 두 번째 위치에 삽입

    cout << "insert 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: insert 후: 100 150 200 300

    // 🔟 erase: 특정 위치의 원소 제거
    it = lst.begin();
    advance(it, 2);  // 세 번째 원소 위치 (200)
    lst.erase(it);   // 세 번째 원소 (200) 제거

    cout << "erase 후: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: erase 후: 100 150 300

    // 1️⃣1️⃣ begin, end: 반복자를 이용한 순회
    cout << "begin ~ end 순회: ";
    for (list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    // 출력: begin ~ end 순회: 100 150 300

    return 0;
}

```

- 정리
    
    
    | 함수 | 설명 |
    | --- | --- |
    | **push_back(value)** | 리스트의 뒤에 `value`를 추가 |
    | **push_front(value)** | 리스트의 앞에 `value`를 추가 |
    | **pop_back()** | 리스트의 뒤에서 원소 제거 |
    | **pop_front()** | 리스트의 앞에서 원소 제거 |
    | **size()** | 리스트의 원소 개수 반환 |
    | **clear()** | 리스트의 모든 원소 삭제 |
    | **empty()** | 리스트가 비어있는지 확인 |
    | **begin()** | 리스트의 첫 번째 원소를 가리키는 반복자 반환 |
    | **end()** | 리스트의 끝을 가리키는 반복자 반환 |
    | **insert(pos, value)** | 특정 위치에 `value`를 삽입 |
    | **erase(pos)** | 특정 위치의 원소를 삭제 |

## 3.2. 생각할 Insight

### **3.2.1.  Q. `std::list`는 어떤 자료구조로 이루어져 있는가?**

`std::list`는 “양방향 연결 리스트 (Doubly Linked List)”로 구현됩니다. 각 원소는 **노드**라고 불리며, 노드는 다음 두 가지 정보를 저장합니다:

- **값 (value)**: 실제 데이터.
- **다음 노드에 대한 포인터 (next)**: 리스트에서 다음 노드를 가리키는 포인터.
- **이전 노드에 대한 포인터 (prev)**: 리스트에서 이전 노드를 가리키는 포인터.

이 구조 덕분에 리스트는 원소의 삽입과 삭제가 매우 빠르며, 특정 위치에서 원소를 효율적으로 이동할 수 있습니다. 단, 순차 접근만 가능하고 **랜덤 액세스**는 불가능합니다.

### **3.2.2. Q. `push`, `pop`, `insert`, `erase`의 복잡도**

`std::list`에서 `push`, `pop`, `insert`, `erase`와 같은 연산의 **시간 복잡도**는 아래와 같습니다:


| 연산 | 복잡도 |
| --- | --- |
| **push_back()** | O(1) |
| **push_front()** | O(1) |
| **pop_back()** | O(1) |
| **pop_front()** | O(1) |
| **insert()** | O(1) **(노드 삽입만 고려)**, **O(n)** (특정 위치를 찾는 데) |
| **erase()** | O(1) **(노드 삭제만 고려)**, **O(n)** (특정 위치를 찾는 데) |

- **push_back() / push_front() / pop_back() / pop_front()**: 연결 리스트에서 처음 또는 끝에 원소를 삽입하거나 삭제하는 것은 O(1)입니다. 연결 리스트는 양방향 링크가 있으므로 포인터만 수정하면 되기 때문입니다.
- **insert() / erase()**: 삽입 또는 삭제하려는 위치를 찾는 데 O(n)의 시간이 걸립니다. 하지만 삽입이나 삭제 작업 자체는 O(1)입니다.

### **3.2.3. Q.`list`에서 인덱스로 접근하려면 어떻게 해야 할지? 인덱스로 접근한다는 생각이 의미가 있는 것일지?**

`std::list`는 **연결 리스트**이므로, **인덱스를 통한 랜덤 접근**을 직접 지원하지 않습니다. 이는 배열과 달리 **연속적인 메모리 공간**을 사용하지 않기 때문입니다.

- 인덱스 접근을 하려면 **반복자를 사용하여** 리스트를 처음부터 끝까지 순차적으로 탐색해야 합니다.

```cpp
cpp
복사편집
list<int>::iterator it = lst.begin();
advance(it, index);  // 인덱스 위치로 반복자를 이동
```

따라서, `std::list`에서 **인덱스로 접근한다는 것은 비효율적**입니다. 연결 리스트는 순차적 접근이 최적화된 자료구조이기 때문에, **인덱스를 사용하는 것보다 반복자를 사용하는 것이 더 나은 접근 방법**입니다.

- **인덱스로 접근**하는 것은 연결 리스트의 특성과 맞지 않으며 비효율적입니다. 연결 리스트에서는 **반복자를 사용한 순차 탐색**이 더 자연스럽고 효율적입니다.

---

### **3.2.4. Q. `vector`와 `list`의 사용 상황**

**`vector`를 사용하는 상황**

- **랜덤 접근이 필요한 경우**: 인덱스를 사용하여 특정 위치에 빠르게 접근해야 할 경우 `std::vector`가 유리합니다. 벡터는 연속적인 메모리 공간을 사용하므로 인덱스 접근이 O(1)입니다.
- **메모리 관리**: 벡터는 내부적으로 메모리를 **연속적으로 할당**하여, 캐시 친화적이며, 동적 크기 조정 기능이 있습니다. 원소의 추가가 많다면 벡터가 더 적합할 수 있습니다.

**`list`를 사용하는 상황**

- **원소의 삽입/삭제가 빈번한 경우**: `std::list`는 원소가 중간에 삽입되거나 삭제될 때 성능이 뛰어납니다. 중간에 삽입/삭제하는데 O(1)의 시간이 걸리며, 원소를 이동시키는 데 효율적입니다.
- **랜덤 접근이 필요하지 않은 경우**: `std::list`는 **순차 접근**에 최적화되어 있기 때문에, 인덱스를 통한 빠른 접근이 필요하지 않거나 원소의 순차적 탐색만 필요한 경우에 적합합니다.

| 상황 | 사용 적합 자료구조 |
| --- | --- |
| 원소 삽입/삭제가 빈번한 경우 | `std::list` |
| 인덱스 접근과 빠른 랜덤 접근이 필요한 경우 | `std::vector` |
| 메모리 연속성 및 캐시 성능을 고려할 경우 | `std::vector` |

---

### **3.2.5. Q.`list`에 `struct`를 적용하려면 어떻게 해야 하는지?**

```cpp
#include <iostream>
#include <list>

using namespace std;

// 구조체 정의
struct Person {
    string name;
    int age;
    Person(string n, int a) : name(n), age(a) {}
};

int main() {
    // list에 구조체 저장
    list<Person> people;

    // 구조체 객체 생성 후 추가
    people.push_back(Person("Alice", 30));
    people.push_back(Person("Bob", 25));
    people.push_back(Person("Charlie", 35));

    // list 출력
    for (list<Person>::iterator it = people.begin(); it != people.end(); ++it) {
        cout << "Name: " << it->name << ", Age: " << it->age << endl;
    }

    return 0;
}

```
