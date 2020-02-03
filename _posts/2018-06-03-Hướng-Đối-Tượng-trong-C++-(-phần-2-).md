<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

# KHỞI TẠO VÀ HỦY - CONSTRUCTOR AND DESTRUCTOR

Link phần 1: [Những nguyên lý cơ bản của lập trình hướng đối tượng](https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-1-).html)

Link phần 3: [Operator Overloading in C++](https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-3-).html)

## Phương thức khởi tạo - Constructor

<b>Tại sao ta lại cần phương thức khởi tạo?</b> Giả sử ta có một class như sau:

```cpp
class Array {
    int *_arr;
    int _size;
};
```

Và khi ta khai báo một đối tượng - object có kiểu là `Array`, thì giá trị mang trong 2 thuộc tính `_arr` và `_size` là giá trị rác và không xác định được, nên nếu có ai đó dùng nhầm thì sẽ gây nguy hiểm như truy cập vào vùng nhớ khác một cách bất hợp pháp hay là sai kết quả. Trong tình huống đó, nếu như trước giờ ta vẫn hay làm thì ta sẽ viết một phương thức "khởi tạo" thủ công giá trị ban đầu cho thuộc tính như sau:

```cpp
public:
    void Init() {
        _arr = nullptr;
        _size = 0;
    }
```

Phương thức thì viết đúng rồi nhưng vẫn còn 1 vấn đề: nếu người dùng quên gọi `Init` thì sao, như thế thì cũng không khác gì ban đầu là giá trị vẫn chưa được khởi tạo phải không. Vậy giải pháp được đặt ra lúc này là ra cần một phương thức khởi tạo sao cho phương thức đó có thể tự động thực thi ngay khi ta vừa khai báo một đối tượng. Đó chính là Constructor.

## Đặc điểm và cách cài đặt phương thức khởi tạo - Constructor

Trong C++ và những ngôn ngữ hỗ trợ hướng đối tượng khác đều có phương thức khởi tạo này, gọi là constructor, và ta khai báo nó như sau:

```cpp
class Array {
    int *_arr;
    int _size;

public:
    Array() {
        _arr = nullptr;
        _size = 0;
    }
};

```

Các bạn có nhận thấy điều gì đặc biệt không? Đúng vậy, đó là:

* Tên của phương thức khởi tạo giống với tên của class.
* Không có kiểu trả về (khác với `void` là trả về kiểu rỗng).

Và mỗi khi ta tạo một đối tượng có kiểu là `Array` thì ngay lập tức phương thức khởi tạo `Array` sẽ được tự động thực thi cho ta nên ta không cần phải gọi đến nó.

Một điểm lưu ý nữa là ta có thể tạo nhiều phương thức khởi tạo, miễn sao nó khác tham số truyền vào là được. Khi tạo một đối tượng mới thì trình biên dịch sẽ dựa vào tham số ta truyền vào để gọi phương thức khởi tạo tương ứng. Đây gọi là overloading function - nạp chồng phương thức / hàm. Ví dụ như sau:

```cpp
Array() {
    _arr = nullptr;
    _size = 0;
}

Array(const int &size) {
    _size = (size > 0) ? size : 0;
    if (_size > 0)
        _arr = new int[_size];
    else
        _arr = nullptr;
}

```

Như vậy, nếu ta khai báo đối tượng bằng cú pháp `Array array;` thì trình biên dịch sẽ gọi đến phương thức khởi tạo đầu tiên; còn nếu ta khai báo bằng cú pháp `Array array(100);` thì lúc này trình biên dịch sẽ gọi đến phương thức khởi tạo thứ hai.

Lưu ý rằng, có 3 loại phương thức khởi tạo sau thường được dùng:

* Khởi tạo mặc định - default constructor: là phương thức khởi tạo không có tham số đầu vào (như phương thức khởi tạo đầu tiên của class `Array` phía trên). Nếu người lập trình viên không khai báo bất kỳ một phương thức khởi tạo nào thì trình biên dịch sẽ ngầm tạo một phương thức khởi tạo rỗng lúc khai báo đối tượng.

    <b>Lưu ý</b>: khi người lập trình viên khai báo bất kỳ một phương thức khởi tạo nào cho lớp thì trình biên dịch sẽ không tự tạo ngầm phương thức khởi tạo mặc định rỗng nữa.
* Phương thức khởi tạo có một hay nhiều tham số: là phương thức khởi tạo có các tham số truyền vào.

    Trong phương thức khởi tạo có tham số thì có một loại phương thức khởi tạo khá đặc biệt:
    * Phương thức khởi tạo sao chép - copy constructor: được dùng để tạo ra một đối tượng từ đối tượng có sẵn, giống như sao chép ấy.

```cpp
Array() { // default constructor with no argument
    _arr = nullptr;
    _size = 0;
}

Array(const Array &origin) { // copy constructor
    _size = origin._size;
    if (_size > 0) {
        _arr = new int[_size];
        for (int i = 0; i < _size; ++i)
            _arr[i] = origin._arr[i];
    } else {
        _arr = nullptr;
    }
}

Array(const int &size) { // constructor with argument(s)
    _size = (size > 0) ? size : 0;
    if (_size > 0)
        _arr = new int[_size];
    else
        _arr = nullptr;
}

```

Hãy chú ý kỹ ở phương thức khởi tạo sao chép - copy constructor: ta sẽ dùng nó với cú pháp như sau:

```cpp
Array array(100);
Array secondArray(array); /* declare an object using copy constructor */
```

Khi ta khai báo object `array(100)`, lúc này trình biên dịch sẽ dùng phương thức khởi tạo có tham số (phương thức khởi tạo thứ 3 trong class `Array` phía trên), và tạo ra một đối tượng có `_size = 100` và `_arr` có 100 phần tử. Sau đó, ta tiếp tục khai báo object `secondArray(array)`, lúc này trình biên dịch sẽ dùng phương thức khởi tạo sao chép (phương thức khởi tạo thứ 2 trong class `Array` phía trên), gán `_size` của array vào `_size` của `secondArray` (100) và tạo ra `_arr` của `secondArray` với 100 phần tử với giá trị của các phần tử trong `secondArray` giống với trong array. Lưu ý rằng phương thức khởi tạo sao chép chỉ có tác dụng sao chép nội dung trong các thuộc tính của đối tượng chứ không sao chép địa chỉ của chúng (shallow copy). Mình sẽ giải thích một chút ở phần này: nếu ta không viết copy constructor thì trình biên dịch vẫn cung cấp cho ta copy constructor mặc định, nhưng đôi khi nó sẽ sai, cụ thể trong trường hợp nếu object A của ta có con trỏ và đang được cấp phát động, thì khi dùng copy constructor mặc định của trình biên dịch để sao chép A cho object B, nó chỉ copy địa chỉ mà con trỏ đó nắm giữ và lúc này cả con trỏ trong object A và con trỏ trong object B đều nắm chung một vùng nhớ, dùng A để thay đổi vùng nhớ đó cũng sẽ có tác động lên B và như thế là sai vì cái ta cần sao chép là nội dung của vùng nhớ đó chứ không phải là địa chỉ của nó.

## Phương thức hủy - Destructor

<b>Tại sao ta lại cần phương thức hủy?</b> Giả sử ta có đoạn code sau:

```cpp
class Array {
    int *_arr;
    int _size;

public:
    Array(const int &size) { // constructor with argument(s)
        _size = (size > 0) ? size : 0;
        if (_size > 0)
            _arr = new int[_size];
        else
            _arr = nullptr;
   }
};

int main() {
    Array array(100);
}

```

Chú ý kỹ thì ta sẽ thấy khi tạo object `array` tức là ta đã cấp phát một mảng 100 phần tử cho thuộc tính `_arr` trong class `Array`. Vậy lúc này khi chương trình kết thúc, đối tượng `array` mất đi thì 100 phần tử ta đã cấp phát sẽ lạc trôi ở đâu? Do đó ta cần phải có 1 phương thức giúp ta tự động dọn dẹp những gì chúng ta đã "bày ra" bên trong đối tượng, đó là phương thức hủy - Destructor.

## Đặc điểm và cách cài đặt phương thức hủy - Destructor

Phương thức hủy trong C++ có những đặc điểm quan trọng sau:

* Cũng giống như phương thức khởi tạo, phương thức hủy có tên giống với tên lớp nhưng có thêm dấu `~` đặt ở ngay phía trước.
* Và cũng như phương thức khởi tạo, phương thức hủy không có kiểu trả về.
* Phương thức hủy không có tham số.
* Mỗi lớp chỉ có duy nhất 1 phương thức hủy, do đó ta không được phép nạp chồng phương thức như phương thức khởi tạo.
* Nếu phương thức khởi tạo là tự động chạy khi ta tạo đối tượng thì phương thức hủy sẽ là tự động thực thi khi đối tượng "hết vòng đời" (hết phạm vi sử dụng).
* Trong quá trình sống của đối tượng có và chỉ có 1 lần phương thức hủy được thực hiện. Giống như ta chỉ chết có 1 lần vậy.

Lưu ý rằng không phải lúc nào cũng cần tạo phương thức hủy, ví dụ như trong trường hợp các lớp không xin cấp phát sử dụng tài nguyên trên bộ nhớ heap của hệ thống thì ta không cần phải viết phương thức hủy.

```cpp
class Array {
    int *_arr;
    int _size;

public:
    Array(const int &size) {
        _size = (size > 0) ? size : 0;
        if (_size > 0)
            _arr = new int[_size];
        else
            _arr = nullptr;
    }

    ~Array() {
        if (_size > 0)
            delete[] _arr;
    }
};
```

Như vậy, nếu ta khai báo như sau:

```cpp
{
    Array array(100);
}
```

Khi khai báo như thế, lúc chương trình chạy ra khỏi block `{ }` thì cũng là lúc đối tượng `array` kết thúc vòng đời (do nó là local variable), và khi đó phương thức hủy sẽ được tự động thực thi.

Code tham khảo tại [đây](https://github.com/1753070/Array_Cpp)

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-2-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-2-).html" data-numposts="5"></div>