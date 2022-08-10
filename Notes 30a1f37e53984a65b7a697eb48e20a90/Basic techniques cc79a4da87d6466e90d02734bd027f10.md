# Basic techniques

# Tổng các ước

```cpp
int sumDiv[N];
	sumDiv[0] = INT_MAX;
	for (int i = 1; i < N; ++i)
		for (int j = i; j <= N; j += i) {
			sumDiv[j] += i;
			sumDiv[j] %= MOD;
		}
```

[Cố định 1 vòng lặp ngoài, khử vòng lặp trong (Fixed Loop) ](Basic%20techniques%20cc79a4da87d6466e90d02734bd027f10/Co%CC%82%CC%81%20%C4%91i%CC%A3nh%201%20vo%CC%80ng%20la%CC%A3%CC%86p%20ngoa%CC%80i,%20khu%CC%9B%CC%89%20vo%CC%80ng%20la%CC%A3%CC%86p%20cb31ee99f3ef45848810ebf4a77e75fb.md)

[2 con trỏ (Two pointers)](Basic%20techniques%20cc79a4da87d6466e90d02734bd027f10/2%20con%20tro%CC%89%20(Two%20pointers)%20f4226ab4d38b4c10b6e2f717b23f4f8c.md)

[Kỹ thuật mảng độ cao](Basic%20techniques%20cc79a4da87d6466e90d02734bd027f10/Ky%CC%83%20thua%CC%A3%CC%82t%20ma%CC%89ng%20%C4%91o%CC%A3%CC%82%20cao%20dad59df985b54c10b3852c77c725ffd1.md)

[Priority queue](Basic%20techniques%20cc79a4da87d6466e90d02734bd027f10/Priority%20queue%20da58e0f8204b44be80b55cce0c3aa2c5.md)