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

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification

Nguyên tắc này có thể được hiểu rằng: các class hoặc hàm nên được thiết kể để mở rộng, nhưng đóng lại để tránh sự thay đổi trực tiếp mã nguồn. Điều này có nghĩa là hệ thống nên được thiết để hỗ trợ các lập trình viên sau này có extend các class có sẵn để cung cấp thêm chức năng thay vì chỉnh sửa trên mã nguồn tồn tại sẵn trong hệ thống.


# 3. Liskov substitution principle
Tạm dịch: Nguyên tắc hoán đổi Liskov

> The Liskov substitution principle (LSP) states that objects should be replacable with instances of their subtypes without alterting the correctness of that program

Nguyên tắc này nghe có vẻ phức tạp, nhưng thực chất có thể diễn ý đơn giản lại như sau: Nếu một class có sử dụng một implemtation của một interface, thì nó phải được thay thế dễ dàng bởi các implementation của interface đó mà không cần sửa gì thêm.

Ví dụ này được lấy lại từ bài viết trước. Chúng ta có thể thấy rằng StandardLogger nad FileLogger cùng implement một interface và chúng có thể thay đổi dễ dàng với nhau mà không cần chỉnh sửa mã nguồn của MyLog.
 
{% highlight php %}
<?php
interface LoggerInterface 
{
    function info($message);
}

class StandardLogger implements LoggerInterface
{

    public function info($message)
    {
        printf("[INFO] %s \n", $message);
    }
}

class FileLogger implements LoggerInterface 
{

    public function info($message) 
    {
        file_put_contents('app.log', sprintf("[INFO] %s \n", $message), FILE_APPEND);
    }
}

class MyLog 
{
    public $logger;

    public function __construct(LoggerInterface $logger) 
    {
        $this->logger = $logger;
    }

    public function info($string)
    {
        return $this->logger->info($string);
    }
}
// Print to standard input/output device
$myLog = new MyLog(new StandardLogger);
$myLog->info('This object depend on another object');
// Write to file
$myFileLog = new MyLog(new FileLogger);
$myFileLog->info('This object depend on another object'); 
{% endhighlight %}

# 4. Interface segregation principle
Tạm dịch: Nguyên tắc tách rời giao diện (lập trình)

> The interface-segregation principle (ISP) states that no client should be forced to depend on methods it does not use.

Nguyên tắc này phát biểu rằng implementation của một interface không nên bị phụ thuộc vào những methods mà nó không dùng. Điều này có nghĩa là các interface phải được sắp xếp và phân chia hợp lý. Thay vì có một **fat** interface chứa tất cả các methods cần được thi công thì nó nên được chia nhỏ ra mà class nào implement nó cũng không có method **thừa**.


# 5. Dependency inversion principle
Tạm dịch: Nguyên tắc nghịch đảo phụ thuộc

