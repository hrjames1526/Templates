# 2 con trỏ (Two pointers)

Với ý tưởng sử dụng 2 con trỏ (2 biến chạy) trong 1 vòng lặp thì độ phức tạp sẽ là O(2n) ≈ O(n) đủ để chạy với dữ liệu 1e6

## Template

### Chạy xuôi

```cpp
int j = 1;
for (int i = 1; i <= n; ++i) {
	while (j <=n) {
		Do something better than you :))
		++j
	}
	...
}
```

### Chạy ngược

```cpp
int i = 1;
int j = n;
while (i <= j) {
	...
	if (you are slepping) ++i;
	else --j;
}
```