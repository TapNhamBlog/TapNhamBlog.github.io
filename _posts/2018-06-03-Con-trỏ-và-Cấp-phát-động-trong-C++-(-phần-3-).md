<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

Ở [phần trước](https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-2-).html) mình đã giới thiệu qua cách sử dụng toán tử đối với con trỏ và thao tác trên mảng bằng con trỏ. Trong phần này mình sẽ giới thiệu qua về kĩ thuật cấp phát động ở mức cơ bản nhất.

# CẤP PHÁT TĨNH - STATIC MEMORY ALLOCATION

Static memory allocation (compile-time allocation) được áp dụng cho các biến static hoặc toàn cục; tức là khi ta khai báo một biến toàn cục thì ta đã áp dụng kĩ thuật này. Một số đặc điểm cơ bản của kĩ thuật này:

* Ta không thể tùy ý thay đổi kích thước vùng nhớ của biến trong quá trình chạy chương trình (tĩnh).
* Vùng nhớ của các biến này được cấp phát ngay khi chạy chương trình.

# CẤP PHÁT TỰ ĐỘNG - AUTOMATIC MEMORY ALLOCATION

Kĩ thuật này được áp dụng đối với các biến cục bộ hoặc các tham số (argument) của hàm, gồm một số đặc điểm sau:

* Vùng nhớ của các biến này được cấp phát khi chương trình chạy vào khối lệnh chứa biến đó.
* Sau khi ra khỏi khối lệnh chứa biến đó thì vùng nhớ được tự động thu hồi.
* Tương tự như cấp phát tĩnh, ta không thể tùy ý thay đổi kích thước trong quá trình chạy.

⇒ Nhược điểm của 2 kiểu cấp phát trên:

* Kích thước vùng nhớ khi được cấp phát tại thời điểm biên dịch phải cố định (là hằng số), ví dụ nếu mình muốn khai báo một mảng số nguyên như sau:

```cpp
int arr[100];
```

Vậy nếu mình nhập vào quá 100 phần tử thì sẽ bị lỗi vì mảng không thể chứa quá 100 phần tử; còn nếu mình nhập chỉ 1 phần tử thì chả phải thừa 99 phần còn lại không dược dùng.

* Với 2 cách thức cấp phát trên thì việc cấp phát và thu hồi vùng nhớ hoàn toàn do chương trình quyết định và ta không thể can thiệp vào như thay đổi mảng 100 phần tử trên thành 200 hay 300,… Hơn nữa, như các bạn đều biết thì biến toàn cục tồn tại mọi nơi trong chương trình từ lúc khai báo đến khi chương trình kết thúc, tức là vùng nhớ cho biến toàn cục có thời gian tồn tại lâu nên khi sử dụng nhiều biến toàn cục sẽ gây lãng phí bộ nhớ rất nhiều.
* Kích thước cho vùng nhớ rất nhỏ…

# THÁP VÙNG NHỚ

![Level of memory](https://tapnhamblog.github.io/_img/Dynamic_memory_level_of_memory.png)

Công dụng lần lượt của các phân vùng:

* Text: hay còn gọi là text segment hoặc code segment, là nơi lưu trữ các mã lệnh đã được biên dịch của chương trình. Những mã lệnh này sẽ được chuyển tới CPU để xử lí khi cần thiết. Code segment chịu sự chi phối 100% của hệ điều hành, khi ta chạy chương trình thì hệ điều hành sẽ đưa các mã lệnh đã được biên dịch lên code segment đầu tiên.
* Data (initialized data segment): hay GVAR (Global VARiable) là phân vùng mà hệ điều hành sử dụng để khởi tạo giá trị cho các kiểu biến toàn cục (global variable), biến toàn cục chỉ có giá trị nội tại trong file chứa nó (static).
* .BSS (uninitialized data segment): cũng được dùng để chứa các biến toàn cục global và static nhưng các biến này chưa được khởi tạo giá trị cụ thể. Ví dụ int a; //giá trị rác.
* Heap segment (free store segment): được sử dụng để cấp phát bộ nhớ thông qua kĩ thuật Dynamic Memory Allocation. Người dùng có thể hoàn toàn kiểm soát bộ nhớ Heap trong quá trình làm việc, và đặc biệt là dung lượng bộ nhớ nó rất lớn, có thể lên đến hàng GB.
* Stack segment (thường gọi tắt là stack): là bộ nhớ được cấp phát cho các tham số của hàm, và các biến cục bộ (local variable). Bộ nhớ này được thực hiện theo quy tắc ngăn xếp (stack – Last In First Out), bộ nhớ này không hoàn toàn thuộc sự kiểm soát của người dùng và dung lượng của nó cũng rất ít, đó hạn chế của việc cấp phát tĩnh.
* Vùng nhớ Unused: vùng nhớ này nằm giữa 2 vùng nhớ stack và heap. Khi bộ nhớ stack hoặc heap đã hết, nếu có chỉ thị cấp phát từ người dùng thì bộ nhớ stack hoặc heap sẽ mở rộng vào vùng nhớ này; vùng nhớ này thực chất là ảo, nó chỉ là ảnh giống như là bộ nhớ ảo vậy.

# CẤP PHÁT ĐỘNG - DYNAMIC MEMORY ALLOCATION

Kĩ thuật cấp phát động dùng để cấp phát một vùng nhớ ở HEAP tại thời điểm run-time hay còn gọi là thời điểm chương trình đang thực thi. Cần phải lưu ý rằng kĩ thuật này dùng để cấp phát 1 vùng nhớ mới chứ không phải tạo ra 1 tên biến mới, và chúng ta kiểm soát vùng nhớ này thông qua biến con trỏ trỏ tới địa chỉ đầu tiên của vùng nhớ.

## Cấp phát vùng nhớ là biến đơn lẻ (dynamically allocate single variable)

Cú pháp cấp phát của của C và C++ có sự khác nhau khá lớn, tùy thuộc vào mỗi người mà chọn nên theo C hay C++.

### Cú pháp của C: ta dùng hàm `malloc` –  memory allocate:

```cpp
// <kiểu dữ liệu của vùng nhớ> *<tên biến con trỏ> = (kiểu dữ liệu của vùng nhớ*)malloc(sizeof(kiểu dữ liệu của vùng nhớ));

//Ví dụ:
int *so_nguyen = (int*)malloc(sizeof(int)); //vùng nhớ 4 bytes kiểu int
double *so_thuc = (double*)malloc(sizeof(double)); //8 bytes kiểu double
char *ki_tu = (char*)malloc(sizeof(char)); //1 byte kiểu char
```

Cách dùng thì dùng như một biến con trỏ thông thường:

```cpp
int *so_nguyen = (int*)malloc(sizeof(int));
*so_nguyen = 10; //lưu trữ 10 ở vùng nhớ mà biến so_nguyen trỏ đến
```

Thu hồi vùng nhớ: đây là một vấn đề rất rất rất quan trọng trong cấp phát động. Đúng là vùng nhớ heap có dung lượng khá lớn, nhưng nó cũng có giới hạn, nên nếu bạn cứ vô tổ chức, vô kỉ luật, vô tội vạ cấp phát vùng nhớ liên tục mà không thu hồi lại sau khi sử dụng xong thì một lúc nào đó sẽ cạn dung lượng và gây ra tình trạng rò rỉ bộ nhớ (memory leak). Trong những hệ thống lớn, chương trình được chạy và các vùng nhớ được cấp phát liên tục nên nếu các lập trình viên không giải phóng vùng nhớ mỗi khi sử dụng xong thì việc cấp phát liên tục trong hệ thống sẽ khiến bộ nhớ bị cạn kiệt.

```cpp
int *so_nguyen = (int*)malloc(sizeof(int));
//do something with so_nguyen here
free(so_nguyen); //dùng free để giải phóng
```

Tuy nhiên, không phải lúc nào ta cũng cấp phát thành công. Ví dụ trong trường hợp bộ nhớ không đủ cho vùng nhớ ta muốn thì việc cấp phát sẽ thất bại và biến con trỏ trả về `NULL`, đó là lí do ta nên kiểm tra `NULL` sau khi cấp phát.

```
int *so_nguyen = (int*)malloc(sizeof(int));
if (so_nguyen == NULL) return false; //if fail then exit
*so_nguyen... //do something with this
free(so_nguyen); //and free it when done
```

### Cú pháp của C++: ta dùng operator `new`

```cpp
// <tên kiểu dữ liệu vùng nhớ> *<tên biến con trỏ> = new <tên kiểu dữ liệu vùng nhớ>;

//ví dụ
int *so_nguyen = new int; //4 bytes kiểu int
double *so_thuc = new double; //8 bytes kiểu double
char *ki_tu = new char; //1 byte kiểu char
```

Thao tác cũng giống như con trỏ thông thường:

```cpp
int *so_nguyen = new int;
*so_nguyen = 10; //lưu 10 vào vùng nhớ do so_nguyen trỏ đến
```

Và tất nhiên, xài xong phải giải phóng vùng nhớ:

```cpp
int *so_nguyen = new int;
delete so_nguyen; //ta dùng operator delete
```

Mẹo nhỏ: ta có thể khởi tạo giá trị tại vùng nhớ đó ngay khi vừa cấp phát:

```cpp
int *so_nguyen = new int(10);
int *so_nguyen2 = new int { *so_nguyen };
```

## Cấp phát vùng nhớ liên tục – dynamically allocate array

### Cú pháp của C: ta dùng 1 trong 2 hàm là `malloc` hoặc `calloc` (contiguous allocation – sự cấp phát liên tục).

```cpp
//malloc(<số phần tử> * sizeof(kiểu dữ liệu của từng phần tử));
//calloc(<số phần tử>, sizeof(kiểu dữ liệu của từng phần tử));

int *arr_malloc = (int*)malloc(100 * sizeof(int));
int *arr_calloc = (int*)calloc(100, sizeof(int));
```

Giải thích một chút về sự khác nhau giữa `malloc` và `calloc`:

`malloc`:

* Ở cú pháp `malloc`, mình xin cấp phát một vùng nhớ kích thước là `100 * sizeof(int)`.
* `malloc` chỉ nhận 1 tham số, đó là kích thước cả vùng nhớ.
* Nhanh hơn `calloc` một chút.

`calloc`:

* Ở cú phát `calloc`, mình xin cấp phát vùng nhớ 100 phần tử liên tục và mỗi phần tử có kích thước `sizeof(int)`.
* Sau khi cấp phát xong thì gán toàn bộ giá trị trong vùng nhớ bằng 0.
* `calloc` nhận 2 tham số là số lượng phần tử cho vùng nhớ và kích thước mỗi phần tử.
* Chậm hơn `malloc` một chút do phải gán toàn bộ giá trị trong vùng nhớ bằng 0.

Sau khi sử dụng xong thì ta cũng dùng `free` để giải phóng vùng nhớ.

### Cú pháp C++: ta vẫn dùng operator `new`

```cpp
// int *arr = new int[<số phần tử>];

int *arr = new int[100];
```

Cách khai báo mảng động ở C++ có phần khá giống với khai báo mảng tĩnh thông thường, đó là lí do một số bạn vẫn chuộng xài cú pháp của C++ do nó dễ nhớ hơn.

Sau khi dùng xong thì ta giải phóng nó:

```cpp
delete[] arr;
```

Lưu ý là có dấu `[]` sau `delete` nếu ta cấp phát mảng động, còn nếu chỉ là biến đơn thì không có `[]`.

# SỰ KHÁC BIỆT GIỮA ĐỘNG VÀ TĨNH

Sự khác biệt có thể thấy rõ nhất đó là ta có thể cung cấp được số lượng phần tử ngay lúc chương trình đang chạy trong cấp phát động:

```cpp
int number_items;
cin >> number_items;
int *arr = new int[number_items];
if (arr == NULL) return false;
for (int i = 0; i < number_items; ++i)     
    cin >> arr[i];
```

Khác biệt thứ 2 đó là ta có thể tùy chỉnh kích thước vùng nhớ đã cấp phát như mở rộng hoặc thu hẹp, bằng cách dùng hàm `realloc` (re-allocation):

```cpp
int *arr = (int*)calloc(10, sizeof(int)); //cấp phát 10 phần tử
realloc(arr, 5 * sizeof(int)); //thu hẹp còn 5 phần tử
```

Đó là cách sử dụng tổng quát hàm `realloc`, nhưng trong một số trường hợp tùy chỉnh thất bại thì ta nên dùng theo cách sau để kiểm tra `NULL` khi thất bại:

```cpp
int *arr = (int*)calloc(10, sizeof(int));
int *arr_new = (int*)realloc(arr, 5 * sizeof(int));
if (arr_new != NULL) //nếu tùy chỉnh thành công, tức là khác NULL
    arr = arr_new;
```

Lưu ý rằng realloc là hàm thuộc C và trong C++ không có hàm hay operator nào dùng để thay đổi kích thước vùng nhớ đã cấp phát (tất nhiên bạn cũng có thể dùng realloc trong C++ nhưng hàm đó vẫn là của C). Nếu bạn muốn thay đổi kích thước vùng nhớ đã cấp phát 1 cách thuần C++ thì đơn giản chỉ là... cấp phát vùng nhớ mới với kích thước mong muốn, copy dữ liệu từ vùng nhớ cũ sang rồi hủy vùng nhớ cũ.

Mở rộng vùng nhớ (thuần C++, không dùng hàm của C như memset, memcpy, realloc...):

```cpp
// mảng cũ
int oldLength = 10;
int *oldArr = new int[oldLength];
// xử lý gì đó với mảng cũ...

// cấp phát mảng mới với số lượng phần tử lớn hơn
int newLength = 20;
int *newArr = new int[newLength];

// copy dữ liệu từ vùng nhớ cũ sang
for (int i = 0; i < oldLength; ++i) {
  newArr[i] = oldArr[i];
}
delete[] oldArr;
oldArr = nullptr;
```

Thu hẹp vùng nhớ (cũng thuần C++ luôn):

```cpp
// mảng cũ
int oldLength = 10;
int *oldArr = new int[oldLength];
// xử lý gì đó với mảng cũ...

// cấp phát mảng mới với số lượng phần tử ít hơn
int newLength = 5;
int *newArr = new int[newLength];

// copy dữ liệu từ vùng nhớ cũ sang
for (int i = 0; i < newLength; ++i) {
  newArr[i] = oldArr[i];
}
delete[] oldArr;
oldArr = nullptr;
```

# TỔNG KẾT

Vậy là sau một hồi đọc cái bài dài miên man này, ta đã biết được:

* Những khuyết điểm của khai báo tĩnh và tự động.
* Tháp phân vùng bộ nhớ và chức năng từng vùng bộ nhớ.
* Khái niệm cấp phát động.
* Cấp phát biến đơn trong C/C++.
* Cấp phát mảng động trong C/C++.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-3-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-3-).html" data-numposts="5"></div>