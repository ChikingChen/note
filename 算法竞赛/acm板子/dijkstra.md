# dijkstra

```c++
vector<int> dis(n + 1, inf);
vector<int> vis(n + 1);
dis[1] = 0;
priority_queue<array<int, 2>> q;
q.push({0, 1});
while(!q.empty()){
	auto [_, x] = q.top(); q.pop();
	if(vis[x] == 1) continue;
	vis[x] = 1;
	for(auto [v, w] : g[x]){
		if(dis[v] > dis[x] + w){
			dis[v] = dis[x] + w;
			q.push({-dis[v], v});
		}
	}
}
```

