<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

<i>Lưu ý: Bài viết này dành cho những bạn đã có kiến thức về C/C++ và bắt đầu học con trỏ hay gặp khó khăn trong quá trình học.</i>

Link phần 2: [Con trỏ với mảng và cấu trúc](https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-2-).html)

Vào thời gian đầu khi mới học con trỏ, chắc hẳn các bạn cảm thấy bỡ ngỡ và gặp khá nhiều rắc rối như: “cái quái gì thế này??”, hay là “tôi là ai, đây là đâu, sao tôi lại ở đây????” và nhiều tình huống hài hước khác nữa. Một phần là do bản chất con trỏ là một dạng kiểu dữ liệu trừu tượng nên khiến cho những người mới học khó hình dung, và một phần nữa là do các tài liệu hướng dẫn không đề cập đến ứng dụng của con trỏ nên ít người hứng thú khi học. Ở series bài viết này mình sẽ hướng dẫn cho các bạn cách sử dụng con trỏ cơ bản mà những người học C/C++ đều phải biết.

# Nguồn gốc con trỏ trong C/C++

Trước tiên mình sẽ nói sơ qua chút về ngôn ngữ Assembly (Hợp Ngữ) – ngôn ngữ lập trình nhanh nhất thế giới (không tính mã máy vì mã máy không được xem như ngôn ngữ lập trình). Một trong những điểm khiến cho Assembly rất mạnh đó là khả năng cho phép lập trình viên tùy chỉnh mọi thứ của nó: như khi ta tạo ra một biến, ta có thể quyết định được biến đó nằm ở vùng nhớ nào (sau này mình sẽ nói thêm về vùng nhớ), có thể tăng hoặc giảm kích thước của vùng nhớ đã cấp cho biến đó tùy thích,… C/C++ đã kế thừa một số đặc điểm này và tổng hợp nó thành con trỏ.

> Con trỏ:  thứ mạnh nhất và nguy hiểm nhất của C/C++, nhắc đến C/C++ là nhắc đến (sự nguy hiểm) con trỏ.

Nghe đến đây chắc nhiều bạn thấy sợ rồi phải không? Nhưng không sao, nếu các bạn cẩn thận thì con trỏ sẽ là công cụ rất hữu dụng cho bạn mà những ngôn ngữ sau này không thể có được.

# Khái niệm về con trỏ

Ta sẽ nhắc lại chút về cái gọi là biến – variable. Biến, theo một cách giải thích ngắn gọn nhất cho người mới bắt đầu thì nó như là một nơi để lưu trữ dữ liệu của ta và dữ liệu đó sẽ được lưu trữ trên RAM.

## Bộ nhớ máy tính (RAM) và biến (Variable)

Mình sẽ giải thích một cách dễ hiểu nhất để các bạn hình dung ra bộ nhớ máy tính như thế nào. Ta giả sử có một khu đất, khu đất đó gồm nhiều lô đất nhỏ, mỗi lô có kích thước ngang nhau (ví dụ 1m vuông), những lô đất đó được đánh số thứ tự để kí hiệu (như 1, 2, 3,…) và có thể chứa nhiều thứ khác nhau như nhà, chuồng trại hay ao hồ,… Bộ nhớ máy tính cũng giống như khu đất đó, gồm nhiều ô nhớ (lô đất) với kích thước mỗi ô nhớ là 1 byte (như mỗi lô đất là 1m vuông), các ô nhớ đó được đánh số (hay địa chỉ bắt đầu từ 0) và dữ liệu của ta sẽ được lưu trữ trong những ô nhớ đó (như là nhà cửa vậy).

Vậy với một biến `int` (tương đương 4 bytes) thì sẽ nằm trên 4 ô nhớ liên tục nhau, biến `char` (tương đương 1 byte) thì sẽ nằm trên 1 ô nhớ, và tất cả những biến đó đều có địa chỉ là địa chỉ của vùng nhớ đã chứa nó.

Ta có thể xem địa chỉ của một biến bất kì bằng cách thêm dấu `&` trước biến đó, ví dụ:

```cpp
int integer = 10;
cout << &integer << endl;
```

Vậy con trỏ là gì? Thực chất con trỏ cũng chỉ là một biến giúp ta lưu trữ được địa chỉ của một vùng nhớ bất kỳ.

# Khai báo con trỏ trong C/C++

Như mọi biến khác, biến con trỏ cũng có kiểu dữ liệu và cần được khai báo:

```cpp
<data_type> *<pointer_variable_name>; //chú ý dấu *
```

Ví dụ:

```cpp
int *pInteger;
double *pDouble;
char *pCh1, *pCh2;
```

Giải thích: tất cả các biến `pInteger`, `pDouble`, `pCh1`, `pCh2` đều là biến con trỏ, trong đó:

* `pInteger` trỏ tới vùng nhớ chứa biến kiểu `int` (4 byte).
* `pDouble` trỏ tới vùng nhớ chứa biến kiểu `double` (8 byte).
* `pCh1`, `pCh2` trỏ tới vùng nhớ chứa biến kiểu `char` (1 byte).

Nếu các bạn thấy khó hiểu khi khai báo như vậy thì có thể sử dụng từ khóa `typedef` để khai báo như sau:

```cpp
typedef <data_type> *<pointer_type_name>;
<pointer_type_name> <pointer_varialbe_name>;
```

Ví dụ:

```cpp
typedef int *ptrInt;
int *pInteger1;
ptrInt pInteger2;
```

Cả 2 biến `pInteger1` và `pInteger2` đều là biến con trỏ và trỏ tới vùng nhớ chứa biến kiểu `int`.

> Thông thường theo một số chuẩn thì việc đặt tên biến con trỏ sẽ thường có chữ p (pointer) phía trước nhằm phân biệt với biến thông thường.

# Liên kết địa chỉ vào biến con trỏ

Khi mới khởi tạo, biến con trỏ mang một giá trị rác, nó chứa một giá trị nào đó và trỏ tới một vùng nhớ không xác định được, ta sẽ liên kết địa chỉ của biến vào biến con trỏ bằng cách sử dụng toán tử `&` như sau:

```cpp
<pointer_variable> = &<normal_variable>;
```

Hành động trên được gọi là <b>reference</b> - tham chiếu đến vùng nhớ. Ví dụ:

```cpp
int num_a = 10, num_b = 5;
int *pNum_a = &num_a, *pNum_b;
pNum_b = &num_b;
```

Tiếp theo, để truy xuất đến ô nhớ mà con trỏ đã trỏ đến, ta sử dụng toán tử `*` như sau:

```cpp
int num_a = 10, *pNum_a = &num_a;
cout << num_a << endl; //giá trị biến num_a: 10
cout << pNum_a << endl; //địa chỉ của biến num_a mà pNum_a đang giữ
cout << *pNum_a << endl; //giá trị tại vùng nhớ mà pNum_a trỏ đến: biến num_a = 10
```

Hành động trên được gọi là de-reference.

Ngoài ra, biến con trỏ cũng là một biến nên nó cũng có địa chỉ của nó:

```cpp
cout << &pNum_a << endl; //địa chỉ của biến con trỏ pNum_a;
```

# Con trỏ `NULL`

Con trỏ `NULL` là con trỏ không trỏ vào đâu cả. Ở đây ta cần lưu ý: con trỏ `NULL` khác với con trỏ chưa được khởi tạo (là con trỏ mà nó trỏ đến vùng nhớ không xác định hay thường gọi là vùng nhớ rác). Nếu các bạn còn nhớ thì `NULL` có khai báo là `#define NULL 0` trong C/C++, tức là `NULL` = 0.

```cpp
int *pNum; //con trỏ chưa được khởi tạo
int *pNull = NULL; //con trỏ NULL
int *pNull2 = 0; //cách khai báo con trỏ NULL thứ 2
int *pNull3 (0); //cách khai báo con trỏ NULL thứ 3
```

# Kích thước con trỏ

Con trỏ chỉ có nhiệm vụ là lưu địa chỉ của biến nên kích thước của mọi con trỏ là như nhau trên cùng một nền tảng:

* MS-DOS thì con trỏ có kích thước 2 bytes (giờ chắc chả ai xài cái này đâu…).
* Nền tảng 32-bit thì con trỏ có kích thước 4 bytes.
* Nền tảng 64-bit thì con trỏ có kích thước 8 bytes.

Ta có thể kiểm chứng bằng đoạn code sau:

```cpp
cout << sizeof(int*) << endl;
cout << sizeof(char*) << endl;
cout << sizeof(double*) << endl;
```

Dù cho kích thước 3 kiểu dữ liệu `int`, `char`, `double` là khác nhau nhưng con trỏ của chúng đều có kích thước giống nhau. Nếu ta cho chạy trên IDE Visual Studio (x86) thì sẽ ra 4 (bytes), còn ở x64 thì sẽ ra 8 (bytes).

Giải thích: địa chỉ mà con trỏ giữ thực chất chính là địa chỉ nằm trên RAM. Với hệ điều hành 32 bit thì dung lượng RAM tối đa nhận được sẽ là 4GB. Xét mỗi ô nhớ sẽ có giá trị là 1 byte (tương đương 8 bit), 4GB sẽ tương đương là 4,294,967,296 byte là 4,294,967,296 ô nhớ, và các ô nhớ sẽ được đánh số từ 0 - 4,294,965,295. Và các bạn đoán xem, với con trỏ kích thước 32 bit, tức là 4 byte, có thể chứa được các số trong khoảng 0 - 4,294,967,295, tức là có thể trỏ tới 4,294,967,296 ô nhớ, cũng chính là số lượng ô nhớ tương ứng ở hệ điều hành 32 bit.

# Truyền địa chỉ - tham biến

Ta đã biết qua 2 cách truyền tham số cho hàm đó là truyền tham chiếu và truyền tham trị:

```cpp
void foo(int &num); //tham chiếu
void bar(int num); //tham trị
```

Bây giờ ta có thêm một dạng truyền tham số nữa, đó là truyền tham biến hay truyền địa chỉ.

```cpp
void foo(int *num);
```

Ta hãy ví dụ minh họa bằng hàm hoán vị hai số nguyên sử dụng truyền tham chiếu và truyền tham biến:

Truyền tham chiếu:

```cpp
#include <iostream>
using namespace std;

void Swap(int &num_a, int &num_b) {
    int temp = num_a;
    num_a = num_b;
    num_b = temp;
}

int main() {
    int a = 10, b = 5;
    Swap(a, b);
    cout << "a = " << a << "; b = " << b << endl;
    return 0;
}
```

truyền tham biến:

```cpp
#include <iostream>
using namespace std;

void Swap(int *num_a, int *num_b)
{
    int temp = *num_a;
    *num_a = *num_b;
    *num_b = temp;
}

int main()
{
    int a = 10, b = 5;
    Swap(&a, &b);
    cout << "a = " << a << "; b = " << b << endl;
    return 0;
}
```

Giải thích: hai tham số trong hàm `Swap` là `int *num_a` và `int *num_b` thực chất là 2 con trỏ, và nó sẽ giữ địa chỉ của 2 biến giá trị được truyền vào khi ta gọi `Swap(&a, &b)` trong hàm `main`. Và như đã đề cập phía trên, để lấy giá trị tại ô nhớ mà con trỏ đang trỏ tới thì ta sẽ sử dụng toán tử `*` như `*num_a` để lấy giá trị tại ô nhớ mà `num_a` đang trỏ tới.

> Trên thực tế thì cách truyền tham số này phổ biến chủ yếu ở C thuần do C không có truyền tham chiếu mà chỉ C++ mới có truyền tham chiếu.

# Một số lưu ý:

* Con trỏ là khái niệm <b>cơ bản</b> quan trọng nhất trong C++.
* Nắm rõ các quy tắc sau: ví dụ `int a, *pa = &a`;
    * `*pa` và `a` đều chỉ nội dung của biến a.
    * `pa` và `&a` đều chỉ địa chỉ biến a.
* Hiểu được reference và de-reference khác nhau thế nào.
* Biết được thêm kiểu truyền tham số mới vào hàm.
* Không nên sử dụng con trỏ khi chưa được khởi tạo, kết quả không lường trước được do giá trị trong đó là giá trị rác.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-1-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-1-).html" data-numposts="5"></div>