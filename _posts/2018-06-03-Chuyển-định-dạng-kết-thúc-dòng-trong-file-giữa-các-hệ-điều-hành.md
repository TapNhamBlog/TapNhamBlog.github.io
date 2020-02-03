<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

Chắc hẳn các bạn cũng biết (hoặc nếu chưa biết thì bây giờ biết) thì mỗi hệ điều hành có một định dạng kết thúc dòng (end-line) riêng trong file, ví dụ với 3 nền tảng phổ biến nhất hiện nay:

* Windows: định dạng `CR-LF`
* Linux: định dạng `LF`
* Classic Mac: định dạng `CR`

<i>Chú thích: `CR` - Carrige Return, tượng trưng cho kí tự `\r`, `LF` - Line Feed, tượng trưng cho kí tự `\n`. Ở Windows, hệ điều hành sẽ nhận ra kết thúc dòng khi bắt gặp cặp kí tự `\r\n`; ở Linux thì khi bắt gặp `\n` và Mac là `\r`.</i>

Điều này có nghĩa, việc tạo một file `.txt` ở hệ điều hành này và đem sang hệ điều hành khác làm việc như thao tác với file trong lập trình có thể gây ra một số lỗi về định dạng. Chính vì thế mình đã viết một cái hàm nho nhỏ (ngôn ngữ C++) dùng để chuyển định dạng file sao cho phù hợp với 3 nền tảng phổ biến trên.

Phương pháp như sau:

1. đầu tiên ta sẽ đọc file cần chuyển định dạng với chế độ mở file nhị phân (binary mode). Tiếp đến ta dùng kiểu dữ liệu mảng động là `vector` và kết hợp với `iterator` trong C++ để lấy được nội dung của file vào chuỗi.
2. Ta sẽ lần lượt tạo 3 hàm chuyển đổi cho 3 hê điều hành kể trên, ví dụ như `convert_win32`, `convert_linux`, `convert_mac`. Thuật toán chuyển đổi rất đơn giản, các bạn mức nhập môn hoàn toàn có thể làm được:
    * Ở Windows: nếu đọc thấy kí tự `\r` mà kí tự sau nó không phải `\n` thì chèn thêm `\n` vào sau; nếu đọc thấy kí tự `\n` mà phía trước không phải là `\r` thì chèn thêm `\r` vào phía trước.
    * Ở Linux: nếu đọc thấy kí tự `\r` mà kí tự phía sau là `\n` thì xóa kí tự `\r` đi, nếu không phải `\n` thì thay thế `\r` bằng `\n`.
    * Ở Mac: nếu đọc thấy kí tự `\n` mà phía trước là `\r` thì xóa `\n` đi, nếu không phải `\r` thì thay thế `\n` bằng `\r`.

    Đấy, hết sức đơn giản luôn.
3. Ta sẽ nhận dạng hệ điều hành thông qua 3 predefined macros này: `_WIN32`, `__linux__` (hoặc `__linux`) và `__APPLE__`. Với Windows thì ta gọi hàm `convert_win32`, Linux là `convert_linux` và Mac là `convert_mac`.
4. Sau khi chỉnh xong thì ta xuất nội dung đã chỉnh sửa ra file gốc ban đầu.

Lí thuyết đủ rồi, giờ code thử nào.

Đầu tiên ta sẽ có hàm chuyển định dạng với tham số truyền vào là tên đường dẫn file:

```cpp
void convert_file_format(const string & path);
```

Đọc từ file với dạng nhị phân và đưa vào mảng động `vector` của thư viện STL thông qua `iterator`:

```cpp
fstream fileInput(path, ios::binary | ios::in);
if (fileInput.fail())
    return;

vector<char> buffer((istreambuf_iterator<char>(fileInput)), (istreambuf_iterator<char>()));
```

Bây giờ đến bước cài đặt 3 hàm chuyển đổi:

Windows:

```cpp
void format_win32(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\r':
            if (data[i + 1] != '\n') data.insert(data.begin() + i + 1, '\n');
            break;
        case '\n':
            if (data[i - 1] != '\r') data.insert(data.begin() + i - 1, '\r');
        }
}
```

Linux:

```cpp
void format_linux(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\r':
            if (data[i + 1] == '\n') data.erase(data.begin() + i, data.begin() + i + 1);
            else
            {
                data.erase(data.begin() + i, data.begin() + i + 1);
                data.insert(data.begin() + i, '\n');
            }
            break;
        }
}
```

Và Mac:

```cpp
void format_mac(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\n':
            if (data[i - 1] == '\r') data.erase(data.begin() + i, data.begin() + i + 1);
            else
            {
                data.erase(data.begin() + i, data.begin() + i + 1);
                data.insert(data.begin() + i, '\r');
            }
            break;
        }
}
```

Tiếp theo là dùng 3 predefined macros để nhận dạng hệ điều hành, gọi hàm ứng với hệ điều hành và đóng file, ta sẽ đặt đoạn code sau trong hàm `conver_file_format` phía trên:

```cpp
#ifdef _WIN32
    format_win32(buffer);
#elif __linux__
    format_linux(buffer);
#else
    format_mac(buffer);
#endif
    fi.close();
```

Nếu bạn đang dùng Windows thì macro được defined sẵn là `_WIN32`, nếu xài Linux thì `__linux__`.

Cuối cùng là xuất chuỗi đã chỉnh sửa ra file ban đầu:

```cpp
ofstream fo(path);
for (size_t i = 0; i < buffer.size(); ++i)
    fo << buffer[i];
fo.close();
```

Cách dùng: trước khi đọc file nào (đọc để lưu dữ liệu) thì các bạn chỉ cần gọi hàm `convert_file_format(<tên đường dẫn đến file>);` là được, quá đơn giản.

Tóm lại: kĩ thuật cài đặt không có gì quá phức tạp, chỉ cần các bạn đã học về xử lí chuỗi, đọc file, cấp phát động thì hoàn toàn có thể làm được.

```cpp
#include <iostream>
#include <string>
#include <fstream>
#include <vector>
#include <iterator>
using namespace std;

void format_win32(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\r':
            if (data[i + 1] != '\n') data.insert(data.begin() + i + 1, '\n');
            break;
        case '\n':
            if (data[i - 1] != '\r') data.insert(data.begin() + i - 1, '\r');
        }
}

void format_linux(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\r':
            if (data[i + 1] == '\n') data.erase(data.begin() + i, data.begin() + i + 1);
            else
            {
                data.erase(data.begin() + i, data.begin() + i + 1);
                data.insert(data.begin() + i, '\n');
            }
            break;
        }
}

void format_mac(vector<char> &data)
{
    for (size_t i = 0; i < data.size(); ++i)
        switch (data[i])
        {
        case '\n':
            if (data[i - 1] == '\r') data.erase(data.begin() + i, data.begin() + i + 1);
            else
            {
                data.erase(data.begin() + i, data.begin() + i + 1);
                data.insert(data.begin() + i, '\r');
            }
            break;
        }
}

void convert_file_format(const string & path) {
    fstream fileInput(path, ios::binary | ios::in);
    if (fileInput.fail())
        return;

    vector<char> buffer((istreambuf_iterator<char>(fileInput)), (istreambuf_iterator<char>()));

#ifdef _WIN32
    format_win32(buffer);
#elif __linux__
    format_linux(buffer);
#else
    format_mac(buffer);
#endif

    fileInput.close();
}

int main() {
    cout << "Enter file's path: ";
    string path;
    getline(cin, path);
    convert_file_format(path);
    return EXIT_SUCCESS;
}
```

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/Chuy%E1%BB%83n-%C4%91%E1%BB%8Bnh-d%E1%BA%A1ng-k%E1%BA%BFt-th%C3%BAc-d%C3%B2ng-trong-file-gi%E1%BB%AFa-c%C3%A1c-h%E1%BB%87-%C4%91i%E1%BB%81u-h%C3%A0nh.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/Chuy%E1%BB%83n-%C4%91%E1%BB%8Bnh-d%E1%BA%A1ng-k%E1%BA%BFt-th%C3%BAc-d%C3%B2ng-trong-file-gi%E1%BB%AFa-c%C3%A1c-h%E1%BB%87-%C4%91i%E1%BB%81u-h%C3%A0nh.html" data-numposts="5"></div>
