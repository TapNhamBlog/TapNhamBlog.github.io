Hôm nay để đổi gió một chút thì mình sẽ viết một bài về trải nghiệm, ở bài đầu tiên này sẽ là trải nghiệm ngày đầu đi thực tập ở Big-O Coding.

## Big-O Coding là gì?

Big-O Coding là một start-up mới thành lập khoảng 1 năm chuyên về đào tạo thuật toán. Các bạn có thể xem qua về trung tâm ở đây: [https://bigocoding.com/](https://bigocoding.com/). Hiện nay trung tâm có 2 khóa đào tạo là Green (dành cho những người chưa biết về thuật toán) và khóa Blue (dành cho những bạn đã xong kiến thức khóa Green), chi tiết thì các bạn xem ở link trên nếu các bạn muốn tìm hiểu thêm.

## Ngày đầu như thế nào?

Khi sáng mình vào thì để làm quen với teammate và anh mentor với anh lead team trong dự án sắp tới. Sau đó thì được giới thiệu qua về những quy tắc cần có (có thể nói là bắt buộc) với bất kì lập trình viên nào mà trường học không bao giờ dạy:

* Don't repeat yourself (DRY): nói ngắn gọn thì ta không được phép đặt 2 đoạn code có chức năng giống nhau trong 1 chương trình vì khi chương trình càng mở rộng thì việc bảo trì sẽ rất khó khăn. Giả sử nếu thông tin mỗi sinh viên gồm tên, mã số, điểm thì ta có đoạn code sau vi phạm DRY:

```cpp
string firstName;
getline(cin, firstName);
int firstId;
cin >> firstId;
int firstScore;
cin >> firstScore;

string secondName;
getline(cin, secondName);
int secondId;
cin >> secondId;
int secondScore;
cin >> secondScore;
```

  Trên thực tế không ai viết code kiểu đó đâu, mình ví dụ thôi nên đừng ném đá bậy bạ. Như các bạn thấy thì đoạn code trên vi phạm DRY do có 2 phần code giống nhau về mặt chức năng (tạo biến chứa thông tin sinh viên và nhập thông tin sinh viên). Nếu chương trình ngày càng mở rộng như không chỉ có 2 sinh viên mà là cả hàng nghìn sinh viên, hay ta đổi kiểu dữ liệu điểm số từ `int` sang `double` thì đoạn code trên sẽ gặp vấn đề rất lớn là tốn rất nhiều thời gian và chi phí sửa chữa, chưa kể có thể gây thêm bug nữa. Vì thế để không vi phạm DRY thì ta cần phải biết cách phân rã chương trình thành các module và kết hợp với các cấu trúc dữ liệu sao cho hợp lý để dễ dàng bảo trì hơn:

  ```cpp
  class Student {
  public:
    void Input() {
      getline(cin, _name);
      getline(cin, _id);
      cin >> _score;

      string cache; // clean keyboard buffer
      getline(cin, cache);
    }

  private:
    string _name;
    string _id;
    double _score;
  };
  ```
  
  Và như thế mỗi lần ta tạo một sinh viên mới thì chỉ cần gọi phương thức `Input()` chứ không cần viêt lại nữa, nên sau này có bảo trì code hay mở rộng thì ta chỉ cần sửa hoặc thêm bên trong class `Student` là được.
* Readable by others (with minimum explain): quy tắc này thì dễ hiểu thôi, đó là ta phải viết code sạch, đẹp và an toàn sao cho những người khác khi đọc vào thì có thể hiểu ngay code ta viết gì mà không cần ta phải giải thích. Để làm được điều này thì phải thực hành rất nhiều bằng cách viết tên biến dễ đọc, tránh những biến kiểu như `int count = 0;` mà nên viết như `int countNumberOfStudentsInList = 0;` để người khác dễ hiểu hơn, và ta phải luôn nhớ viết document mô tả rõ ràng cho các hàm hay module ta viết để khi có ai đọc code ta thì sẽ đọc phần document đó trước tiên nhằm có cái nhìn tổng quát cho người đó. À mà trừ khi bạn thi ACM-ICPC hay Olympic tin học thì lúc này tốc độ viết và độ chính xác cao, thời gian thực thi nhanh là quan trọng hơn nên cái này có thể bỏ qua.
* Test early, Test when it small, write code to test: nói cách khác là ta phải luôn kiểm tra tính đúng đắn của mỗi đoạn code sau khi ta viết xong và đừng để đến sau khi viết xong vài trăm dòng rồi mới kiểm tra. Nếu ta kiểm tra code thường xuyên thì sẽ phát hiện được lỗi sớm và dễ dàng sửa chửa hơn, còn nếu để chậm thì kiểm tra rất khó khăn và khi phát hiện ra lỗi cũng rất khó sửa chửa do code của ta kết dính từng chùm với nhau nên sửa một đoạn có thể gây lỗi cho những đoạn khác dính với nó.
* Self driven: dịch ra là tự giác. Khi được giao việc thì phải tự giác thực hiện, đừng để người khác nhắc nhở và khi thực hiện phải có trách nhiệm với nó (làm ít nhất là đủ công việc được giao, làm đúng hạn, làm đúng kết quả).
* Refactor and optimize continuously: hãy tối ưu code sau khi <b>hoàn thành</b> một đoạn code chức năng nào đó. Điều này giúp code ta chạy nhanh hơn, rõ ràng và dễ đọc để thuận tiện sau này bảo trì. Lưu ý là ta chỉ tối ưu sau khi hoàn thành code cho chức năng nào đó, vì sao lại như thế? Hãy đọc bài [này](https://toidicodedao.com/2016/09/27/optimize-code/).

Sau khi mình được giới thiệu và hướng dẫn qua những quy tắc trên thì sau đó anh mentor team mình nói qua về dự án sắp tới và hướng dẫn học công nghệ mới nhằm phục vụ cho dự án sắp tới (JavaScript và React/Redux) cùng với một số cài đặt và thiết lập môi trường phát triển. Mình sẽ dùng:

* [Visual Studio Code](https://code.visualstudio.com/): một text editor đa nền tảng đến từ công ty logo 4 ô vuông 4 màu Microsoft. Mình sẽ code JavaScript và React trên đây cùng với một số extension khác nhằm hỗ trợ cho quá trình code.
* Quản lý source code với Git và [BitBucket](https://bitbucket.org/): BitBucket là một nền tảng quản lý source code trên web và hỗ trợ tạo remote repository với private miễn phí (không phải tính tiền như GitHub).
* Quản lí công việc với [Trello](https://trello.com/): trello là một ứng dụng đa nền tảng dùng để quản lý các công việc với mô hình [Kanban](https://leankit.com/learn/kanban/kanban-board/) (công việc được chia thành 3 phần chính là To Do - sẽ làm, Doing - đang thực hiện và Done - đã thực hiện).
* Thảo luận team với [Slack](https://slack.com/): nếu dân game thủ có Discord là nền tảng chat thì dân developer có Slack. Slack là một dạng chat app dành cho team và mỗi phòng chat sẽ được gọi là workspace. Ở mỗi workspace thì bạn có thể tạo ra nhiều kênh chat (channel) dành cho những công việc riêng biệt khác nhau trong team. Điểm mạnh của Slack đó chính là khả năng kết nối với BitBucket (GitHub cũng được nha) và Trello. Ví dụ mỗi khi bạn commit lên git thì Slack sẽ hiện ra thông báo và toàn bộ thành viên trong team sẽ thấy và ta không cần phải báo cáo tiến độ cho leader mỗi lần commit nữa.

Sau đó thì mình đi ăn trưa các kiểu rồi chiều đến thì ngồi tự học JavaScript (ngôn ngữ bựa và khắm lọ gì đâu ấy), và thế là hết ngày thực tập đầu tiên.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>


<div class="fb-like" data-href="https://tapnhamblog.github.io/2018/06/07/Tr%E1%BA%A3i-nghi%E1%BB%87m-ng%C3%A0y-%C4%91%E1%BA%A7u-th%E1%BB%B1c-t%E1%BA%ADp-%E1%BB%9F-Big-O-Coding.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://tapnhamblog.github.io/2018/06/07/Tr%E1%BA%A3i-nghi%E1%BB%87m-ng%C3%A0y-%C4%91%E1%BA%A7u-th%E1%BB%B1c-t%E1%BA%ADp-%E1%BB%9F-Big-O-Coding.html" data-numposts="5"></div>
