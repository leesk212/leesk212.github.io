---

toc: True
tags: PS CPP CPP_STL

---

# for, Auto 사용,

### **📌 동작 방식**

이 문장은 `str` 문자열의 각 문자(char)를 **처음부터 끝까지 순차적으로 가져와** `c`에 저장하고, `st.push(c)`를 실행하는 반복문입니다.

```cpp
for (char c : str) {
    st.push(c);
}
```

이 코드는 **C++의 범위 기반 for 루프 (range-based for loop)** 를 사용한 것입니다.

### **`auto`와 `const auto&` 차이점**

### **1️⃣ `auto` (값 복사)**

```cpp
for (auto c : str) {
    st.push(c);
}
```

- `str`에서 **각 문자를 복사**하여 `c`에 저장 (새로운 변수 `c`가 생성됨)
- 성능이 크게 문제되지 않지만, 큰 데이터 타입에서는 복사 비용이 증가할 수 있음

### **2️⃣ `const auto&` (참조)**

```cpp
for (const auto& c : str) {
    st.push(c);
}
```

- `c`는 **`str`의 문자를 직접 참조**하므로 **복사 비용을 줄일 수 있음**
- **읽기 전용(`const`)로 설정**하여 `c` 값을 변경할 수 없도록 함
- `str`이 매우 크다면 **참조를 사용하는 것이 더 효율적**

💡 **문자열처럼 가벼운 데이터는 `auto`로 충분하지만, 큰 데이터(예: 벡터, 맵)를 다룰 때는 `const auto&`가 성능 면에서 유리!** 🚀
