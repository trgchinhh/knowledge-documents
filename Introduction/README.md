# Những kiến thức cần thiết trước khi nhập môn lập trình 

Lập trình không chỉ riêng việc chúng ta viết code mà là còn làm việc và tìm hiểu với rất nhiều thứ và công nghệ xung quanh
Việc trang bị trước những kiến thức này giúp bạn hiểu rõ hơn về môi trường máy tính, trình biên dịch, ... Phần nào giúp bạn hiểu rõ hơn với công việc sau này  


## Lập trình là gì ?
Lập trình là lập ra trình tự thường dùng trong ngữ cảnh có máy tính, ám chỉ việc viết những dòng lệnh sao cho máy tính hiểu và chạy được đoạn mã đó. Viết mã cũng cần phải có ứng dụng dành riêng cho việc đó, tiếp theo chúng ta cùng tìm hiểu về công cụ viết mã     


## Hello World 
In ra dòng Hello World trong những ngôn ngữ phổ biến hiện nay 
C++
```cpp
#include <iostream>
using namespace std;

int main(){
    cout << "Hello World" << endl;
    return 0;
}
```
C#
```cs
Console.WriteLine("Hello World");
```
Java
```java
public class Program {
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```

Python 
```py
print("Hello World")
```

JavaScript
```js
Console.log("Hello World")
```


## Công cụ viết mã
Việc viết mã cùng cần đến những công cụ (app riêng) sinh ra cho các coder/dev, đánh dấu và tô màu các cú pháp, từ khóa làm nổi bật lên giúp ta quan sát code dễ dàng hơn hoặc thậm chí khi có lỗi 
Chúng còn chia làm 2 nhánh riêng biệt 
- IDE (Integrated Development Environment): là môi trường phát triển tích hợp, tức 1 phần mềm cung cấp nhiều công cụ cần thiết để lập trình trong 1 nơi 
    + Visual Studio
    + Android Studio
    + CLion
    + IntelliJ IDEA
- Text Editor: là phần mềm chỉ viết code mà không có khả năng biên dịch, gợi ý, và chạy chương trình. Phải gọi đến trình biên dịch bên ngoài 
    + Sublime Text
    + Visual Studio Code
    + Vim/NeoVim
    + Notepad++
> Lưu ý: IDE bao gồm của Text Editor nhưng ngược lại thì không
Nếu dung IDE thì chỉ cần tải trình dịch trong đó và code ấn Run là chạy được (không cần cài thêm trình dịch bên ngoài)
Nhưng nếu dùng Text Editor thì phải cài riêng bộ trình dịch bên ngoài mới Run được 
Vậy trình dịch là gì chúng ta cùng tìm hiểu về nó


## Trình dịch là gì ?
Trình dịch là chương trình sẽ dịch toàn bộ code sang ngôn ngữ máy để máy tính hiểu và chạy được 
Trình dịch còn được chia thành 2 loại là `trình biên dịch (Compiler)` và `trình thông dịch (Interpreter)`
- Trình biên dịch:
    - Thường dùng cho các ngôn ngữ bật thấp (C/C++/Rust)
    - Là chương trình sẽ dịch từ code -> mã máy (hoặc bytecode)
    - Sau đó mới gọi đến chương trình sau khi biên dịch để chạy 
    - Ưu điểm: chương trình có hiệu năng cao hơn và gần với tầng máy 
    - Nhược điểm: biên dịch sẽ hơi lâu nếu chương trình lớn 
- Trình thông dịch:
    - Thường dùng cho các ngôn ngữ bậc cao (Python/JS) 
    - Là chương trình chạy trực tiếp code mà không cần biên dịch ra file mã máy
    - Ưu điểm: bắt đầu chạy sẽ rất nhanh vì không cần biên dịch   
    - Nhược điểm: hiệu năng sẽ không bằng ngôn ngữ cấp thấp 
- Đặc biệt
    - Đối với 1 vài ngôn ngữ đặc biệt như Java thì cần 2 trình dịch là biên và thông dịch 
    - Vì biên và thông dịch đều có ưu điểm khác nhau nêu kết hợp lại thì Java cũng cho ra hiệu năng khác 
Sau khi cài trình dịch việc tiếp theo là cài đường dẫn trong môi trường đến trình dịch để gọi được mọi nơi 
Vậy biến môi trường là gì chúng ta cùng tìm hiểu phần kế tiếp   


## Biến môi trường 
Biến môi trường (Enviroment Variable) là cách gọi dễ hiểu cho các đường dẫn đến những chương trình hay cần dùng đến như trình dịch. 
- VD minh họa không/có setup:
    - Trình dịch nằm ở đường dẫn: `C:/Program/g++/g++.exe`
    - Mỗi lần muốn dịch chương trình nào đó phải gọi: `C:/Program/g++/g++.exe main.cpp -o main.exe`
    - Trong khi nếu đã setup thì chỉ cần gọi: `g++ main.cpp -o main.exe`
- Các bước setup biến môi trường:
    - Ấn windows tìm env chọn edit enviroment variable
    - Sẽ có 2 ô 1 là User variable, 2 là System variable
        + Chọn Path ô 1 nếu bạn muốn chỉ User này set đường dẫn đến trình dịch 
        + Chọn Path ô 2 nếu bạn muốn set đường dẫn cho toàn hệ thống 
    - Sau khi mở ra bảng Path thì chọn New dán đường dẫn đến trình dịch vào và ấn OK
    - Mở terminal ra ở vị trí khác chổ chứa trình dịch gõ `g++ --version`
    - Nếu ra phiên bản thì coi như setup thành công 
=> Việc setup biến môi trường làm cho cách gọi chương trình hay dùng gọn hơn rất nhiều 
Tiếp theo chúng ta tìm hiểu về Terminal 


## Terminal là gì ? 
Terminal (thiết bị đầu cuối) là cửa sổ giao diện dùng để nhập lệnh bằng bàn phím và hiển thị kết quả dạng văn bản. Terminal chỉ lo phần nhập/xuất, không tự xử lý lệnh 
Lệnh được xử lý bởi **shell** chạy trong terminal
- Các **shell** phổ biến hiện nay:
    - Command Prompt, Powershell (Windows)
    - bash, zsh (Linux/macOS)
Khi nhắc đến Terminal hay Shell thì các bạn sẽ nghe nhiều về CLI vậy nó là gì và có bao nhiêu môi trường như thế ?
Có tổng 3 môi trường: GUI, CLI, TUI
- GUI (Graphic User Interface): môi trường giao diện đồ họa
    - Là các ứng dụng như Chrome hay Word/Excel cơ bản dùng hàng ngày  
- CLI (Command Line Interface): môi trường giao diện dòng lệnh 
    - Là môi trường như trong terminal chỉ có viết lệnh để cho Shell xử lý lệnh của bạn và thực thi, như g++, python
- TUI (Text-based User Interface): môi trường giao diện dạng văn bản
    - Là môi trường trong terminal nhưng dùng các ký tự văn bản vẽ thành các bố cục có cấu trúc menu, cửa sổ, nút, bảng, có thể điều khiển bằng chuột hoặc phím như một ứng dụng GUI, như neovim, btop, cava, lazygit, tuistore