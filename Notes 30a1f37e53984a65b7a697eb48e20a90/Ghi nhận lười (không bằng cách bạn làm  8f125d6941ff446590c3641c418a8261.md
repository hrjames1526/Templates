# Ghi nhận lười (không bằng cách bạn làm bài của thầy =))) (Lazy update)

# 

```cpp
	while (q--) {
		int i1, j1, i2, j2, D;
		cin >> i1 >> j1 >> i2 >> j2 >> D;
		d[i2][j2] += D;
		d[i1 - 1][j2] -= D;
		d[i2][j1 - 1] -= D;
		d[i1 - 1][j1 - 1] += D;
	}
	// update row
	for (int j = 1; j <= n; ++j) 	
		for (int i = m - 1; i >= 1; --i) d[i][j] += d[i + 1][j];
   	// update column
   	for (int i = 1; i <= m; ++i)
   		for (int j = n - 1; j >= 1; --j) d[i][j] += d[i][j + 1];

   	for (int i = 1; i <= m; ++i)
   		for (int j = 1; j <= n; ++j) a[i][j] += d[i][j];
```

![Untitled](Ghi%20nha%CC%A3%CC%82n%20lu%CC%9Bo%CC%9B%CC%80i%20(kho%CC%82ng%20ba%CC%86%CC%80ng%20ca%CC%81ch%20ba%CC%A3n%20la%CC%80m%20%208f125d6941ff446590c3641c418a8261/Untitled.png)

Code 1 cách hết sức bình thường :)) 

```cpp
int q; cin >> q;
while (q--) {
	int u, v, Δ;
	cin >> u >> v >> Δ;
	for (int i = u; i <= v; ++i) a[i] += Δ;
}
```

Độ phức tạp còn cao hơn cả điểm hoá của bạn 🐧

⇒ Cần cách tốt hơn để điểm hoá của bạn không thấp bằng thuật toán trên

Sử dụng 1 mảng cập nhập lười hơn độ thường xuyên đi chạy bộ 😀

Mảng d (Lazy array) với d[i] có nghĩa là từ vị trí i → n sẽ tăng a[i] → a[n] lên Δ đơn vị

Để cập nhập trong 1 khoảng [u, v] thì ta sẽ cập nhập 2 vị trí như sau

```cpp
d[u] += Δ;
dơ[v + 1] -= Δ;
```

Do u ≤ v và tăng các số sau u lên Δ đơn vị, để giới hạn đến vị trí v thì kể từ v + 1 ta phải giảm xuống đi Δ đơn vị

VD

![Untitled](Ghi%20nha%CC%A3%CC%82n%20lu%CC%9Bo%CC%9B%CC%80i%20(kho%CC%82ng%20ba%CC%86%CC%80ng%20ca%CC%81ch%20ba%CC%A3n%20la%CC%80m%20%208f125d6941ff446590c3641c418a8261/Untitled%201.png)

Do tăng từ i → n nên i với i - 1 có ảnh hưởng lẫn nhau

nên sau khi ghi dữ liệu lười ta cần phải cập nhập lại mảng

```cpp
for (int i = 1; i <= n; ++i) d[i] += d[i - 1];
```

Do đó dãy a sau q phép ~~thần thông~~ biến đổi thì a[i] sẽ tăng lên d[i] đơn vị

```cpp
for (int i = 1; i <= n; ++i)
	ans[i] = a[i] + d[i];
```

# Lazy update 2D