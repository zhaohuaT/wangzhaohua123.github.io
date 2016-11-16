---
layout: post
title: Pdo数据库操作
category: PHP
tag: [php]
date : 2015-08-05
excerpt: Pdo数据库操作
---


******
<!-- more -->

##PDO数据库总结

1 PDO 使用的必要性：

       Mysqli  虽然比MySQL更加优化但是只支持mysql 数据库，PDO 支持大部分数据库。而PDO支持大部分数据库，同时又很好的与处理机制、错误处理机制、面向对象的变成模式。

2 PDO   数据库的流程

      （实例化PDO 对象 à 通过传递的数据库类型判断炒作数据库的方法，然后通过传过来的数据库信息连接数据库 à 连接成功通过PDO 的方法、属性及常量操作数据库进行增删改查）

这个过程就是PDO 数据库抽象层

3   PDO：mysql 官方封装的，采用的是面向对象的编程思想，使用c 语言开发的数据库抽象层

4   如何使用：

     
      B 实例化PDO 对象，调用属性，方法。

      <?PHP

//连接数据库的信息

//第一个参数:   连接数据库的类型:主机名；数据库名

$db="mysql:host=localhost;dbname=user";

$user="root";

$pas="";

$pdo =new PDO($db,$user,$pas);

$sql="select* from user limit 2";

//查询语句使用  query() 返回的是PDOstatement对象

//如果想获得具体的数据   则需要获得这个对象的方法   fetchAll()参数是类常量，表示返回什么格式的数据

$result=$pdo->query($sql);

$rows=$result->fetchAll(PDO::FETCH_BOTH);

var_dump($rows);

//增删改使用   PDOstatement 对象的exec()

$sql1="updateuser set user_name='he' where user_id='1'";

$e=$pdo->exec($sql1);

var_dump($e);

?>

5 预编译处理机制

      流程：把操作的代码与要操作的数据分离开 à用PDO->prepare() 把操作的代码预编译 à然后用预编译后返回的pdostatement对象的bindparam()绑定上数据à用预编译后返回的pdostatement对象的excute()方法执行代码

      <?php

//预编译：  PDO::prepare($sql);   返回PDOstatement对象

//给预编译的结果绑定数据：PDOstatement->bindParam();  

//执行编译结果：PDOstatement->execute();

$db="mysql:host=localhost;dbname=user";

$user="root";

$pas="";

$pdo =new PDO($db,$user,$pas);

//先将sql语句中的数据部分用占位符代替     : 占位符名

$sql="insertinto user values(null,:name,:pass,:email)";

//预编译  prepre() 参数是不带任何数据的sql语句

$re=$pdo->prepare($sql);             //返回一个PDOstatement对象

//绑定数据       返回的PDOstatement->bindParam(); 

$name="hai";

$pass_word='123';

$email="com";

$re->bindParam(':name',$name);

$re->bindParam(':pass',$pass_word);

$re->bindParam(':email',$email);

$re->execute();

?>

6 PDO 错误处理机制

      在mysql中mysql_error()  mysql_errno() 获得错误的信息和错误的编号

      PDO 默认情况下不提示错误，只提示语法错误

      ErrorInfo() 获得错误的信息

      ErrorCode() 获得错误的编号

      <?php

//PDO::quote()方法，处理用户数据

//习惯上，我们将需要参与sql执行的用户数据，采用quote方法进行处理，保证数据的正确性

$db="mysql:host=localhost;dbname=user";

$user="root";

$pas="";

$pdo =new PDO($db,$user,$pas);

$sql="select* from goods limit 2";

$re=$sql->fetchAll(PDO::FETCH_BOTH);

$pdo->errorInfo();

?>

通常默认的是静默模式：不提示

警告模式：发出警告但是不影响代码继续向下执行

异常模式：出现错误后抛出一个异常，需要捕获异常并作出处理否则会出现致命错误不再向下执行。

如何切换模式：

      使用pdo 的setAttribute(模式名称，模式的值)

      模式名称：PDO：：ATTR——ERRMODE

      模式的值：PDO::ERRMODE_WARNING警告 

            PDO::ERRMODE_SILENT 静默 

PDO::ERRMODE_EXCEPTION异常

抛出异常及处理：

<?php

$db="mysql:host=localhost;dbname=user";

$user="root";

$pas="";

$pdo =new PDO($db,$user,$pas);

//默认情况下 PDO是静默模式，如何切换呢？通过setAttribute()来切换模式，使用PDO这个类的方法

   //同样，errorInfo()  errorCode() 这些方法也是属于PDO这个类的，那么只能是PDO这个类的实例才能调用

   //模式也是

   //$pdo ->setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_WARNING);

   //异常模式：

   $pdo->setAttribute(PDO::ATTR_ERRMODE,PDO::ERRMODE_EXCEPTION);

   $sql = 'select* from good';

   try{

        //尝试可能会出错误的代码

        $pdo ->query($sql);

   }catch(PDOException$e){

        //现在捕获异常后，自己看着办，是让他显示出来呢，还是输出到日志文件里呢？

        //通常是将错误信息输出到日志文件里

        //var_dump($e->getMessage());

        file_put_contents('D://mysql.log',$e->getMessage());

   }

?>

补充：

1，测试 PDO类的quote() 方法的作用

      quote可以给数据加上自身所带的符号，例如传递过来的变量值需要加单引号

2，比较PDOStatement对象的fetch(), fetchAll(), fetchColumn()方法的区别

      fetchAll()    获取pdostatement对象中的所有行数据 参数有FETCH_BOTH，FETCH_NUM，FETCH_ASSOC

      fetch()获取查询结果中一行记录并可以通过参数的不同显示的方式不同，可用循环实现显示所有航记录

      fetchColumn()  获得下一行的一列  可以通过循环获取多行的单一的列
