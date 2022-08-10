# Kỹ thuật mảng độ cao

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20ma%CC%89ng%20%C4%91o%CC%A3%CC%82%20cao%20dad59df985b54c10b3852c77c725ffd1/Untitled.png)

Cách làm thông thường : tạo mảng tiền tố 2D → tính → ĐPT O(n^4) quá lớn

Cần cải tiến đến O(n^2)

# Kỹ thuật mảng độ cao :

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20ma%CC%89ng%20%C4%91o%CC%A3%CC%82%20cao%20dad59df985b54c10b3852c77c725ffd1/Untitled%201.png)

## Tư tưởng : duyệt lần lượt i từ 1 → hàng cuối cùng

Với mỗi giá trị i được duyệt, vd điểm m, ta sẽ đặt đường thẳng song song với chiều của j, ta sẽ thao tác HCN có đáy là đường thẳng song song đi qua m, tức HCN với 4 đỉnh màu đỏ

```cpp
for (int i = 1; i <= m; ++i) {
	// Thao tac voi HCN co day la m
	...
	/// Duyet theo chieu cua j
	for (int j = 1; j <= n; ++j) {
		// Do something :))
	}
}
```

Trở lại với bài toán đầu tiêu đề

Với mỗi đường thẳng song song đi qua m thì ta sẽ tìm HCN có đáy là đường thẳng đi qua m :))) và diện tích là lớn nhất

Cần làm

- Lập mảng h[1..n] với h[i] là số ô chứa số 1 liên tiếp nhiều nhất

![Untitled](Ky%CC%83%20thua%CC%A3%CC%82t%20ma%CC%89ng%20%C4%91o%CC%A3%CC%82%20cao%20dad59df985b54c10b3852c77c725ffd1/Untitled%202.png)

- Làm bài @ [Kỹ thuật stack (tạm gọi là Poster)](../Ky%CC%83%20thua%CC%A3%CC%82t%20stack%20(ta%CC%A3m%20go%CC%A3i%20la%CC%80%20Poster)%2069e8743acf994df5b0ca69c85ff50970.md)

⇒ Tìm tấm poster có diện tích lớn nhất sao cho nó chứa toàn số 1 ⇒ tương tự nhau

### ⇒ Ý tưởng chung : đặt đáy là 1 đường thẳng duyệt theo i, từ đó đưa về bài toán 1 chiều và áp dụng kiến thức đã học để hoàn thiện