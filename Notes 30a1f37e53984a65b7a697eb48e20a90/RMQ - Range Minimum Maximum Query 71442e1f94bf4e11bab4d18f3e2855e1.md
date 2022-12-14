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

L??m theo c??ch b??nh th?????ng ki???u

```cpp
Set f[i][j] = max(a[i], a[i + 1],..., a[j])
```

N???u x??y d???ng ???????c f ??? Tr??? l???i trong O(1) l?? ??i???u hi???n nhi??n

***Nh?????c ??i???m***

- T???n nhi???u b??? nh??? v?? kh??ng ????? ????? ch???y ???????c, v?? l??m m???ng f[N][N] v???i f[i][j] l?? min c???a ??o???n i ??? j, n???u nh?? v???i N = 1e5 th?? b??? nh??? c???n d??ng l?? N x N = 1e10 ??? qu?? l???n, kh??ng kh??? thi
- ????? ph???c t???p r???t cao, h??n c??? ??i???m v??n thi GK c???a b???n

??? C???n c??ch ti???p c???n kh??c hay h??i l???i h???a c???a nyc : Sparse Table (b???ng th??a)

C??? th???

```cpp
f[i][0] = max(a[i]) = a[i]
f[i][1] = max(a[i], a[i + 1] (2^1)
f[i][2] = max(a[i], a[i + 1], a[i + 2], a[i + 3]) (2^2)
...
f[i][k] = max(a[i], a[i + 1], a[i + 2],..., a[i + 2^k - 1] (2^k)
```

L???p b???ng f v???i f[i][k] c?? ngh??a l?? min c???a 2^k s??? b???t ?????u t??? i ??? l??u kh??ng qu?? N * log2N ??? 1e6 v???i N = 1e5 ??? v???a ????? b??? nh??? m?? ch????ng tr??nh c?? th??? ????p ???ng

C??ch x??y d???ng m???ng f

Gi??? s??? t??nh ???????c f[i][0], f[i][1],..., f[i][k - 1] ???i, t??m f[i][k]

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%201.png)

```cpp
=> f[i][k] = max(f[i][k - 1], f[i + 2^(k - 1)][k - 1])
```

L??m th??? n??o ????? s??? d???ng f t??nh max(u, v) v???i u ??? v c???a 1 ??o???n b???t k????

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%202.png)

C???n t??m k l???n nh???t sao cho 2^k ??? v - u + 1 < = > k ??? log2(v - u + 1)

Ta th???y t??? u ?????n ??i???m m??u ????? c?? ????? d??i l?? k, v?? t???n t???i 1 ??o???n n???a c?? ????? d??i k l?? ??o???n ??i???m m??u cam ?????n v, v?? t???t nhi??n 2 ??o???n n??y ph???i giao nhau, n???u kh??ng ta c?? th??? t??ng ????? d??i 1 ??o???n l??n 1

```cpp
k = log2(v - u + 1)
max(u, v) = max(f[u][k], f[v - 2^k + 1][k])
```

Code

# RMQ 2D

?????t f[i][j][k][l] l?? gi?? tr??? nh??? nh???t c???a HCN

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%203.png)

C??c tr?????ng h???p suy bi???n

TH1/ Th??nh 1 ??

```cpp
f[i][j][0][0] = a[i][j]
```

TH2/ Th??nh 1 h??ng

```cpp
f[i][j][0][l] = min(f[i][j][0][l - 1], f[i][j + 2 ^ (l - 1)][0][l - 1])
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%204.png)

TH3/ Th??nh 1 c???t

```cpp
f[i][j][k][0] = min(f[i][j][k][0], f[i + 2 ^ (k - 1)][k - 1][0])
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%205.png)

TH4/ T???ng qu??t

```cpp
I = f[i][j][k - 1][l - 1]
II = f[i][j + 2 ^ (l - 1)][k - 1][l - 1]
III = f[i + 2 ^ ( k - 1)][j][k - 1][l - 1]
IV = f[i + 2 ^ (k - 1)][j + 2 ^ (l - 1)][k - 1][l - 1]

=> f[i][j][k][l] = max(I, II, III, IV)
```

![Untitled](RMQ%20-%20Range%20Minimum%20Maximum%20Query%2071442e1f94bf4e11bab4d18f3e2855e1/Untitled%206.png)

C??ch t??m min c???a (u1, v1, u2, v2)

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