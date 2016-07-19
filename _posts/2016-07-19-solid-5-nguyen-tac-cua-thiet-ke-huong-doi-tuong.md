---
layout: post
title: "SOLID - 5 nguyên tắc của thiết kế hướng đối tượng"
date: 2016-07-19 2:10 AM
categories: [web-development, progrmaming, oriented-design]
author: hungnq1989
tags : [php, software-engineering, oriented-design]
description: "Nguyên tắc SOLID cơ bản kèm ví dụ"
image: /assets/images/laravel.jpg
comments: true
published: false
---

!["Laravel"](/assets/images/solid.jpg "solid princiles"){: .center-image }

# 0. Mục lục
1. Single responsibility principle
2. Open/closed principle
3. Liskov substitution principle
4. Interface segregation principle
5. Dependency inversion principle

# 1. Single responsibility principle
Tạm dịch: Nguyên tắc đơn nhiệm
Nguyên tắc đơn nhiệm phát biểu rằng: mỗi class hay method chỉ nên có một và chỉ một công năng (a.ka. chịu trách nhiệm cho một việc). <s>Thường thì nguyên tắc này rất dễ bị vi phạm, đặc biệt là những người mới học lập trình. Kiểu như một class Util chịu trách nhiệm cho mọi chức năng trong ứng dụng.</s>

Nguyên tắc này được lần đầu giới thiệu bởi [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_Cecil_Martin) (Principles of Object Oriented Design), ông định nghĩa rằng "responsibility" là **một lý do để thay đổi**

>In the context of the Single Responsibility Principle (SRP) we define a responsibility to be “a reason for change.” If you can think of more than one motive for changing a class, then that class has more than one responsibility. - [Bob Martin](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)

Đổi với method, tương đối dễ để áp dụng nguyên tắc này. Ví dụ trong lập trình game hàm kiểm tra va chạm (tạm gọi `detectCollision()`) không nên thay đổi điểm của người chơi, nếu hàm `detectCollision()` làm một việc khác ngoài việc "detect collision" thì nó đã vi phạm nguyên tắc đơn nhiệm.

# 2. Open/closed principle
Tạm dịch: Nguyên tắc đóng/mở


# 3. Liskov substitution principle
Tạm dịch: Nguyên tắc hoán đổi Liskov


# 4. Interface segregation principle
Tạm dịch: Nguyên tắc tách rời giao diện (lập trình)

# 5. Dependency inversion principle
Tạm dịch: Nguyên tắc nghịch đảo phụ thuộc

