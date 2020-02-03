<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

# INHERITANCE - KẾ THỪA TRONG HƯỚNG ĐỐI TƯỢNG

Link phần 3: [Operator Overloading trong C++](https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-3-).html)

## Kế thừa là gì?

### Giới thiệu

Khi chúng ta định nghĩa một khái niệm trong hướng đối tượng, giả sử như chiếc xe chẳng hạn, thì ta không đơn thuần chỉ là định nghĩa chiếc xe mà còn phải định nghĩa những thứ liên quan đến chiếc xe như buồng máy xe, bánh xe,…

Để định nghĩa thì trước giờ ta vẫn dùng class, tuy nhiên giữa các sự vật với nhau cũng có những mối liên hệ nhất định, ví dụ như khi ta có hình chữ nhật và hình vuông thì ai cũng biết hình vuông cũng chính là hình chữ nhật; vậy làm thế nào để ta có thể mô tả được mối quan hệ ấy bên trong code?

Trên thực tế, một trong những tính năng quan trọng nhất của hướng đối tượng chính là việc <b>tái sử dụng các class và các mã nguồn</b>. Tất cả những thuộc tính và phương thức của class này cũng có thể là thuộc tính và phương thức của class khác. Ví dụ khi ta nói hình vuông cũng là hình chữ nhật thì chiều dài, chiều rộng trong hình chữ nhật vẫn áp dụng được cho hình vuông và cách tính diện tích của hình chữ nhật vẫn áp dụng được cho hình vuông.

### Kế thừa

Như lúc nãy, ta có thể nói (chú ý chữ in đậm): hình vuông “<b>LÀ</b>” hình chữ nhật – `Square` “<b>IS A</b>” `Rectangle`; học sinh “<b>LÀ</b>” con người – `Student` “<b>IS A</b>” `Person`, và quan hệ như thế này ta gọi là kế thừa.

Giả sử ta có class `Person` và class `Studen`t là như sau:

![Example of Student and Person seperate](http://huaanhminh.github.io/_img/OOP_Person_Student_Seperate.png)

Như các bạn thấy, `Student` cũng là `Person`, nên tên (`_name`) và tuổi (`_age`) của `Student` cũng chính là tên và tuổi của `Person`, vậy thay vì làm 2 class tách biệt thế thì ta có cách nào mô hình hóa quan hệ giữa 2 class này không? Hãy xem thử hình sau:

![Example of Student inherit from Person](http://huaanhminh.github.io/_img/OOP_Person_Student_Inherit.png)

Class Student kế thừa từ class `Person`, nên lúc này class `Person` được gọi là <b>Base class</b> – class cha, còn class `Student` được gọi là <b>Derived class</b> – class con; và ta dùng mũi tên trắng đi từ Derived class trỏ đến Base class như hình trên.

Ngoài ra ta cũng có thể kế thừa nhiều tầng với nhiều nhánh con như sau:

![Example multilevel-hierachical inheritance](http://huaanhminh.github.io/_img/OOP_Multilevel_Hierachical_Inheritance_Example.png)

Theo hình trên thì ta có class `Person` là Base class, class `Student` lúc này sẽ vừa là Base class vừa là Derived class còn 3 class Regular Student, College Student và In-service Student là Derived classses.

### Cú pháp

Trong C++, khi ta nói Derived class kế thừa từ Base class thì ta sẽ minh họa như sau trong code:

```cpp
class <Derived class> : <access-specifier> <Base class> {

};
```

Lưu ý, giả ta ta có class B kế thừa từ A:

* Thuộc tính và phương thức nào có phạm vi là `public` trong A sẽ trở thành thuộc tính và phương thức trong B.
* `private` trong A sẽ trở thành một phần của B nhưng chúng chỉ được truy cập thông qua `public` hay `protected` của A (do `private` không thể truy cập từ ngoài class).

### Từ khóa `protected`:

Nếu thuộc tính hay phương thức của class A có phạm vi truy cập là `protected` thì class B kế thừa từ A sẽ truy cập được vào những thuộc tính hay phương thức ấy nhưng nếu như bên ngoài class thì không thể truy cập, ví dụ:

```cpp
#include <iostream>
using namespace std;

class A {
protected:
    int _attributeInA = 10;
};

class B : public A {
public:
    B() {
        cout << _attributeInA << endl;
    }
};

int main() {
    B objectOfB;
    return EXIT_SUCCESS;
}
```

Hãy thử chạy đoạn code trên và xem kết quả là gì.

## Access Specifier

Có 3 mức độ sau:

* `public`: những gì có phạm vi `public` và `protected` của Base class sẽ trở thành `public` và `protected` của Derived class.
* `protected`: Những gì `public` và `protected` của Base class sẽ trở thành `protected` của Derived class.
* `private`: `public` và `protected` của base class sẽ trở thành `private` của Derived class.

## Inheritance - member functions

Member functions trong Base class (`protected` và `public`) sẽ được kế thừa bởi Derived class, ngoại trừ những phương thức sau:

* Constructor.
* Destructor.
* Assignment Operator.

Tức là những phương thức trên bạn cần phải viết ở mỗi Derived class.

### Constructor in Inheritance

Khi ta tạo một object có kiểu thuộc Derived class thì:

* Constructor của Base class sẽ được gọi đầu tiên.
* Constructor của Derived class sẽ được gọi tiếp theo.
* Trong constructor của Derived class thì ta có thể chọn loại constructor của Base class để gọi. Nếu ta không chọn thì trình biên dịch sẽ chọn default constructor của Base class.

Ví dụ:

```cpp
#include <iostream>
using namespace std;

class A {
public:
    A() {
        cout << "Default constructor in A" << endl;
    }

    A(const int &value) {
        cout << "Called from A with: " << value << endl;
    }
};

class B : public A {
public:
    B(const int &value) : A(value) {
        cout << "Called from B" << endl;
    }
};

int main() {
    B objectOfB(10);
    return EXIT_SUCCESS;
}
```

Hãy chạy thử code trên và xem kết quả.

### Destructor in Inheritance

Khi một object kết thúc vòng đời thì:

* Destructor từ Derived class sẽ được gọi trước.
* Destructor từ Base class sẽ được gọi tiếp theo.

Lưu ý: nếu ta có sử dụng con trỏ và bộ nhớ động thì những gì của Base class nên được hủy trong destructor của Base class và những gì của Derived class thì phải được hủy trong Derived class.

```cpp
class A {
protected:
    int *a = new int(10);

public:
    A() = default;

    ~A() {
        cout << "Des from A" << endl;
        delete a;   // Correct
    }
};

class B : public A {
public:
    B() = default;

    ~B() {
        cout << "Des from B" << endl;
        // delete a; No! a is from A so you should delete a in A.
    }
};
```

Về thứ tự gọi constructor và destructor giữa Base class và Derived class <b>trong C++</b>, hãy nhớ rằng:

> Cái gì vào trước thì ra sau cùng.

Lưu ý rằng câu trên chỉ đúng với C++, một số ngôn ngữ khác như Java hay C# thì ngược lại.

### Re-define member functions

Đôi khi vì yêu cầu kỹ thuật chúng ta cũng cần phải viết lại một số hàm mà Derived class kế thừa từ Base class (ví dụ Base class đã có hàm Foo nhưng ta đôi khi ta vẫn cần viết lại hàm Foo khác trong Derived class), và được gọi là Re-define.

Lưu ý: khi ta re-define một hàm trong Base class thì khi ta gọi object với kiểu của Derived class thì trình biên dịch sẽ ẩn đi (không cho gọi) hàm đó trong Base class. Ví dụ:

```cpp
#include <iostream>
using namespace std;

class Rectangle {
public:
    void Speak(const int &length, const int &width) {
        cout << "I am a Rectangle with: length = " << length << " and width = " << width << endl;
    }
};

class Square : public Rectangle {
public:
    void Speak(const int &length) {
        cout << "I am a Square with: length = " << length << endl;
    }
};

int main() {
    Square square;
    square.Speak(10);
    // square.Speak(10, 5); Wrong!
    return EXIT_SUCCESS;
}
```

Vậy để ta có thể gọi `Speak` của class `Rectangle` trong object có kiểu `Square` thì làm thế nào?

### Từ khóa `using`

Cũng là đoạn code như trên, nhưng lúc này ta hãy sửa một chút như sau:

```cpp
#include <iostream>
using namespace std;

class Rectangle {
public:
    void Speak(const int &length, const int &width) {
        cout << "I am a Rectangle with: length = " << length << " and width = " << width << endl;
    }
};

class Square : public Rectangle {
public:
    using Rectangle::Speak; // Add this using
    void Speak(const int &length) {
        cout << "I am a Square with: length = " << length << endl;
    }
};

int main() {
    Square square;
    square.Speak(10);
    square.Speak(10, 5); // Now it's correct
    return EXIT_SUCCESS;
}
```

Hãy chạy thử và xem kết quả.

### Assignment operator

Như đã đề cập, toán tử gán = không được kế thừa từ Base class. Và dưới đây là cách để ta cài đặt toán tử gán = cho Derived class.

* Đầu tiên, trong `operator=` của Derived class ta phải gọi đến hàm `operator=` của Base class để gán những phần thuộc Base class vào object trước.
* Tiếp theo cài đặt phần gán cho những phần còn lại mà thuộc Derived class.

Nghe khó hiểu nhỉ, hãy xem ví dụ sau:

```cpp
class Person {
protected:
    string _name;
    string _address;
    int _age;
    bool _gender;

public:
    Person & operator=(const Person & person) {
        _name = person._name;
        _address = person._address;
        _age = person._age;
        _gender = person._gender;
        return *this;
    }
};

class Student : public Person {
private:
    string _id;
    double _gpa;

public:
    Student & operator=(const Student & student) {
        Person::operator=(student);
        _id = student._id;
        _gpa = student._gpa;
        return *this;
    }
};

class Worker : public Person {
private:
    double _salary;

public:
    Worker & operator=(const Worker & worker) {
        Person::operator=(worker);
        _salary = worker._salary;
        return *this;
    }
};
```

Như các bạn thấy, ở mỗi Derived class (`Student` và `Worker`) của Base class `Person`, trong hàm `operator=` của các Derived class ta sẽ gọi đến hàm `operator=` của Base class. Vì sao ta phải làm như vậy? Các bạn có thể không làm cũng được, nhưng hãy xem code trên, nếu các bạn không gọi đến `operator=` của Base class trong Derived class, có phải là bạn phải thêm đoạn code sau cho mỗi Derived class không:

```cpp
_name = person._name;
_address = person._address;
_age = person._age;
_gender = person._gender;
```

Vì mục đích của `operator=` là để gán giá trị, nên nếu như những thuộc tính `_name`, `_address`, `_age`, `_gender` thuộc Base class thì hãy gọi `operator=` của Base class để nó thực hiện gán những thuộc tính đó chứ ta đừng mất cài đặt lại việc gán của Base class làm gì cho mệt phải không.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-4-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://huaanhminh.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-Ph%E1%BA%A7n-4-).html" data-numposts="5"></div>