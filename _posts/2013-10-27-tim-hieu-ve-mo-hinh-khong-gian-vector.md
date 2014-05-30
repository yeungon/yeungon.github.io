---
layout: post
title: "Tìm hiểu về mô hình không gian vector"
date: 2014-04-12 14:28:54
categories: natural-language-processing
author: hungnq1989
tags : [natural-language-processing, full-text-search]
---
{% include JB/setup %}

## Tìm hiểu về Mô hình không gian vector

### Giới thiệu

Nói một cách ngắn gọn, Vector space model (Mô hình không gian vector) là một mô hình đại số (algebraic model) thể hiện thông tin văn bản như một vector, các phần tử của vector này thể hiện mức độ quan trọng của một từ và cả sự xuất hiện hay không xuất hiện (Bag of words) của nó trong một tài liệu.

Mô hình này biểu diễn văn bản như những điểm trong không gian Euclid n-chiều, mỗi chiều tương ứng với một từ trong tập hợp các từ. Phần tử thứ i, là di của vector văn bản cho biết số lần mà từ thứ i xuất hiện trong văn bản. Sự tương đồng của hai văn bản được định nghĩa là khoảng cách giữa các điểm, hoặc là góc giữa những vector trong không gian.

Mỗi từ trong không gian vector sẽ có một trọng số, có nhiều phương pháp xếp hạng khác nhau, nhưng tf-idf (term frequency–inverse document frequency) là một phương pháp phổ biến để đánh giá và xếp hạng một từ trong một tài liệu. MySQL fulltext search cũng sử dụng phương pháp này. Về cơ bản thì tf-idf là một kỹ thuật (cụ thể là ranking function) giúp chuyển đổi thông tin dưới dạng văn bản thành một Vector space model thông qua các trọng số. Vector space model và tf-idf được phát triển bởi Gerard Salton vào đầu thập niên 1960s.

Mặc dù đơn giản, nhưng mô hình không gian vector và những biến thể của nó hiện nay vẫn là cách phổ biến để biểu diễn văn bản trong Data mining và Information retrieval. Tuy nhiên, một trong những điểm yếu của vector space model số chiều lớn (high-dimensonal), có khoảng cỡ chục triệu chiều trong không gian vector nếu như chúng ta áp dụng nó vào web search engine.


### Thể hiện văn bản như vector và term frequency

Ví dụ:
Hàng đầu tiên là danh sách các vở kịch của Shakespeare. Cột đầu tiên là các từ, và các ô thể hiện sự xuất hiện hay không xuất hiện của các từ đó trong các tác phẩm.

| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
|           | Anthony and Cleopatra | Julius Caesar | The Tempest | Hamlet | Othello | Macbeth |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Anthony   |                     1 |             1 |           0 |      0 |       0 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Brutus    |                     1 |             1 |           0 |      1 |       0 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Caesar    |                     1 |             1 |           0 |      1 |       0 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Calpurnia |                     0 |             1 |           0 |      0 |       1 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Cleopatra |                     1 |             0 |           0 |      0 |       0 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Mercy     |                     1 |             0 |           1 |      1 |       1 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Worser    |                     1 |             0 |           1 |      1 |       1 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
Binary incidence matrix (Introduction to Information Retrieval)


Và mỗi tài liệu được biễu diễn dưới dạng một vector, ví dụ Julius Caesar  $\left[\begin{matrix} 1 \\ 1 \\ 1 \\1 \\0 \\0 \\0 \end{matrix}\right]$

Các ô thể hiện sự số lần xuất hiện của các từ trong các tác phẩm. Nó được gọi là **term frequency**.


| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
|           | Anthony and Cleopatra | Julius Caesar | The Tempest | Hamlet | Othello | Macbeth |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Anthony   |                   157 |             1 |           0 |      0 |       0 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Brutus    |                     4 |             1 |           0 |      1 |       0 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Caesar    |                   232 |             1 |           0 |      1 |       0 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Calpurnia |                     0 |             1 |           0 |      0 |       1 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Cleopatra |                    57 |             0 |           0 |      0 |       0 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Mercy     |                     2 |             0 |           1 |      1 |       1 |       1 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |
| Worser    |                     2 |             0 |           1 |      1 |       1 |       0 |
| --------- | --------------------- | ------------- | ----------- | ------ | ------- | ------- |


Và mỗi tài liệu được biễu diễn dưới dạng một vector, ví dụ Julius Caesar $\left[\begin{matrix} 73 \\ 157 \\ 227 &nbsp;\\19 \\0 \\0 \\0 \end{matrix}\right]$

Bây giờ mỗi tài liệu (trong trường hợp này là tác phẩm) được thể hiện dưới dạng một vector đếm.

Term frequency tft,d xác định số lần từ t xuất hiện trong tài liệu d. Nhưng chỉ tần suất xuất hiện của một từ thôi thì chưa đủ.

Ví dụ trong một tài liệu, sự xuất hiện của một từ 10 lần thì tài liệu đó được coi là phù hợp hơn tài liệu mà từ đó chỉ xuất hiện 1 lần. Nhưng không phải là phù hợp hơn tài liệu kia 10 lần. Sự phù hợp không tỷ lệ thuận với số lần xuất hiện của từ đó trong một tài liệu.

## Phương pháp tính trọng số tần suất logarit (log-frequency)

Log-frequency của một term t trong document d được tính như sau:

wt,d=1+log(tft,d) 
Nếu tft,d>0, nếu không thì wt,d=0

Nếu từ đó không xuất hiện trong một tài liệu, thì tft,d bằng 0. Và bởi vì log(0) là một số âm vô cùng, cho nên chúng ta phải cộng 1.

Một từ xuất hiện trong tài liệu:
1 lần có w=1
2 lần w=1.3
10 lần w=2
1000 lần w=4
Điểm cho một cặp document-query được tính bằng tổng của các trọng số của term t trong cả document d và query q = ∑t∈q∩d(1+log(tft,d))
Điểm sẽ bằng 0 nếu như query terms không xuất hiện trong document

