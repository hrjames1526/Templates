# Point & Range queries

# Fenwick Tree (Binary Indexed Tree - BIT)

- *Fenwick Tree*

```cpp
struct fenwick {
	int size;
	vector<long long> bit;
	void init(int n) {
		size = n;
		bit.assign(n + 1, 0);
	}
	void update(int u, int v) {
		for (; u <= size; u += u & (-u)) bit[u] += v;
	}
	long long get(int u) {
		long long ans = 0;
		for (; u > 0; u -= u & (-u)) ans += bit[u];
		return ans;
	}
};
```

- *Fenwick Tree Max/ Min*

```cpp
struct fenwick {
	int size;
	vector<int> bit;
	void init(int n) {
		size = n;
		bit.assign(n + 1, INT_MIN);
	}
	void update(int u, int v) {
		for (; u <= size; u += u & (-u)) bit[u] = max(bit[u], v);
	}
	int get(int u) {
		int ans = 0;
		for (; u > 0; u -= u & (-u)) ans = max(ans, bit[u]);
		return ans;
	}
};
```

- rFenwick Tree

```cpp
struct fenwick {
	int size;
	vector<int> bit;
	void init(int n) {
		size = n;
		bit.assign(n + 1, INT_MIN);
	}
	void update(int u, int v) {
		for (; u > 0; u -= u & (-u)) bit[u] = max(bit[u], v);
	}
	int get(int u) {
		int ans = 0;
		for (; u <= size; u += u & (-u)) ans = max(ans, bit[u]);
		return ans;
	}
};
```

Note:

```cpp
dad(u) : u -= u & (-u)
prev(u) : u += u & (-u)
```

- *Fenwick Tree 2D*

```cpp
struct FenwickTree2D {
    vector<vector<int>> bit;
    int n, m;
 
    void init(int row, int col) {
    	m = row;
    	n = col;
    	bit.resize(row + 1, vector<int>(col + 1, 0)); 
    }
 
    int get(int x, int y) {
        int ans = 0;
        for (int i = x; i >= 0; i = (i & (i + 1)) - 1)
            for (int j = y; j >= 0; j = (j & (j + 1)) - 1)
                ans += bit[i][j];
        return ans;
    }
 
    void update(int x, int y, int delta) {
        for (int i = x; i < n; i = i | (i + 1))
            for (int j = y; j < m; j = j | (j + 1))
                bit[i][j] += delta;
    }
};
```

- Fenwick Tree with increasing range query - similar to INCSUMST

```cpp
struct fenwick{
	int size;
	vector<int> Mul, Add;
	void init(int n) {
		size = n;
		Mul.assign(n + 1, 0);
		Add.assign(n + 1, 0);
	}
	void upd(int u, int mul, int add) {
		for (; u <= size; u += u & (-u)) {
			Mul[u] += mul;
			Add[u] += add;
		}
	}
	void update(int l, int r, int v) {
		upd(l, v, -v * (l - 1));
		upd(r, -v, v * r);
	}
	int get(int u) {
		int mul = 0;
		int add = 0;
		int start = u;
		for (; u > 0; u -= u & (-u)) {
			mul += Mul[u];
			add += Add[u];
		}
		return mul * start + add;
	}
	int query(int u, int v) {
		return get(v) - get(u - 1);
	}
	
} tree;
```

- Fenwick Tree for set

![Untitled](Point%20&%20Range%20queries%20e116552b28d24c0eab6ebbc9b169b556/Untitled.png)

```cpp
#include<bits/stdc++.h>
using namespace std;
struct fenwick{
	int size;
	vector<int> bit, a, exist;
	void init(int n) {
		size = n;
		bit.assign(n + 1, 0);
		exist.assign(n + 1, 0);
		a.assign(n + 1, 0);
	}
	void update(int u, int v) {
		for (; u <= size; u += u & (-u)) bit[u] += v;
	}
	int get(int u) {
		int ans = 0;
		for (; u > 0; u -= u & (-u)) ans += bit[u];
		return ans;
	}
	void update_element(int u, int v) {
		a[u] = v;
	}
	void update_set(set<int> s) {
		int i = 0;
		for (auto x = s.begin(); x != s.end(); ++x)
			a[++i] = *x;
	}
	void add(int u) {
		int k = lower_bound(a.begin() + 1, a.end(), u)  - a.begin();
		if (exist[k]) return;
		exist[k] = 1;
		update(k, 1);	
	}
	void erase(int u) {
		int k = lower_bound(a.begin() + 1, a.end(), u)  - a.begin();
		if (k > size || a[k] != u || !exist[k]) return;
		exist[k] = 0;
		update(k, -1); 
	}
	int rankkth(int k) {
		if (get(size) < k) return INT_MAX;
		int lo = 0, hi = size;
		while (hi - lo > 1) {
			int mid = (lo + hi) / 2;
			if (get(mid) >= k) 
				hi = mid;
			else
				lo = mid;
		}
		return a[hi];
	}
	int count(int u) {
		int k = lower_bound(a.begin() + 1, a.end(), u) - a.begin() - 1;
		return get(k);
	}
} T;
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	int q; cin >> q;
	vector<char> type(q, 0);
	vector<int> d(q, 0);
	for (int i = 0; i < q; ++i) {
		cin >> type[i] >> d[i];
	}
	set<int> s;
	for (int i = 0; i < q; ++i)
		if (type[i] == 'I') s.insert(d[i]); 
	int n = s.size();
	T.init(n);
	int v = 0;
//	T.update_set(s);
	    for (auto x = s.begin(); x != s.end(); ++x)
    	T.update_element(++v, *x);
	for (int i = 0; i < q; ++i)
		switch (type[i]) {
			case 'I':
				T.add(d[i]);
				break;
			case 'D':
				T.erase(d[i]);
				break;
			case 'K': {
					int ans = T.rankkth(d[i]);
					if (ans != INT_MAX) cout << ans << '\n';
					else cout << "invalid" << '\n';
				}
				break;
			case 'C':
				cout << T.count(d[i]) << '\n';
				break;
		}	
}
```

# Segment Tree

- *Basic Segment Tree Max / Sum*

```cpp
struct STMAX{
	vector<int> T;
	void init(int n) {
		T.resize(n * 4, 0);
	}
	void update(int root, int L, int R, int u, int v) { // v -> a[u]
		if (u < L || u > R) return;
		if (L == R) {
			T[root] = v;
			return;
		}
		int MID = (L + R) / 2;
		if (u <= MID) update(root * 2, L, MID, u, v);
		else update(root * 2 + 1, MID + 1, R, u, v);
		T[root] = max(T[root * 2], T[root * 2 + 1]);
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || l > R) return INT_MIN;
		if (l <= L && R <= r) return T[root];
		int MID = (L + R) / 2;
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		return max(p1, p2); 
	}
};
```

```cpp
struct STSUM{
	vector<int> T;
	void init(int n) {
		T.resize(n * 4, 0);
	}
	void update(int root, int L, int R, int u, int v) {
		if (u < L || u > R) return;
		if (L == R) {
			T[root] = v;
			return;
		}
		int MID = (L + R) / 2;
		if (u <= MID) update(root * 2, L, MID, u, v);
		else update(root * 2 + 1, MID + 1, R, u, v);
		T[root] = T[root * 2] + T[root * 2 + 1];
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || R < l) return 0;
		if (l <= L && R <= r) return T[root];
		int MID = (L + R) / 2;
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		return p1 + p2;
	}
};
```

- Element-increasing Segment Tree Max / Sum

```cpp
struct INCMAXST{
	struct lazytree{
		int lazy, value;
	};
	vector<lazytree> T;
	void init(int n) {
		T.resize(n * 4, {0, 0});
	}
	void down(int root) {
		int t = T[root].lazy;
		T[root * 2].lazy += t;
		T[root * 2].value += t;
		T[root * 2 + 1].lazy += t;
		T[root * 2 + 1].value += t;
		
		T[root].lazy = 0;
	}
	void update(int root, int L, int R, int l, int r, int val) {
		if (r < L || R < l) return;
		if (l <= L && R <= r) {
			T[root].value += val;
			T[root].lazy += val;
			return;
		}
		int MID = (L + R) / 2;
		down(root);
		update(root * 2, L, MID, l, r, val);
		update(root * 2 + 1, MID + 1, R, l, r, val);
		T[root].value = max(T[root * 2].value, T[root * 2 + 1].value);
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || R < l) return INT_MIN;
		if (l <= L && R <= r) return T[root].value;
		int MID = (L + R) / 2;
		down(root);
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		return max(p1, p2);
	}
};
```

```cpp
struct INCSUMST{
	vector<int> value, it;
	void init(int n) {
		value.resize(n * 4, 0);
		it.resize(n * 4, 0);
	}	
	void down(int root) {
		value[root * 2] += value[root];
		value[root * 2 + 1] += value[root];
		value[root] = 0;
	}
	void up(int root, int L, int R) {
		int MID = (L + R) / 2;
		int left = it[root * 2] + (MID - L + 1) * value[root * 2];
		int right = it[root * 2 + 1] + (R - MID) * value[root * 2 + 1];
		it[root] = left + right;
	}
	void update(int root, int L, int R, int l, int r, int val) {
		if (r < L || R < l) return;
		if (l <= L && R <= r) {
			value[root] += val;
			return;
		}
		int MID = (L + R) / 2;
		down(root);
		update(root * 2, L, MID, l, r, val);
		update(root * 2 + 1, MID + 1, R, l, r, val);
		up(root, L, R);
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || R < l) return 0;
		if (l <= L && R <= r) return it[root] + (R - L + 1) * value[root];
		int MID = (L + R) / 2;
		down(root);
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		up(root, L, R);
		return p1 + p2;
	}
};
```

- Element-replacing Segment Tree Max / Sum

```cpp
struct REPMAXST{
	vector<int> it, dt, nho;
	void init(int n) {
		it.resize(n * 4, INT_MIN);
		dt.resize(n * 4, 0);
		nho.resize(n * 4, 0);
	}
	void up(int root) {
		int left = (nho[root * 2] ? dt[root * 2] : it[root * 2]);
		int right = (nho[root * 2 + 1] ? dt[root * 2 + 1] : it[root * 2 + 1]);
		it[root] = max(left, right);
	}	
	void down(int root) {
		dt[root * 2] = dt[root * 2 + 1] = dt[root];
		nho[root * 2] = nho[root * 2 + 1] = 1;
		nho[root] = 0;
	}
	void update(int root, int L, int R, int l, int r, int val) {
		if (r < L || R < l) return;
		if (l <= L && R <= r) {
			nho[root] = 1;
			dt[root] = val;
			return;
		}
		int MID = (L + R) / 2;
		if (nho[root]) down(root);
		update(root * 2, L, MID, l, r, val);
		update(root * 2 + 1, MID + 1, R, l, r, val);
		up(root);
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || R < l) return INT_MIN;
		if (l <= L && R <= r) return (nho[root] ? dt[root] : it[root]);
		int MID = (L + R) / 2;
		if (nho[root]) down(root);
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		up(root);
		return max(p1, p2);
	}
};
```

```cpp
struct REPSUMST{
	vector<int> it, dt, nho;
	void init(int n) {
		it.resize(n * 4, 0);
		dt.resize(n * 4, 0);
		nho.resize(n * 4, 0);
	}
	void up(int root, int L, int R) {
		int MID = (L + R) / 2;
		int left = (nho[root * 2] ? dt[root * 2] * (MID - L + 1) : it[root * 2]);
		int right = (nho[root * 2 + 1] ? dt[root * 2 + 1] * (R - MID) : it[root * 2 + 1]);
		it[root] = left + right;
	}	
	void down(int root) {
		dt[root * 2] = dt[root * 2 + 1] = dt[root];
		nho[root * 2] = nho[root * 2 + 1] = 1;
		nho[root] = 0;
	}
	void update(int root, int L, int R, int l, int r, int val) {
		if (r < L || R < l) return;
		if (l <= L && R <= r) {
			nho[root] = 1;
			dt[root] = val;
			return;
		}
		int MID = (L + R) / 2;
		if (nho[root]) down(root);
		update(root * 2, L, MID, l, r, val);
		update(root * 2 + 1, MID + 1, R, l, r, val);
		up(root, L, R);
	}
	int get(int root, int L, int R, int l, int r) {
		if (r < L || R < l) return 0;
		if (l <= L && R <= r) return (nho[root] ? dt[root] * (R - L + 1) : it[root]);
		int MID = (L + R) / 2;
		if (nho[root]) down(root);
		int p1 = get(root * 2, L, MID, l, r);
		int p2 = get(root * 2 + 1, MID + 1, R, l, r);
		up(root, L, R);
		return p1 + p2;
	}
};
```