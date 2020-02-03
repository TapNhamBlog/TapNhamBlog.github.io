<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

# POLYMORPHISM - ĐA HÌNH TRONG HƯỚNG ĐỐI TƯỢNG

<i>Đây là một khái niệm khá trừu tượng và khó hiểu nên bài viết này sẽ hơi dài...</i>

Link phần 4: [Inheritance trong C++](https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-4-).html)

## Quan hệ IS-A và HAS-A

Trong phần trước mình đã có giới thiệu qua về quan hệ IS-A được dùng trong kế thừa: nếu ta muốn `class Derived` kế thừa `class Base` thì ta phải đảm bảo rằng `class Derived` cũng là `class Base` (IS-A), giống như hình tam giác đều là kế thừa từ hình tam giác cân vì tam giác đều cũng <b>là</b> tam giác cân.

Thế còn quan hệ HAS-A thì sao? Đơn giản thôi, ta có quan hệ HAS-A khi trong `class A` có chứa thành phần kiểu `class B`. Ví dụ: ta có `class Student` và `class School`, trong `class School` có chứa mảng với kiểu `class Student` (giống như <b>trường học</b> thì <b>chứa</b> <b>học sinh</b>) nên ta gọi đây là quan hệ HAS-A. Ví dụ khác: ta có `class Point` và `class Triangle`; trong `class Triangle` thì <b>chứa</b> 3 điểm (3 thuộc tính) kiểu `class Point` nên đây cũng là quan hệ HAS-A.

![Example of HAS-A relationship](https://tapnhamblog.github.io/_img/HAS-A_Example.png)

```cpp
class Point {
public:
    double _x, _y;
};

class Triangle {
private:
    Point _angles[3]; // Triangle has Point
};
```

## Con trỏ từ Base class đến Derived class

Giả sử ta có `class Derived` kế thừa từ `class Base` thì ta có thể cho một biến con trỏ kiểu `Base` trỏ đến vùng nhớ có kiểu `Derived`.

Nghe có vẻ khó hiểu nhỉ, hãy xem thử ví dụ sau:

```cpp
// In this example, class Cat is inherited from class Animal
Animal *anAnimal;
Cat aCat;
anAnimal = &aCat;
```

Tại sao ta lại có thể làm như trên? Đơn giản thôi, vì Mèo cũng <b>là</b> Động Vật, do đó ta hoàn toàn có thể cho con trỏ kiểu `Animal` trỏ đến vùng nhớ kiểu `Cat`.

Ta đã tiến một bước gần hơn với khái niệm "Đa hình", hãy xem thử ví dụ sau:

```cpp
// Dog, Cat, Dolphin, Shark are inherited from Animal
Animal *anAnimal;

Cat cat;
Dog dog;
Dolphin dolphin;
Shark shark;

anAnimal = &cat;
anAnimal = &dog;
anAnimal = &shark;
anAnimal = &dolphin;
```

Như các bạn thấy, đối tượng `anAnimal` có thể thể mang <b>hình hài</b> của bất kì con vật nào.

## Ép kiểu "cục súc" không rõ ràng trong kế thừa - Implicit type conversion (type casting) in inheritance

C++ là một ngôn ngữ có khả năng ép kiểu rất triệt để: nếu ta có `class Derived` kế thừa từ `class Base`, thì ta hoàn toàn có thể đưa một đối tượng kiểu `Derived` vào tham số của một hàm có tham số kiểu `Base` vì `Derived` cũng là `Base` nên C++ sẽ tự ép kiểu ngầm định cho ta.

```cpp
Process(const Animal &animal);

int main() {
    Cat cat;
    process(cat); // This is OK in C++
}
```

## Liên kết tĩnh - Statitc Binding

Giả sử ta có: `class A` có phương thức `void print()`, `class B` kế thừa từ `class A` và cũng có phương thức `void print()`, `class C` kế thừa từ `class B` và cũng có phương thức `void print()`

```cpp
C c;
B b;

c.print(); // Call print of C
b.print(); // Call print of B
```

Vậy nếu ta muốn đối tượng `c` phía trên gọi đến phương thức `void print()` của đối tượng `b` thì sao?

```cpp
c.B::print(); // Call print of B
```

Vậy tình huống tiếp theo sẽ là: liệu ta có thể áp dụng static binding vào con trỏ từ `Base` đến `Derived` được không? Hãy xem ví dụ sau:

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    void Speak() {
        cout << "I am just an animal." << endl;
    }
};

class Cat : public Animal {
public:
    void Speak() {
        cout << "Meo meo" << endl;
    }
};

int main() {
    Animal *animal;
    Cat cat;
    animal = &cat;
    animal->Speak();
}
```

Đừng vội copy-paste vào IDE để chạy, trước tiên hãy đoán thử xem nó sẽ xuất ra kết quả gì? Ta có một biến con trỏ kiểu `Animal` trỏ vào vùng nhớ kiểu `Cat`, thứ ta mong muốn khi gọi phương thức `Speak` đó là biến con trỏ `animal` này sẽ kêu như con mèo, tức là xuất ra `meo meo`, nhưng...

Kết quả lại là `I am just an animal`. Tại sao lại như vậy? Biến trỏ vào vùng nhớ kiểu `Cat` thì phải gọi đến `Speak` của `Cat` chứ sao lại gọi `Speak` của `Animal`? Lí giải cho điều này đó là vì liên kết của ta là liên kết tĩnh - static binding. Vậy liên kết tĩnh là gì?

Khi ta khai báo một biến nào đó, ví dụ `int a;` thì kiểu dữ liệu của `a` sẽ được quyết định ngay lúc source code của ta được biên dịch, do đó nó là tĩnh. Ví dụ trên cũng vậy, khi ta khai báo `Animal *animal;` thì trình biên dịch sẽ không thể biết được là ta muốn dùng `animal` để trỏ đi đâu về đâu nên nó sẽ mặc định những gì mà `animal` trỏ là trỏ đến `class Animal`. Nói dễ hiểu hơn, giả sử ta có `Dog`, `Cat`, `Shark`, `Dolphin` cùng kế thừa từ `Animal` và ta có khai báo biến con trỏ `Animal *animal;` thì trình biên dịch sẽ không biết được ta đang muốn `animal` trỏ đến `Dog` hay `Cat` các kiểu nên nó sẽ mặc định là trỏ đến vùng nhớ của Base luôn là `Animal`. Vậy để khắc phục thì ta làm sao?

## Hàm ảo và liên kết động - Virtual function and Dynamic Binding

Hàm ảo, như cái tên của nó đã nói: đây là một hàm trừu tượng, tức là nó có thể biến hóa khôn lường, không rõ hình dạng, lúc thế này lúc thế kia. Còn nếu nói theo từ ngữ chuyên môn thì hàm ảo là một hàm mà khi ta dùng nó, trình biên dịch sẽ quyết định ngay tại thời điểm đang thực thi, đối tượng đang mang kiểu dữ liệu class nào để gọi phương thức của chính class đó, tức là liên kết động - Dynamic Binding.

Nghe khó hiểu nhỉ, vậy ta sẽ lấy thử ví dụ trên một lần nữa, nhưng, lần này ta sẽ thêm từ khóa `virtual` (ảo) vào trước phương thức `Speak` của `Animal`

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void Speak() {
        cout << "I am just an animal." << endl;
    }
};

class Cat : public Animal {
public:
    void Speak() {
        cout << "Meo meo" << endl;
    }
};

int main() {
    Animal *animal;
    Cat cat;
    animal = &cat;
    animal->Speak();
}
```

Khi chạy thử, kết quả lúc này sẽ là `Meo meo`, vì sao lại như thế? Khi ta dùng `virtual` cho phương thức `Speak` của `Animal`, lúc này ta đã biến `Speak` thành một phương thức ảo và `Speak` có thể "biến hóa" nội dung của nó tùy theo kiểu dữ liệu của đối tượng chứa nó đang trỏ đến. Trong ví dụ thì ta dùng lệnh `animal = &cat;` để cho biến con trỏ kiểu `Animal *` trỏ đến vùng nhớ kiểu `Cat`, tức là lúc này, `animal` sẽ là con mèo (`Cat`), do đó phương thức `Speak` trong nó cũng sẽ "biến hóa" thành `Speak` của kiểu `Cat`. Việc ta khai báo phương thức `Speak` trong `Cat` như trên được gọi là `override` (ghi đè). Ghi đè tức là việc ta khai báo `Speak` trong `Cat` và nó sẽ đè lên phương thức ảo `Speak` có sẵn trong `Animal`.

## Đa hình, sự kết hợp giữa hàm ảo và liên kết động - Polymorphism: a combination of Virtual Function and Dynamic Binding

Đa hình - tức là đa hình dạng, rằng một đối tượng với kiểu dữ liệu `class A` có thể biến đổi hình dạng của mình thành các kiểu dữ liệu của các class khác (có kế thừa từ `A`). Giống như ta cho `Dog`, `Cat` kế thừa từ `Animal` thì một biến con trỏ kiểu `Animal` có thể biến hình thành các kiểu `Dog` hay `Cat` gì tùy ý và khi biến đổi xong thành kiểu `Dog`, nó sẽ là con chó, nó sẽ hành động như con chó và nếu cho nó biến đổi thành `Cat` thì nó sẽ là mèo và hành động như con mèo.

```cpp
Animal *animal;

animal = new Dog; // I am a dog
delete animal;

animal = new Cat; // Now I am a cat!
delete animal;
```

Và đa hình cũng có một câu nói rất nổi tiếng về nó:

> If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck. (Nếu như "nó" nhìn như con vịt, bơi như con vịt, kêu như con vịt thì nó là con vịt.)

Nói thêm một chút về câu nói trên...

Trên thực tế, có một nhà thơ người Ấn Độ tên là James Whitcomb Riley cũng có một câu nói tương tự:

> When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck.


Bây giờ ta hãy đặt ra một tình huống nhỏ như sau và hãy đoán xem thử kết quả là gì:

```cpp
class Animal {
public:
    virtual void Speak() {
        cout << "I am just an animal." << endl;
    }
};

class Cat : public Animal {
public:
    virtual void Speak() {
        cout << "Meo meo" << endl;
    }
};

class BengalCat : public Cat {
public:
};

int main() {
    Animal *animal;
    BengalCat cat;
    animal = &cat;
    animal->Speak();
}
```

Lúc này kết quả sẽ là `Meo meo` do trong `class BengalCat` (kế thừa từ `Cat`) ta không hề viết phương thức `Speak` để ghi đè lên `Speak` của `Cat` nên trình biên dịch sẽ thực thi phương thức `Speak` của class cha gần nhất với `BengalCat` để thực thi đó là `Cat`.

# Phương thức hủy ảo - Virtual Destructor

Khi áp dụng đa hình, nếu `class Derived` kế thừa từ `Base` có dùng cấp phát động (hay dùng gì đó mà cần dọn dẹp) thì ta bắt buộc phải khai báo phương thức hủy ảo trong `Base` nhằm tránh việc bị thất thoát dữ liệu.

Hãy xem ví dụ sau:

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    ~Base() {
        cout << "Destructor from Base" << endl;
    }
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Destructor from Derived" << endl;
    }
};

int main() {
    Base *object = new Derived;
    delete object;
}
```

Khi ta chạy chương trình trên, ta sẽ thấy là chỉ có destructor của `Base` được gọi còn `Derived` thì không nên nếu như trong `Derived` mà ta có khai báo cấp phát động các kiểu thì sẽ dẫn đến thất thoát dữ liệu nghiêm trọng! Vì sao lại có hiện tượng như thế?

Khi ta không dùng `virtual` thì đó sẽ là liên kết tĩnh - static binding, tức là khi khai báo `Base *object = new Derived;` thì trình biên dịch sẽ mặc định destructor cho `object` là destructor thuộc `Base` chứ không phải `Derived`, do đó ta cần phải khai báo từ khóa `virtual` cho destructor của `Base`.

Nãy giờ cứ ảo ảo rồi trừu tượng các kiểu nghe mệt quá, còn gì nữa không? Thật ra là còn cái này nữa:

## Hàm thuần ảo - Pure virtual function

Giả sử ta có tình huống sau:

```cpp
int main() {
    Animal animal;
    animal.Speak(); // Unclear situation...
}
```

Khi ta khai báo `Animal animal;` và gọi `animal.Speak();`, vậy lúc đó ta đang gọi `Speak` của animal nào? `Speak` của `Cat` hay `Dog`? Những tình huống như thế thì ta sẽ áp dụng hàm thuần ảo vào.

Hàm thuần ảo là một hàm ảo, nhưng, nó không có phần cài đặt hàm, chỉ có khai báo thôi.

```cpp
class Animal {
public:
    virtual void Speak() = 0;
};
```

Trong C++ thì hàm thuần ảo có cú pháp là `virtual <return_type_of_method> <name_of_method>() = 0;`. Hãy chú ý đến phần `= 0` phía cuối hàm. Như đã nói, hàm thuần ảo chỉ có phần khai báo chứ không có phần cài đặt.

## Lớp thuần ảo - Abstract Class

Khi một class có chứa một phương thức thuần ảo như trên thì class đó sẽ thành lớp thuần ảo - abstract class. Đặc điểm của lớp thuần ảo đó là không thể khai báo theo cách thông thường được, ví dụ:

```cpp
class Animal {
public:
    virtual void Speak() = 0;
};

int main() {
    Animal animal; // Error!
}
```

Khi ta khai báo abstract class như trên thì mọi Derived class kế thừa từ abstract class phải có phần khai báo và cài đặt cho phương thức thuần ảo của abstract class hoặc ta phải khai báo lại phương thức thuần ảo đã có ở abstract class vào Derived class như ví dụ dưới đây:

```cpp
class Animal {
public:
    virtual void Speak() = 0;
};

class Cat : public Animal {
public:
    virtual void Speak() = 0; // Cat will become abstract class
};
```

Ứng dụng của abstract class đó là ta có thể tạo ra một cái nền có sẵn (interface) gồm các phương thức cần thiết cho các class con của nó mà ta không cần thiết phải cài đặt các phương thức đó.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/29/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-5-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/29/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-5-).html" data-numposts="5"></div>