# Hàng đợi 2 đầu quản lý max/ min (not finished)

Ý tưởng : Giả sử ta có 1 tập hợp các phần tử luôn thay đổi

Khi bỏ đi x :

- Nếu x < max ⇒ max không đổi
- Nếu x = max ⇒ max = giá trị lớn thứ nhì

⇒ Để quản lý max, ta cần phải lưu 1 danh sách

![Untitled](Ha%CC%80ng%20%C4%91o%CC%9B%CC%A3i%202%20%C4%91a%CC%82%CC%80u%20qua%CC%89n%20ly%CC%81%20max%20min%20(not%20finishe%2022977cfd11b54d58ac6a2d2ba1ec45bc/Untitled.png)

Nếu thêm phần tử x = vào

- Nếu x > qR ⇒ qR không bao giờ là giá trị max → bỏ đi