# PHP+MySQL博客（初级）

使用`html+php+mysql`，只有登录，注册，注销功能。

> 此版本为初级版，功能为最基础功能。
>
> 更多功能请移步：[PHP+MySQL博客（高级）]
>
> 高级版功能如下：（html+php+mysql+css）
>
> 1. 登录注册
> 2. 用户密码修改
> 3. 头像上传及修改
> 4. 发布文章、浏览文章
> 5. 文章评论及回复

## connect.php

```php
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NUC 连接</title>
</head>

<?php
$servername = "localhost";
$username = "root";
$password = "root";
$dbname = "study";

$conn = new mysqli($servername, $username, $password);

if ($conn->connect_error) {
    die("数据连接失败" . $conn->connect_error);
}

$conn2 = new mysqli($servername, $username, $password, $dbname);

if ($conn2->connect_error) {
    echo ("无法连接到数据库，开始自动创建数据库<br>");

    $sql = "CREATE DATABASE IF NOT EXISTS " . $dbname;

    if ($conn->query($sql) === TRUE) {
        echo "数据库创建成功";

        $createtable = "CREATE TABLE IF NOT EXISTS study_tbl(
            `id` INT(6) AUTO_INCREMENT PRIMARY KEY,
            `user` VARCHAR(30) NOT NULL,
            `pass` VARCHAR(30) NOT NULL,
            `phone` VARCHAR(11) NOT NULL,
            `file` VARCHAR(30)
        )";

        if ($conn2->query($createtable) === TRUE) {
        } else {
            die("数据表创建失败" . $conn2->connect_error);
        }
    } else {
        die("数据库创建失败" . $conn->connect_error);
    }
} else {
    $createtable = "CREATE TABLE IF NOT EXISTS study_tbl(
        `id` INT(6) AUTO_INCREMENT PRIMARY KEY,
        `user` VARCHAR(30) NOT NULL,
        `pass` VARCHAR(30) NOT NULL,
        `phone` VARCHAR(11) NOT NULL,
        `file` VARCHAR(30)
    )";

    if ($conn2->query($createtable) === TRUE) {
    } else {
        die("数据表创建失败" . $conn2->connect_error);
    }
}
?>
```



## denglu.php

```php
<?php
include 'connect.php';
$username = $_POST['username'];
$password = $_POST['password'];
if (isset($username) && isset($password)) {
    $chaxun = "select user,pass from study_tbl where user='$username' and pass='$password';";
    $result = $conn2->query($chaxun);
    if ($result->num_rows > 0) {
        session_start();
        $_SESSION['login'] = "true";
        header('location:youxi.php');
    } else {
        $login = "登陆失败";
        // session_start();
        $_SESSION['login'] = "false";
        // session_destroy();
    }
} else {
    $login = "用户和密码不能为空！";
}
?>


<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NUC 登录</title>
</head>

<body>
    <h1>欢迎来到此页面进行登录</h1>
    <form action="" method="POST">
        账户：<input type="text" name="username" value=""><br>
        密码：<input type="password" name="password" value=""><br>
        <input type="submit" value="提交"><br>
    </form>
    <a href="zhuce.php">点击此处进行注册</a><br>
    <?php echo "$login" ?>
</body>

</html>
```



## session.php

```php
<?php
session_start();
// echo $_SESSION['login'];
if ($_SESSION['login'] == true) {
    echo "您已登陆成功，<a href=zhuxiao.php>点击注销</a><br>";
} else {
    $_SESSION["login"] == false;
    die("您无权访问，<a href=denglu.php>点击此处登录</a><br>");
}
?>
```



## youxi.php

```HTML
<?php
include "session.php";
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NUC 游戏</title>
    <h1>游戏页面</h1>
    <a href="select.php">点击账号以及ID的对应关系</a><br>
    <a href="jsp.php">数字炸弹</a><br>
</head>

</html>
```



## zhuce.php

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NUC 注册</title>
</head>

</html>


<?php
include "connect.php";
$name = $_POST['name'];
$password1 = $_POST['password1'];
$password2 = $_POST['password2'];
$phone = $_POST['phone'];
$tupian = $_FILES['tupian'];

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    if (empty($name) || empty($password1) || empty($password2) || empty($phone)) {
        $ERR = "用户、密码和手机号码不能为空";
    } else if (strlen($password1) < 6) {
        $ERR = "密码长度不足六位";
    } else if ($password1 != $password2) {
        $ERR = "两次输入的密码不一致";
    } else if (strlen($phone) != "11") {
        $ERR = "手机号码格式有问题";
    } else {
        if (file_exists("tupian/" . $_FILES['tupian']['name'])) {
            echo $_FILES['tupian']['name'] . "文件已经存在";
        } else {
            move_uploaded_file($_FILES["tupian"]["tmp_name"], "tupian/" . $_FILES["tupian"]["name"]);
        }

        
        $upload = "insert into study_tbl(user,pass,phone,file) value('$name','$password1','$phone','$tupian');";
        if ($conn2->query($upload) === TRUE) {
            $ERR = "注册成功！";
        } else {
            $ERR = "注册失败";
        }
    }
}

?>

<form action="" method="POST" enctype="multipart/form-data">
    名字：<input type="text" name="name"> <br>
    密码：<input type="password" name="password1"><br>
    重新输入密码：<input type="password" name="password2"><br>
    请输入手机号码：<input type="text" name="phone"><br>
    上传头像:<input type="file" name="tupian"></br>
    <input type="submit" value="提交">
</form>

<a href="denglu.php">点此返回登录页面</a><br>

<?php
echo $ERR;
echo "你注册的用户为：" . $name . "<br>";
echo "你注册的手机号码为：" . $phone . "<br>";
?>

<img src="<?php echo "tupian/" . $_FILES["tupian"]["name"]; ?>" alt="上传的图片" />
```



## zhuxiao.php

```php
<?php
session_start();
$_SESSION['login'] = "false";
session_destroy();
header('location:denglu.php');
?>
```

