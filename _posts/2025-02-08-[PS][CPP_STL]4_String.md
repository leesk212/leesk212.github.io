---

toc: True
tags: PS CPP CPP_STL

---

# 4. String ★★★

## 4.1 method 활용 예제

```cpp
#include <iostream>
#include <string>

using namespace std;

int main() {
    // 1️⃣ 문자열 선언
    string str = "Hello, World!";

    // 2️⃣ size: 문자열의 길이 (문자 수) 반환
    cout << "문자열 길이 (size): " << str.size() << endl;
    // 출력: 문자열 길이 (size): 13

    // 3️⃣ clear: 문자열 비우기
    str.clear();
    cout << "clear() 후 문자열 길이: " << str.size() << endl;
    // 출력: clear() 후 문자열 길이: 0

    // 4️⃣ empty: 문자열이 비어있는지 확인
    cout << "문자열이 비어있는지 확인 (empty): " << (str.empty() ? "비어 있음" : "비어 있지 않음") << endl;
    // 출력: 문자열이 비어있는지 확인 (empty): 비어 있음

    // 문자열을 다시 설정
    str = "Hello, World!";

    // 5️⃣ substr: 문자열의 부분 문자열 추출
    string sub_str = str.substr(7, 5); // 인덱스 7부터 5글자 추출
    cout << "부분 문자열 (substr): " << sub_str << endl;
    // 출력: 부분 문자열 (substr): World

    // 6️⃣ find: 특정 문자열을 찾기 (찾으면 인덱스를 반환, 못 찾으면 -1 반환)
    size_t found = str.find("World");
    if (found != string::npos) {
        cout << "'World'가 발견된ㅏ 위치: " << found << endl;
    } else {
        cout << "'World'를 찾을 수 없음" << endl;
    }
    // 출력: 'World'가 발견된 위치: 7

    // 7️⃣ insert: 특정 위치에 문자열 삽입
    str.insert(5, " Beautiful");
    cout << "insert() 후 문자열: " << str << endl;
    // 출력: insert() 후 문자열: Hello Beautiful, World!

    // 8️⃣ erase: 특정 위치부터 문자열 지우기
    str.erase(5, 11); // 인덱스 5부터 11개의 문자 삭제
    cout << "erase() 후 문자열: " << str << endl;
    // 출력: erase() 후 문자열: Hello, World!

    return 0;
}

```

- 정리
    
    
    | 함수 | 설명 |
    | --- | --- |
    | **size()** | 문자열의 길이(문자의 개수)를 반환. |
    | **clear()** | 문자열을 비운다. 길이가 0이 된다. |
    | **empty()** | 문자열이 비어 있는지 확인. 비어 있으면 `true`, 아니면 `false`. |
    | **substr(pos, len)** | 주어진 위치(`pos`)부터 `len` 길이만큼의 부분 문자열을 반환. |
    | **find(str)** | 특정 문자열 `str`을 찾고, 첫 번째 발생 위치의 인덱스를 반환. 못 찾으면 `string::npos`를 반환. |
    | **insert(pos, str)** | 문자열의 `pos` 위치에 문자열 `str`을 삽입. |
    | **erase(pos, len)** | `pos` 위치부터 `len` 길이만큼의 문자열을 삭제. |

## 4.2 생각할 Insight

### **4.2.1. Q.`string` 자료구조의 구성**

`string`은 C++ 표준 라이브러리에서 제공하는 문자열 클래스. 내부적으로는 **동적 배열**을 사용하여 문자열을 저장하고 있습니다. `string`은 내부적으로 `char` 배열을 관리하며, 문자열을 동적으로 처리할 수 있는 다양한 기능을 제공합니다. 일반적으로 `string`의 내부 구조는 다음과 같습니다:

- **동적 배열 (Dynamic Array)**: 문자열의 데이터를 저장하는 메모리 공간.
- **문자열 길이**: 현재 문자열의 길이를 저장하는 변수.
- **메모리 관리**: 문자열이 변경될 때, 크기를 자동으로 확장하거나 축소하는 메커니즘이 포함되어 있습니다. `string`은 **버퍼링 기법**을 사용하여 성능을 최적화합니다. 예를 들어, 문자열의 크기가 커지면 일괄적으로 버퍼를 확장하고, 줄어들면 메모리를 줄일 수 있습니다.

### **4.2.2. Q.`string`과 `char*`의 차이**

- **`string`**: C++에서 제공하는 클래스 형태의 문자열입니다. 동적 메모리를 사용하여 문자열의 크기를 자동으로 관리합니다. 메모리 할당, 크기 조정, 문자열 조작 기능을 내장하고 있어 다양한 편리한 메서드를 제공합니다.
    
    예시:
    
    ```cpp
    string str = "Hello, World!";
    ```
    
- **`char*`**: C 스타일 문자열로, 문자 배열의 포인터입니다. 문자열의 끝은 널 문자 `'\0'`로 구분되며, 크기를 따로 추적하지 않으므로 사용자가 직접 메모리 관리와 크기 조정을 해야 합니다.
    
    예시:
    
    ```cpp
    char* str = "Hello, World!";
    ```
    

차이점:

- **메모리 관리**: `string`은 메모리를 자동으로 관리하며, `char*`는 메모리 관리가 수동적입니다.
- **편의성**: `string`은 문자열 길이 추적, 동적 확장, 복잡한 문자열 조작을 쉽게 할 수 있는 다양한 메서드를 제공합니다. 반면, `char*`은 문자열 크기 및 메모리 관리를 사용자가 직접 해야 하므로 더 복잡합니다.
- **안전성**: `string`은 메모리 오버플로우를 방지하는 기능을 제공하지만, `char*`은 안전하지 않으며 버퍼 오버플로우 문제를 유발할 수 있습니다.

---

### **4.3.3. Q. `string`과 `char*` 간의 변환 방법**

- **`string`에서 `char*`로 변환**

`string`을 `char*`로 변환하려면 `string::c_str()` 메서드를 사용하여 C 스타일 문자열을 얻을 수 있습니다. 이때 반환된 `char*`는 **읽기 전용**입니다.

```cpp
string str = "Hello, World!";
const char* cstr = str.c_str();
```

- **`char*`에서 `string`으로 변환**

`char*`를 `string`으로 변환하려면 `string` 생성자를 사용하여 쉽게 할 수 있습니다.

```cpp
char* cstr = "Hello, World!";
string str(cstr);
```

---

### **4.3.4. Q. `string` 값 변경 (수정, append, 중간 수정 등)**

`string`에서는 다양한 방법으로 문자열 값을 변경할 수 있습니다:

- **수정 (수정 가능한 문자 참조)**

문자열의 특정 위치에 있는 문자를 변경하려면 `[]` 연산자를 사용하거나, `at()` 메서드를 사용할 수 있습니다. (인덱스가 범위를 벗어나면 `at()`은 예외를 던집니다.)

```cpp
string str = "Hello, World!";
str[7] = 'w';  // 특정 문자 수정
// str == "Hello, world!"
```

- **append (문자열 추가)**

`string`에 문자열을 추가하려면 `append()` 또는 `+=` 연산자를 사용할 수 있습니다.

```cpp
string str = "Hello";
str.append(", World!");
// 또는
str += "!!!";
// str == "Hello, World!!!"
```

- **중간 문자열 수정 (insert, erase)**

문자열 중간에 삽입하거나 삭제하려면 `insert()`와 `erase()` 메서드를 사용할 수 있습니다.

```cpp
string str = "Hello, World!";
str.insert(5, ", Beautiful");
// str == "Hello, Beautiful World!"
str.erase(5, 10);  // 인덱스 5부터 10글자 삭제
// str == "Hello World!"
```

---

### **4.3.5. Q.`substr` 메서드 동작 및 타겟 길이를 모를 때 중간부터 끝까지 검색하기**

`substr()`는 문자열의 일부를 추출하는 메서드입니다. 두 가지 매개변수를 가질 수 있습니다:

- **`pos`**: 시작 위치 (0부터 시작)
- **`len`**: 추출할 길이

만약 길이를 모를 경우, `substr`의 두 번째 매개변수를 생략하거나 `string::npos`를 사용하여 문자열의 끝까지 추출할 수 있습니다.

**-예시 1**: `substr`을 사용하여 특정 위치부터 끝까지 추출

```cpp
string str = "Hello, World!";
string sub = str.substr(7);  // 인덱스 7부터 끝까지 추출
cout << sub << endl;  // 출력: World!
```

**-예시 2**: 길이를 모를 때, 문자열의 끝까지 추출하기

```cpp
string str = "Hello, World!";
string sub = str.substr(7, string::npos);  // 인덱스 7부터 끝까지
cout << sub << endl;  // 출력: World!
```

`string::npos`는 문자열의 끝을 나타내는 값으로, 이를 사용하면 문자열의 끝까지 자를 수 있습니다.

**-중간부터 끝까지 검색하기**

만약 `substr`을 사용하여 **중간부터 끝까지의 문자열을 추출**하려면, 시작 인덱스를 `find()` 메서드를 이용하여 검색한 후 해당 인덱스에서부터 끝까지 추출할 수 있습니다.

```cpp
string str = "Hello, World!";
size_t pos = str.find("World");  // "World"라는 부분 문자열을 찾음
if (pos != string::npos) {
    string sub = str.substr(pos);  // "World"부터 끝까지 추출
    cout << sub << endl;  // 출력: World!
}
```


map 이랑 set 내에 struct 자료형 활용하면서 Index tree같은 테크닉도 필요할 경우 있음

정렬된 상태가 필요없다면 unorded_map → 사용
