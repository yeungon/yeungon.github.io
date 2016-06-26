---
layout: post
title: "Laravel : Dependency Injection và IoC conainter"
date: 2016-06-25 22:20 PM
categories: [web-development]
author: hungnq1989
tags : [dependency-injection, ioc-container, php, laravel]
description: "Laravel : Dependency Injection và IoC conainter"
image: /assets/posts/tu-dong-nghia-trong-elasticsearch/analyzer.png
comments: true
published: false
---

# Dependency Injection là gì?

Nói ngắn gọn, thì dependency injection (DI) chỉ đơn giản là cung cấp cho một object những object nó phụ thuộc (dependencies) từ bên ngoài truyền vào mà không phải khởi tạo từ trong hàm dựng. Điều này giúp ứng dụng linh động hơn và dễ test hơn, vì ta có thể dễ dàng dùng Mock class để test.

Thử lấy một ví dụ tổng quát không sử dụng kỹ thuật DI như sau:

{% highlight php %}
<?php
class StandardLogger {

    public function info($message)
    {
      printf("[INFO] %s \n", $message);
    }
}


class Log {
    public $logger;

    public function __construct() {
        $this->logger = new StandardLogger();
    }

    public function info($string)
    {
        return $this->logger->info($string);
    }
}

//Main application, somewhere else
$log = new Log();
$log->info('This object depend on another object');
{% endhighlight %}

Ví vụ trên là một ví dụ mang tính tượng trưng, nhưng vấn đề chính được nêu ra là class Log bị phụ thuộc vào class StandardLogger. Nói cách khác là class Log bị dính chặt vào class StandardLogger. Hiện tại thì khi chúng ta muốn chuyển sang một loại logger khác (ví dụ như FileLogger hay MongoDBLogger) chúng ta phải sửa lại hàm dựng của class Log.


Để giải quyết vấn đề phụ thuộc này, chúng ta chỉ cần sửa lại hàm dựng của class Log nhận một tham số là logger là xong. Hay còn gọi là **decouple** hàm dựng của class Log với những class khác mà nó phụ thuộc. Việc này đơn giản được gọi là **Dependency Injection**. Tah-dah, đơn giản quá phải không? Cho nên có người nói DI là một cái tên 25 dollars cho một khái niệm trị giá 5 cents. Nguyên văn của [James Shore](http://www.jamesshore.com/Blog/Dependency-Injection-Demystified.html):

> "Dependency Injection" is a 25-dollar term for a 5-cent concept. [...] Dependency injection means giving an object its instance variables. [...].

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

class Log 
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
$log = new Log(new StandardLogger);
$log->info('This object depend on another object');
// Write to file
$log = new Log(new FileLogger);
$log->info('This object depend on another object'); 
{% endhighlight %}

# IoC Conainter là gì?

Sau khi áp dụng kỹ thuật DI này, thì một vấn đề khác lại nảy sinh, đó là làm thế nào chúng ta biết được lớp Log này phụ thuộc vào những lớp nào để khởi tạo nó? Việc này nghe có vẻ đơn giản, nhưng có khả năng xảy ra trường hợp lớp Log phụ thuộc vào lớp MySQLLogger, còn lớp MySQLLogger phụ thuộc vào lớp DatabaseAccess nào đó. Và nó gây rất nhiều khó khăn cho việc khởi tạo một object mà chúng ta cần, bởi vì danh sách các lớp phụ thuộc lồng nhau rất sâu (deeply nested class dependencies).

Để giải quyết điều này, người ta nghĩ ra Dependency Injection Container hay còn gọi là Inversion of Control Containter (IoC Container). Thuật ngữ Inversion of Control mang tính tổng quá hơn Dependency Injection, từ đây về sau mình sẽ dù IoC Container thay cho Dependecy Injection Container. Về bản chất thì IoC Conainter là một tấm bản đồ, hay một dịch vụ đăng ký. Nó cho ta biết một lớp phụ thuộc vào những lớp nào khác. Nó có thể phân giải được những class phụ thuộc bằng kỹ thuật Reflection, hoặc từ danh sách đã được developer đăng ký trước đó.

Thật ra mà nói DI là một khái niệm không mới mẻ gì, nhất là trong thế giới Java. Martin Folwer đã viết về [Dependency Pattern và IoC container từ năm 2004](http://martinfowler.com/articles/injection.html). Tuy nhiên cho đến gần đầy thì mới có một vài frameworks PHP sử dụng DI và IoC container như [Laravel](https://laravel.com/docs/4.2/ioc) hay [Pimple](http://pimple.sensiolabs.org/).

