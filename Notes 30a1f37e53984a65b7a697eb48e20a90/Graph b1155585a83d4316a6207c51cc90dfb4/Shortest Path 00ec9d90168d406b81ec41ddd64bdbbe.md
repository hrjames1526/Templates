# Shortest Path

# Dijkstra

```cpp
void dijkstra(int start, vector<int> &d, vector<int> &p) {
	d.resize(n + 1, INT_MAX);
	p.resize(n + 1, 0);

	set<pair<int, int> > q;
	d[start] = 0;
	q.insert({d[start], start});
	while (q.size()) {
		int u = q.begin()->second; q.erase(q.begin());
		for (auto x : adj[u]) {
			int v = x.first, L = x.second;
			if (d[u] + L < d[v]) {
				if (d[v] < INT_MAX) q.erase({d[v], v});
				d[v] = d[u] + L;
				p[v] = u;
				q.insert({d[v], v});
			}
		}
	}
}
```

# FordBellman

```cpp
bool FordBellman(int start, vector<int> &d, vector<int> &p) {
	d.resize(n + 1, INT_MAX);
	p.resize(n + 1, 0);
	vector<bool> inqueue(n + 1, false);
	vector<int> cnt(n + 1, 0);
	queue<int> q;
	d[start] = 0;
	q.push(start);
	inqueue[start] = true;
	while (q.size()) {
		int u = q.front(); q.pop();
		inqueue[u] = false;
		for (auto x : adj[u]) {
			int v = x.first, L = x.second;
			if (d[u] + L < d[v]) {
				d[v] = d[u] + L;
				p[v] = u;
				if (!inqueue[v]) {
					inqueue[v] = true;
					++cnt[v];
					q.push(v);
					if (cnt[v] > n) return false;
				}
			}
		}
	}
	return true;
}
```

# BFS 0 - 1

```cpp
void BFS01(int start, vector<int> &d, vector<int> &p) {
	d.resize(n + 1, INT_MAX);
	p.resize(n + 1, 0);
	d[start] = 0;
	deque<int> q;
	q.push_front(start);
	while (q.size()) {
		int u = q.front(); q.pop_front();
		for (auto x : adj[u]) {
			int v = x.first, L = x.second;
			if (d[u] + L < d[v]) {
				d[v] = d[u] + L;
				p[v] = u;
				if (L == 1) q.push_back(v);
				else q.push_front(v);
			}
		}
	}
}
```

```cpp
vector<int> path;
	for (int v = 3; v != 0; v = p[v]) path.push_back(v);
	reverse(path.begin(), path.end());
```