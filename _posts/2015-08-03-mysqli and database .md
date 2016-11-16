---
layout: post
title: Mysqli数据库操作
category: PHP
tag: [php]
date : 2015-08-03
excerpt: Mysqli数据库操作
---

******

<!-- more -->

1.　　与Mysql数据库交互时，首先要建立连接，最后要断开连接，这包括与服务器连接并选择一个数据库，以及最后关闭连接。与Mysql几乎所有的特性一样，这一点可以使用面向对象的方法来完成，也可以采用过程的方式完成。

1.1　　创建一个Mysqli的对象
	
	1 <?php
	2 $mysqli=new mysqli();//实例化mysqli
	3 ?>

1.2　　连接Mysql的主机、用户、密码、数据库

	1 <?php
	2 $mysqli=new mysqli();//实例化mysqli
	3 $mysqli->connect('localhost','root','admin','test');
	4 ?>

1.3　　创建连接参数的Mysqli对象

	1 <?php
	2 $mysqli=new mysqli('localhost','root','admin','test');//实例化mysqli
	3 //$mysqli->connect('localhost','root','admin','test');
	4 ?>

1.4　　更换默认连接的数据库

	1 <?php
	2 $mysqli=new mysqli();//实例化mysqli
	3 $mysqli->connect('localhost','root','admin','test');
	4 $mysqli->select_db('abc');//更换连接默认的数据库为abc数据库
	5 $mysqli->close();//别忘了关闭你的"小资源";
	6 ?>

2　　处理连接错误

2.1　　如果无法连接Mysql数据库，那么这个页面不太可能继续完成预期的工作，因此，一定要注意监视连接错误并相应地做出反应。Mysqli拓展包含有很多特性可以用来抓取错误信息，例如：mysqli_connect_errno() 和 mysqli_connect_error()方法。

1 mysqli_connect_errno()函数返回链接数据库返回的错误号。
2 mysqli_connect_error()函数返回连接数据库返回的错误代码。

使用方法：


	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     echo'数据库连接错误,错误信息是.'.mysqli_connect_error();
	 6     exit();
	 7 }else{
	 8     echo '数据库连接成功';
	 9 }
	10 $mysqli->close();//别忘了关闭你的"小资源";
	11 ?>


　　这里用了面向过程的方法来判断数据库是否连接成功。那么为什么不用面向对象的方法判断呢？我的理解是 判断数据库连接成功 之前首先要证明你要实例化的对象是否可用。。所以用了面向过程的方法来判断。（如果有不对的地方大家也可以一起讨论讨论，我自己也是个小学渣）

 
2.2　　mysqli_error——返回数据库操作错误信息。mysqli_errno——返回数据库操作错误代码


	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     echo'数据库连接错误,错误信息是.'.mysqli_connect_error();
	 6     exit();
	 7 }else{
	 8     echo '数据库连接成功<br/>';
	 9 }
	10 if (!$mysqli->query("SET a=1")) {//$mysqli->query()执行sql语句
	11     echo "错误信息是: ".$mysqli->error.'<br/>';
	12     echo "错误代码是: ".$mysqli->errno;
	13 }
	14 $mysqli->close();//别忘了关闭你的"小资源";
	15 ?>



输出：

数据库连接成功
错误信息是: Unknown system variable 'a'
错误代码是: 1193

 

3　　与数据库进行交互。

① 绝大多数查询都与创建(Creation)、获取(Retrieval)、更新(Update)和删除(Deletion)任务有关，这些任务统称为CRUD。

② 获取数据网页程序大多数工作都是在获取和格式化所请求的数据。为此，要向数据库发送SELECT查询，再对结果进行迭代处理，将各行输出给浏览器，并按照自己的要求输出。　


	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 print_r($result->fetch_row());//将结果集的第一行输出
	11 $result->free();//释放查询内存(销毁)
	12 $mysqli->close();//别忘了关闭你的"小资源";
	13 ?>

![](/images/images/mysql.png)
 

输出：

Array ( [0] => 1 [1] => 秋田嘉 [2] => 123456 )

3.1　　解析查询结果

一旦执行了查询并准备好结果集，下面就可以解析获取到的结果行了。你可以使用多个方法来获取各行中的字段，具体选择哪一个方法主要取决于个人喜好，因为只是引用字段的方法有所不同。

3.1.1　　将结果集放到对象中由于你可能会使用mysqli的面向对象的语法，所以完全可以采用面向对象的方式管理结果集。

可以使用　　fetch_object()　　方法来完成。

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 /*
	11 $row = $result->fetch_object();//将结果集包装成对象
	12 echo $row->user;//输出对象中的一个字段（属性）
	13 */
	14 while (!!$row = $result->fetch_object()){////遍历所有的用户名称
	15 echo $row->user."<br>";
	16 var_dump($row);
	17 }
	18 $result->free();//释放查询内存(销毁)
	19 $mysqli->close();//别忘了关闭你的"小资源";
	20 ?>

输出：

 

秋田嘉 

object(stdClass)#3 (3) { ["id"]=> string(1) "1" ["user"]=> string(9) "秋田嘉" ["psw"]=> string(6) "123456" }
刘德华 

object(stdClass)#4 (3) { ["id"]=> string(1) "2" ["user"]=> string(9) "刘德华" ["psw"]=> string(6) "654321" }

 

3.1.2　　使用索引数组和关联数组　　fetch_array();

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 
	11 $row = $result->fetch_array();//将结果集包装成数组(索引+关联)
	12 echo $row[1].'<br/>';//输出下标是1的字段（属性）
	13 var_dump($row);
	14 /*
	15 while (!!$row = $result->fetch_array()){//遍历全部
	16 echo $row[1]."<br/>";
	17 }*/
	18 
	19 $result->free();//释放查询内存(销毁)
	20 $mysqli->close();//别忘了关闭你的"小资源";
	21 ?>

输出：

秋田嘉
array(6) { [0]=> string(1) "1" ["id"]=> string(1) "1" [1]=> string(9) "秋田嘉" ["user"]=> string(9) "秋田嘉" [2]=> string(6) "123456" ["psw"]=> string(6) "123456" }

 

3.1.3　　使用索引数组　　fetch_row();


	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 $row = $result->fetch_row();//将结果集包装成索引数组
	11 echo $row[2];
	12 $result->free();//释放查询内存(销毁)
	13 $mysqli->close();//别忘了关闭你的"小资源";
	14 ?>

输出：

123456

3.1.4　　使用关联数组　　fetch_assoc();

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 $row = $result->fetch_assoc();//将结果集包装成关联数组
	11 echo $row['psw'];
	12 $result->free();//释放查询内存(销毁)
	13 $mysqli->close();//别忘了关闭你的"小资源";
	14 ?>

输出：

123456

4　　确定所选择的行和受影响的行

　　通常希望能够确定SELECT查询返回的行数，或者受INSERT、UPDATE或DELET查询影响的行数。

我们可以使用num_rows和affected_rows两个属性

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 echo $result->num_rows;//当使用查询时，想了解SELECT查询了多少行，可以使用num_rows。
	11 echo"<br/>";
	12 echo $mysqli->affected_rows;//当使用查询时，想了解SELECT、INSERT、UPDATE、DELETE查询时影响的行数，可以使用affected_rows;注意，它是$mysqli下的属性
	13 $result->free();//释放查询内存(销毁)
	14 $mysqli->close();//别忘了关闭你的"小资源";
	15 ?>

输出：

2
2

5　　移动指针的操作和获取字段

　　当你并不想从第一条数据开始获取，或者并不想从第一个字段获取，你可以使用数据指针移动或者字段指针移动的方式调整到恰当的位置。当然，你还可以获取字段的名称及其相关的属性

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 $sql='select * from user';//创建一句SQL语句
	 9 $result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	10 echo $result->field_count;//计算有多少条字段
	11 echo"<br/>";//输出 3
	12 $field = $result->fetch_field();//获取字段的名称
	13 echo $field->name;//输出id
	14 echo '<br/>';
	15 while (!!$field = $result->fetch_field()) {//遍历字段
	16 echo $field->name.'<br />';
	17 }
	18 $result->free();//释放查询内存(销毁)
	19 $mysqli->close();//别忘了关闭你的"小资源";
	20 ?>

输出：

3
id
user
psw

5.1

	<?php
	$mysqli=new mysqli();//实例化mysqli
	$mysqli->connect('localhost','root','admin','test');
	if(mysqli_connect_error()){
	    exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	}
	$mysqli->set_charset("UTF8");//设置数据库编码
	$sql='select * from user';//创建一句SQL语句
	$result=$mysqli->query($sql);//执行sql语句把结果集赋给$result
	print_r($result->fetch_fields());//一次性取得字段数组
	$result->free();//释放查询内存(销毁)
	$mysqli->close();//别忘了关闭你的"小资源";
	?>

输出：

	 1 Array
	 2 (
	 3     [0] => stdClass Object
	 4         (
	 5             [name] => id
	 6             [orgname] => id
	 7             [table] => user
	 8             [orgtable] => user
	 9             [def] => 
	10             [db] => test
	11             [catalog] => def
	12             [max_length] => 1
	13             [length] => 11
	14             [charsetnr] => 63
	15             [flags] => 49667
	16             [type] => 3
	17             [decimals] => 0
	18         )
	19 
	20     [1] => stdClass Object
	21         (
	22             [name] => user
	23             [orgname] => user
	24             [table] => user
	25             [orgtable] => user
	26             [def] => 
	27             [db] => test
	28             [catalog] => def
	29             [max_length] => 9
	30             [length] => 765
	31             [charsetnr] => 33
	32             [flags] => 4097
	33             [type] => 253
	34             [decimals] => 0
	35         )
	36 
	37     [2] => stdClass Object
	38         (
	39             [name] => psw
	40             [orgname] => psw
	41             [table] => user
	42             [orgtable] => user
	43             [def] => 
	44             [db] => test
	45             [catalog] => def
	46             [max_length] => 6
	47             [length] => 765
	48             [charsetnr] => 33
	49             [flags] => 4097
	50             [type] => 253
	51             [decimals] => 0
	52         )
	53 
	54 )

5.2　　

1 $result->data_seek(1);//移动数据指针
2 $result->field_seek(1);//移动字段指针

 
 6　　执行多条SQL语句

　　有的时候，我们需要在一张页面上同时执行多条SQL语句，之前的方法就是分别创建多个结果集然后使用。但这样资源消耗很大，也不利于管理。 PHP提供了执行多条SQL语句的方法$mysqli->multi_query();

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 //创建多条SQL语句
	 9 $sql .= "SELECT * FROM user;";
	10 $sql .= "SELECT * FROM photo;";
	11 $sql .= "SELECT * FROM article";
	12 if ($mysqli->multi_query($sql)) {//开始执行多条SQL语句
	13 $result = $mysqli->store_result();
	14 print_r($result->fetch_array());//开始获取第一条SQL语句的结果集
	15 $mysqli->next_result();//将结果集指针移到下一个
	16 $result = $mysqli->store_result();
	17 print_r($result->fetch_array());
	18 $mysqli->next_result();
	19 $result = $mysqli->store_result();
	20 print_r($result->fetch_array());
	21 } else {
	22 echo 'sql语句有误！';
	23 }
	24 $result->free();//释放查询内存(销毁)
	25 $mysqli->close();//别忘了关闭你的"小资源";
	26 ?>

 
7　　执行数据库事务

事务(transaction)是作为整个一个单元的一组有序的数据库操作。如果一组中的所有操作都成功，则认为事务成功，即使只有一个失败操作，事务也不成功。如果所有操作成功完成，事务则提交(commit)，其修改将作用于所有其他数据库进程。如果一个操作失败，则事务将回滚(roll back)，该事务所有操作的影响都将取消。首先，您的MySQL是InnoDB或BDB引擎的一种，一般来说，你安装了AppServ的集成包，你选择InnoDB的引擎的数据库即可。如果你建立的表不是InnoDB，可以在phpmyadmin里修改

	 1 <?php
	 2 $mysqli=new mysqli();//实例化mysqli
	 3 $mysqli->connect('localhost','root','admin','test');
	 4 if(mysqli_connect_error()){
	 5     exit('数据库连接错误,错误信息是.'.mysqli_connect_error());
	 6 }
	 7 $mysqli->set_charset("UTF8");//设置数据库编码
	 8 //首先你必须关闭自动提交数据
	 9 $mysqli->autocommit(false);
	10 //创建一个SQL语句，必须同时运行成功，不能出现一个成功，一个失败
	11 $sql .= "UPDATE `friend` SET state=state+5 WHERE id=1;";
	12 $sql .= "UPDATE `flower` SET tg_flower=flower-5 WHERE id=1;";
	13 //执行两条SQL语句
	14 if ($mysqli->multi_query($sql)) {
	15 //获取第一条SQL一影响的行数
	16 $success = $mysqli->affected_rows == 1 ? true : false;
	17 //下移，第二条SQL
	18 $mysqli->next_result();
	19 //获取第二条SQL影响的行数
	20 $success2 = $mysqli->affected_rows == 1 ? true : false;
	21 //判断是否都正常通过了，两个SQL
	22 if ($success && $success2) {
	23 $mysqli->commit();
	24 echo '完美提交！';
	25 } else {
	26 $mysqli->rollback();
	27 echo '程序出现异常！';}
	28 } else {
	29 echo "SQL语句有误：".$mysqli->errno.$mysqli->error;
	30 }
	31 //最后还必须开启自动提交
	32 $mysqli->autocommit(true);
	33 $result->free();//释放查询内存(销毁)
	34 $mysqli->close();//别忘了关闭你的"小资源";
	35 ?>