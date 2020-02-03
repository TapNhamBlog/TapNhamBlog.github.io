<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>

Link phần 3: [Con trỏ và cấp phát động trong C/C++](https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-3-).html)

Ở [phần trước](https://tapnhamblog.github.io/2018/06/03/Con-tr%E1%BB%8F-v%C3%A0-C%E1%BA%A5p-ph%C3%A1t-%C4%91%E1%BB%99ng-trong-C++-(-ph%E1%BA%A7n-1-).html), mình đã giới thiệu về khái niệm về con trỏ và một số thao tác cơ bản. trong phần này, mình sẽ hướng dẫn thao tác trên mảng bằng con trỏ bằng cách sử dụng toán tử và cấu trúc struct sử dụng con trỏ.

# TOÁN TỬ TRÊN CON TRỎ

Giả sử ta có con trỏ với khai báo:

```cpp
int num = 0;
int *pNum = &num;
```

Ta có thể thực hiện những toán tử nhất định để thay đổi vùng nhớ mà con trỏ pNum đang giữ, bao gồm: `+`, `-`, `>`, `>=`, `<`, `<=`, `==`, `!=` (không có phép `*`, `/`, `%`).

## Phép cộng

```cpp
pNum = pNum + 1; //+ 1 ⇔ + 1 * <kích thước kiểu dữ liệu của con trỏ> (ở đây int = 4)
```

Giả sử `pNum` lưu địa chỉ là 1, sau khi thực hiện phép toán trên thì địa chỉ mới của pNum sẽ là `1 + 1 * sizeof(int)` = 5.

Lưu ý:

* Ta có thể sử dụng hai toán tử `++` và `+=` với con trỏ.
* Cần phải nhớ kích thước của kiểu dữ liệu trước khi thực hiện các phép toán, như `int` = 4 bytes, `char` = 1 byte, `double` = 8 bytes,…

## Phép trừ

```cpp
pNum = pNum - 1; //- 1 ⇔ - (1 * <kích thước kiểu dữ liệu của con trỏ>)
```

Giả sử `pNum` lưu địa chỉ là 5, sau khi thực hiện phép toán thì địa chỉ mới của `pNum` sẽ là `5 -  (1 * sizeof(int))` = 1.

Lưu ý: ta có thể sử dụng 2 toán tử -- và -= với con trỏ.

## Các phép so sánh

Ta có thể dùng các phép so sánh như `>`, `>=`, `<`, `<=`, `==`, `!=` để so sánh vị trí địa chỉ 2 con trỏ.

Lưu ý: việc sử dụng những toán tử toán học (`+`, `-`, `++`, `--`, `+=`, `-=`) cho biến con trỏ mà không có mục đích rõ ràng có thể gây xung đột vùng nhớ. Chúng ta chỉ nên sử dụng khi con trỏ đó trỏ đến mảng một chiều vì những phần tử trên đó có địa chỉ liên tục nhau.

# CON TRỎ VÀ MẢNG MỘT CHIỀU

Trước hết ta cần phân biệt những khái niệm sau:

## Con trỏ hằng (pointer to constant)

Là con trỏ mà nó trỏ đến một hằng số, giá trị mà con trỏ đó trỏ đến không thế thay đổi được (read-only) nhưng ta vẫn có thể đổi vùng nhớ của con trỏ đó. Ví dụ:

```cpp
const int num = 10;
const int *pNum = &num; //chú ý từ khóa const phía trước
*pNum = 10; //Lỗi khi biên dịch
```

Con trỏ hằng cũng có thể trỏ đến một biến thông thường như sau:

```cpp
int num = 10;
const int *pNum = &num;
*pNum = 10; //lỗi khi biên dịch
```

## Hằng con trỏ (constant pointer)

Là con trỏ chỉ gán địa chỉ một lần khi khởi tạo, điều này có nghĩa sau khi khai báo xong thì địa chỉ con trỏ không thể thay đổi được. Lưu ý: địa chỉ con trỏ không thay đổi được nhưng giá trị tại vùng nhớ mà nó trỏ đến vẫn có thể thay đổi được.

```cpp
int num = 10;
int num2 = 5;
int *const pNum = &num; //chú ý vị trí của từ khóa const
*pNum = 100; //hợp lệ
pNum = &num2; //lỗi khi biên dịch
```

## Hằng con trỏ hằng (constant pointer to constant)

đây là con trỏ tổng hợp từ 2 loại trên, địa chỉ lưu ở con trỏ không thể thay đổi sau khi khởi tạo và giá trị tại vùng nhớ đó cũng không thể thay đổi được.

```cpp
int num = 10;
const int *const pNum = &num; //chú ý có đến 2 từ const
int num2 = 5;
pNum = &num2; //lỗi khi biên dịch
*pNum = 100; //lỗi khi biên dịch
```

## Mảng một chiều

Giả sử ta có khai báo mảng sau: 

```cpp
int arr[3];
```

Tên mảng arr là một hằng con trỏ nên ta không thể thay đổi giá trị tức là địa chỉ được lưu ở con trỏ này. Giá trị được lưu ở hằng con trỏ `arr` là địa chỉ của phần tử đầu tiên trong mảng `arr` ⇔ `arr` == `&arr[0]`.

## Con trỏ đến mảng một chiều

Do ta không thể thay đổi được địa chỉ lưu ở hằng con trỏ nên ta sẽ khai báo một biến con trỏ khác để trỏ đến mảng.

```cpp
int arr[3], *pArr;
pArr = arr; //cách 1
pArr = &arr[0]; //cách 2
```

### Toán tử trên con trỏ đến mảng một chiều

Để thao tác giữa các phần tử trên mảng một chiều, ta sẽ sử dụng những toán tử ở phần <b>TOÁN TỬ TRÊN CON TRỎ</b>.

Di chuyển giữa các phần tử bằng toán tử cộng và trừ:

```cpp
int arr[3];
int *pArr = arr;
*pArr = 0; //⇔ arr[0] = 0;
*(pArr + 1) = 2; //⇔ arr[1] = 2;
pArr += 2; //⇔ pArr = &arr[2];
*pArr = 5; //⇔ arr[2] = 5;
*(pArr - 2) = 1; //⇔ arr[0] = 1;
``` 

Tính khoảng cách giữa 2 con trỏ:

```cpp
int arr[3];
int *pArr1 = &arr[0], *pArr2 = &arr[2];
cout << pArr2 - pArr1 << endl; //2
cout << pArr1 - pArr2 << endl; //-2
```

`pArr2 – pArr1` (hoặc ngược lại) cho ta khoảng cách (theo số phần tử) giữa 2 con trỏ cùng kiểu dữ liệu.

```cpp
pArr2 - pArr1 ⇔ (<địa chỉ arr[2]> - <địa chỉ arr[0]>) / sizeof(int) = 2
```

### Nhập và xuất mảng một chiều

Nhập mảng

```cpp
#include <iostream>
#include <stdio.h>
using namespace std;
int main() {
    int a[10], n = 10, *pa = a;
    for (int i = 0; i < n; i++) {         
        /* Nhập theo cú pháp C */         
        scanf("%d", &a[i]);         
        scanf("%d", &pa[i]); //chú ý đoạn này khi nhập         
        scanf("%d", a + i);         
        scanf("%d", pa + i); //chú ý đoạn này khi nhập         
        scanf("%d", a++); //Lỗi khi biên dịch, tại sao?         
        scanf("%d", pa++);         

        /* Hãy so sánh với cú pháp nhập C++ */         
        cin >> a[i];
        cin >> pa[i]; //chú ý đoạn này khi nhập
        cin >> *(a + i); 
        cin >> *(pa + i); //chú ý đoạn này khi nhập
        cin >> *(pa++);
    }
    return 0;
}
```

Nếu ta để ý sẽ thấy, hàm `scanf` của C thực chất có tham số truyền vào là tham biến (truyền địa chỉ) nên khi truyền biến phải có dấu & phía trước để lấy địa chỉ của biến đó; còn ở C++, phương thức `cin` nhận vào là tham chiếu nên khi truyền địa chỉ vào phải có dấu `*` phía trước để lấy giá trị lưu tại địa chỉ đó.

Xuất mảng

```cpp
#include <iostream>
using namespace std;
int main() {
    int a[10] = { 0 }, n = 10, *pa = a;
    for (int i = 0; i < n; i++) {
        cout << a[i] << endl;
        cout << pa[i] << endl; //chú ý đoạn này khi xuất
        cout << *(a + i) << endl;
        cout << *(pa + i) << endl; //chú ý đoạn này khi xuất
        cout << *(pa++) << endl; 
    }
    return 0;
}
```

### Truyền mảng bằng địa chỉ vào tham số hàm

```cpp
#include <iostream>
using namespace std;
void OutputArray(int a[10], int n) {
    for (int i = 0; i < n; i++)
        cout << *(a++) << endl; //OK
}

int main() {
    int a[10] = { 0 }, n = 10, *pa = a;
    for (int i = 0; i < n; i++)
        cout << *(a++) << endl; //Lỗi khi biên dịch
    OutputArray(pa, n);
    return 0;
}
```

Lưu ý: đối số mảng truyền cho hàm không phải là hằng con trỏ.

## Con trỏ và cấu trúc `struct`

Có 2 cách sử dụng:

```cpp
<tên biến con trỏ cấu trúc>-><tên thành phần>
```

hoặc

```cpp
(*<tên biến con trỏ cấu trúc>).tên thành phần
```

```cpp
struct Fraction //Phân số {
    int numerator, denominator; //tử số và mẫu số
};

Fraction Fr, *pFr = &Fr; //pFr là con trỏ có kiểu Fraction
Fr.numerator = 1;  Fr.denominator = 2;
pFr->numerator = 1;  pFr->denominator = 2; //tương đương dòng trên
(*pFr).numerator = 1; (*pFr).denominator = 2; //đúng, nhưng dùng -> hay hơn
```

# TỔNG KẾT

Tóm lại, ở phần 2 này, mình đã giới thiệu về toán tử trên con trỏ; mảng và con trỏ bao gồm thao tác trên các phần tử, nhập xuất trên mảng bằng con trỏ, truyền tham số mảng vào hàm bằng con trỏ; cấu trúc và con trỏ, cách khai báo, cách sử dụng.

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