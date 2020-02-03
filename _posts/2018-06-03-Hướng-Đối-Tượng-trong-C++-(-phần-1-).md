<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

# NHỮNG NGUYÊN LÝ CƠ BẢN CỦA LẬP TRÌNH HƯỚNG ĐỐI TƯỢNG

Link phần 2: [Constructor và Destructor trong C++](https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-2-).html)

## Khái niệm cơ bản

### Lập trình hướng đối tượng là gì, có ăn được không?
Lập trình hướng đối tượng - Object Oriented Programming (OOP) là một phương pháp tư duy thiết kế cách viết chương trình trong lập trình, hay nói ngắn gọn là cách tổ chức và bố trí source code của chương trình.

### Vì sao ta phải học lập trình hướng đối tượng?
Hướng đối tượng là một mẫu hình thiết kế source code thông dụng nhất trên thế giới hiện nay khi viết các sản phẩm dịch vụ hay phần mềm như app, website,... Hầu như tất cả đều ứng dụng lập trình hướng đối tượng, trừ những nhóm sau:

* Lập trình nhúng (ứng dụng trong vi mạch điện tử).
* Lập trình cấp thấp (như viết kernel cho hệ điều hành).
* Lập trình hàm ứng dụng trong khoa học máy tính và AI (functional programming).

### Sơ đồ UML - Unified Modeling Language
UML là ngôn ngữ giúp ta mô hình hóa cách bố trí source code của ta trong dự án, giúp cho ta có cái nhìn toàn diện và bao quát về các khía cạnh trong dự án, ví dụ như hình sau:

![Example of UML](https://tapnhamblog.github.io/_img/UML_example.png)

## Thế lập trình hướng đối tượng là như thế nào?

Trong lập trình hướng đối tượng, ta nhìn mọi sự vật, hiện tượng dưới dạng các đối tượng, ví dụ như chiếc xe là đối tượng, căn nhà là đối tượng, thời tiết là đối tượng,...

Một đối tượng sẽ bao gồm 2 thành phần là các thuộc tính (attributes) và những hành vi (behaviours), trong đó:

* Thuộc tính: những gì tạo nên đặc điểm của đối tượng đó, như chiếc xe thì có những thuộc tính như màu sắc, giá tiền, kích thước, mã hiệu,... Khi biểu diễn trong các ngôn ngữ lập trình thì thuộc tính sẽ là các biến giá trị.
    ```cpp
    double price;
    string name;
    // ...
    ```
* Hành vi: những gì mà đối tượng đó có thể thực hiện được hay có thể làm được, như chiếc xe thì có thể chạy, có thể rẽ trái phải, có thể dừng,... Khi biểu diễn trong các ngôn ngữ lập trình thì hành vi sẽ là các hàm, và trong hướng đối tượng thì chúng được gọi là phương thức (method).
    ```cpp
    void Run() { }
    void Stop() { }
    ```

Dưới đây là tư duy cơ bản nhất về hướng đối tượng mà ai học cũng phải biết:

> Trong hướng đối tượng, ta chỉ quan tâm đối tượng đó có thể làm được những gì chứ không quan tâm nó việc đó thế nào.

Ví dụ thực tế: khi bạn bật máy tính, thì những gì các bạn quan tâm là cái máy tính sẽ được bật khi các bạn nhấn vô nút khởi động chứ đâu phải là bên trong máy tính diễn ra những gì khi nhấn nút.

Giải thích bằng code: giả sử chúng ta có một chuỗi và bây giờ ta muốn xóa 1 phần tử ở đầu chuỗi thì sẽ làm thế nào trong phương pháp lập trình thủ tục?

```cpp
char name[] = "Hua Anh Minh";
for (int i = 0; i < strlen(name) - 1; ++i)
    name[i] = name[i + 1];
```

Nhưng cách làm đó là chúng ta đang quan tâm đến việc nó thực hiện như thế nào, còn nếu ta dùng theo tư duy của hướng đối tượng thì sẽ như sau:

```cpp
string name = "Hua Anh Minh";
name.erase(0, 1);
```

Đúng vậy, ta chỉ cần biết là khi gọi cái hàm/phương thức erase như thế thì kí tự đầu chuỗi sẽ được xóa chứ không cần biết nó xóa thế nào.

Vậy nếu ta tóm tắt về tư duy hướng đối tượng thì ta sẽ có câu: <b>đừng lo về chuyện không phải của mình.</b>

## Encapsulation - Tính đóng gói

Một đối tượng ta có thể xem như là một chiếc hộp vậy, và ta sẽ đóng gói bên trong nó như sau:

* Sâu bên trong chiếc hộp, nơi mà ta không chạm vào được sẽ là thuộc tính, ta gọi việc này là data hiding (sẽ nói thêm bên dưới) - ẩn dữ liệu. Nhưng có một lưu ý: thuộc tính không nhất thiết phải ẩn đi, cái đó tùy thuộc vào bạn thiết kế ra sao theo vấn đề được đưa ra.
* Bao xung dữ liệu, phần mà ta có thể tác động được là hành vi. Một lưu ý nữa: đôi khi hành vi cũng có thể phải ẩn đi, cũng giống như các bạn đâu có ai thay đồ giữa công cộng phải không.

### Data hiding

Một cách dễ hiểu khi nói về ẩn dữ liệu của đối tượng đó là giống như tiền đặt trong két sắt thì phải được ẩn đi và để lấy được tiền thì phải thông qua những phương thức đặc biệt đó là phải mở khóa được két sắt vậy.

## Khái niệm - từ khóa `Class` trong C++

Trong các ngôn ngữ lập trình, để thể hiện chiếc “hộp” mang những đặc tính của đối tượng thì ta sẽ diễn tả dưới dạng `class`, ta có thể gọi đơn giản là tạo ra một kiểu dữ liệu mô tả chiếc hộp đó. Chúng ta khai báo một biến mang kiểu dữ liệu đó, thì biến đó được xem như là một đối tượng, ta gọi việc này là thực thể/cụ thể hóa (instance). Ví dụ ta có một lớp `HocSinh`, ta khai báo một biến là `StudentA` mang
kiểu `HocSinh`, thì lúc này biến `StudentA` đó được xem là đối tượng thể hiện cho lớp `HocSinh` đó.

Vậy bây giờ ta sẽ tóm gọn lại những gì cơ bản nhất về tư duy hướng đối tượng như sau: giả sử ta có một lớp `Rectangle` với những thông tin sau:

* Chiều dài, chiều rộng là thuộc tính của hình chữ nhật, và ta không thể tùy tiện truy cập vào chúng được (do độ dài phải lớn hơn 0) nên ta phải ẩn chúng đi => data hiding.
* Tính chu vi, Tính diện tích, kiểm tra hình vuông là những phương thức của hình chữ nhật, ta hoàn toàn có thể truy cập vào chúng nên không cần ẩn đi.
* Khi ta gọi lớp hình chữ nhật với các phương thức trên, ta chỉ cần biết là khi gọi phương thức tính chu vi thì các hình chữ nhật nó sẽ ném vào mặt bạn
chu vi của nó chứ bạn không cần biết là nó tính bằng cách gì => tư duy của hướng đối tượng.
* Tổng hợp thuộc tính và phương thức => tính đóng gói – encapsulation.

## Từ lý thuyết dài dòng đến thực hành

Ta sẽ lấy ví dụ class `Rectangle` trên để thực hành.

1. Đầu tiên ta sẽ khai báo lớp bằng cú pháp `class <tên_lớp> {};` như sau: `class Rectangle {};`.
2. Tiếp theo là các thuộc tính chiều dài và chiều rộng, do các thuộc tính này cần phải được ẩn nên ta dùng từ khóa `private` như thế này:
    ```cpp
    private:
        double _length, _width;
    ```
    Ở đây, `private` có nghĩa là riêng tư, khi ta khai báo một thuộc tính với phạm vi `private` thì thuộc tính đó không thể nhìn thấy từ bên ngoài được, như hình sau:
    
    ![Error access to private member](https://tapnhamblog.github.io/_img/error_access_to_private_member.PNG)

    Và tất nhiên, trái ngược với `private` là `public`, khi khai báo `public` thì ta có thể dễ dàng truy cập vào những gì đã được `public` từ bên ngoài class.
3. Nếu ta muốn lấy giá trị của chiều dài hay thay đổi giá trị chiều dài mà ta đã ẩn nó đi thì phải làm sao? Thông qua 2 phương thức getter và setter:

    ```cpp
    double getLength() {
        return _length;
    }
    ```

    `getLength` là một getter, nó sẽ cho ta biết được giá trị hiện tại của chiều dài.

    ```cpp
    bool setLength(double length) {
        if (length <= 0) {
            _length = 1;
            return false;
        }

        _length = length;
        return true;
    }
    ```

    `setLength` là một setter, ta sẽ dùng nó để gán giá trị cho chiều dài. Như mình đã nói, độ dài bắt buộc phải lớn hơn 0, nên nếu ta để thuộc tính chiều dài và chiều rộng dưới dạng `public` thì lỡ như người khác gán 0 vào thì sẽ vô lí, do đó ta cần phải ẩn nó đi và chỉ được phép thay đổi thông qua setter như trên. Khi dùng setter như thế, nếu người dùng gán giá trị bé hơn hoặc bằng 0 cho chiều dài, thì lập tức chiều dài sẽ được gán bằng 1 và hàm trả về `false` là sai nhằm thông báo cho người dùng.

    Các bạn hãy thử tự làm 2 phương thức getter và setter cho chiều rộng.
4. Khai báo đối tượng
    ```cpp
    Rectangle newRectangle;

    newRectangle.setLength(10);
    newRectangle.setWidth(5);

    cout << "The rectangle's perimeter: " << newRectangle.Perimeter() << endl;
    ```

    Ở dòng đầu tiên, ta khởi tạo đối tượng `newRectangle` có kiểu là `Rectangle`. Tiếp đến, ta sẽ gán 10 và 5 vào cho chiều dài và rộng thông qua 2 phương thức setter. Và cuối cùng là xuất ra chu vi.

    Do các bạn code toàn bộ class `Rectangle` nên chưa thấy rõ tư duy của hướng đối tượng bên trong nó. Bây giờ các bạn tưởng tượng cái class `Rectangle` này không phải do các bạn viết mà là một ai đó. Trong class `Rectangle` này có phương thức tính chu vi, và nếu các bạn muốn tính chu vi thì chỉ cần gọi cái phương thức tính chu vi chứ không cần biết là chu vi sẽ được tính như thế nào bên trong phương thức đó. Tư duy hướng đối tượng là như thế.

5. Toàn bộ source code các bạn có thể check ở [đây](https://github.com/1753070/Rectangle).

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-1-).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/03/H%C6%B0%E1%BB%9Bng-%C4%90%E1%BB%91i-T%C6%B0%E1%BB%A3ng-trong-C++-(-ph%E1%BA%A7n-1-).html" data-numposts="5"></div>