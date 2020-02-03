<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

Design Pattern là một chủ đề rất khó trong hướng đối tượng và có thể xem nó như là đỉnh cao nhất của hướng đối tượng. Đa số các bạn khi mới tiếp xúc về nó thì thường sẽ dễ bị choáng ngộp và cảm thấy rất khó hiểu vì lượng kiến thức về design pattern là rất nhiều. Nếu mà nói chi tiết về design pattern thì nói cả buổi cũng chưa xong đâu. Nhưng, đừng vì thấy nó khó quá mà bỏ cuộc vì nếu nắm được design pattern thì kĩ năng viết code của bạn, đặc biệt là khi code hướng đối tượng trong thực tế sẽ khá hơn rất nhiều.

Trong bài viết này thì mình sẽ đi qua 2 phần đó là giới thiệu tổng quan về design pattern và hướng dẫn về mẫu Singleton trong design pattern.

## Design pattern là gì

Nếu nói rõ ràng nhất thì design pattern chính là một tập hợp các phương pháp (hay có thể gọi là mẫu - pattern) thiết kế các source code của ta sao cho rõ ràng, dễ hiểu, dễ bảo trì, dễ sửa chữa, dễ nâng cấp. Tất nhiên là trên thực tế thì dù hiện nay chúng ta có khá nhiều mẫu trong bộ design pattern nhưng vẫn chưa có một phần mềm hay hệ thống nào đáp ứng toàn bộ tiêu chí trên cả.

> No programming technique solves all problems. No programming language produces only correct results. No programmer should start each project from scratch.

Design pattern sẽ được chia thành 3 nhóm chính sau và mỗi nhóm sẽ có nhiều pattern nhỏ bên trong:

* Creational patterns: là bộ tập hợp những pattern giúp cho việc khởi tạo các đối tượng. Trước giờ ta chỉ biết rằng khi khởi tạo đối tượng thì constructor sẽ được gọi thế này thế kia, nhưng các dự án thực tế thì không đơn giản như những bài tập trên trường, không phải ta cứ muốn khởi tạo kiểu như `Student student;` là được vì nếu như cứ khởi tạo vô tội vạ như thế thì nát cả hệ thống.
* Structural patterns: là bộ tập hợp những pattern giúp ta thiết kế cấu trúc bên trong các class và mối liên kết giữa chúng sao cho hợp lý như nên dùng quan hệ nào giữa HAS-A, IS-A, friend,... nhằm giúp cho hệ thống ít bị kết dính nhất có thể. Hệ thống kết dính có thể nói đơn giản là như một bức tường với các viên gạch kết dính với nhau, khi bạn đập bức tường thì có khả năng toàn bộ bức tường sẽ đổ xuống, giống như hệ thống mà kết dính mà một phần bên trong chạy sai là cả hệ thống có nguy cơ sụp đổ theo.
* Behavioral patterns: như cái tên của nó, đây là bộ tập hợp những pattern mà chúng có liên quan đến hành vi của các đối tượng, hay nói rõ hơn là thiết kế cách thức mà các phương thức của đối tượng hoạt động sao cho hợp lý.

Bất kì pattern nào thì khi học ta đều phải áp dụng 3 bước sau:

1. Vấn đề mà pattern đó giải quyết.
2. Sơ đồ UML minh họa. Đa số các bạn bỏ qua bước này nhưng trên thực tế khi làm sản phẩm thì việc vẽ UML là khâu đầu tiên và quan trọng nhất vì nó sẽ quyết định nên cấu trúc của hệ thống chúng ta và nếu vẽ UML không tốt thì hệ thống cũng sẽ nát theo.
3. Code minh họa.

Khi học về design pattern thì ta không những phải hiểu nó mà còn cần phải biết cách khéo léo áp dụng nó vào project của ta một cách hợp lý để đạt được hiệu quả tối đa.

Anh Hoàng bên blog [Tôi Đi Code Dạo](https://toidicodedao.com/2016/03/01/nhap-mon-design-pattern-phong-cach-kiem-hiep/) đã nói như sau về việc học design pattern:

> Kẻ sĩ dùng design pattern cũng chia làm ba cảnh giới. Kẻ sơ nhập thì nhìn đâu cũng thấy pattern, chỉ lo áp dụng, nhét rất nhiều pattern vào mà không quan tâm đến thiết kế. Lăn lộn giang hồ một thời gian, đến cảnh giới cao thủ, sẽ học được rằng khi nào cần dùng pattern, khi nào không. Đến cấp bậc đại sư, chỉ dùng pattern khi đã rõ lợi hại của nó, biết lấy sự đơn giản hài hòa của design tổng thể làm trọng. Có thể tổng kết quá trình này bằng một câu: "Khi chưa học đạo, ta thấy núi là núi, sông là sông. Khi mới học đạo, ta thấy núi không phải là núi, sông không phải là sông. Sau khi học đạo, ta lại thấy núi chỉ là núi sông chỉ là sông."

## Singleton pattern

Đây thường là một trong những pattern đầu tiên mà trường học thường dạy vì nó có phần ít trừu tượng hơn các pattern khác và giúp cho người học dễ dàng hình dung ra nó là gì và như thế nào. Ý mình ở đây chỉ là dễ hiểu thôi chứ áp dụng thì chả có pattern nào dễ áp dụng đâu nhé vì trên thực tế khi làm việc ta thường phải áp dụng nhiều pattern vào trong dự án chứ không phải chỉ có 1 cái.

### Bài toán mà Singleton giải quyết

Singleton thuộc vào nhóm Creational pattern. Nói đến đây thì chắc bạn cũng đã biết nó dùng để thiết kế cho việc khởi tạo đối tượng. Vậy cụ thể là như thế nào?

Hãy tưởng tượng mỗi lần ta khởi động máy tính, log in vào Windows là việc khởi tạo một đối tượng. Hãy thử nghĩ xem chuyện quái gì sẽ xảy ra nếu ta khởi tạo nhiều đối tượng, hay nói đúng hơn là ta log in vào 1 máy tính nhiều lần 1 lúc, sẽ rất là dị nếu như trên một cái máy tính đang có nhiều account đăng nhập một lúc phải không nào, chả ai biết nó sẽ xảy ra chuyện gì nữa. Vậy để tránh những vấn đề quái dị như trên, ta cần phải có biện pháp sao cho ta chỉ có thể tạo ra duy nhất 1 đối tượng và không thể tạo thêm nhiều đối tượng khác có cùng kiểu dữ liệu đó cho toàn hệ thống, giống như khi ta mở máy tính thì ta chỉ có thể log in vô một account ngay lúc đó mà thôi.

### Mô hình UML

Mô hình UML của singleton có lẽ là đơn giản nhất trong tất cả vì nó chỉ có như sau mà thôi:

![singleton UML](https://tapnhamblog.github.io/_img/singleton.png)

Giải thích:

* class của ta sẽ có tên là `Singleton` và đây là sẽ là class với nội dung sẽ là dạng tĩnh - `static`. Lí do ta phải đặt tĩnh thì hãy xem bên dưới.
* `instance` sẽ là một biến con trỏ kiểu Singleton nhằm mục đích để kiểm tra xem có đối tượng kiểu Singleton nào đã được tạo chưa. Nếu ta chưa tạo bất kì đối tượng kiểu Singleton nào thì biến instance này sẽ có giá trị là `nullptr`. Nếu ta đã từng tạo ra một đối tượng kiểu Singleton rồi thì biến instance này sẽ trỏ đến vùng nhớ của đối tượng đó. Biến instance này bắt buộc phải là `static`, vì nó dùng để kiểm tra xem đã có đối tượng nào được tạo ra hay chưa. Nếu ta không đặt `static` thì khi ta tạo ra đối tượng mới, `instance` sẽ có giá trị mới (do nó không tĩnh) nó sẽ trỏ sang vùng nhớ nào đó khác chứ không phải là vùng nhớ của đối tượng cũ và như vậy thì sẽ có 2 đối tượng được tạo ra. Do đó ta cần phải đặt `instance` là `static` để vùng nhớ của `instance` là duy nhất để kiểm soát đối tượng.
* `getInstance` sẽ dùng để tạo ra 1 đối tượng duy nhất kiểu `Singleton`, tức là dù nếu như ta gọi hàm này nhiều lần thì hàm vẫn sẽ trả ra con trỏ và trỏ đến đúng 1 vùng nhớ duy nhất kiểu `Singleton`. Lưu ý rằng `getInstance` cũng sẽ là một hàm `static`. Lí do là vì nếu ta không đặt `static` thì ta cần phải khai báo đối tượng thông qua constructor, và việc khai báo thông qua constructor thì sẽ tạo ra nhiều đối tượng khác nhau và sai quy tắc của Singleton.
* `Singleton`: constructor của các class cần áp dụng Singleton bắt buộc phải đặt ở `private`. Nếu ta đặt ở `public` thì ta sẽ có thể gọi nhiều lần ở bên ngoài dẫn đến việc có nhiều đối tượng được tạo ra và sai quy tắc của Singleton.

### Code mẫu

Code Singleton thì không có gì quá phức tạp, vài dòng là xong ngay:

```cpp
class Singleton {
public:
    static Singleton * GetInstance();
private:
    static Singleton * _instance;

    Singleton() = default;

    ~Singleton() = default;
};

Singleton * Singleton::_instance = nullptr;

Singleton * Singleton::GetInstance() {
    // nếu như chưa có đối tượng nào được tạo thì tạo ra đối tượng mới.
    if (!_instance)
        _instance = new Singleton;
    return _instance;
}

int main() {
    Singleton *firstInstance = Singleton::GetInstance();
    cout << &*firstInstance << endl;
    Singleton *secondInstance = Singleton::GetInstance();
    cout << &*secondInstance << endl;
    delete 
}
```

Sau khi chạy code trên thì ta sẽ thấy dù ta gọi hàm `GetInstance` những 2 lần nhưng địa chỉ vùng nhớ mà con trỏ chúng trỏ đến là giống nhau, tức là chỉ có 1 đối tượng duy nhất được tạo ra.

Tuy nhiên, code trên vẫn còn thiếu sót: các bạn hãy bổ sung 2 dòng code sau vào hàm `main` và chạy lại:

```cpp
Singleton *thirdInstance = new Singleton(*firstInstance);
cout << &*thirdInstance << endl;
```

Kết quả khi chạy là địa chỉ vùng nhớ mà `thirdInstance` trỏ đến khác hoàn toàn với với 2 địa chỉ trên, tức là đã có đối tượng thứ 2 được tạo ra.

Để khắc phục điều này thì ta cần ngăn chặn việc trình biên dịch tạo ra các hàm như copy constructor, assignment operator một cách tự động, và ta sẽ làm điều đó với từ khóa `= delete`;

`= delete` là một từ khóa đặc biệt trong C++. Nó ngăn chặn trình biên dịch không tạo ra các hàm có từ khóa `= delete` một cách tự động. Ví dụ khi ta dùng `= delete` cho copy constructor thì nó có nghĩa là ta thông báo cho trình biên dịch biết ta muốn xóa hàm copy constructor nên đừng tạo ra nó nữa. Và nếu trình biên dịch không tạo ra được copy constructor cũng như assignment operator thì sẽ không gặp lỗi như trên.

```cpp
class Singleton {
public:
    static Singleton * GetInstance();
private:
    static Singleton * _instance;

    Singleton() = default;

    ~Singleton() = default;

    Singleton(const Singleton &) = delete;

    Singleton & operator=(const Singleton &) = delete;
};
```

Và như vậy là ta đã giải quyết được lỗi trên.

Lí do mình viết không có giải phóng vùng nhớ là vì khi áp dụng Singleton ta thường dùng với các hệ thống chạy liên tục không ngừng nghỉ và đối tượng Singleton sẽ chạy suốt như thế. Chính vì thế mà việc giải phóng vùng nhớ là không cần thiết do ta chỉ hủy đối tượng khi tắt hệ thống đi, mà khi tắt hệ thống thì nó tự reset ram luôn rồi. Tuy nhiên nếu các bạn muốn giải phóng thì có thể bổ sung hàm sau vào class `Singleton`:

```cpp
static void DeleteInstance() {
    if (!_instance)
        delete _instance;
    _instance = nullptr;
}
```

Và cuối chương trình ta gọi hàm `DeleteIntance` thôi.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/07/14/Design-Pattern-trong-C++-(Ph%E1%BA%A7n-1).html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/07/14/Design-Pattern-trong-C++-(Ph%E1%BA%A7n-1).html" data-numposts="5"></div>