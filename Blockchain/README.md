# Blockchain 

Blockchain (chuỗi khối) là công nghệ sổ cái phân tán xuất hiện từ năm 2008 với Bitcoin, dựa trên ý tưởng mật mã có từ trước đó. Những năm gần đây nó được chú ý nhiều hơn nhất là trong Dev và Crypto nhờ tính phi tập trung, minh bạch, đồng thuận và khả năng đảm bảo toàn vẹn dữ liệu, có tiềm năng thay đổi trong tài chính nhờ tính minh bạch cao không qua trung gian (bên thứ 3) như ngân hàng.
Trước hết tìm hiểu về Blockchain, chúng ta sẽ tìm hiểu Hash, 1 công nghệ không thể thiếu trong Blockchain


## Hash là gì ?
Hash (băm) là hàm băm 1 chiều, khác với mã hóa có giải mã song song thì băm không thể nào giải mã được
- Mục đích: dùng để kiểm tra tính toàn vẹn, định danh của nội dung 
- Các loại hash phổ biến hiện nay
    + SHA-256/SHA-512 [độ dài: 256/512 bit]: phổ biến, độ dài an toàn 
    + SHA-3 (Keccak) [độ dài: 224-512 bit]: chuẩn mới của NIST
    + BLAKE2/3 [độ dài: 256-512 bit]: rất nhanh, an toàn 
    + Keccak-256 [độ dài: 256 bit]: dùng trong Ethereum
- Cách hoạt động: 
    [Nội dung dài ngắn tùy ý] -> [Hàm Băm] -> [Mã băm (dài cố định)]
    + Nội dung có thể dài ngắn (1 ký tự -> bài văn cũng được)
    + Khi đi qua hàm băm (tùy loại) thì sẽ cho ra mã băm có độ dài khác nhau 
    VD: SHA-256 (256 bit đầu ra) sẽ cho ra mã băm có độ dài 64 ký tự 
- Cái hay của Hash là chỉ cần thay đổi 1 ký tự trong nội dung sẽ cho ra mã băm khác 
Hash rất mạnh và ứng dụng nhiều trong Blockchain trong đó ứng dụng nhiều vào Merkle Tree, chúng ta cùng tìm hiểu về nó


## Merkle Tree là gì ?
Merkle Tree (Cây Merkle) là cấu trúc dữ liệu (dạng cây nhị phân) mà mỗi nút lá là hash của một mảnh dữ liệu (trong blockchain là một giao dịch vì 1 block chứa nhiều giao dịch), còn mỗi nút cha là hash của 2 nút con gộp lại. Cứ làm vậy cho đến nút cuối cùng à Merkle Root, đại diện cho toàn bộ dữ liệu

Ví dụ với 4 giao dịch
```txt
                                Root = H(H1234 + H5678)         
                       /                                        \
          H1234 = H(H12 + H34)                      H5678 = H(H56 + H78)
            /              \                          /              \
   H12 = H(H1+H2)    H34 = H(H3+H4)         H56 = H(H5+H6)    H78 = H(H7+H8)
      /      \          /      \                /      \          /      \
H1=H(GD1) H2=H(GD2) H3=H(GD3) H4=H(GD4)   H5=H(GD5) H6=H(GD6) H7=H(GD7) H8=H(GD8)
``` 
Tại sao dùng cây Merkle
- Phát hiện thay đổi: sửa 1 giao dịch thì hash lá đổi kéo theo hash nút cha và hash nút gốc cũng thay đổi theo 
- Xác minh nhanh: chứng minh 1 giao dịch nằm trong block thì chỉ cần khoảng log₂(n) hash, vì là cây nhị phân. VD Block 1000 giao dịch chỉ cần khoảng 10 hash
- Tính gọn nhẹ: 1 Block chỉ cần chứa hash của Merkle Root thay vì toàn bộ danh sách giao dịch trong block


## Block là gì ?
Block (khối) hiểu đơn giản là 1 khối chứa dữ liệu có hạn như chứa 10 giao dịch hay 50 giao dịch hay thậm chí là n giao dịch trong 1 khoảng thời gian được hệ thống quy định
- Các thành phần trong Block 
    + Index block: số thứ tự block 
    + Data block: dữ liệu của block (giao dịch chẳng hạn)
    + Previous hash: mã băm của Block trước đó 
    + Merkle root: hash gốc của cây Merkle gom toàn bộ giao dịch trong block
    + Current hash: mã băm của block hiện tại
    + Timestampt: thời gian Block được tạo
    + Difficulty: độ khó của bài toán PoW
    + Nonce: số nonce (PoW)
Vì sao cần phải lưu hash của Block trước đó, tiếp theo chúng ta cùng tìm hiểu tiếp về Blockchain


## Blockchain là gì ?
Blockchain (chuỗi khối) là 1 chuỗi các khối nối liền với nhau bằng mã Hash (đã giải thích phía trên). Nghe giống như danh sách liên kết (Link list) đã được học, trong DSLK thì các Node trỏ đến Node tiếp theo, còn trong Blockchain thì Block sau lưu Hash của block trước và cứ thế nối tiếp nhau thành 1 chuỗi dài

Minh họa DSLK
```txt
head
  |
  v
+------+-------+     +------+-------+     +------+-------+     +------+-------+
| data | *next |---->| data | *next |---->| data | *next |---->| data | *next |----> NULL
+------+-------+     +------+-------+     +------+-------+     +------+-------+
   Node 1              Node 2              Node 3              Node 4
```

Minh họa Blockchain 
```txt
Genesis block
      |
      v
+-----------------+                  +-----------------+                  +-----------------+                  +-----------------+
| index: 0        |                  | index: 1        |                  | index: 2        |                  | index: 3        |
| timestamp       |                  | timestamp       |                  | timestamp       |                  | timestamp       |
| difficulty      |   previous hash  | difficulty      |   previous hash  | difficulty      |   previous hash  | previous hash   |
| previous hash   |<-----------------| previous hash   |<-----------------| previous hash   |<-----------------| previous hash   |
| merkleRoot      |                  | merkleRoot      |                  | merkleRoot      |                  | merkleRoot      |
| nonce           |                  | nonce           |                  | nonce           |                  | nonce           |
| data (GD)       |                  | data (GD)       |                  | data (GD)       |                  | data (GD)       |
| current hash    |                  | current hash    |                  | current hash    |                  | current hash    |
+-----------------+                  +-----------------+                  +-----------------+                  +-----------------+
     Block 0                              Block 1                              Block 2                              Block 3
```

Vì sao phải liên kết các Block với nhau bằng mã Hash của block trước đó ?
- Để bảo toàn tính toàn vẹn dữ liệu cho Block
    VD: Nếu có ai cố tình sửa nội dung giao dịch ở block 1 -> hash block 1 sẽ thay đổi làm cho tất cả hash phía sau thay đổi
      => Hash sau khi thay đổi không còn khớp với hash chính thức của các block
      => Phát hiện ra sửa đổi
- Để tạo độ khó cho người thay đổi 
Vậy độ khó ở đây là gì, chúng ta cùng tìm hiểu tiếp về Mining 


## Mining 

