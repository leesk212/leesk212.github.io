---
tags: git etc
---

1. 특정파일을 제외하고 commit 하고자 한다면, ```.gitingnore```이라는 파일을 만들면된다.
2. 해당 파일 내부에 다음과 같이 ```# manually ignored```를 선언해 준뒤, 밑칸부터 제외할 파일들을 적어주면된다.

![image](https://user-images.githubusercontent.com/67637935/159214511-82319480-2e6e-4563-8523-c78045dff38d.png)

3. 그리고 일반적으로 ```git add .```를 수행해주면 gitignore 파일에 적혀있는 파일 이외에 모든 파일들이 commit 준비 상태가 정상적으로 된다.
