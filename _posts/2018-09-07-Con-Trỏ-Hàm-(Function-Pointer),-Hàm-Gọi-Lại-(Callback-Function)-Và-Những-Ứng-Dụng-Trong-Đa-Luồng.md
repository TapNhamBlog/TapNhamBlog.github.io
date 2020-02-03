<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

## Khái niệm về con trỏ hàm

Nhắc lại một chút về kiến thức phần bộ nhớ của chương trình khi thực thi (tháp phân vùng bộ nhớ) với 6 tầng sau:

1. Stack: đây là bộ nhớ do hệ thống quản lí, người lập trình không được phép can thiệp trực tiếp vào. Khi ta tạo một biến trong một khối lệnh nào đó (như khối lệnh `if – else`, khối lệnh `for`, `while`, `do – while`,…) hay tạo biến trong hàm (kể cả hàm `main`) thì bộ nhớ của biến đó sẽ nằm trong bộ nhớ stack.
2. Unused memory: đây là phần bộ nhớ rỗng nằm giữa 2 tầng bộ nhớ stack và heap. Phần bộ nhớ này có công dụng bổ sung thêm bộ nhớ cho 2 tầng stack và heap trong một số trường hợp cần thiết như thiếu bộ nhớ.
3. Heap: đây là tầng bộ nhớ dành cho việc cấp phát động, do lập trình viên tự quản lí. Khi ta tạo một con trỏ và dùng cấp phát động thì vùng nhớ mà con trỏ đó trỏ đến là nằm trong vùng nhớ Heap.
4. BSS và GVAR: 2 tầng vùng nhớ cho biến toàn cục, giữa BSS và GVAR có vài điểm khác nhau nhưng mình sẽ không đề cập trong bài này.
5. Text: đây chính là vùng nhớ ta sẽ dùng trong bài viết này, được gọi là vùng nhớ ảo. Khi ta biên dịch chương trình, mã nguồn của ta, cụ thể là các hàm, các dòng lệnh sẽ được nạp lên RAM để dùng khi cần thiết, và vùng nhớ dùng để nạp lên đó là vùng nhớ Text này, hay còn được gọi là Code Segment.

### Thế con trỏ hàm là gì?

Đơn giản thôi, đó là con trỏ mà nó trỏ đến địa chỉ của một hàm mà địa chỉ đó nằm trên Text. Mỗi một hàm sẽ có cho mình một địa chỉ như biến vậy, ví dụ để kiểm tra địa chỉ hàm main thì ta có thể làm như sau:

```cpp
int main() {
    std::cout << main << std::endl;
}
```

Trông có vẻ lạ nhỉ, nhưng nếu ta xem xét kĩ thì ta sẽ nhận ra ngay đó là tên hàm chính là con trỏ hàm, do đó khi ta dùng lệnh như trên thì ta sẽ xuất ra địa chỉ của con trỏ hàm đó. Trên thực tế thì các bước khi chạy một chương trình như sau:

1. Chạy chương trình từng dòng lệnh.
2. Gặp lời gọi hàm, hệ điều hành sẽ tìm đến địa chỉ của hàm dựa vào tên hàm đó (do tên hàm cũng là một con trỏ nên nó có địa chỉ).
3. Hệ điều hành chuyển mã lệnh của hàm vào CPU tiếp tục xử lí.
4. Quay lại bước 1.

### Cú pháp khai báo con trỏ hàm

Cú pháp khai báo con trỏ hàm khá khác biệt so với cách khai báo thông thường. Giả sử ta có hàm `Swap` như sau:

```cpp
void Swap(int &first, int &second) {
    int temp = first;
    first = second;
    second = temp;
}
```

Thế bây giờ nếu ta muốn khai báo một con trỏ đến hàm `Swap` thì sao? Ta có cú pháp tổng quát sau:

```
<return_type> (*<name_of_pointer>)(<data_type_of_parameter>) = <function_name>;
```

Trông khó hiểu nhỉ, ta hãy phân tích thử xem:

* `return_type`: đây là kiểu trả về của hàm, ví dụ hàm `Swap` có kiểu trả về là `void` thì `return_type` ở đây là `void`.
* `(*<name_of_pointer)`: tên con trỏ hàm bạn muốn đặt, ví dụ như ở đây mình sẽ gọi là `*pSwap` cho dễ hình dung.
* `(<data_type_of_parameter>)`: đây là kiểu dữ liệu của tham số truyền vào hàm. Ví dụ như hàm `Swap` có 2 tham số `int &` thì ở đây mình sẽ ghi là `(int &, int &)`.

Vậy tóm lại biến của ta khi khai báo sẽ là:

```cpp
void(*pSwap) (int &, int &) = Swap;
```

Ơ nhưng mà mấy cái con trỏ hàm này thì làm quái gì, cứ gọi hàm như bình thường cũng được có phải không? Tất nhiên là được, nhưng, trong nhiều tình huống việc KHÔNG dùng con trỏ hàm như thế sẽ rất vất vả và khiến code nặng nề hơn, ví dụ như sau:

Giả sử ta muốn viết một hàm sắp xếp mảng số nguyên, nhưng: hàm đó phải tổng quát và sắp xếp được cả tăng dần và giảm dần.

Đơn giản thôi, để làm cách thông thường cho xem:

```cpp
// state: true -> ascending; state: false -> descending
void SortArray(int* &arr, const int &size, const bool &state) {
    for (int i = 0; i < size - 1; ++i)
        for (int j = i + 1; j < size; ++j)
            if (state && arr[i] > arr[j])
                Swap(arr[i], arr[j]);
            else if (!state && arr[i] < arr[j])
                Swap(arr[i], arr[j]);
}
```

Nếu làm cách thông thường, ta sẽ khá lệ thuộc vào biến `state` để kiểm tra xem là ta đang cần sắp tăng hay giảm, tức là phải tốn thêm bước kiểm tra và nếu với mảng lớn thì nó sẽ gây chậm chương trình. Chưa kể nếu đoạn code trên phần `if-else` dùng để kiểm tra `state` được mở rộng ra nhiều và nhiều hơn thì sẽ khiến hàm trở nên rất rối và khó debug. Vậy nếu ta dùng con trỏ hàm thì sao?

```cpp
void Ascending(int &first, int &second) {
    if (first > second)
        Swap(first, second);
}

void Descending(int &first, int &second) {
    if (first < second)
        Swap(first, second);
}

void SortArray(int* &arr, const int &size, void(*CheckSort)(int &, int &)) {
    for (int i = 0; i < size - 1; ++i)
        for (int j = i + 1; j < size; ++j)
            CheckSort(arr[i], arr[j]);
}
```

Vậy mỗi khi ta muốn sắp xếp mảng, ví dụ như tăng dần thì ta chỉ cần gọi

```cpp
SortArray(arr, size, Ascending);
```

Còn nếu là giảm dần thì sẽ gọi:

```cpp
SortArray(arr, size, Descending);
```

Và như thế ta không cần phải kiểm tra xem người dùng muốn xếp kiểu nào nữa, sẽ tiện hơn rất nhiều so với việc dùng if – else; và nếu về sau ta có mở rộng thêm nhiều kiểu sắp xếp nữa thì ta chỉ cần viết 1 hàm khác rồi gọi lại bên trong `SortArray`, code sẽ trở nên gọn gàng hơn và dễ debug hơn rất nhiều.

Một số bạn đọc đến đây chắc sẽ nghĩ nó cầu kì và dở hơi. Nhưng, đó là cách mà các hàm trong C++ hiện nay thực hiện. Hàm `SortArray` như trên là mình viết dựa trên hàm `sort` gốc của C++, và hàm `sort` của C++ thì dùng con trỏ hàm như thế chứ không phải do mình bịa ra đâu. [Link](www.cplusplus.com/reference/algorithm/sort).

Và với cách dùng con trỏ hàm như thế, ta có khái niệm sau:

## Callback function

Callback function chính là hành động khi ta đưa toàn bộ nội dung một hàm vào tham số hàm khác (bằng cách dùng con trỏ hàm như trên) để ta có thể sử dụng hàm đó với nhiều cách thức khác nhau.

Hơi khó hiểu nhỉ, mình sẽ lấy 1 ví dụ đơn giản: mấy cái app bạn hay dùng ấy, có các nút bấm với các chức năng khác nhau. Vậy làm thế nào hệ thống biết khi bấm nút nào thì sẽ thực hiện chức năng của nút đó, như bấm nút like thì sẽ thực hiện thao tác like?

* Cách thông thường: ta có một hàm bấm nút, với một loại nút bấm ta sẽ sử dụng if để bấm, và tức là nếu có vài chục cái nút như trên Facebook thì ta phải làm vài chục cái if và với mỗi if ta phải gọi hàm tương ứng để thực hiện chức năng, tức là gọi vài chục cái hàm. Code ta sẽ có dạng như thế này":

```cpp
if (...) {

}
else if (...) {

}
else if (...) {

}
else if (...) {

}
else if (...) {

}
...
```

* Cách thông minh, dùng callback function: ta có một hàm bấm nút, khi người dùng nhấn nút gì thì truyền cái hàm chức năng của nút đó vô hàm dưới dạng con trỏ hàm để hàm đó gọi 1 phát là xong ngay.

Nếu các bạn vẫn chưa hiểu rõ thì mình có thêm 1 ví dụ sau, bằng cách dùng đa luồng trong C++, cũng là ứng dụng mà bài viết này đề cập.

## Đa luồng trong C++

### Khái niệm về đa luồng

Hãy viết thử ứng dụng sau: In ra màn hình câu "This app is running" liên tục. Trong quá trình in ra như thế, nếu người dùng nhấn phím ESC thì dừng việc in lại và thoát chương trình.

Lúc này, khi nói đến việc nhấn ESC để dừng chương trình, các bạn có thể sẽ nghĩ ngay đến hàm `_getch()` có trong thư viện `conio.h` của C++. Nhưng, khi ta dùng hàm `_getch()`, chương trình sẽ dừng lại để chờ ta nhấn phím vào rồi mới thực hiện lệnh kế tiếp, tức là nếu như ta đang in ra màn hình câu "This app is running" thì khi gặp lệnh `_getch()` việc in sẽ bị tạm dừng lại và không đúng với yêu cầu của ta là phải in liên tục.

Giải pháp: ta sẽ áp dụng kĩ thuật đa luồng. Chương trình bình thường khi ta chạy là chạy trên 1 luồng duy nhất (luồng `main`), và được gọi là chạy đồng bộ (synchronously), tức là chương trình sẽ chạy từng lệnh một từ trên xuống dưới, hết lệnh này sẽ đến lệnh kế tiếp (có thể hiểu như hiệu ứng domino, khi quân cờ này đổ thì quân tiếp theo mới đổ xuống). Còn đa luồng thì sao, đa luồng tức là ta sẽ bổ sung thêm 1 luồng nữa song song với luồng `main`, các lệnh sẽ không chạy tuần tự như hiệu ứng domino mà sẽ chạy song song với nhau, và được gọi là chạy bất đồng bộ (asynchronously).

Nghe có vẻ khó hiểu nhỉ, mình sẽ lấy ví dụ cho bt trên: ta sẽ có 2 luồng, 1 luồng dùng để chạy lệnh in ra màn hình câu "This app is running", luồng còn lại sẽ chạy lệnh `_getch()` để lấy phím từ người dùng. Nếu chương trình gặp lệnh `_getch()` thì chỉ có luồng nào chứ lệnh `_getch()` mới bị tạm dừng để chờ người dùng nhập vào, còn luồng kia sẽ vẫn tiếp tục được thực thi.

### Dùng hàm callback trong đa luồng

Để dùng được cú pháp đa luồng thì ta bắt buộc phải dùng hàm callback:

```cpp
// thread <tên_luồng_mới>(<hàm_callback>);

thread subThread(callbackFunction);
```

Khi gọi lệnh trên, ta sẽ tạo ra một luồng mới tên là `subThread` và luồng đó sẽ chạy đoạn code bên trong hàm `callbackFunction`. Tại sao ta lại phải dùng hàm callback ở đó? Đơn giản thôi, vì mấy ông viết ra cú pháp `thread` làm quái gì biết bạn muốn dùng đa luồng với mục đích gì nên mấy ông đó đâu có cách nào để viết `if-else` được. Bắt buộc phải dùng callback để các bạn quăng hàm gì vào thì sẽ chạy ngay hàm đó chứ không cần phải `if-else`.

Hay như đại biểu quốc hội đi lấy ý kiến cử tri, mấy ông đại biểu đó làm gì biết mấy bạn muốn ý kiến gì nên mấy ông đó phải phát giấy ra cho các bạn điền ý kiến vào (tờ giấy tượng trưng cho hàm callback) và mấy ổng sẽ cầm tờ giấy đó để đi thực hiện những ý kiến đó (về lý thuyết là thế, về thực tế mấy ông đó có thực hiện hay không là việc khác).

Vậy đoạn code trên sẽ như sau:

```cpp
#include <iostream>
#include <thread>
#include <Windows.h>
#include <string>
#include <conio.h>
using namespace std;

bool STOP = false;

void Exit(thread * subProcess) {
	system("cls");
	STOP = true;
	subProcess->join();
}

void PrintNumber() {
	string message = "This app is running.";
	while (!STOP) {
		cout << message << endl;
		Sleep(1000);
	}
}

int main() {
	thread subProcess(PrintNumber);
	while (true) {
		if (_getch() == 27) {
			Exit(&subProcess);
			return EXIT_SUCCESS;
		}
	}
}
```

Các bạn đừng để ý đến hàm `Exit`, hàm đó đơn giản chỉ là để dừng việc hoạt động của luồng phụ.

Ở dòng `thread subProcess(PrintNumber);`, khi chương trình chạy lệnh đó tức là chương trình sẽ tạo ra luồng phụ tên là `subProcess` và chạy hàm `PrintNumber` phía trên.

Ở luồng `main`, chương trình sẽ thực hiện đoạn code trong vòng lặp `while (true)`, khi gặp lệnh `_getch()`, chỉ có luồng `main` là bị tạm dừng để chờ người dùng nhấn phím, còn luồng `subProcess` vẫn tiếp tục chạy - bất đồng bộ (asynchronously).

Khi người dùng nhấn phím ESC (mã ASCII là 27), luồng `main` sẽ thực hiện việc gọi hàm `Exit`, bên trong `Exit` thì biến toàn cục `STOP` sẽ được gán giá trị `true`, luồng `subProcess` vẫn đang chạy, khi nó lặp và kiểm tra thấy `STOP` là true thì nó dừng lại và sau khi thực hiện xong thì luồng `subProcess` sẽ được nhập lại với luồng `main` nhờ vào lệnh `join` ta đã gọi (lệnh `join` dùng để nhập một luồng phụ vào luồng `main`), trở về dạng đồng bộ (synchronously) và thoát chương trình.

### Ứng dụng của đa luồng

Trên thực tế các bạn gặp đa luồng hằng ngày mà không biết ấy. Ví dụ khi ta lướt Facebook, mỗi nút bấm trong đó có thể xem là luồng phụ, các nút đó cũng có một cơ chế riêng để chờ người dùng nhấn vào (có thể hiểu là giống như `_getch()` trong ví dụ trên). Nếu ta không dùng đa luồng thì khi chương trình chạy đến những nút đó, nó sẽ bị dừng lại, ta sẽ không thể lướt Facebook nữa và chỉ khi nào nhấn nút thì mới lướt được tiếp; còn nếu dùng đa luồng, mỗi nút đó là một luồng phụ, luồng chính là cái newsfeed của ta vẫn chạy, ta vẫn lướt Facebook được (giống như in ra "This app is running" liên tục như ví dụ trên).

## Đa luồng trong class

Các bạn có thể thấy đoạn code ví dụ trên mình viết là có dùng biến toàn cục, như thế là không tốt, ta có thể viết lại dưới dạng class:

```cpp
class A {
public:
	A() {
		thread subProcess(PrintNumber);
		while (true) {
			if (_getch() == 27) {
				Exit(&subProcess);
				return;
			}
		}
	}

	void Exit(thread * subProcess) {
		system("cls");
		STOP = true;
		subProcess->join();
	}

	void PrintNumber() {
		string message = "This app is running.";
		while (!STOP) {
			cout << message << endl;
			Sleep(1000);
		}
	}
private:
	bool STOP = false;
};

int main() {
	A a;
}
```

Nhưng khi ta chạy đoạn code trên thì nó sẽ bị lỗi:

> `A::PrintNumber`: non-standard syntax, use '&' to create a pointer to member.

Vì sao lại thế?

Thật ra, khi dùng đa luồng, các hàm callback ta truyền vào sẽ thường phải là hàm tĩnh. Các hàm tĩnh có một đặc điểm là địa chỉ của nó sẽ không bao giờ đổi trong suốt quá trình thực thi, nên trình biên dịch có thể dễ dàng truy xuất đến địa chỉ của nó. Hàm trong class thì lại khác, mỗi lần ta khai báo một đối tượng kiểu `A` mới thì địa chỉ hàm `PrintNumber` bên trong nó sẽ không hề giống với địa chỉ của bất kì hàm `PrintNumber` của các đối tượng kiểu `A` khác và trình biên dịch sẽ không biết là ta muốn truy xuất thế nào nên báo lỗi. Lúc này, ta sẽ phải dùng toán tử reference `&` và toán tử scope `::` để trình biên dịch biết là ta đang gọi đến `PrintNumber` bên trong `A`:

```cpp
thread subProcess(&A::PrintNumber);
```

Lúc này, trình biên dịch sẽ xem `PrintNumber` bên trong lời gọi `subProcess` như là một hàm tĩnh (do ta quăng địa chỉ vào rồi), và lúc này sẽ phát sinh vấn đề khác, nằm ở biến `STOP` bên trong `PrintNumber`. Chắc các bạn còn nhớ về hàm tĩnh trong class, đó là hàm tĩnh không được phép truy cập vào `this` của class, mà hàm `PrintNumber` lúc này đã được `subProcess` xem như là hàm tĩnh nên nó không thể truy cập vào thuộc tính `this->STOP` trong `A` được. Vậy ta khắc phục thế nào?

Lúc này, điều mà ta muốn đó là hàm `PrintNumber` có thể nhận dạng, hay "bắt" được sự thay đổi giá trị của thuộc tính `STOP` bên trong nó có phải không. Và, nếu ta muốn hàm `PrintNumber` "bắt" được thuộc tính nào đó thì nó cần phải biết là thuộc tính nó cần bắt tên là gì, giống như cảnh sát muốn bắt tội phạm thì cần phải biết họ tên cũng như khuôn mặt tội phạm thì mới bắt được chứ. Vậy để bắt được những gì thuộc về `this` bên trong hàm `PrintNumber` thì ta cần phải quăng nguyên cái `this` vào trong `PrintNumber` như sau:

```cpp
thread subProcess(&A::PrintNumber, this);
```

Bằng cách dùng cú pháp trên, hàm `PrintNumber` lúc này đã biết nó cần phải "bắt" được `this` của `A` bên trong nó, vậy là ta có thể truy cập đến `this` bên trong một phương thức tĩnh, quá hay phải không.

## Tổng kết

Trong bài này ta đã biết được:

* Con trỏ hàm là gì
* Bản chất thực sự của hàm callback là gì trong C++ và ứng dụng của nó.
* Khái niệm cơ bản về đa luồng và cách dùng callback trong đa luồng.
* Cách dùng đa luồng trong class và "bắt" (truy cập) `this` bên trong hàm tĩnh của class.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://huaanhminh.github.io/2018/09/07/Con-Tr%E1%BB%8F-H%C3%A0m-(Function-Pointer),-H%C3%A0m-G%E1%BB%8Di-L%E1%BA%A1i-(Callback-Function)-V%C3%A0-Nh%E1%BB%AFng-%E1%BB%A8ng-D%E1%BB%A5ng-Trong-%C4%90a-Lu%E1%BB%93ng.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://huaanhminh.github.io/2018/09/07/Con-Tr%E1%BB%8F-H%C3%A0m-(Function-Pointer),-H%C3%A0m-G%E1%BB%8Di-L%E1%BA%A1i-(Callback-Function)-V%C3%A0-Nh%E1%BB%AFng-%E1%BB%A8ng-D%E1%BB%A5ng-Trong-%C4%90a-Lu%E1%BB%93ng.html" data-numposts="5"></div>