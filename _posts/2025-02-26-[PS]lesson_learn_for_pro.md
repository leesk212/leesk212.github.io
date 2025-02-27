# 구현

> 1. visited 함수를 거리형식으로 증가시키는 것 vs bool 타입으로 고정하고 queue에 dist를 가져가는것

* 물론 문제마다 다르겠지만, 속도관련해서 큰 차이가 없었다.

> 2. 다익에 visited 함수를 넣고 말고 하는것

![dijkstra_slower(1)](https://github.com/user-attachments/assets/b4b1eb21-321b-48d6-83a7-18db13300851)


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

		if (next_dist == 0 || next_dist == 9999999) {
			continue;
		}
		if (gates[next_node].enable == 0) {
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



> 3. 다익에 pq를 써야할까? 찾는 노드에 도착하면 바로 종료해도될까?

* pq로 구현을 안할 수도 있다. 왜냐하면 pq는 하나의 옵션 기능인 느낌이다ㅣ.
* 다익(by pq)를 진행하게되면, 한 점에서 최소 비용으로 다음 노드를 찾아가는게 보장되는 것이다.
* 따라서 찾고자 하는 노드에 도착하게되면 바로 종료시켜도된다.

```cpp
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
```

> 4. 다익을 쓸때 dist 백터

* 다익을 쓰자! 하게되면 웬만하면 노드의 수는 정해져있을 것이다. (아니면 정해지게 만들어야지)
* 그러면 가능하다면 동적할당을 쓰자. (다익의 노드수가 더럽게 많다면 다익은 많이 느려진다.)

```cpp
int* dists = new int[total_gate_cnt + 1];
```

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

