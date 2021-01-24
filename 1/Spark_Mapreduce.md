
## Spark là gì?
**Apache Spark** là _một framework nguồn mở được sử dụng chủ yếu phân tích Dữ liệu lớn, học máy và xử lý thời gian thực._ Framework này cung cấp một giao diện đầy đủ chức năng cho các lập trình viên và nhà phát triển - giao diện này thực hiện rất tốt công việc hỗ trợ lập trình cụm phức tạp khác nhau và các nhiệm vụ học máy.

## MapReduce là gì?
**MapReduce** là mô hình được thiết kế độc quyền bởi Google, nó có khả năng lập trình xử lý các tập dữ liệu lớn song song và phân tán thuật toán trên 1 cụm máy tính. MapReduce trở thành một trong những thành ngữ tổng quát hóa trong thời gian gần đây.
MapReduce sẽ bao gồm 2 thủ tục sau: Map() và Reduce().

 - Thủ tục Map() bao gồm lọc (filter) và phân loại (sort) trên dữ liệu.
 - Thủ tục Reduce() thực hiện quá trình tổng hợp dữ liệu.

Đây là mô hình dựa vào các khái niệm biển đối của bản đồ và reduce những chức năng lập trình theo hướng chức năng. Thư viện của thủ tục Map() và Reduce() sẽ được viết bằng nhiều loại ngôn ngữ khác nhau.


## **Các ưu điểm nổi bật của MapReduce**

Mapreduce được ưa chuộng sử dụng như vậy bởi nó sở hữu nhiều ưu điểm vượt trội như sau:

-   MapReduce có khả năng xử lý dễ dàng mọi bài toán có lượng dữ liệu lớn nhờ khả năng tác vụ phân tích và tính toán phức tạp. Nó có thể xử lý nhanh chóng cho ra kết quả dễ dàng chỉ trong khoảng thời gian ngắn.
-   Mapreduce có khả năng chạy song song trên các máy có sự phân tán khác nhau. Với khả năng hoạt động độc lập kết hợp phân tán, xử lý các lỗi kỹ thuật để mang lại nhiều hiệu quả cho toàn hệ thống.
-   MapRedue có khả năng thực hiện trên nhiều nguồn ngôn ngữ lập trình khác nhau như: Java, C, C++, Python, Perl, Ruby,… tương ứng với nó là những thư viện hỗ trợ.

## Ví dụ
Project mẫu trên Colab dùng Spark đọc vào một file văn bản và đếm số từ trên bản, lọc ra k từ có tần suất xuất hiện nhiều nhất: https://colab.research.google.com/drive/14tbyeqyZhQb4ZpSvmjFLKHewtdohjBDN?usp=sharing

