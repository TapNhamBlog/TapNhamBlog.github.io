<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

Ở bài viết trước ([Tính đa hình trong C/C++](https://tapnhamblog.github.io/2018/06/29/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-5-).html)), mình đã giới thiệu xong 4 nguyên lý cơ bản nhất về hướng đối tượng mà ai học về nó cũng phải biết:
  * Tính đóng gói (Encapsulation).
  * Tính kế thừa (Inheritance).
  * Tính trừu tượng (Abstraction).
  * Tính đa hình (Polymorphism).

Trong bài này mình sẽ hướng dẫn qua một số kĩ thuật nho nhỏ cơ bản mà ta có thể áp dụng trong quá trình code để giúp công việc dễ dàng hơn.

## Nhận dạng kiểu dữ liệu của object hiện tại

Như các bạn đã biết về tính đa hình: một đối tượng có thể thay đổi "hình dạng" của nó, và hình dạng ở đây chính là kiểu dữ liệu; vậy nếu kiểu dữ liệu có thể thay đổi như thế thì làm sao để ta biết hiện tại nó đang mang kiểu dữ liệu gì?

Đơn giản thôi, ta chỉ cần tạo một hàm có tên là `Type` ở các class là được và hàm này sẽ trả về tên của class đó.

```cpp
#include <iostream>
#include <string>

class Base {
public:
  virtual std::string Type() = 0;
};

class DerivedA : public Base {
public:
  std::string Type() override {
    return "DerivedA";
  }
};

class DerivedB : public Base {
public:
  std::string Type() override {
    return "DerivedB";
  }
};

int main() {
  Base *object = nullptr;

  object = new DerivedA;
  std::cout << object->Type() << std::endl;
  delete object;

  object = new DerivedB;
  std::cout << object->Type() << std::endl;
  delete object;
}
```

## Nhân bản đối tượng - Clone Object

Không phải nói dài dòng: nhân bản đối tượng - clone object là kĩ thuật để ta tạo ra bản sao y hệt của một đối tượng.

Ơ... thế thì nói làm gì, chả phải đã có copy constructor cũng như `operator=` rồi, cần kĩ thuật quái gì nữa.

Thật ra, nếu như ta khai báo object tĩnh như `A a;` thì dùng nhân bản cũng giống như dùng dao mổ trâu giết gà vậy. Nhưng, ở đây ta đang làm việc với đa hình, object nó có thể biến đổi kiểu liên tục, ta hãy xem ví dụ sau:

```cpp
class Base {

};

class DerivedA : public Base {

};

class DerivedB: public Base {

};

void Func(Base* &object) {
  // giả sử bây giờ ta muốn tạo bản sao cho object thì sao nhỉ...
  DerivedA copyObjA = object; // Ơ, lỡ object là kiểu DerivedB thì sao...
  DerivedB copyObjB = object; // thế nếu object lại là kiểu DerivedA thì sao =.=
}

int main() {

}
```

Như các bạn thấy trong ví dụ trên, dùng copy constructor hoàn toàn vô dụng, do `object` có thể là kiểu `DerivedA` hoặc là `DerivedB` hay thậm chí là `Base`. Vậy nếu ta muốn copy đối tượng thì bắt buộc phải dùng kĩ thuật nhân bản:

```cpp
class Base {
public:
  // Ta sẽ dùng một hàm ảo tên là Clone để thực hiện việc nhân bản
  virtual Base * Clone() = 0;
};

// Ở mỗi derived class ta sẽ override hàm Clone

class DerivedA : public Base {
public:
  DerivedA * Clone() {
    return new DerivedA(*this); // ta sẽ dùng copy-constructor để copy nội dung hiện tại của object.
  }
};

class DerivedB: public Base {
public:
  DerivedB * Clone() {
    return new DerivedB(*this);
  }
};

void Func(Base* &object) {
  // ta sẽ không cần quan tâm object đang là kiểu gì nữa, chỉ cần gọi hàm Clone là được
  Base *newObject = object->Clone();

  // à nhớ delete nha...
  delete newObject;
}

int main() {

}
```

Kĩ thuật quá hay phải không, ta không cần phải biết `object` đang trỏ tới vùng nhớ kiểu gì để gọi copy constructor mà chỉ cần gọi hàm `Clone` là được, tự `object` sẽ copy chính nó và gán cho `newObject`.

## Template cho cấu trúc với kiểu dữ liệu tùy biến

Đặt vấn đề như sau: người dùng muốn hoán vị 2 số nguyên. Nếu như từ trước giờ thì ta vẫn sẽ viết là:

```cpp
void Swap(int &first, int &second) {
  int temp = first;
  first = second;
  second = temp;
}
```

Tuy nhiên, lúc này người dùng lại nổi hứng lên và muốn hoán vị 2 số thực thì phải làm sao, không lẽ ta phải viết thêm cái hàm `Swap` với tham số là 2 số thực kiểu `double`? Tất nhiên là không, như thế sẽ vi phạm quy tắc sau:

### DRY - Don't Repeat Yourself

Quy tắc DRY được định nghĩa rõ ràng như sau: trong 1 chương trình không được phép có 2 đoạn code giống nhau về mặt chức năng hoặc cú pháp của nó. Tại sao lại như thế? Hãy dùng 2 hàm `Swap` trên làm ví dụ: giả sử sau này ta phát sinh thêm nhiều loại hàm `Swap` nữa, như hoán vị phân số (`Fraction`), hoán vị kiểu này kiểu kia,... và nếu như ta thay đổi cấu trúc của 1 hàm `Swap` thì những hàm còn lại ta cũng phải kiểm tra để thay đổi theo, hoặc nếu 1 hàm bị lỗi thì rất có thể những hàm còn lại cũng có khả năng bị lỗi theo. Việc đó dẫn đến những khó khăn trong quá trình mở rộng phần mềm hay sửa lỗi. Vậy ta phải làm gì để khắc phục vấn đề trên?

### `template` trong C++

Việc lúc này ta cần đó là viết 1 hàm `Swap` tổng quát nhất mà nó có thể áp dụng cho mọi kiểu dữ liệu. Ta chỉ cần quăng 2 biến có cùng kiểu dữ liệu vào, bất kể kiểu dữ liệu gì thì cũng sẽ hoán vị được.

Trong C++ cung cấp cho ta 1 từ khóa để làm điều đó, chính là `template`. Ta hãy thử làm ví dụ với hàm `Swap`:

```cpp
template <typename Type>
void Swap(Type &first, Type &second) {
  Type temp = first;
  first = second;
  second = temp;
}

// You can use template <class Type> instead of typename.
```

Ở đoạn code trên, ta tự định nghĩa một kiểu `Type` làm vỏ bọc bằng cách dùng `template`. `Type` chỉ là một vỏ bọc, nó không phải là một kiểu dữ liệu xác định và `Type` chỉ xác định khi ta gọi hàm `Swap` và truyền vào nó 2 biến thì lúc này `Type` sẽ là kiểu dữ liệu của 2 biến đó. Ví dụ ta gọi `Swap` để hoán vị 2 số nguyên kiểu `int`, khi gọi như thế thì `Type` lúc này sẽ trở thành `int`, còn nếu ta hoán vị 2 số thực kiểu `double` thì `Type` sẽ trở thành `double`.

## `template` cho class

`template` ngoài ra còn có thể áp dụng cho class. Ví dụ ta muốn viết một class `Array` để lưu trữ và thao tác trên mảng thì ta có thể dùng `template` để biến kiểu dữ liệu cho mảng trong `Array` là kiểu tùy ý:

```cpp
template <class Type>
class Array {
public:
  Array() : _arr(nullptr), _size(0) { }

  Array(const int &size) : _arr(new int[size]), _size(size) { }

  ~Array() {
    delete[] _arr;
    _arr = nullptr;
    _size = 0;
  }

private:
  Type *_arr;
  int _size;
};

int main() {
  Array<int> intArr; // int array
  Array<double> doubleArr; // double array
}
```

## Hàm static trong class

Giả sử ta có một class gọi là `Console`, class đó chứa các hàm để tùy chỉnh màn hình console của ta như chỉnh kích thước, chỉnh font chữ, chỉnh màu,... và điểm đặc biệt đó là các hàm này không có mối liên hệ gì với nhau (không gọi lẫn nhau, không dùng bất kì thuộc tính nào trong class).

```cpp
class Console {
public:
  void ChangeSize(const int &height, const int &width);

  void ChangeBackgroundColor(const int &colorCode);

  void ChangeFontFamily(const string &fontName);

  void ChangeFontSize(const int &size);

  // ...
};
```

Vấn đề tiếp theo, ở hàm `A` ta muốn chỉnh kích thước màn hình console lại, rồi ở hàm `B` ta muốn chỉnh màu console, rồi `C` chỉnh cái này trên console, rồi `D` chỉnh các kia trên console,...

Nếu làm theo cách thông thường thì có phải là ở mỗi hàm `A`, `B`, `C`,... ta sẽ khai báo một đối tượng có kiểu `Console` rồi gọi đến phương thức bên trong nó để tùy chỉnh theo ý muốn. Nhưng, như thế thì hơi rườm rà và phức tạp vì mỗi lần muốn chỉnh gì đó là ta lại phải khai báo đối tượng kiểu `Console` mới ở những nơi ta muốn chỉnh. Vậy có cách nào để ta không cần khai báo đối tượng mà vẫn dùng được các phương thức bên trong class đó không, nhất là khi các phương thức trong class đó không có liên hệ với nhau như class `Console` phía trên.

Giải pháp ở đây đó là dùng phương thức `static`. Trước khi giải thích ý nghĩa của phương thức `static` thì mình sẽ cho các bạn xem code ví dụ trước:

```cpp
class Console {
public:
  static void ChangeSize(const int &height, const int &width);

  static void ChangeBackgroundColor(const int &colorCode);

  static void ChangeFontFamily(const string &fontName);

  static void ChangeFontSize(const int &size);
};

void A() {
  Console::ChangeSize(1080, 960);
}

void B() {
  Console::ChangeFontFamily("SF Mono");
}
```

Như các bạn đã thấy, khi dùng `static`, ta có thể sử dụng các phương thức của `Console` trong các hàm `A`, `B` mà không cần phải khai báo đối tượng kiểu `Console` để gọi hàm. Vậy phương thức `static` là gì?

Phương thức `static` cũng như thuộc tính `static` là những thành phần tĩnh bên trong class. Khi chạy chương trình thì địa chỉ của những thành phần đó là cố định và không thay đổi. Ví dụ trong class `A` có 1 thuộc tính `static` tên là `temp` thì khi ta khai báo bao nhiêu đối tượng kiểu `A` đi nữa thì thuộc tính `temp` bên trong các đối tượng đó vẫn có chung 1 vùng nhớ, nên khi ta thay đổi `temp` ở một đối tượng thì `temp` trong những đối tượng còn lại cũng sẽ bị thay đổi theo do chúng có chung 1 vùng nhớ.

Một số điểm lưu ý về thành phần `static`:

  * Các thành phần `static` được ứng dụng rất nhiều vì nó linh hoạt, cho phép ta sử dụng các thành phần đó mà không cần phải làm thủ tục khai báo đối tượng.
  * Các phương thức `static` không thể dùng con trỏ `this` bên trong nó, đó là lí do mình đặt vấn đề ban đầu là các phương thức này không được phép có liên hệ với những phần còn lại bên trong class do nó không dùng được `this` bên trong nó.
  * Ta có thể dùng các thành phần `static` bằng cách dùng cú pháp `<class_name>::<static_member_name>`.

Còn tiếp...

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/07/05/M%E1%BB%99t-S%E1%BB%91-K%E1%BB%B9-Thu%E1%BA%ADt-C%C6%A1-B%E1%BA%A3n-Trong-H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/07/05/M%E1%BB%99t-S%E1%BB%91-K%E1%BB%B9-Thu%E1%BA%ADt-C%C6%A1-B%E1%BA%A3n-Trong-H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng.html" data-numposts="5"></div>