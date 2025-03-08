# 프로까지 고정(3try)
- 1차 : 다익스트라 
- 2차 : 뭔 깡구현 ??? (도로길 병합)
- 3차 : STL 활용 깡구현 (인기 검색어)

---

## 알고리즘

### 다익스트라

![dijkstra_slower(1)](https://github.com/user-attachments/assets/b4b1eb21-321b-48d6-83a7-18db13300851)


> 1. visited 함수를 거리형식으로 증가시키는 것 vs bool 타입으로 고정하고 queue에 dist를 가져가는것

* 물론 문제마다 다르겠지만, 속도관련해서 큰 차이가 없었다.


> 2. 다익을 쓸때 dist 백터

* 다익을 쓰자! 하게되면 웬만하면 노드의 수는 정해져있을 것이다. (아니면 정해지게 만들어야지)
* 그러면 가능하다면 동적할당을 쓰자. (다익의 노드수가 더럽게 많다면 다익은 많이 느려진다.)

```cpp
int* dists = new int[total_gate_cnt + 1];
```


> 3. 다익에 pq를 써야할까? 찾는 노드에 도착하면 바로 종료해도될까?

* pq로 구현을 안할 수도 있다. 왜냐하면 pq는 하나의 옵션 기능인 느낌이다ㅣ.
* 다익(by pq)를 진행하게되면, 한 점에서 최소 비용으로 다음 노드를 찾아가는게 보장되는 것이다.
* 따라서 찾고자 하는 노드에 도착하게되면 바로 종료시켜도된다.

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
```


> 4. 다익에 visited 함수를 넣고 말고 하는것

* 넣는게 훨씬 빠르다.(안넣어도 된다는건 chatGPT의 할루시..)
  다만 어떻게 넣는가에 대해서는 문제마다 다르지만,
  나는 pq에 넣기 직전에 넣는게 원론적으로 이해가 빠르다.
  뭔가 다음 갈 노드 queue에 넣기전에, 방문했으면 안넣는 느낌

```cpp
while (!pq.empty()) {
	int cur_dist = pq.top().first;
	int cur_node = pq.top().second;
	pq.pop();

	if (cur_node == end) { //도착
		dist = dists[end];
		break;
	}

	for (int next_node = 1; next_node < total_gate_cnt + 1; next_node++) {
		int next_dist = gate_graph[cur_node][next_node];

		if (next_dist == 0 || next_dist == 9999999) { //문제따라서 사용
			continue;
		}
		if (gates[next_node].enable == 0) { //문제따라서 사용
			continue;
		}

		if (dists[next_node] > dists[cur_node] + next_dist) {
			dists[next_node] = dists[cur_node] + next_dist;

			if (visited[next_node] != 0) {
				continue;
			}
			visited[next_node] = 1;
			pq.push({ dists[next_node], next_node });
		}

	}
}
```

> 5. 다익으로 최장거리가 가능할까? 불가능

* 지금 생각해보면 당연하지만, ACM_CRAFT 문제 풀때는 될거라고 생각하고 계속 디버깅했다..
* 다익은 최소거리를 보장시켜서만 갈 수 있다. (왜냐면 음수가 안되기 떄문에)
* 그럼 최장거리 보장하면서 갈 수 있는 방법은 어떤 것이 있을까?
  1. 다익+DAG(위상정렬) --> ACM Craft
     * 핵심은 위상정렬을 활용하여, 진입차수가 0인 것들을 미리 queue에 넣어놓는 다는 것이다.

        	
	<details>
 	<summary> 코드 </summary>
		
	```cpp
	#include <iostream>
	#include <vector>
	#include <queue>
	
	using namespace std;
	
	const int INF = -1e9;
	
	void longestPathInDAG(int n, vector<vector<pair<int, int>>>& adj) {
	    vector<int> inDegree(n, 0), dist(n, INF);
	    queue<int> q;
	
	    // 진입 차수 계산
	    for (int u = 0; u < n; u++) {
	        for (int j = 0; j < adj[u].size(); j++) {
	            int v = adj[u][j].first;
	            inDegree[v]++;
	        }
	    }
	
	    // 진입 차수가 0인 노드 큐에 삽입
	    for (int i = 0; i < n; i++) {
	        if (inDegree[i] == 0) {
	            q.push(i);
	            dist[i] = 0; // 시작점
	        }
	    }
	
	    // 위상 정렬 수행하면서 최장 경로 갱신
	    while (!q.empty()) {
	        int u = q.front();
	        q.pop();
	
	        for (int j = 0; j < adj[u].size(); j++) {
	            int v = adj[u][j].first;
	            int w = adj[u][j].second;
	
	            dist[v] = max(dist[v], dist[u] + w);
	            if (--inDegree[v] == 0) {
	                q.push(v);
	            }
	        }
	    }
	
	    // 결과 출력
	    for (int i = 0; i < n; i++) {
	        cout << "Node " << i << ": " << (dist[i] == INF ? "Unreachable" : to_string(dist[i])) << endl;
	    }
	}
	
	int main() {
	    int n = 6;
	    vector<vector<pair<int, int>>> adj(n);
	
	    // 그래프 간선 추가 (DAG)
	    adj[0].push_back(make_pair(1, 5));
	    adj[0].push_back(make_pair(2, 3));
	    adj[1].push_back(make_pair(3, 6));
	    adj[1].push_back(make_pair(2, 2));
	    adj[2].push_back(make_pair(4, 4));
	    adj[2].push_back(make_pair(5, 2));
	    adj[2].push_back(make_pair(3, 7));
	    adj[3].push_back(make_pair(5, 1));
	    adj[4].push_back(make_pair(5, 3));
	
	    longestPathInDAG(n, adj);
	
	    return 0;
	}
	
 	```
 
  	</details>


  2. 벨만포드 알고리즘
	* 음수 가중치 가능
	
	<details>
 	<summary> 코드 </summary>

	```cpp
	#include <iostream>
	#include <vector>
	#include <limits>
	
	using namespace std;
	
	const int INF = -1e9;
	
	void longestPathBellmanFord(int n, int start, vector<vector<pair<int, int>>>& edges) {
	    vector<int> dist(n, INF);
	    dist[start] = 0;
	
	    // (노드 수 - 1)번 반복하며 갱신
	    for (int i = 0; i < n - 1; i++) {
	        for (int u = 0; u < n; u++) {
	            for (int j = 0; j < edges[u].size(); j++) {
	                int v = edges[u][j].first;
	                int w = edges[u][j].second;
	
	                if (dist[u] != INF) {
	                    dist[v] = max(dist[v], dist[u] + w);
	                }
	            }
	        }
	    }
	
	    // 결과 출력
	    for (int i = 0; i < n; i++) {
	        cout << "Node " << i << ": " << (dist[i] == INF ? "Unreachable" : to_string(dist[i])) << endl;
	    }
	}
	
	int main() {
	    int n = 6, start = 0;
	    vector<vector<pair<int, int>>> edges(n);
	
	    // 그래프 간선 추가
	    edges[0].push_back(make_pair(1, 5));
	    edges[0].push_back(make_pair(2, 3));
	    edges[1].push_back(make_pair(3, 6));
	    edges[1].push_back(make_pair(2, 2));
	    edges[2].push_back(make_pair(4, 4));
	    edges[2].push_back(make_pair(5, 2));
	    edges[2].push_back(make_pair(3, 7));
	    edges[3].push_back(make_pair(5, 1));
	    edges[4].push_back(make_pair(5, 3));
	
	    longestPathBellmanFord(n, start, edges);
	
	    return 0;
	}

 	```
 
	</details>





## 구현

# 시험

> 1. 시험장의 컴파일러

* 다행히 구조체를 생성할때 생성자를 안적어도되지만, 만약 비슷한 에러가 뜬다면 되는것들은 페어로 처리하자

```cpp
struct gate_info {
    pair<int, int> loc;
    int enable;
 
    // 생성자 함수 추가
    gate_info(pair<int, int> loc = {0, 0}, int enable = 1) : loc(loc), enable(enable) {}
};
 
```

