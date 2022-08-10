# Kỹ thuật stack (tạm gọi là Poster)

# Kỹ thuật tìm 1 đoạn lớn nhất để a[i] đạt min

Code

```cpp
a[0] = -INT_MAX;
	a[n + 1] = -INT_MAX;
// Mang previous
	for (int i = 1; i <= n; ++i) {
		int j = i - 1;
		while (a[j] >= a[i]) j = prev[j];
		prev[i] = j;
	}

// Mang next
	for (int i = n; i >= 1; --i) {
		int j = i + 1;
		while (a[j] >= a[i]) j = next[j];
		next[i] = j;
	}
```

# Tìm phần tử đầu tiên về bên phải ≤ val

```cpp
long long f[N][LOG];
long long a[N];
void implement() {
	for (int i = 1; i <= n; i ++) f[i][0] = a[i];
	for (int j = 1; j <= LOG; j ++) for (int i = 1; i <= n - (1LL << j) + 1; i ++)
		f[i][j] = min(f[i][j - 1], f[i + (1LL << (j - 1))][j - 1]);
}
 
int getans(int pos, long long val) {
	for (int i = LOG; i >= 0; i --) 
		if (pos + (1LL << i) - 1 <= n) {
			if (f[pos][i] > val) pos += (1LL << i);
		}
 
	return pos;
}
```

Tìm

- k = prev[i] sao cho a[k] là giá trị đầu tiên < a[i] về bên trái
- l = next[i] sao cho a[l] là giá trị đầu tiên < a[i] về bên phải

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970/Untitled.png)

Ta sẽ đặt j = i - 1, nếu

- a[j] < a[i] thì đặt luôn prev[i] = j bởi a[j] sẵn là số đầu tiên nhỏ hơn a[i] kể từ bên trái
- nếu a[i] ≥ a[j] thì ta sẽ đặt j = prev[j], khi đó j sẽ giảm dần về bên trái cho đến khi a[j] < a[i]

VD

Giả sử với dãy [1, 3, 5, 2, 8, 4, 1] 

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970/Untitled%201.png)

Ta đặt chỉ số đầu của mảng bằng 

- -∞ (nếu trong mảng có số âm)
- -1 nếu cả dãy là số dương
- Hoặc đặt -∞ cả 2 TH cho ok (thường -∞ đặt là -1e9 hoặc -1e18 với số lớn hơn)

Giả sử ta cần tìm số nhỏ nhất về bên trái nhỏ hơn 1, do 1 sẵn là số nhỏ nhất trong mảng rồi, cần canh đầu dãy là 1 số **nhỏ** hơn số 1 để không bị dính vào vòng lặp vô tận. Khi đó, chỉ số nhỏ nhất mà nhỏ hơn 1 là chỉ số 0, tức a[0] = -1 < a[1], hay prev[1] = 0

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970/Untitled%202.png)

Nhìn theo VD trên

- a[j] = 5, a[i] = 2 ⇒ a[j] ≥ a[i], ta nhảy cóc đến chỉ số k đầu tiên bên trái sao cho a[k] ≤ a[j], tức là a[k] ≤ a[i] không làm mất tính tổng quát của thuật toán, ta có thể thấy k = 1, đặt luôn j = k, hay j = prev[j]
- với j = 1 thì trỏ đến chỉ số 0 của mảng, vì a[0] = -1, a[1] = 1 ⇒ a[0] ≤ a[1], đặt k’ = prev[k] hay k’ = prev[j (mới)]

Từ 2 điều trên ta đưa ra nhận xét

với k’ = prev[k], k = prev[j], j = prev[i]

⇒ k’ ≤ k ≤ j ≤ i

⇒ a[k’] ≤ a[k] ≤ a[j], và do vòng lặp cho đến khi mà a[j] ≤ a[i] nên a[k’]  ≤ a[i], theo tính chất của mảng prev là chỉ số đầu tiên bên trái nhỏ hơn a[i] ⇒ đpcm

VD2

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970/Untitled%203.png)

Nói tóm gọn lại, từ 4 → 2 → 1 → 0

⇒ ĐPT O(2n) ≈ O(n), kỹ thuật như 2 con trỏ chạy tịnh tiến trong 1 mảng

Tương tự với mảng next nhưng là giảm dần vòng lặp, đặt đầu cuối của mảng a là a[n + 1] = -INT_MAX

Ứng dụng

Tìm độ dài đoạn con sao cho giá trị nhỏ nhất của nó là K

Khi đó lập mảng prev[1..n] và next[1..n]

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970/Untitled%204.png)

Dễ nhận ra điểm đầu và cuối của đoạn là prev[i] + 1 và next[i] - 1

do đó độ dài đoạn con có min = a[i] là k = [(nex[ti] - 1) - (prev[i] + 1)] + 1 = next[i] - prev[i] - 1 (điểm đầu - điểm cuối + 1)

⇒ Trả lời truy vấn trong O(1) được