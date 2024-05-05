---
toc: True
tags: Coding_TEST CPP
---

# 1. BFS 방문처리 시점

[현이의 로봇 청소기](https://www.acmicpc.net/problem/30106)

해당 문제를 풀다가 방문처리 로직을 실패해서 계속 시간초과가 떴다..

그 이유를 판단하고 싶었다.

그존에 BFS 로직은 다음과 같다.

---

```cpp
void bfs(int x,int y) {
	queue<pair<int,int>> q;
	q.push({ x,y });
	visited[x][y] = 1;


	while (!q.empty()) {
		
		int cur_x = q.front().first;
		int cur_y = q.front().second;
		q.pop();

  	visited[cur_x][cur_y] = 1; // 문제가 되는 로직
	

		for (int i = 0; i < 4; i++) {
			int next_x = cur_x + garo[i];
			int next_y = cur_y + sero[i];

			if (next_x <0 || next_y <0 || next_x >= N || next_y >= M) {
				continue;
			}

			if (visited[next_x][next_y] == 1) {
				continue;
			}

			int diff = abs(map[cur_x][cur_y] - map[next_x][next_y]);
			if (diff > K) {
				continue;
			}
			
			q.push({ next_x, next_y });
		}

	}

}
```

---

```cpp
void bfs(int x,int y) {
	queue<pair<int,int>> q;
	q.push({ x,y });
	visited[x][y] = 1;


	while (!q.empty()) {
		
		int cur_x = q.front().first;
		int cur_y = q.front().second;


		q.pop();

		for (int i = 0; i < 4; i++) {
			int next_x = cur_x + garo[i];
			int next_y = cur_y + sero[i];

			if (next_x <0 || next_y <0 || next_x >= N || next_y >= M) {
				continue;
			}

			if (visited[next_x][next_y] == 1) {
				continue;
			}

			int diff = abs(map[cur_x][cur_y] - map[next_x][next_y]);
			if (diff > K) {
				continue;
			}
			
			q.push({ next_x, next_y });
			visited[next_x][next_y] = 1; //다음으로 바꿔주니 정상 동작하였다.
		}

	}

}
```

그 이유를 고찰해보니, queue에 넣고 바로 방문처리를 하지 않으면 


# 2. BFS,DFS 중에 어떤걸 써야할까?

혹자의 판단은 다음과 같다.

[SAS_장OO] [오후 1:17] 보통 상하좌우 움직이면
[SAS_장OO] [오후 1:17] Bfs 가 편함
[SAS_장OO] [오후 1:18] Dfs는 탐색시킨 결과를 꼭 확인하고 돌아가고 반복하는 케이스들
[SAS_장OO] [오후 1:18] Ex) permutation 구하기
[SAS_장OO] [오후 1:18] 맵 기쥰으로 뭐 막 돌아다닌다 = bfs

+) 구글링한 결과는 다음과 같다. 

<ref : https://foameraserblue.tistory.com/188>
🟦 그렇다면 DFS와 BFS를 선택함에 있어 어떤 기준이 필요할까

내 개인적인 경험으로(나는 DFS를 더 자주 사용한다) 대부분의 경우 DFS와 BFS 어느것을 선택해도 무방한 문제들이 많다. (필자가 경험한 코딩테스트 수준의 알고리즘)

나는 보통 DFS는 재귀적인 특징과 백트래킹을 이용한, 모든 경우를 하나하나 전부 탐색하는 완전탐색 문제를 풀때 선호하고(대표적으로 조합 순열 구현)
BFS는 depth(깊이)특징을 이용한 문제(대표적으로 최단경로)를 풀어야할때 선호한다.

DFS
모든 노드를 확인해야 할 경우
경로의 특징을 저장해야할 경우 (서로 다른 가중치를 갖고 있다던가 제약이 있을 때)
BFS
최단 거리
임의의 경로 찾기 (미로탐색)

