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
Mining (đào) là thuật ngữ dùng nhiều trong Blockchain và Crypto (tiền điện tử), có thể nhiều bạn đã nghe nhiều về "Đào Coin" nhưng chưa hiểu là làm gì
khi 1 Block mới đã có data nó chưa được thêm vào Chain ngay mà phải được tính toán theo 1 chuẩn bài toán nào đó thường được gọi là PoW (Proof of Work) tạm dịch là bằng chứng công việc
Bài toán có thể là mã Hash của Block khi tính ra thì phải cộng với 1 số ngẫu nhiên nào đó sao cho đầu mã Hash có 5 số 0 (đây là độ khó của bài toán) được gọi là Difficulty
Làm sao để biết được số ngẫu nhiên là số nào ? Không còn cách nào khác ngoài thử từng số 1 bắt đầu từ 0, đó cũng chính là số Nonce trong Block 
Số Nonce trong thật tế để tìm được có thể lên đến hàng tỷ, vì thế cần có dàn máy tính khủng nhiều GPU hay máy đào chuyên dụng
Quá trình đó được gọi là đào 

Trở lại ví dụ hồi nảy 
- Khi 1 người cố ý thay đổi 1 giá trị giao dịch trong 1 Block không chỉ phải Hash lại Block đó mà còn phải Hash tất cả các Block phía sau theo chuẩn PoW của hệ thống đưa ra 
- Điều này làm cho ông Hacker đó tiêu tốn nhiều thời gian và tiền bạc 
=> Rất khó để thay đổi được 

Giả sử ông Hacker đó có 1 dàn máy tính siêu khủng có thể Hash lại được nguyên 1 Chain đằng sau đó thì có mất an toàn ?
- Câu trả lời là `KHÔNG`
- Blockchain là công nghệ phân tán trên mạng ngang hàng, không phải mô hình Client-Server
Vậy mạng ngang hàng là gì, chúng ta cùng tìm hiểu tiếp về Mạng ngang hàng (P2P)


## Mạng ngang hàng (P2P)
Mạng ngang hàng (P2P) là cách các máy tính (máy đào) tham gia vào mạng lưới Blockchain dưới dạng ngang hàng 
Hiểu đơn giản không phải như mô hình Client - Server (máy khách - máy chủ)
Mà tất cả các máy tính tham gia đều ngang hàng như nhau (xem là Node)
Các Node nối với nhau bằng mạng và truyền dữ liệu cho nhau 
Tất cả các Node đều giữ 1 bản sao của Blockchain 
Nếu có 1 Node ngắt kết nối (chết) thì mạng vẫn chạy nhờ cơ chế duy trì kết nối 

```txt
                      +-----------+ 
                      |   Node 1  | 
                      +-----------+ 
                     /             \ 
                    /               \ 
       +-----------+                 +-----------+ 
       |   Node 2  |-----------------|   Node 3  | 
       +-----------+ \             / +-----------+ 
          |           \           /           | 
          |            \         /            | 
          |             \       /             | 
          |              \     /              | 
          |               \   /               | 
          |                \ /                | 
          |                 X                 | 
          |                / \                | 
          |               /   \               | 
          |              /     \              | 
          |             /       \             | 
          |            /         \            | 
          |           /           \           | 
       +-----------+ /             \ +-----------+ 
       |   Node 6  |-----------------|   Node 4  | 
       +-----------+                 +-----------+ 
                  \                 / 
                   \               / 
                     +-----------+ 
                     |   Node 5  | 
                     +-----------+ 
```

Trở lại ví dụ ông Hacker
- Nếu ông ấy có thay đổi được tất cả các mã Hash phía sau (đã bao gồm PoW) của Blockchain của 1 Node 
- Cũng không sao vì khi ấy Node đó sẽ hỏi tất cả các Node khác trong Blockchain có giống kết quả (Hash)
- Phải nhận được trên `51%` tổng số máy tính tham gia vào mạng lưới thì mới được chấp nhận kết quả 
=> Không phải cứ tính lại Hash của 1 Node mà phải trên 51% mới tổng số Node mới được chấp thuận, vì vậy việc cố gắng sửa dữ liệu trong Blockchain gần như không thể  


## So sánh Blockchain với Database 
Database (cơ sở dữ liệu như SQL server, MySQL, MongoDB, ...) lưu trữ dữ liệu có thể sửa chữa được và can thiệp bởi bên thứ 3 
- VD: Bạn chơi và nạp tiền vào game, số dư trong database sẽ được update
Còn Blockchain thì ngược lại, nhờ tính minh bạch và bất biến nên khi dữ liệu đã được đưa vào Block thì không thể nào sửa được


## Ưu và nhược điểm 
Ưu điểm: 
    - Phi tập trung: không phụ thuộc một máy chủ hay bên trung gian 
    - Bất biến: dữ liệu đã ghi vào Block gần như không thể sửa
    - Minh bạch: mọi Node đều có thể xác minh giao dịch
    - Chống giả mạo: Cơ chế đồng thuận (PoW) và chữ ký số (RSA) 
Nhược điểm: 
    - Hiệu năng thấp 
    - Tốn tài nguyên: PoW tiêu thụ nhiều điện năng cho máy đào và sức tính toán 
    - Dung lượng cao: mỗi Node phải lưu toàn bộ lịch sử 
    - Dễ mất tài sản: mất private-key coi như mất tài sản vĩnh viễn 
    - Phức tạp: triển khai và bảo trì khó hơn database truyền thống 


## Kết luận
Blockchain là một công nghệ đổi mới trong lưu trữ dữ liệu, nổi bật ở tính phi tập trung, minh bạch và bất biến, và được ứng dụng rộng rãi trong tiền mã hóa như BTC, ETH. Tuy nhiên, đây chưa phải cuộc cách mạng làm thay đổi hoàn toàn cách bảo mật dữ liệu hiện nay, vì còn nhiều hạn chế: chi phí cao, tiêu thụ năng lượng lớn (với PoW), tốc độ xử lý thấp, khó mở rộng và khung pháp lý chưa hoàn thiện. Những điều này khiến blockchain chưa dễ tiếp cận với các doanh nghiệp hiện đại, vốn thường ưu tiên hiệu năng và sự đơn giản. Vì vậy, blockchain phù hợp nhất với các bài toán nhiều bên không tin tưởng nhau cần chia sẻ dữ liệu chung, còn với các hệ thống thông thường, cơ sở dữ liệu truyền thống vẫn là lựa chọn tối ưu