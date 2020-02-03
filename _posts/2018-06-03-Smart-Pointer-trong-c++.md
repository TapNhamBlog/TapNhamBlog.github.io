<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

<link rel="icon" type="image/png" href="https://tapnhamblog.github.io/_img/HuaAnhMinh.ico">

Đặt vấn đề: có phải từ trước đến giờ khi cấp phát bộ nhớ động trong C++ thì bạn vẫn hay dùng một trong 2 cách sau:

```cpp
int *newInt = new int;
int n;
cin >> n;
int *newArr = new int[n];
```

Và sau đó bạn giải phóng vùng nhớ đã cấp phát với lệnh:

```cpp
delete newInt;
delete[] newArr;
```

Nhưng giả sử nếu bạn quên gọi `delete` để giải phóng vùng nhớ thì sao? Cho dù ta có giỏi đến mấy thì vẫn phải có sai sót phải không. Đó là lí do các lập trình viên pro đã nghĩ ra một kỹ thuật nhằm kiểm soát con trỏ sao cho an toàn hơn, đó là <b>Smart Pointer</b>.

Smart Pointer là một kỹ thuật nhằm tạo ra một con trỏ với kiểu dữ liệu tùy ý và điểm mạnh nhất của con trỏ này là sẽ tự động giải phóng vùng nhớ cho ta. Để hiểu được hết về Smart Pointer thì ta cần phải đi qua một số khái niệm và kĩ thuật sau:

## Kỹ thuật `template`

Giả sử ta có một hàm Swap số nguyên như sau:

```cpp
void Swap(int &first, int &second) {
    int third = first;
    first = second;
    second = temp;
}
```

Không có gì quá khó hiểu phải không, ta gọi hàm `Swap`, quăng vào đó 2 biến số và nó sẽ đảo giá trị cho ta. Nhưng, nếu bây giờ ta muốn swap nhiều thứ khác thì sao, như là Swap 2 phân số, Swap vị trí 2 học sinh, Swap các kiểu dữ liệu trên trời dưới đất, không lẽ ta phải viết thêm một đống hàm Swap cho mỗi kiểu dữ liệu? Tất nhiên là không, trong C++ ta có một kỹ thuật khác đó là dùng `template` như sau:

```cpp
template <typename T>
void Swap(T &first, T &second) {
    T third = first;
    first = second;
    second = third;
}
```

Nói đơn giản là như sau, khi ta dùng template và định nghĩ kiểu T như thế, khi ta đặt một tham số nào đó có kiểu T thì ta có thể đưa bất kì kiểu dữ liệu nào vào T. Với hàm Swap dùng template như thế thì ta có thể gọi Swap với bất kì kiểu dữ liệu nào. Và khi gọi hàm ta sẽ dùng như sau:

```cpp
int a = 1, b = 2;
Swap(a, b);
cout << a << " " << b << endl;
```

Khi gọi Swap, ta sẽ thêm `int` vào nhằm cho trình biên dịch biết ta muốn T sẽ trở thành kiểu dữ liệu nào. Nếu ta có muốn Swap phân số thì thay là `<Fraction>`, học sinh là `<Student>`,…

## RAII – Resource Acquisition Is Initialization

Đây là kĩ thuật áp dụng trong Smart Pointer, đó là ta sẽ ràng buộc một biến / object cục bộ với một biến / object cấp phát động. Khi biến / object cục bộ kết thúc vòng đời thì biến / object cấp phát động ràng buộc với nó cũng sẽ tự giải phóng. Ta khai báo một class như sau:

```cpp
template <typename Type>
class SmartPointer {
    Type *_ptr;
public:
    SmartPointer(Type *ptr = nullptr) : _ptr(ptr) { };

    ~SmartPointer() {
        delete[] _ptr;
        _ptr = nullptr;
    }
};
```

Và ta gọi nó như sau:

```cpp
SmartPointer<int> newPtr(new int);
```

Như các bạn thấy, ta khai báo một object có kiểu `SmartPointer`với kiểu của con trỏ là `int`. Ta truyền vào tham số cho constructor là `new int`, tức là ta đã cấp phát một vùng nhớ kiểu `int` cho con trỏ `_ptr` bên trong `SmartPointer`. Khi biến `newPtr` có kiểu `SmartPointer` kết thúc vòng đời, ví dụ như ra khỏi `{ }` (phạm vi hoạt động) hay kết thúc chương trình thì lúc này biến `_ptr` bên trong cũng sẽ tự giải phóng bằng cách gọi destructor.

Nhưng lúc này sẽ có một vấn đề nhỏ, đó là nếu ta muốn truy cập vào nội dung bên trong `newPtr` thì sao, do `_ptr` là có phạm vi truy cập là `private` nên ta không thể truy cập từ ngoài được. Lúc này ta sẽ overload cho 2 operator đó là `*` (de-reference operator) và `->` (point-to reference operator).

Ta sẽ bổ sung thêm 2 phương thức sau vào class `SmartPointer`:

```cpp
Type &operator*() {
    return *_ptr;
}

Type *operator->() {
    return _ptr;
}
```

Operator `*` giúp ta lấy được toàn bộ nội dung của vùng nhớ con trỏ hướng đến và operator `->` giúp ta trỏ vào từng thành phần bên trong vùng nhớ đó (áp dụng khi con trỏ có kiểu là struct hay class).

```cpp
template <typename Type>;
class SmartPointer {
    Type *_ptr;
public:
    SmartPointer(Type *ptr = nullptr) : _ptr(ptr) { };

    Type &operator*() {
        return *_ptr;
    }
    
    Type *operator->() {
        return _ptr;
    }
    
    ~SmartPointer() {
        delete[] _ptr;
        _ptr = nullptr;
    }
};

struct Test {
    int temp = 10;
};

int main() {
    SmartPointer<int> first(new int);

    SmartPointer<int> second(new Test);
    cout << second->temp << endl;

    SmartPointer<int> third(new int[10]);
            
    int *ptr = new int[20];
    SmartPointer<int> fourth(ptr);

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


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/Smart-Pointer-trong-c++.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/Smart-Pointer-trong-c++.html" data-numposts="5"></div>
