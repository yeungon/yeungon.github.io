---
layout: post
title: "Vector space model"
date: 2014-10-18 11:00 PM
categories: [en]
author: hungnq1989
tags : [natural-language-processing, information-retrieval]
comments: true

---

### Introduction

In brief, Vector space model is an algebraic model for representing text document as a vector, and elements of this vector represent the importance of a word and the occurrences of each words (Bags of words) in a document.

<!--more-->

This model represents document as points in N-dimension Euclid space, and each dimension corresponds for a word in a set of words. The i-th element is di of document vector, tells us the number of times which the i-th word occurs in the document.The similarities between two documents is defined as distance between points or the angle between vectors in space.

Each words in vector space has a weighted score, there are several approach to calculate these scores, but tf-idf (term frequency–inverse document frequency) is a common technique for weighting and ranking a word in a document. MySQL also use it-idf for its full-text search ranking mechanism. Basically, tf-idf is a technique (specifically is a ranking function) which help convert information in text form to a vector space model through weighted score. Vector space model is developed by Gerard Salton in the early of 1960s.

Despite its simplicity, vector space model and its variants is the common approach to represent document in Data mining and Information retrieval. However, one of its weaknesses is high-dimensonal, there are multi-million dimensions in vector space if we apply vector space model to build a search engine.


![Hình minh họa của Christian S. Perone](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/vector_space.png)
Hình minh họa của [Christian S. Perone](https://plus.google.com/118258566074039785562/posts)

### Represent document as vector and term frequency

**Example:**
The column headers are the list of Shakespeare's plays. The first column is the list of  words, and the others present the appearance of those words in the plays.

{:.table.table-bordered}
|           | Anthony and Cleopatra | Julius Caesar | The Tempest | Hamlet | Othello | Macbeth |
| --------- | --------------------: | ------------: | ----------: | -----: | ------: | ------: |
| Anthony   |                     1 |             1 |           0 |      0 |       0 |       1 |
| Brutus    |                     1 |             1 |           0 |      1 |       0 |       0 |
| Caesar    |                     1 |             1 |           0 |      1 |       0 |       1 |
| Calpurnia |                     0 |             1 |           0 |      0 |       1 |       0 |
| Cleopatra |                     1 |             0 |           0 |      0 |       0 |       0 |
| Mercy     |                     1 |             0 |           1 |      1 |       1 |       1 |
| Worser    |                     1 |             0 |           1 |      1 |       1 |       0 |

Binary incidence matrix (Introduction to Information Retrieval)

Each document is presented in form of a vector, e.g: Julius Caesar  $$\left[\begin{matrix} 1 \\ 1 \\ 1 \\1 \\0 \\0 \\0 \end{matrix}\right]$$

The cells display the number of word's appearance in the plays. It is called **term frequency**.

{:.table.table-bordered}
|           | Anthony and Cleopatra | Julius Caesar | The Tempest | Hamlet | Othello | Macbeth |
| --------- | --------------------: | ------------: | ----------: | -----: | ------: | ------: |
| Anthony   |                   157 |            73 |           0 |      0 |       0 |       1 |
| Brutus    |                     4 |           157 |           0 |      2 |       0 |       0 |
| Caesar    |                   232 |           227 |           0 |      2 |       0 |       1 |
| Calpurnia |                     0 |            19 |           0 |      0 |       1 |       0 |
| Cleopatra |                    57 |             0 |           0 |      0 |       0 |       0 |
| Mercy     |                     2 |             0 |           3 |      8 |       5 |       8 |
| Worser    |                     2 |             0 |           1 |      1 |       1 |       0 |

Now each document (play in this case)  is represented in form of a vector of scores, for example Julius Caesar $$\left[\begin{matrix} 73 \\ 157 \\ 227 \\19 \\0 \\0 \\0 \end{matrix}\right]$$

Term frequency tft,d define the number of times that the word t occurs in document d. However, there is not enough information to score the relevance based on its occurences.

For example, a word has 10 occurrences in a document is considered as more relevant than the document which that word only has 1 occurrence. But it does not mean the relevance is 10 times more than the other document. The relevance **IS NOT** proportional with the occurrences of a word in the document.

## Log-frequency weighting

Log-frequency of a term t in document d is calculated as: $$w_{t,d} = 1 + \log(tf_{t,d})$$

If  $$tf_{t,d}  > 0$$, otherwise $$w_{t,d} = 0$$

If a word does not appear in a document, then tft,d equals 0. Because log(0) is negative infinity, therefore we have to add it with 1.

One word which appear in a document:
1 time w = 1
2 times w = 1.3
10 times w = 2
1000 time w = 4

Score of a pair of document-query is calculated by sum up all weight of term $$t$$ in whole document $$d$$ and query $$q$$ = $$\sum_{t\in q \cap d }({1 + \log(tf_{t,d})})$$. The score is 0 if query terms do not appear in the document.

### Inverse document weighting

A rare word is more important than those words which has higher appearance frequencies. In each language, there are words that repeat many times but meaningless in term of searching (e.g: in English they are a, the, to, of etc.), in full-text search they are called **stopwords**.

In term frequency, the more times a word appear in a document, the higher score it has. Therefore, we need another evaluation technique for ranking rare words, because it carries more information than the common words in a document.

For example, in a set of document related to car industry, the keywrod "car" is likely appear in every almost document. In order to overcome this weakness, people developed a mechanism for eliminating that effects and enhance the precision when calculating the relevance between a document and a query. The idea is reduce the weight of word which has high document frequency, by sum up all number of documents (N) and divide by the number of document that a word appears in.

If we call $$dft$$ is the number of documents contain a term $$t$$, then $$dft$$ is the score for the usefulness of $$t$$ ($$dft$$ is smaller than $$N$$ → the total number of documents) . 

We define the weight $$idf$$ of a term $$t$$ as: $$idf_t = \log_{10} (N/df_t)$$

We use $$\log_{10} (N/df_t)$$ instead of $$N/dft$$ to reduce the effect of $$idf$$, as mentioned above because of the occurrences of a word does not reflect its importance in term of meaning. In MySQL they used $$\log_{10} ((N-nf/nf)$$

[(See more about MySQL)](http://dev.mysql.com/doc/internals/en/full-text-search.html)

### The effect of $$idf$$ on ranking

$$idf$$ does not affect on the document ranking (againts a query), it only help classify document. $$idf$$ only affects on document ranking if query has at least 2 terms. For example, if user searches for a keywords "capricious person", $$idf$$ will make "capricious" has higher score in the final ranking compare to the word "person", because it is more common.

### Collection and  Document frequency

{:.table.table-bordered}
|    term   |    dtf    |         idft         |
| :-------- | --------: | -------------------: |
| calpurnia |         1 |   log(1,000,000/1)=6 |
| animal    |       100 | log(1,000,000/100)=4 |
| sunday    |     1,000 |                    3 |
| fly       |    10,000 |                    2 |
| under     |   100,000 |                    1 |
| the       | 1,000,000 |                    0 |

Collection frquency of a term $$t$$ is the numbers occurrence of t in the set of documents. One example about $$idf$$,
assume that N=1,000,000

Collection frequency của từ t là số lần xuất hiện của t trong tập hợp các tài liệu. Một ví dụ về idf, giả sử N=1,000,000. Mỗi một từ trong tập hợp sẽ có một giá trị idf tương ứng. Còn document frequency biểu diễn số document trong collection chứa một term t nào đó.

### Queries as vectors

In order to find a phrase in a set of documents (like when we perform a full-text query), we need to compare that phrase (query) against that set of docuemtns. The idea is we consider the query as a vector, and we will rank these documents based on theirs similarity with the query.

To ranking the documents we have (and returns set of documents with high rank), we have compare the query versus the set of documents. The document which is more close to the query has higher score.

To compare two vectors, we can calculate the distance between two vector, or the angle between them. However, the distance calculation is not precise, because the distance is large with those vectors have different lengths ( for example: document $$d'$$ is doubled in length of document $$d$$)

The Euclid distance between $$\vec q$$  và $$\vec d2$$ is big, even if the distribution of words in query $$q$$ and $$d2$$ document is pretty similar.


![Euclid distance](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/distance.png)

In the picture, we can see the distance between $$\vec q$$  và $$\vec d2$$ is pretty big, although the distribution of terms in query $$q$$ and document $$d2$$ is pretty similar. Therefore, we will calculate the score based on **ANGLE** in vector space rather than the distance between them.

### Using angle instead of distance

![Sử dụng góc](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/angle.png)

Through experimental, appending a document to itself and we have another document $$d'$$. In term of meaning, these two document is identical about the content. However, the vector d’ is bigger than vector d twice and has the same direction with d. But the Euclid distance between these two document is big, although they are the same in term of content.

![Khoảng cách euclid](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/euclidean_distance.png)

Cho nên, thay vì xếp hạng tài liệu dựa trên khoảng cách Euclid, thì chúng ta nên xếp hạng dựa trên góc giữa tài liệu và câu truy vấn.

Ta có thể thấy góc giữa hai vector càng lớn thì cosin hay điểm xếp hạng sẽ càng thấp. Khi góc giữa vector bằng 0 thì điểm sẽ lớn nhất (=1).

Có hai cách ghi tương đương nhau:

* Xếp hạng tài liệu theo thứ tự giảm dần dựa trên góc giữa query và document
* Xếp hạng tài liệu theo thứ tự tăng dần dựa trên cosin của query và document

![Cos curve](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/cos_curve.jpg)

Tài liệu được xếp hạng bởi giá trị cosine giảm dần: 

* cos(d,q) = 1 khi d = q
* cos(d,q) = 0 khi tài liệu d và query q không có bất kỳ từ chung nào.

### Tại sao phải chuẩn hóa độ dài của tài liệu?

Với cái tài liệu dài (hơn):
Tài liệu dài có tần suất các từ xuất hiện cao hơn (higher term frequencies). Một từ giống nhau sẽ có khả năng xuất hiện thường xuyên hơn.
Có nhiều từ hơn, tăng khả năng xuất hiện của các từ trùng với câu truy vấn.

Sự  “chuẩn hóa cosine” giảm sự ảnh hưởng của tài liệu dài (so với tài liệu ngắn). Một vector có thể được chuẩn hóa (về độ dài) bằng cách chia từng phần tử của nó cho độ dài của nó. Để tính độ dài của vector chúng ta làm như sau (định mức L2 hay L2 norm):

$$\mid\mid\vec x\mid\mid=\sqrt{\sum_i{x^2_i}}$$

Giả sử chúng ta có vector d $$\left[\begin{matrix} 3 \\ 4 \end{matrix}\right] → \sqrt{\sum{x^2}} = \sqrt{3^2 + 4^2}=5$$

Giả sử chúng ta có vector d' $$\left[\begin{matrix} 6 \\ 8 \end{matrix}\right] → \sqrt{\sum{x^2}} = \sqrt{6^2 + 8^2}=10$$

Chia một vector cho định mức L2 của nó sẽ tạo ra một vector đơn vị chiều dài [(unit length vector)](http://en.wikipedia.org/wiki/Unit_vector)

![Khoảng cách euclid](/assets/posts/tim-hieu-ve-mo-hinh-khong-gian-vector/euclidean_distance.png)

Khoảng cách Euclid giữa hai vector là khá lớn (trong khi góc bằng nhau). Sau khi chuẩn hóa hai tài liệu d và d’chúng ta có hai vector hoàn toàn giống nhau  $$\left[\begin{matrix} 0.6 \\ 0.8 \end{matrix}\right]$$. Những tài liệu dài và ngắn bây giờ (sau khi chuẩn hóa) sẽ có trọng số so sánh được.

Mặc dù vậy cách chuẩn hóa này vẫn chưa đúng hoàn toàn, cho nên người ta còn giới thiệu thêm phương pháp "pivoted document length normalization".

### Cosine similarity

$$cos(\vec q, \vec d)= \dfrac{\vec q, \vec d}{\mid\vec q\mid \mid\vec d\mid} = \dfrac{\vec q}{\mid\vec q\mid} . \dfrac{\vec d}{\mid\vec d\mid} = \dfrac{\sum_{i=1}^{\mid v \mid} q_i d_i}{\sqrt{\sum_{i=1}^{\mid v \mid} q^2_i}  \sqrt{\sum_{i=1}^{\mid v \mid} d^2_i}}$$

$$q_i$$ là trọng số tf-idf của từ i trong câu truy vấn.
$$d_i$$ là là trọng số tf-idf của từ i trong tài liệu
$$cos(\vec q, \vec d)$$ là sự tương đồng cosine giữa $$\vec q$$ và $$\vec d$$ hay là cosine của góc giữa $$\vec q$$ và $$\vec d$$

Đối với những vector đã được chuẩn hóa về độ dài, sự tương đồng cosine chỉ đơn giản là tích vô hướng của hai vector (scalar product).

$$cos(\vec q, \vec d) = \vec q . \vec d = \sum_{i=1}^{\mid v \mid} q_i d_i$$
$$\quad\quad \text{for q,d length-normalized}$$

Còn về vấn đề hiện thực hóa những kỹ thuật và lý thuyết ở trên thành code như thế nào thì chúng ta có thể tham khảo thêm bài viết sau: [Short Introduction to Vector Space Model](http://pyevolve.sourceforge.net/wordpress/?p=1589)

### References:

* Christopher D. Manning, Prabhakar Raghavan & Hinrich Schütze, 2008, 'Introduction to Information Retrieval', 27 Oct 2013, <[http://nlp.stanford.edu/IR-book/html/htmledition/irbook.html](http://nlp.stanford.edu/IR-book/html/htmledition/irbook.html)>
* Christian S. Perone, 2011, 'Machine Learning Text feature extraction (tf-idf) – Part I', 27 Oct 2013, <[http://pyevolve.sourceforge.net/wordpress/?p=1589](http://pyevolve.sourceforge.net/wordpress/?p=1589)>
* Nguyễn Tuấn Đăng, 2002, 'Khai mỏ văn bản tiếng Việt với bản độ tự tổ chức', 27 Oct 2013, <[http://www.nsl.hcmus.edu.vn/greenstone/collect/thesiskh/index/assoc/HASH740b.dir/0.pdf](http://www.nsl.hcmus.edu.vn/greenstone/collect/thesiskh/index/assoc/HASH740b.dir/0.pdf)>
