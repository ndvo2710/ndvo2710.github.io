
## TABLE OF CONTENTS:


[POST 1: # Dùng model trong R để xác định đặc tính phân phối biến liên tục:](#post1)

[POST 2: # Tại sao đồ thị biểu diễn phân phối chuẩn lại có hình chuông Úp ?](#post2)

[POST 3: # Hồi quy đa thức bậc cao](#post3)

[POST 4: # Ngữ pháp mô hình trong R](#post4)

[POST 5: # Mô hình hồi quy Poisson](#post5)

[POST 6: #  Bộ slide bài giảng của Richard McElreath về mô hình Bayes](#post6)

[POST 7: # Phân chia dữ liệu Train & Test](#post7)

[POST 8: # Box-Cox Transformation](#post8)

[POST 9: # Skill set số 2: các chiêu thức Trích xuất dữ liệu R](#post9)

[POST 10: # Dùng BAYES thay cho ANOVA](#post10)

[POST 11: # Dùng BAYES thay cho test t--Student](#post11)

[POST 12: # Skill set số 3: Thiết lập tương phản cho biến phân nhóm trong ANOVA](#post12)

[POST 13: # Skill set số 4:](#post13)

[POST 14: # Mô hình cây phân loại sử dụng CARET](#post14)

[POST 15: # Mô hình hồi quy với CARET](#post15)

[POST 16: # Đồng Đồng diễn diễn mô mô hình hình với với CARETensemble CARETensemble](#post16)

[POST 17: # Mô hình Logistic Logistic BAYES](#post17)

[POST 18: # Mô hình Neural network /CARET](#post18)

[POST 19: # Sử dụng R xây dựng mô hình hồi quy tuyến tính theo Bayes](#post19)

[POST 20: # Tóm tắt những chỉ số quan trọng khi diễn giải kết quả suy diễn BAYES.](#post20)

[POST 21: # Hồi quy nhị thức âm BAYES BAYES](#post21)

[POST 22: # So sánh 2 phân nhóm độc lập theo BAYES:](#post22)

[POST 23: # Hồi quy Beta theo Bayes:](#post23)

[POST 24: # Tips:](#post24)

[POST 25: # So So sánh sánh phẩm phẩm chất chất mô mô hình hình phân phân loại loại](#post25)

[POST 26: # Vietnam Machine Learning R Forum:](#post26)

[POST 27: # Book Reccomendation: ](#post27)

[POST 28: # Book Reccomendation: Applied Survival Analysis Using R](#post28)

[POST 29: # Hoán chuyển dữ liệu sử dụng CARET:](#post29)

[POST 30: # Book Recommendation: Analysis of Large and Complex Data:](#post30)

[POST 31: # Case study 17: ](#post31)

[POST 32: # FFTrees:](#post32)

[POST 33: # Quy trình Bootstrap:](#post33)



<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post33'></a>


## Quy trình Bootstrap:

1) Chuẩn bị câu hỏi dưới dạng 1 function
2) Triệu hồi thần may mắn
3) Đặt câu hỏi cho nữ thần
4) Chờ đợi câu trả lời

[Files](https://drive.google.com/file/d/0B1vaOU1uB8DPMS1SUXFkcmZKNmc/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post32'></a>


## FFTrees:

Tuyệt, mới nhắc đến cây là sáng nay cây mọc lên rào rào trên Rbloggers.
Package mới huấn luyện mô hình cây, cú pháp dễ như ăn kẹo.
Không sâu bằng caret nhưng các bác sĩ có thể sử dụng dễ dàng hơn...

https://cran.rstudio.com/web/packages/FFTrees/index.html

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-10-50.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post31'></a>


## Case study 17: 

Mục tiêu nghiên cứu: Phân loại nguy cơ của nhồi máu cơ tim dựa vào 5 tiêu chí :Tuổi, Mức Cholesterol máu : Thấp, TB, cao, Thói quen hút thuốc lá, Trình độ học vấn, Trạng thái cân nặng: bình thường/Cao. Trên một mẫu 1004 trường hợp
Chúng tôi ứng dụng mô hình cây với algorithm CART, kiểm chứng chéo 10x10 trên 80% dataset và kiểm định độc lập trên 20% còn lại
Mô hình kết quả có hiệu năng cao (95%CI độ chính xác từ 80.9 - 90.3%).
Kết luận: 
Mô hình cây phân loại (algorithm CART) là một phương pháp hữu ích cho nghiên cứu, thực hành y học: Nên phổ biến nó cho nhiều đồng nghiệp biết và ứng dụng vào thực tiễn ngay từ bây giờ
Kiểm chứng chéo cho phép tối ưu hóa hiệu năng của mô hình phân loại: Nên nghĩ đến phương pháp này khi xây dựng thiết kế nghiên cứu
Caret là 1 công cụ rất hữu dụng: Nên sử dụng caret làm giao thức để thực hành Machine learning
R mạnh và linh hoạt hơn rất nhiều so với các gói công cụ thống kê thương mại khác: Hãy học và dùng R nhiều hơn

[files1](https://drive.google.com/file/d/0B1vaOU1uB8DPbzdFT19lT3g2c0U/view)

[pdf](https://drive.google.com/file/d/0B1vaOU1uB8DPbEVGaU9uamxKSm8/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post30'></a>


## Book Recommendation: Analysis of Large and Complex Data:

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-15-12.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post29'></a>


## Hoán chuyển dữ liệu sử dụng CARET:

[pdf file](https://drive.google.com/file/d/0B1vaOU1uB8DPRERFWEJaWVBpQ0U/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post28'></a>


## Book Reccomendation: Applied Survival Analysis Using R

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-24-11.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post27'></a>


## Book Reccomendation: 

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-25-42.png)


<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post26'></a>


## Vietnam Machine Learning R Forum:

Diễn đàn Machine Learning R chính thức mở cửa hộm nay (trùng hợp với fete nationale Française)
http://machinelearningvn.freeforums.net/
Hiện nay nó mới là một căn nhà trống: Chúng tôi cần có thời gian để xây dựng nội dung của Thư viện sách, dữ liệu và bài giảng phong phú hơn; tuy nhiên các bạn đã có thể bắt đầu đăng nhập và viết bài.
Xin chân thành cảm ơn các bạn.

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-27-40.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post25'></a>


## So So sánh sánh phẩm phẩm chất chất mô mô hình hình phân phân loại loại

Bài 5 trong series caret: so sánh khả năng dự báo của mô hình phân loại : đồ thị trực quan và kiểm định t.

[pdf file](https://drive.google.com/file/d/0B1vaOU1uB8DPUWxtVjBXTWJfd0k/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post24'></a>


## Tips:

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-29-21.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post23'></a>


## Hồi quy Beta theo Bayes:

[PDF Files](https://sites.google.com/site/bayesforvietnam/home/bai-giang-slides/HoiquyBeta%20Bayes.pdf?attredirects=0)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post22'></a>


## So sánh 2 phân nhóm độc lập theo BAYES:

[Youtube link](https://www.youtube.com/watch?v=SrxdKqCo9v0&feature=youtu.be)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post21'></a>


## Hồi Hồi quy quy nhị nhịthức thức âmâm BAYES BAYES

[PDF File](https://sites.google.com/site/bayesforvietnam/home/bai-giang-slides/Negbinomial%20Bayes.pdf?attredirects=0&d=1)


<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post20'></a>


## Tóm tắt những chỉ số quan trọng khi diễn giải kết quả suy diễn BAYES.

Nhắc lại: Suy diễn Bayes dựa trên nền tảng: Mức độ khả tín, dựa vào tỉ trọng chứng cứ. Nó không ép buộc các bạn phải tin vào 1 giá trị p duy nhất, mà đưa ra bằng chứng cho các bạn tự quyết định.

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-35-11.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-35-25.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post19'></a>


## Sử dụng R xây dựng mô hình hồi quy tuyến tính theo Bayes

[Youtube Link](https://www.youtube.com/watch?v=8DAaWKoMfvw)
[Materials](https://sites.google.com/site/bayesforvietnam/home)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post18'></a>


## Mô hình Neural network /CARET

Bài thứ 4 của series CARET sẽ hướng dẫn thực hành về Neural Network sử dụng phương pháp nnet. NN, còn gọi là mạng thần kinh nhân tạo, là một giải pháp thay thế cho logistic model. Nó có ưu thế cũng như nhược điểm riêng, nhưng rất đáng để thử nghiệm.

[PDF file](https://drive.google.com/file/d/0B1vaOU1uB8DPdmpGYXRDR3Q5Q1k/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post17'></a>


## Mô hình Logistic Logistic BAYES

Đây là bài thứ 4 của series Bayes. Bài này sẽ bàn về mô hình Logistic theo Bayes. Mình đã viết thêm 1 số code cho phép khảo sát phân phối hậu nghiệm của Odds-ratio cũng như loại trừ giả thuyết H0 liên quan tới khoảng vô nghĩa (ROPE) và ngưỡng ý nghĩa (CompVal) theo cách của John Kruschke.

[Pdf File](https://drive.google.com/file/d/0B1vaOU1uB8DPclRaX1NXUE5haUk/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post16'></a>


## Đồng Đồng diễn diễn mô mô hình hình với với CARETensemble CARETensemble

Bài thứ 3 trong series CARET sẽ giới thiệu về tập hợp Mô hình (có nơi dịch là Đồng diễn), một phương pháp tối ưu hóa khả năng dự báo bằng cách can thiệp lên variance và/hoặc bias.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPRjVEdmhIbEdYOWM/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post15'></a>


## Mô hình hồi quy với CARET

Bài thứ 2 trong series CARET.
Mục tiêu của bài này là hướng dẫn Huấn luyện và Kiểm định mô hình hồi quy trong caret.
5 kiểu mô hình khác nhau sẽ được thử nghiệm: GLM, GLM đa thức bậc cao, Cây hồi quy, GAM với spline và LASSO.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPRHJtSUctVV9iaEk/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post14'></a>


## Mô hình cây phân loại sử dụng CARET

Bài này mở đầu series CARET.
CARET là một giao thức tổng quát cho phép kết nối với hàng trăm package khác nhau trong R, với ứng dụng là Mô hình dự báo và Machine Learning.
CARET hỗ trợ 217 dạng mô hình khác nhau. Bài này sẽ ứng dụng mô hình cây kiểu CART vào chẩn đoán.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPaWpWd3dzMnBSRHM/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post13'></a>


## Skill set số 4:

Một trong những trở ngại khó chịu khi phân tích Longitudinal data là định dạng của chúng. Có 2 kiểu định dạng phổ biến là Long data và Wide data. Một số package và thao tác chỉ tương thích với 1 trong 2: Long hoặc Wide. Sau đây là giải pháp chuyển đổi giữa chúng...

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-48-26.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-48-49.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post12'></a>


## Skill set số 3: Thiết lập tương phản cho biến phân nhóm trong ANOVA

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPYzY4VXAyOTFvMlk/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post11'></a>


## Dùng BAYES thay cho test t--Student

Bài tiếp theo trong series BAYES bàn về cách thay thế test t Student bằng phương pháp Bayes (Thực ra cả 2 đều dựa trên phân phối t của Student và một mô hình tuyến tính). Một lần nữa bạn sẽ thấy là Bayes mạnh và linh hoạt hơn rất nhiều.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPLWJLQjFkem5EY2M/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post10'></a>


## Dùng BAYES thay cho ANOVA

Đây là bài đầu tiên trong series Bayes, mình sẽ trình bày lại chi tiết, đầy đủ mọi công đoạn phân tích BAYES bằng package brms.
Chủ đề của bài này là dùng Bayes thay thế cho ANOVA. Bạn sẽ thấy sức mạnh và tính linh hoạt của phương pháp này.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPZ19xMGNmOWpJNVk/view)


<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post9'></a>


## Skill set số 2: các chiêu thức Trích xuất dữ liệu R

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-55-13.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-55-27.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-55-44.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-55-58.png)

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_02-56-11.png)


<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post8'></a>


## Box-Cox Transformation

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPbnBRdVFRY2JFUFU/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post7'></a>


## Phân chia dữ liệu Train & Test

Phân chia dữ liệu là công đoạn phổ biến trước khi phân tích bằng mô hình, thông thường ta chia (ngẫu nhiên) data lớn thành 2 subset, một tên là training dùng để dựng mô hình, subset kia tên là testing, hay validation, dùng để kiểm định mô hình.

Mình xin chia sẻ với các bạn cách tạo 1 hàm duy nhất trong R để làm việc này, từ những lệnh cơ bản

```r
splitdata=function(dataframe, seed=NULL,ratio=NULL) {
if (!is.null(seed)) set.seed(seed)
index <- 1:nrow(dataframe)
trainid <- sample(index, trunc(length(index)*ratio))
trainsubset <- dataframe[trainid, ]
testsubset <- dataframe[-trainid, ]
list(training=trainsubset,testing=testsubset)
}
```

Cách sử dụng hàm này:

1. Load data vào R: ví dụ `data = read.csv("dataset.csv")`

2. Áp dụng hàm splitdata trên data với cú pháp: tên đối tượng `split = splitdata`(tên data, seed=con số tùy chọn, ratio=tỉ lệ mẫu train so với data ban đầu)

Ghi chú: tỉ lệ mẫu train tùy bạn chọn, nếu sample size đủ lớn bạn có thể đặt tỉ lệ này = 0.5; bằng không bạn có thể đặt tỉ lệ từ 0.7 tới 0.9 để đảm bảo đủ để dựng mô hình (quan trọng hơn). 

con số seed tùy bạn chọn, để tái lập kết quả

Thí dụ:

`split=splitdata(data, seed=123, 0.7)`

sẽ cắt data thành 2 phần, 70% dùng dựng mô hình, 30% còn lại dùng để kiểm định

3. Tạo đối tượng cho mẫu train và test

```r
train=split$training
test=split$testing
```

Bây giờ bạn đã có 2 subset là train và test như ý rồi nha.

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_03-00-15.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post6'></a>


##  Bộ slide bài giảng của Richard McElreath về mô hình Bayes

[Web link](https://speakerdeck.com/rmcelreath)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post5'></a>


## Mô hình hồi quy Poisson

Tập mới nhất của serie GLM sẽ bàn về mô hình Poisson.
Trong bài này, Bs. Nhi sẽ hướng dẫn các bạn dựng model Poisson bằng 3 cách khác nhau, bao gồm suy diễn Bayes, làm sao diễn giải mô hình và bật mí về quan hệ giữa Poisson model và phân tích bảng chéo.

[PDF File](https://drive.google.com/file/d/0B1vaOU1uB8DPYlhXWWFDV0JBdzA/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post4'></a>


## Ngữ pháp mô hình trong R

![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_03-03-22.png)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post3'></a>


## Hồi quy đa thức bậc cao

Polynomial regression 
Đây là bài thứ 2 trong series GLM; đề tài là hồi quy đa thức bậc cao. Bài này sẽ trình bày cả 5 phương pháp để dựng mô hình hồi quy đa thức bậc cao, bao gồm Trực giao, phân đoạn, riêng phần, Spline bậc 3 và Spline bù trừ

[PDF File](https://drive.google.com/file/d/0B1wk5GIjnbH5d3NFWXVVa013UFk/view)

<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post2'></a>


## Tại sao đồ thị biểu diễn phân phối chuẩn lại có hình chuông Úp ?

Trò chơi nhỏ sau đây sẽ giúp bạn trả lời câu hỏi này

1) Trong R, bạn tạo 3 đối tượng x, mu và s như sau :

```r
x=runif(100,-10,10)
s=sd(x)
mu=mean(x)
```

Gọi package ggplot2

2) Ta bắt đầu với y1 = (x-mu) bình phương

```r
y1=(-(x-mu)^2)
qplot(x,y1)
```

3)Tiếp tục với y2 = y1 chia cho 2 lần phương sai sigma của x

```r
y2=(-(x-mu)^2/(2*s^2))
qplot(x,y2)
```

4) Rồi y3 = exp(y2)

```r
y3=exp(-(x-mu)^2/(2*s^2))
qplot(x,y3)
```

5) Cuối cùng là y4 : xác suất phân phối Gaussian đầy đủ

```r
y4=(1/(s*sqrt(2*pi)))*exp(-(x-mu)^2/(2*s^2))
qplot(x,y4)
```

<b><u>Nhận xét :</u></b> Chính y1 quyết định hình dạng đồ thị của hàm phân phối chuẩn, nó có hình chuông úp vì y1 là hàm số bậc 2.
Tất cả những phần còn lại trong y3 và y4 chỉ là râu ria, vd: y3 dùng hàm exponential để đảm bảo giá trị xác suất dương, còn y4 để đảm bảo tổng xác suất =1


![image](https://dl.dropboxusercontent.com/u/27868566/shots/shot_2016-09-16_03-07-43.png)


<hr>
<hr>
<hr>
<hr>
<hr>

<a id='post1'></a>

## Dùng model trong R để xác định đặc tính phân phối biến liên tục:


Như dự kiến, mình xin giới thiệu bài đầu tiên trong serie glm; bài này có mục tiêu kép là:
(1) Ngược = Sử dụng glm và AIC để xác định kiểu phân phối phù hợp nhất cho 1 biến liên tục;
và (2) Xuôi = Xác định kiểu phân phối cho Y trước khi tiến hành dựng bất kì model nào với Y là response variable.
Package được sử dụng là gamlss. Đây là package mạnh nhất mình từng biết về glm.

[Pdf File](https://drive.google.com/file/d/0B1vaOU1uB8DPd0pZeVllUFYxVzA/view)




