# Tổng quan về Spark và Mapreduce

## I. Tổng quan về Apache Spark

### 1. Spark là gì?
**Apache Spark** là _một framework nguồn mở được sử dụng chủ yếu phân tích Dữ liệu lớn, học máy và xử lý thời gian thực._ Framework này cung cấp một giao diện đầy đủ chức năng cho các lập trình viên và nhà phát triển - giao diện này thực hiện rất tốt công việc hỗ trợ lập trình cụm phức tạp khác nhau và các nhiệm vụ học máy.

### 2. Các thành phần của Apache Spark
  Matei Zaharia là cha đẻ của Spark, ông này sử dụng Hadoop từ những ngày đầu phát triển spark. Đến năm 2009 ông viết Apache Spark để giải quyết những bài toán học máy ở đại học UC Berkely vì Hadoop MapReduce hoạt động không hiệu quả cho những bài toán này. Sau đó ông nhận ra rằng Spark không chỉ hữu ích cho học máy mà còn cho cả việc xử lý luồng dữ liệu hoàn chỉnh.
  
  Thành phần cơ bản của Spark là Spark Core: cung cấp những chức năng cơ bản nhất của Spark như lập lịch cho các tác vụ, quản lý bộ nhớ, fault recovery, tương tác với các hệ thống lưu trữ…Đặc biệt, Spark Core cung cấp API để định nghĩa RDD (Resilient Distributed DataSet) là tập hợp của các item được phân tán trên các node của cluster và có thể được xử lý song song.
  
  Spark có thể chạy trên nhiều loại Cluster Managers như Hadoop YARN, Apache Mesos hoặc trên chính cluster manager được cung cấp bởi Spark được gọi là Standalone Scheduler.
  
  Spark SQL cho phép truy vấn dữ liệu cấu trúc qua các câu lệnh SQL. Spark SQL có thể thao tác với nhiều nguồn dữ liệu như Hive tables, Parquet, và JSON.
  
  Spark Streaming cung cấp API để dễ dàng xử lý dữ liệu stream
MLlib Cung cấp rất nhiều thuật toán của học máy như: classification, regression, clustering, collaborative filtering… Còn GraphX là thư viện để xử lý đồ thị.

### 3. Những tính năng nổi bật
- “Spark as a Service”: Giao diện REST để quản lí (submit, start, stop, xem trạng thái) spark job, spark context
- Tăng tốc, giảm độ trễ thực thi job xuống mức chỉ tính bằng giây bằng cách tạo sẵn spark context cho các job dùng chung.
- Stop job đang chạy bằng cách stop spark context
- Bỏ bước upload gói jar lúc start job làm cho job được start nhanh hơn.
- Cung cấp hai cơ chế chạy job đồng bộ và bất đồng bộ
- Cho phép cache RDD theo tên , tăng tính chia sẻ và sử dụng lại RDD giữa các job
- Hỗ trợ viết spark job bằng cú pháp SQL
- Dễ dàng tích hợp với các công cụ báo cáo như: Business Intelligence, Analytics, Data Integration Tools

### 4. Quản lý bộ nhớ của Apache Spark
  Spark giải quyết các vấn đề vấn đề xung quanh định nghĩa Resilient Distributed Datasets (RDDs). RDDs hỗ trợ hai kiểu thao tác thao tác: transformations và action. Thao tác chuyển đổi(tranformation) tạo ra dataset từ dữ liệu có sẵn. Thao tác actions trả về giá trị cho chương trình điều khiển (driver program) sau khi thực hiện tính toán trên dataset.

  Spark thực hiện đưa các thao tác RDD chuyển đổi vào DAG (Directed Acyclic Graph) và bắt đầu thực hiện. Khi một action được gọi trên RDD, Spark sẽ tạo DAG và chuyển cho DAG scheduler. DAG scheduler chia các thao tác thành các nhóm (stage) khác nhau của các task. Mỗi Stage bao gồm các task dựa trên phân vùng của dữ liệu đầu vào có thể pipline với nhau và có thể thực hiện một cách độc lập trên một máy worker. DAG scheduler sắp xếp các thao tác phù hợp với quá trình thực hiện theo thời gian sao cho tối ưu nhất. Ví dụ: các thao tác map sẽ được đưa vào cùng một stage do không xảy ra shuffle dữ liệu giữa các stage. Kết quả cuối cùng của DAG scheduler là một tập các stage. Các Stages được chuyển cho Task Scheduler. Task Scheduler sẽ chạy các task thông qua cluster manager (Spark Standalone/Yarn/Mesos). Task scheduler không biết về sự phụ thuộc của các stages. Nó chỉ chịu trách nhiệm thực hiện sắp xếp các task một cách tối ưu nhất.
  
  Mỗi Worker bao gồm một hoặc nhiều Excuter. Các excuter chịu trách nhiệm thực hiện các task trên các luồng riêng biệt. Việc chia nhỏ các task giúp đem lại hiệu năng cao hơn, giảm thiểu ảnh hưởng của dữ liệu không đối xứng (kích thước các file không đồng đều).
Quá trình Spark xây dựng DAG: Có hai kiểu chuyển đổi có thể áp dụng trên các RDDs đó là chuyển đổi hẹp và chuyển đổi rộng:
– Chuyển đổi hẹp: không yêu cầu xáo trộn dữ liệu vượt qua các phân vùng (partition).Ví dụ như các thao tác map, filter,..
– Chuyển đổi rộng yêu cầu dữ liệu phải xáo trộn. Ví dụ: reduceByKey, sortByKey, groupByKey,…
  
  Spark truy cập dữ liệu được lưu trữ ở các nguồn khác nhau như: HDFS, Local Disk, RAM. Cache Manager sử dụng Block Manager để quản lý dữ liệu. Cache Manager quản lý dữ liệu nào được Cache trên RAM, thông thường là dữ liệu được sử dụng thường xuyên nhất, nó cũng có thể được xác định thủ công bằng tay sử dụng phương thức persit hoặc cache. Nếu kích thước RAM không đủ chứa dữ liệu thì dữ liệu sẽ được lưu trữ sang Tachyon và cuối cùng là lưu trữ lên đĩa. Khi dữ liệu(RDD) không được lưu trữ trên RAM, khi có nhu cầu sử dụng đến, chúng sẽ được recompute lại. Nó sử dụng một khái niệm là “storage level” để quản lý cấp độ của lưu trữ của dữ liệu.

## II. Tổng quan về Mapreduce

### 1. MapReduce là gì?
**MapReduce** là mô hình được thiết kế độc quyền bởi Google, nó có khả năng lập trình xử lý các tập dữ liệu lớn song song và phân tán thuật toán trên 1 cụm máy tính. MapReduce trở thành một trong những thành ngữ tổng quát hóa trong thời gian gần đây.

### 2. Mapreduce và các thủ tục chính
MapReduce sẽ bao gồm 2 thủ tục sau: Map() và Reduce().

 - Thủ tục Map() bao gồm lọc (filter) và phân loại (sort) trên dữ liệu.
 - Thủ tục Reduce() thực hiện quá trình tổng hợp dữ liệu.

Đây là mô hình dựa vào các khái niệm biển đối của bản đồ và reduce những chức năng lập trình theo hướng chức năng. Thư viện của thủ tục Map() và Reduce() sẽ được viết bằng nhiều loại ngôn ngữ khác nhau.

### 3. Các ưu điểm nổi bật của MapReduce

Mapreduce được ưa chuộng sử dụng như vậy bởi nó sở hữu nhiều ưu điểm vượt trội như sau:

-   MapReduce có khả năng xử lý dễ dàng mọi bài toán có lượng dữ liệu lớn nhờ khả năng tác vụ phân tích và tính toán phức tạp. Nó có thể xử lý nhanh chóng cho ra kết quả dễ dàng chỉ trong khoảng thời gian ngắn.
-   Mapreduce có khả năng chạy song song trên các máy có sự phân tán khác nhau. Với khả năng hoạt động độc lập kết hợp phân tán, xử lý các lỗi kỹ thuật để mang lại nhiều hiệu quả cho toàn hệ thống.
-   MapRedue có khả năng thực hiện trên nhiều nguồn ngôn ngữ lập trình khác nhau như: Java, C, C++, Python, Perl, Ruby,… tương ứng với nó là những thư viện hỗ trợ.

### 4. Nguyên tắc hoạt động 
Mapreduce hoạt động dựa vào nguyên tắc chính là “Chia để trị”, như sau:
- Phân chia các dữ liệu cần xử lý thành nhiều phần nhỏ trước khi thực hiện. 
- Xử lý các vấn đề nhỏ theo phương thức song song trên các máy tính rồi phân tán hoạt động theo hướng độc lập.
Tiến hành tổng hợp những kết quả thu được để đề ra được kết quả sau cùng. 

### 5. Các bước hoạt động của MapReduce
- Bước 1: Tiến hành chuẩn bị các dữ liệu đầu vào để cho Map() có thể xử lý.
- Bước 2: Lập trình viên thực thi các mã Map() để xử  lý. 
- Bước 3: Tiến hành trộn lẫn các dữ liệu được xuất ra bởi Map() vào trong Reduce Processor
- Bước 4: Tiến hành thực thi tiếp mã Reduce() để có thể xử lý tiếp các dữ liệu cần thiết.  
- Bước 5: Thực hiện tạo các dữ liệu xuất ra cuối cùng. 

### 6. Luồng dữ liệu nền tảng của Mapreduce
- Input Reader
- Map Function
- Partition Function
- Compare Function
- Reduce Function
- Output Writer

### 7.Ứng dụng của Mapreduce
MapReduce được ứng dụng cho việc thống kê hàng loạt những số liệu cụ thể như sau: 
- Thực hiện thống kê cho các từ khóa được xuất hiện ở trong các tài liệu, bài viết, văn bản hoặc được cập nhật trên hệ thống fanpage, website,…
- Khi số lượng các bài viết đã được thống kê thì tài liệu sẽ có chứa các từ khóa đó. 
- Thống kê được những câu lệnh match, pattern bên trong các tài liệu đó
- Khi thống kê được số lượng các URLs có xuất hiện bên trong một webpages. 
- Thống kê được các lượt truy cập của khách hàng sao cho nó có thể tương ứng với các URLs.
- Thống kê được tất cả từ khóa có trên website, hostname,…

# Tài liệu tham khảo
- https://vn.bitdegree.org/huong-dan/apache-spark/
- https://viblo.asia/p/tong-quan-ve-apache-spark-cho-he-thong-big-data-RQqKLxR6K7z
- https://viblo.asia/p/tim-hieu-ve-hadoop-bJzKmOBXl9N

## Ví dụ
Project mẫu trên Colab dùng Spark đọc vào một file văn bản và đếm số từ trên bản, lọc ra k từ có tần suất xuất hiện nhiều nhất: https://colab.research.google.com/drive/1toVrt9yCZa1GI5kCtXvT2ARQu3G8cnEy?usp=sharing

