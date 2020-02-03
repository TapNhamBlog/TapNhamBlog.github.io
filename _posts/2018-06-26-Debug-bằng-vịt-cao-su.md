<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<script>
  (adsbygoogle = window.adsbygoogle || []).push({
    google_ad_client: "ca-pub-9249300980094732",
    enable_page_level_ads: true
  });
</script>
Nghe tiêu đề có vẻ xàm nhỉ, gì mà debug bằng vịt cao su là thế nào?

Trước khi giải thích debug bằng vịt cao su là gì thì ta hãy nhớ lại một chút: có phải là trong quá trình viết code, đôi khi gặp những đoạn code khó hiểu thì các bạn vẫn hay ngồi lẩm bẩm một mình những câu kiểu như "đoạn này dùng làm gì ấy nhỉ", hay là khi gặp code lỗi thì các bạn lại thường có xu hướng "chửi thầm" như "thằng cờ hó nào viết code ngu vậy" hay những câu đại loại thế...

Và do đó phương pháp debug bằng vịt cao su ra đời.

Debug bằng vịt cao su - Rubber Duck Debuggin là một phương pháp debug code nhưng không phải kiểu debug trên mấy cái IDE mà là bạn sẽ cầm theo một con vịt cao su, và giải thích code cho nó, giải thích từng dòng, nói chuyện với nó khi gặp code khó hiểu cũng như trút giận lên nó khi code có lỗi.

Đọc đến đây chắc nhiều bạn sẽ tưởng mình bị xàm, nhưng không, đây thật sự là một phương pháp debug rất hiệu quả và đã được giới thiệu lần đầu trong cuốn sách The Pragmatic Programmer (sách hay nên đọc). Phương pháp này có 3 lợi ích rõ ràng dễ thấy sau:

* Giúp cải thiện khả năng nói: nó cũng giống như những người chưa có kinh nghiệm diễn thuyết thì họ sẽ thường tập nói bằng cách đứng trước gương, thì ở đây ta chỉ cần thay gương bằng con vịt là được.
* Giúp bạn hiểu rõ vấn đề hơn: khi đọc một đoạn code xong, có thể các bạn nghĩ là đã hiểu về đoạn code đó rồi; nhưng thật ra chưa chắc đâu, chỉ khi nào các bạn có thể tự bản thân giải thích lại đoạn code đó một cách rõ ràng theo cách bạn hiểu thì mới được. Các bạn có thể thực hiện phương pháp giải thích code với một người bạn nào đó, nhưng vấn đề là không phải lúc nào mọi người cũng rảnh mà nghe bạn giải thích nên việc dùng một vật tượng trưng như con vịt sẽ hợp lý hơn.
* Giúp bạn xả stress: khi làm project thì thường các bạn sẽ làm nhóm với nhau, và không phải lúc nào các đồng đội của bạn cũng có khả năng code đúng hoàn toàn 100% vì họ cũng chỉ là người, code sai là chuyện chắc chắn sẽ xảy ra. Khi review code chéo lẫn nhau, các bạn có thể sẽ thấy không vui và bực bội khi đồng đội code sai code xấu các kiểu, và những lúc đó nếu có một con vịt để có thể trút giận lên nó sẽ tốt hơn là so với việc trút giận lên đầu người đồng đội kia, tránh việc mất đoàn kết trong team; và đặt trường hợp bạn là người code sai thì bạn cũng đâu muốn bị đồng đội trút giận lên đầu phải không, do đó nên có vịt để ta có thể trút giận vào và khi nguôi giận thì ta sẽ dùng lời lẽ hợp lí để giải thích code cho người đồng đội kia.

![Rubber Duck Debugging](http://huaanhminh.github.io/_img/RubberDuckDebugging.png)

Trên thực tế, trang web nổi tiếng đó là [Stack Exchange](https://stackexchange.com/) cũng đã từng giới thiệu một tính năng trên web họ đó là Quack Overflow. Khi sử dụng tính năng này, người dùng sẽ được chat với một con vịt, đúng vậy, một con vịt và con vịt đó sẽ phản hồi lại những vấn đề của ta và đưa ra gợi ý giải quyết.

Nói tóm lại, mỗi lập trình viên nên mang theo cho mình một con vịt cao su để debug.

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/vi_VN/sdk.js#xfbml=1&version=v3.0';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<div class="fb-like" data-href="https://huaanhminh.github.io/2018/06/26/Debug-b%E1%BA%B1ng-v%E1%BB%8Bt-cao-su.html" data-layout="standard" data-action="like" data-size="small" data-show-faces="true" data-share="true"></div>

<div class="fb-comments" data-href="https://huaanhminh.github.io/2018/06/26/Debug-b%E1%BA%B1ng-v%E1%BB%8Bt-cao-su.html" data-numposts="5"></div>