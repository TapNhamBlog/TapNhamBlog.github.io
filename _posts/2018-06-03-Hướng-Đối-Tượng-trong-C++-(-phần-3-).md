<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

# NẠP CHỒNG TOÁN TỬ - OPERATOR OVERLOADING

Link phần 2: [Constructors và Destructors trong C++](https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-2-).html)

Link phần 4: [Inheritance in OOP - Kế thừa trong hướng đối tượng](https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-4-).html)

## Nạp chồng hàm - Function Overloading

Ta hãy đặt ra một tình huống như sau: người dùng muốn ta viết một hàm cộng 2 số nguyên:

```cpp
int SumTwoInts(int firstInt, int secondInt) {
    return firstInt + secondInt;
}
```

Đơn giản phải không. Nhưng nếu lúc này người dùng lại muốn ta viết hàm cộng 3 số nguyên thì sao, không lẽ viết hàm int `SumThreeInts(int firstInt, int secondInt, int thirdInt)`? Chúng ta có thể thấy rõ ràng các hàm này có một đặc điểm giống nhau đó là cùng chung một chức năng cộng nhiều số nguyên, vậy có cách nào để “nhóm” các hàm này lại với nhau cho thống nhất về mặt tính năng của nó không? Đó chính là nạp chồng hàm.

Nạp chồng hàm là một “nhóm” các hàm giống nhau về tên và chức năng nhưng khác biệt về cách gọi chúng. Nạp chồng hàm được dùng khi ta muốn thực hiện một hành động nhưng với nhiều cách thức khác nhau, như ở ví dụ trên là ta muốn cộng số nguyên với nhiều cách thức như cộng 2 số, cộng 3 số,... Và với ví dụ trên ta có thể biến 2 hàm trên thành 1 “nhóm” hàm nạp chồng như sau:

```cpp
int SumInts(int firstInt, int secondInt) {
    return firstInt + secondInt;
}

int SumInts(int firstInt, int secondInt, int thirdInt) {
    return firstInt + secondInt + thirdInt;
}
```

Như các bạn thấy, 2 hàm trên hoàn toàn giống nhau về tên, chỉ khác nhau về cách gọi. Giả sử nếu ta gọi `int integer = SumInts(0, 1);` thì trình biên dịch sẽ hiểu là ta đang muốn gọi hàm `SumInts` đầu tiên với tham số truyền vào là 2 số nguyên; còn nếu ta gọi `int integer = SumInts(0, 1, 2);` thì trình biên dịch sẽ lấy hàm `SumInts` thứ hai để thực thi với tham số truyền vào là 3 số nguyên.

Một số lưu ý nhỏ về nạp chồng hàm / phương thức:

* Tên gọi phải giống nhau.
* Tham số truyền vào phải khác nhau giữa các hàm (khác về số lượng tham số hoặc kiểu dữ liệu tham số hoặc cả 2).
* Cùng tham số nhưng khác kiểu dữ liệu trả về thì không được xem là nạp chồng.
* Tất cả các nạp chồng phải thể hiện rõ một chức năng duy nhất, ví dụ có 3 nhóm nạp chồng là tạo chuỗi, chèn chuỗi, xóa chuỗi, thì các hàm trong 3 nhóm đó phải thể hiện đúng chức năng của nhóm đó.

## Nạp chồng toán tử - operator overloading

Ta hãy đặt ra một tình huống như sau: giả sử ta có class phân số

```cpp
class Fraction {
    int _num, _denom;
};
```

Nếu bây giờ ta muốn cộng 2 phân số với nhau thì sao? Có phải trước giờ ta vẫn hay làm bằng cách tạo một hàm `Add` như bên dưới:

```cpp
public:
    Fraction Add(Fraction &secondFraction) {
        Fraction result;
        result._denom = _denom * secondFraction._denom;
        result._num = _num * secondFraction._denom + secondFraction._num * _denom;
        return result;
    }
```

Cách trên thì đúng nhưng vẫn chưa tường minh, nó không thể hiện rõ việc cộng 2 phân số. Hãy nhớ lại mục trên, có phải nạp chồng hàm là khi ta muốn thể hiện một hành động với nhiều cách khác nhau; và ở ví dụ này, cộng 2 phân số cũng là cộng, vậy có cách nào để ta có thể nạp chồng toán tử cộng này không? Rất đơn giản, cách khai báo như sau:

```cpp
<return_type> operator<type_of_operator>(arguments);
```

* Giả sử ta muốn cộng 2 phân số, vậy lúc này `<type_of_operator>` của ta sẽ là `+`.
* Khi 2 phân số cộng nhau thì kết quả của nó sẽ là một phân số, vậy `<return_type>` của nó sẽ là `Fraction`.
* Lúc này ta chỉ mới có 1 phân số, vậy nếu muốn cộng phân số thứ hai thì ta phải truyền phân số thứ 2 vào tham số, vậy arguments sẽ là `Fraction secondFraction` để ta cộng vào cho phân số hiện tại.

Và kết quả là:

```cpp
Fraction operator+(Fraction secondFraction) {
        Fraction result;
        result._denom = _denom * secondFraction._denom;
        result._num = _num * secondFraction._denom + secondFraction._num * _denom;
        return result;
    }
```

Nếu ta muốn cộng 2 phân số thì chỉ việc dùng như phép cộng thông thường:

```cpp
Fraction first(1, 3), second(1, 2);
Fraction third = first + third;
```

Với cách nạp chồng toán tử như thế thì các phép toán, so sánh,... của ta sẽ trông tường mình và dễ hiểu hơn nhiều so với việc viết thành phương thức `Add` như lúc trước. Dưới đây là bảng các toán tử các bạn có thể nạp chồng:

 |   |   |   |   |   |   |   |
 |---|---|---|---|---|---|---|
 | `+` | `-` | `*` | `/` | `%` | `^` | `&` |
 | `|` | `~` | `!` | <b>`=`</b> | `<` | `>` | `+=` |
 | `-=` | `*=` | `/=` | `%=` | `^=` | `&=` | `|=` |
| `<<` | `>>` | `<<=` | `>>=` | `==` | `!=` | `<=` |
| `>=` | `&&` | `||` | `++` | `--` | `->*` | `,` |
| <b>`->`</b> | <b>`[]`</b> | <b>`()`</b> | `new` | `new[]` | `delete` | `delete[]` |

Một số lưu ý khi dùng nạp chồng toán tử:

* Các toán tử `::` hoặc `.` hoặc `.*` không được khai báo bởi lập trình viên.
* Các toán tử `sizeof`, `typeid`, `?:` không được nạp chồng.
* Các toán tử `=`, `->`, `[]`, `()` chỉ có thể được nạp chồng bởi các hàm non-static.
* Toán tử phải thể hiện đúng những gì người dùng muốn, không được phép có những trường hợp như làm toán tử `+` nhưng lại thực hiện phép `*` bên trong.
* Toán tử khi áp dụng phải đúng logic, ví dụ việc tăng thời gian (giờ, phút, giây) bằng cách `+` hoặc `-` cho giây là hợp lý nhưng `*` hoặc `/` là không đúng logic.
* Khi nạp chồng cho 1 toán tử nào đó thì nên nạp chồng cho toàn bộ những toán tử có cùng tính chất với nó, ví dụ nếu ta nạp chồng cho toán tử `>` thì phải nạp chồng các toán tử như `<`, `==`, `>=`, `<=` vì chúng cùng 1 tính chất là dùng để so sánh.

## Hàm `friend`

Hàm `friend` là một hàm đặc biệt trong hướng đối tượng C++, giả sử class A có hàm `friend` tên là `Foo` thì hàm `Foo` sẽ không thuộc class A nhưng vẫn có thể truy cập vào các thuộc tính `private` hay `protected` bên trong class A. Có một số lưu ý sau về hàm `friend`:

* Các phương thức bình thường trong một class sẽ được gọi với cú pháp kiểu như `A.Foo()`, còn với hàm friend thì sẽ gọi với cú pháp `Foo(A)`.
* Không nên lạm dụng hàm `friend` nếu không cần thiết. Trong bất kì tình huống nào nếu được dùng phương thức như bình thường thì hãy dùng phương thức, chỉ khi nào bắt buộc xài hàm `friend` thì mới nên xài.
* Hàm `friend` không vi phạm tính đóng gói trong hướng đối tượng, cũng giống như ngoài đời thực ta có thể chia sẻ những bí mật của ta cho bạn bè thân.
* Đôi khi hàm `friend` sẽ rất hữu dụng, ví dụng nếu ta muốn nạp chồng các operator như `<<`, `>>` dùng để nhập xuất với `istream` (`cin`), `ostream` (`cout`), `ifstream`, `ofstream`.

Cách khai báo hàm `friend` rất đơn giản, chỉ việc đặt từ khóa friend trước phần khai báo của hàm. Ví dụ ta sẽ tạo hàm `Add` 2 phân số như sau:

```cpp
friend Fraction Add(Fraction first, Fraction second) {
        return Fraction(first._num * second._denom + first._denom * second. _denom,
        first._denom * second._denom);
}
```

Và ta dùng nó như sau:

```cpp
Fraction first(1, 3), second(1, 2);
Fraction third = Add(first, second);
```

## Nạp chồng `cin` và `cout` - Overloading `cin` and `cout`

Chúng ta không được phép truy cập và chỉnh sửa 2 thư viện `istream` và `ostream`, do đó ta không thể nạp chồng 2 toán tử `<<` và `>>` như một phương thức trong class của ta được. Đó là lí do ta phải dùng hàm friend cho 2 toán tử này vì hàm `friend`.
Cú pháp dùng:

```cpp
friend istream &operator>>(istream &in, Fraction &fraction) {
        in >> fraction._num >> fraction._denom;
        return in;
}
```

Và để dùng thì:

```cpp
Fraction first(1, 3);
cin >> first;
```

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-3-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-3-).html" data-numposts="5"></div>