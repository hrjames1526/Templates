# Cố định 1 vòng lặp ngoài, khử vòng lặp trong (Fixed Loop)

![Untitled](Co%CC%82%CC%81%20%C4%91i%CC%A3nh%201%20vo%CC%80ng%20la%CC%A3%CC%86p%20ngoa%CC%80i,%20khu%CC%9B%CC%89%20vo%CC%80ng%20la%CC%A3%CC%86p%20cb31ee99f3ef45848810ebf4a77e75fb/Untitled.png)

```cpp
for (int i = 1; i <= n; ++i) 
	for (int j = 1; j <= n; ++j) {
		int segment1 = prefix[j] - prefix[i - 1];
		int segment2 = prefix[n] - prefix[j - 1] + s[i]; 
		ans = max({ans, segment1, segment2});
	}
```

We need a better method 🥴

Cố định 1 vòng lặp, ta sẽ cố định vòng lặp j  để thuật toán giảm còn xuống O(n)

khi vòng j cố định thì

- segment1 = prefix[j] - prefix[i - 1] → **max** ⇒ prefix[i - 1] → **min**
- segment2 = prefix[n] - prefix[j - 1] + prefix[i] ⇒ **max** → prefix[i] → **max**

Vậy nếu cố định vòng lặp j cần phải xác định GTLN và TGNN của các vị s trước vị trí j

```cpp
int smax = smin = 0;
	for (int j = 1; j <= n; ++j) {
		int t = prefix[j] - smin;
		ans = max(ans, t);
		t = prefix[n] - prefix[j - 1] + smax;
		ans = max(ans, t);
		smin = min(smin, prefix[j]);
		smax = max(smax, prefix[j]);
	}
```