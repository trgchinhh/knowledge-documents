# Git

## Git là gì ?
Git là 1 công cụ quản lý phiên bản phân tán được Linus Tovard cha đẻ của Linux Kernel tạo ra

Minh họa nhánh Git
```txt
                      Test Feature 1
               (Y)----(Y)----(Y)----(Y)---                                                [HEAD]
              /                           \                                                 |
             /                             \                                                v
    (M)-----+-----(M)-----(M)-+---(M)-------(M)-----(M)-----(M)-----(M)------+-----(M)-----(M)
    Main            \          \                           /                /
                     \          \                         /                /
                      \          (S)----(S)----(S)----(S)-                /
                       \              Test Feature 2                     /
                        \                                               /
                         (T)--------(T)--------(T)--------(T)--------(T)
                                        Test Feature 3
```

## Git được dùng để làm gì ?
Git được dùng chính thức trong quản lý phiên bản mã nguồn trên máy của bạn, mỗi thay đổi ở hiện tại khi lưu lại thì git như 1 máy ảnh chúng chụp lại những thay đổi đó, nó giúp bạn xem lại được những thay đổi và quay lại phiên bản mong muốn mà không phải lưu code ở nhiều chổ. Ngoài ra git có thể tạo nhiều nhánh để thực hiện trong nhiều việc khác như tạo 1 tính năng khác mà không muốn viết trên nhánh chính ta có thể tạo thêm 1 nhánh test lúc này ta có 2 luồng làm việc song song mà không sợ code xung đột với nhau. 


## Cách dùng Git 
Tải git ở trang chủ rồi đặt set path trong biến môi trường 
Nếu dùng các công cụ quản lý gói trên terminal như Winget, Scoop hay Choco thì dùng lệnh như sau
- Nếu dùng Winget: `winget install git`
- Nếu dùng Scoop: `scoop install git`
- Nếu dùng Choco: `choco install git -y`
> Riêng với cách cài bằng công cụ quản lý gói thì nó sẽ set path trong biến môi trường mà không cần cài thủ công

Mô phỏng quy trình hoạt động
```txt
 Working Dir        Staging Area         Local Repo         Remote (GitHub)
      |                  |                   |                    |
      |------ add ------>|                   |                    |
      |                  |----- commit ----->|                    |
      |                  |                   |------- push ------>|
      |                  |                   |<----- fetch -------|
      |<---------- merge / checkout ---------|                    |
      |<-------------------- pull (fetch + merge) ----------------|
      |<-------------------- clone (tải toàn bộ) -----------------|
```

### Các lệnh cơ bản 
- Lệnh `git init`: tạo kho lưu trữ phiên bản tại dự án
- File `.gitignore`: bỏ vào những file hay thư mục không muốn git theo dõi, thường là những file hay folder chứa nội dung nhạy cảm như API hay account, password, và các file .env
- Lệnh `git status`: để xem trạng thái thay đổi, những fie hay thư mục màu đỏ là những file mới được thêm hay mới thay đổi và chưa được theo dõi
- Lệnh `git add <tên file>`: dùng theo dõi những file chưa được theo dõi, nếu muốn theo dõi nhanh tất cả file thì dùng lệnh `git add .`
- Lệnh `git commit -m <nội dung>`: dùng lưu lại thay đổi (chụp ảnh) phiên bản hiện tại 
> Sau khi commit thì mọi thay đổi đều được git ghi lại và so sánh khác nhau (diff) với bản đã commit vừa rồi

### Các lệnh nâng cao 

#### Branch
Là lệnh làm việc trên các nhánh, Git có thể tạo nhiều nhánh trên 1 dự án để có thể code những tính năng thử nghiệm mà không lo làm hư hay xung đột với code chính 
- `git branch`: dùng liệt kê nhánh, và nhánh hiện tại đang làm việc
- `git branch <tên branch mới>`: dùng để tạo nhánh khác 
- `git checkout -b <tên nhánh mới>`: dùng để tạo và chuyển sang nhánh mới
- `git switch -c <tên nhánh mới>`: như trên (cú pháp mới)
- `git checkout <tên nhánh>`: chuyển nhánh (cú pháp cũ)
- `git switch <tên nhánh>`: chuyển nhánh (cú pháp mới)
- `git branch -M <tên nhánh mới>`: dùng để đổi tên nhánh mặc định từ master sang main

#### Clone 
Là lệnh tải repo từ trên GitHub về máy của bạn 
- `git clone <url>`: tải repo từ GitHub về máy (local) của bạn
- `git clone <url> <tên thư mục>`: tải repo từ GitHub vào thư mục đó

#### Pull
Là lệnh dùng để lấy những thay đổi mới nhất từ repo trên GitHub về máy, muốn pull được phải có repo sẵn trên máy 
-  `git pull`: tải những thay đổi mới nhất về máy 

#### Merge
Là lệnh hợp nhất 1 code 2 nhánh với nhau, thường là merge từ nhánh làm tính năng mới như `*test` về nhánh `*main`
Trước khi merge phải chuyển nhánh sang nhánh muốn muốn nhận code 
- `git switch <master/main>`
- `git merge <nhánh muốn merge>`: chọn nhánh có tính năng muốn merge vào nhánh hiện tại
- `git merge --abort`: hủy merge đang dở (thường dùng khi có xung đột)
- `git merge --continue`: tiếp tục merge sau khi giải quyết xung đột 

#### Push 
Là lệnh dùng để đẩy code từ máy bạn (local) lên repo trên GitHub
Thường có 2 cách để push lên repo GitHub
1. Chưa tạo repo trên GitHub
    - Nếu chưa có tài khoản thì đăng ký và đăng nhập trên GitHub
    - Tạo repo trên GitHub và đặt tên repo 
    - Sau khi ấn add sẽ có bảng hướng dẫn để nguyên vậy
    - Dự án trong máy (local) thì `git add .` và `git commit -m <miêu tả thay đổi>` như hướng dẫn phía trên
    - Đổi tên nhánh từ master thành main bằng lệnh: `git branch -M main`
    - Gắn remote url để git biết nên push lên repo nào bằng lệnh: `git remote add origin <url>` hoặc copy từ huống dẫn GitHub cho nhanh 
    - Đẩy dự án lên GitHub bằng lệnh: `git push -u origin main`
    > Sau khi push lần đầu, những lần sau chỉ cần `git add .` -> `git commit -m <miêu tả thay đổi>` -> `git push` là được    
2. Đã tạo repo trên GitHub
    Đối với repo đã có trên GitHub thì thường là trường hợp clone về máy nên repo đã có remote origin url 
    - Dự án trong máy (local) thì `git add .` và `git commit -m <miêu tả thay đổi>` như hướng dẫn phía trên 
    - Kiểm tra url push bằng lệnh: `git remote -v`
    - Nếu không đúng hoặc url GitHub có thay đổi, đổi url lại bằng lệnh: `git remote set-url origin <url mới>`
    - Đẩy dự án lên GitHub bằng lệnh: `git push`


## Phân biệt Git và GitHub
Git là công cụ trên máy cá nhân (local) dùng để quản lý phiên bản mã nguồn
GitHub là 1 mạng xã hội như Facebook cho lập trình viên, ở đó có thể tạo repo lưu trữ mã nguồn để mọi người cùng nhau đóng góp học hỏi và cộng tác lẫn nhau, hoặc dùng để học tập, làm việc nhóm với nhau bằng GitHub thông qua công cụ Git
