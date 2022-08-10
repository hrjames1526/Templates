# RMQ - Range Minimum/ Maximum Query

# Template

```cpp
#include<bits/stdc++.h>
using namespace std;
int n;
const int N = 1e5 + 5;
int a[N];
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	cin >> n;	
	int fmax[n + 1][25], fmin[n + 1][25];
	int m = (int)(log2(n));
	for (int i = 1; i <= n; ++i) {
		cin >> a[i];
		fmax[i][0] = a[i];
		fmin[i][0] = a[i];
	}
	for (int k = 1; k <= m; ++k)
		for (int i = 1; i + (1 << k) - 1 <= n; ++i) {
			fmax[i][k] = max(fmax[i][k - 1], fmax[i + (1 << (k - 1))][k - 1]);
			fmin[i][k] = min(fmin[i][k - 1], fmin[i + (1 << (k - 1))][k - 1]);
		}
	int t; cin >> t;
	while (t--) {
		int u, v;
		cin >> u >> v;
		int k = (int)log2(v - u + 1);
		int x = max(fmax[u][k], fmax[v - (1 << k) + 1][k]);
		int y = min(fmin[u][k], fmin[v - (1 << k) + 1][k]);
		cout << x << " " << y << '\n';
	}
}
```

```cpp
#include<bits/stdc++.h>
using namespace std;
int m, n;
int a[501][501];
int main() {
	ios_base::sync_with_stdio(false);
	cin.tie(0); cout.tie(0);
	cin >> m >> n;
	int K = (int)log2(m), L = (int)log2(n);
	int f[m + 1][n + 1][K + 1][L + 1];
	for (int i = 1; i <= m; ++i) 
		for (int j = 1; j <= n; ++j) {
			cin >> a[i][j];
			f[i][j][0][0] = a[i][j];
		}
	for (int l = 1; l <= L; ++l) 
		for (int i = 1; i <= m; ++i)
			for (int j = 1; j + ( 1 << l) - 1 <= n; ++j) 
				f[i][j][0][l] = min(f[i][j][0][l - 1], f[i][j + (1 << (l - 1))][0][l - 1]);
	for (int k = 1; k <= K; ++k) 
		for (int i = 1; i + (1 << k) - 1 <= m; ++i)
			for (int j = 1; j <= n; ++j) 
				f[i][j][k][0] = min(f[i][j][k - 1][0], f[i + (1 << (k - 1))][j][k - 1][0]);
	for (int k = 1; k <= K; ++k)
		for (int l = 1; l <= L; ++l) 
			for (int i = 1; i + (1 << k) - 1 <= m; ++i)
				for (int j = 1; j + (1 << l) - 1 <= n; ++j) {
					int part1 = f[i][j][k - 1][l - 1];
					int part2 = f[i][j + (1 << (l - 1))][k - 1][l - 1];
					int part3 = f[i + (1 << (k - 1))][j][k - 1][l - 1];
					int part4 = f[i + (1 << (k - 1))][j + (1 << (l - 1))][k - 1][l - 1];
					f[i][j][k][l] = min({part1, part2, part3, part4});
				}
	int q; cin >> q;
	while (q--) {
		int u1, v1, u2, v2;
		cin >> u1 >> v1 >> u2 >> v2;
		int k = (int)log2(u2 - u1 + 1), l = (int)log2(v2 - v1 + 1);
		int part1 = f[u1][v1][k][l];
		int part2 = f[u1][v2 - (1 << l) + 1][k][l];
		int part3 = f[u2 - (1 << k) + 1][v1][k][l];
		int part4 = f[u2 - (1 << k) + 1][v2 - (1 << l) + 1][k][l];
		int ans = min({part1, part2, part3, part4});
		cout << ans << '\n';	
	}
}
```

# RMQ 1D

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled.png)

Làm theo cách bình thường kiểu

```cpp
Set f[i][j] = max(a[i], a[i + 1],..., a[j])
```

Nếu xây dựng được f ⇒ Trả lời trong O(1) là điều hiển nhiên

***Nhược điểm***

- Tốn nhiều bộ nhớ và không đủ để chạy được, vì làm mảng f[N][N] với f[i][j] là min của đoạn i → j, nếu như với N = 1e5 thì bộ nhớ cần dùng là N x N = 1e10 → quá lớn, không khả thi
- Độ phức tạp rất cao, hơn cả điểm văn thi GK của bạn

⇒ Cần cách tiếp cận khác hay hơi lời hứa của nyc : Sparse Table (bảng thưa)

Cụ thể

```cpp
f[i][0] = max(a[i]) = a[i]
f[i][1] = max(a[i], a[i + 1] (2^1)
f[i][2] = max(a[i], a[i + 1], a[i + 2], a[i + 3]) (2^2)
...
f[i][k] = max(a[i], a[i + 1], a[i + 2],..., a[i + 2^k - 1] (2^k)
```

Lập bảng f với f[i][k] có nghĩa là min của 2^k số bắt đầu từ i → lưu không quá N * log2N ≈ 1e6 với N = 1e5 ⇒ vừa đủ bộ nhớ mà chương trình có thể đáp ứng

Cách xây dựng mảng f

Giả sử tính được f[i][0], f[i][1],..., f[i][k - 1] ∀i, tìm f[i][k]

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%201.png)

```cpp
=> f[i][k] = max(f[i][k - 1], f[i + 2^(k - 1)][k - 1])
```

Làm thế nào để sử dụng f tính max(u, v) với u ≤ v của 1 đoạn bất kỳ?

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%202.png)

Cần tìm k lớn nhất sao cho 2^k ≤ v - u + 1 < = > k ≤ log2(v - u + 1)

Ta thấy từ u đến điểm màu đỏ có độ dài là k, và tồn tại 1 đoạn nữa có độ dài k là đoạn điểm màu cam đến v, và tất nhiên 2 đoạn này phải giao nhau, nếu không ta có thể tăng độ dài 1 đoạn lên 1

```cpp
k = log2(v - u + 1)
max(u, v) = max(f[u][k], f[v - 2^k + 1][k])
```

Code

# RMQ 2D

Đặt f[i][j][k][l] là giá trị nhỏ nhất của HCN

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%203.png)

Các trường hợp suy biến

TH1/ Thành 1 ô

```cpp
f[i][j][0][0] = a[i][j]
```

TH2/ Thành 1 hàng

```cpp
f[i][j][0][l] = min(f[i][j][0][l - 1], f[i][j + 2 ^ (l - 1)][0][l - 1])
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%204.png)

TH3/ Thành 1 cột

```cpp
f[i][j][k][0] = min(f[i][j][k][0], f[i + 2 ^ (k - 1)][k - 1][0])
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%205.png)

TH4/ Tổng quát

```cpp
I = f[i][j][k - 1][l - 1]
II = f[i][j + 2 ^ (l - 1)][k - 1][l - 1]
III = f[i + 2 ^ ( k - 1)][j][k - 1][l - 1]
IV = f[i + 2 ^ (k - 1)][j + 2 ^ (l - 1)][k - 1][l - 1]

=> f[i][j][k][l] = max(I, II, III, IV)
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%206.png)

Cách tìm min của (u1, v1, u2, v2)

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%207.png)

```cpp
int k = log2(u2 - u1 + 1)
int l = log2(v2 - v1 + 1)
T1 = f[u1][v1][k][l]
T2 = f[u1][v2 - 2 ^ l + 1][k][l]
T3 = f[u2 - 2 ^ k + 1][v1][k][l]
T4 = f[u2 - 2 ^ k - 1][v2 - 2 ^ k + 1][k][l]
min(u, v) = min(T1, T2, T3, T4)
```

Code