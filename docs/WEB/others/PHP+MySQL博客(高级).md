# PHP+MySQL博客（高级）



#### 功能如下：（html+php+mysql+css）

1. 登录注册
2. 用户密码修改
3. 头像上传及修改
4. 发布文章、浏览文章
5. 文章评论及回复



#### 文件说明：

1. `connect.php` 用于连接数据库等操作，其他文件包含此文件。
2. `article_list.php` 用于文章、评论、回复的显示，以及评论和回复的发表
3. `article.html` 博文发表的纯html框架，数据提交至publish_article.php
4. `change.php` 用于修改密码
5. `homepage.php` 主页面，包含五个功能入口，分别是个人信息、博客发表、博客浏览、头像修改、密码修改
6. `index.html` 登陆界面，数据提交至login.php
7. `login.php` 在后端进行登陆验证，数据由index.html提交
8. `persion.php` "个人信息"功能页面，用于显示用户电话、头像等
9. `publish_article.php` 向数据库中提交文章（标题、正文、作者、提交时间），数据由article.html提交
10. `register.php` 用于注册，php和html包含在同一文件中
11. `session.php` 用于处理session会话，其他文件包含此文件
12. `upimg.html` "头像修改”功能，纯html框架，数据提交至upimg.php
13. `upimg.php` "头像修改”功能，数据由upimg.html提交
14. `zhuxiao.php` 注销功能（退出登录）



#### 改进空间

1. 对中文支持并不友好（头像上传、博客发表等）
2. 只进行了最简单的功能实现，SQL注入等并没有防御措施
3. CSS都是内联在单个文件之内的，之后可以进行外联



#### 其他说明：

1. 图片存放于根目录下的 tupian 文件夹

2. 此博客使用`MySQL`，使用了四个数据库，分别是 user、replies、comments、articles
   至于每个表中具体字段，建议查看或直接运行此SQL文件：



##### SQL 仅结构

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : localhost_3306
 Source Server Type    : MySQL
 Source Server Version : 50553
 Source Host           : localhost:3306
 Source Schema         : test

 Target Server Type    : MySQL
 Target Server Version : 50553
 File Encoding         : 65001

 Date: 20/02/2024 12:57:55
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for articles
-- ----------------------------
DROP TABLE IF EXISTS `articles`;
CREATE TABLE `articles`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `author` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `publish_date` datetime NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 4 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for comments
-- ----------------------------
DROP TABLE IF EXISTS `comments`;
CREATE TABLE `comments`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `article_id` int(11) NOT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `username` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `created_at` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 2 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for replies
-- ----------------------------
DROP TABLE IF EXISTS `replies`;
CREATE TABLE `replies`  (
  `id` int(11) NOT NULL,
  `comment_id` int(11) NULL DEFAULT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `username` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `created_at` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`  (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `user` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `pass` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `photo` varchar(225) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `phone` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 8 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;

```



##### SQL 结构和数据

```sql
/*
 Navicat Premium Data Transfer

 Source Server         : localhost_3306
 Source Server Type    : MySQL
 Source Server Version : 50553
 Source Host           : localhost:3306
 Source Schema         : test

 Target Server Type    : MySQL
 Target Server Version : 50553
 File Encoding         : 65001

 Date: 20/02/2024 12:58:02
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for articles
-- ----------------------------
DROP TABLE IF EXISTS `articles`;
CREATE TABLE `articles`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `title` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `author` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `publish_date` datetime NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 4 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of articles
-- ----------------------------
INSERT INTO `articles` VALUES (1, 'Test', 'This is a test.', 'hou', '2024-02-18 00:00:00');
INSERT INTO `articles` VALUES (3, '2-Test', 'adadadadad', 'Aniston', '2024-02-19 21:04:56');

-- ----------------------------
-- Table structure for comments
-- ----------------------------
DROP TABLE IF EXISTS `comments`;
CREATE TABLE `comments`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `article_id` int(11) NOT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `username` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `created_at` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 2 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of comments
-- ----------------------------
INSERT INTO `comments` VALUES (1, 1, 'This is a comment.', 'pinglun', '2024-02-19 20:22:03');

-- ----------------------------
-- Table structure for replies
-- ----------------------------
DROP TABLE IF EXISTS `replies`;
CREATE TABLE `replies`  (
  `id` int(11) NOT NULL,
  `comment_id` int(11) NULL DEFAULT NULL,
  `content` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `username` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `created_at` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of replies
-- ----------------------------
INSERT INTO `replies` VALUES (0, 1, 'This is a replie.', 'huifu', '2024-02-19 20:30:25');

-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`  (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `user` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `pass` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL,
  `photo` varchar(225) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `phone` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = MyISAM AUTO_INCREMENT = 8 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES (1, 'Aniston', '123123', 'tupian/Pic.jpg', NULL);
INSERT INTO `user` VALUES (2, 'hou', '123123', 'tupian/te.jpg', '12312341234');

SET FOREIGN_KEY_CHECKS = 1;

```





## 具体文件代码：

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
$dbname = "test";

$conn = new mysqli($servername, $username, $password, $dbname);

// 设置字符编码，防止中文乱码
mysqli_query($conn, 'set names utf8');
mysqli_query($conn, 'set character set utf8');

if ($conn->connect_error) {
    die("数据连接失败" . $conn->connect_error);
}
?>
```



## article_list.php

```php
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>文章浏览</title>
    <style>
        /* 文章整体布局 */
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 2em auto;
            max-width: 800px;
            padding: 0 20px;
        }

        h2 {
            font-size: 24px;
            color: #333;
            font-weight: bold;
            line-height: 1.5;
            margin-top: 1em;
            margin-bottom: 0.5em;
            text-align: center;
        }

        /* 文章内容样式 */
        .article {
            border: 1px solid #ccc;
            border-radius: 5px;
            margin-bottom: 2em;
            padding: 1em;
        }
        
        .article p {
            margin: 0.5em 0;
        }
        
        /* 评论样式 */
        .comments {
            margin-top: 1em;
        }
        
        .comment,
        .reply {
            margin-bottom: 1em;
            border-left: 2px solid #ddd;
            padding-left: 1em;
        }
        
        .comment h3,
        .reply h3 {
            margin-bottom: 0.5em;
            font-size: 18px;
            color: #666;
        }
        
        .comment form,
        .reply form {
            display: flex;
            flex-direction: column;
        }
        
        .comment form input,
        .comment form textarea,
        .reply form input,
        .reply form textarea {
            margin-bottom: 0.5em;
            width: 100%;
        }
        
        .comment form input[type="submit"],
        .reply form input[type="submit"] {
            background-color: #007BFF;
            border: none;
            color: white;
            cursor: pointer;
            padding: 0.5em 1em;
        }
        
        .comment form input[type="submit"]:hover,
        .reply form input[type="submit"]:hover {
            background-color: #0056b3;
        }
    </style>
</head>

<body>
<h2>文章浏览</h2>



<?php
error_reporting(0);
// 连接数据库
include "connect.php";
include "session.php";
// $conn = mysqli_connect('localhost', 'root', 'root', 'test');

// 处理评论表单的提交
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // 提交的表单数据
    if (isset($_POST['comment_id']) && isset($_POST['reply_content']) && isset($_POST['reply_username'])) {
        $commentId = $_POST['comment_id'];
        $replyContent = $_POST['reply_content'];
        $replyUsername = $_POST['reply_username'];

        // 将回复插入到数据库
        $replySql = "INSERT INTO replies (comment_id, content, username, created_at) VALUES ($commentId, '$replyContent', '$replyUsername', NOW())";
        mysqli_query($conn, $replySql);
    } elseif (isset($_POST['article_id']) && isset($_POST['comment_content']) && isset($_POST['comment_username'])) {
        $articleId = $_POST['article_id'];
        $commentContent = $_POST['comment_content'];
        $commentUsername = $_POST['comment_username'];

        // 将评论插入到数据库
        $commentSql = "INSERT INTO comments (article_id, content, username, created_at) VALUES ($articleId, '$commentContent', '$commentUsername', NOW())";
        mysqli_query($conn, $commentSql);
    }
}

// 从数据库中检索文章列表
$sql = "SELECT * FROM articles";
$result = mysqli_query($conn, $sql);

echo "<br><a href=homepage.php>点击返回主页</a></br>";


// 显示文章列表
while ($row = mysqli_fetch_assoc($result)) {
    echo '<h2>' . $row['title'] . '</h2>';
    echo '<p>作者：' . $row['author'] . '</p>';
    echo '<p>发布日期：' . $row['publish_date'] . '</p>';
    echo '<p>' . $row['content'] . '</p>';

    // 从数据库中获取文章评论
    $articleId = $row['id'];

    $commentsSql = "SELECT id, content, username, created_at FROM comments WHERE article_id = $articleId";
    $commentsResult = mysqli_query($conn, $commentsSql);

    // 显示文章评论
    while ($commentRow = mysqli_fetch_assoc($commentsResult)) {
        echo '<div>';
        echo '<p>评论人：' . $commentRow['username'] . '</p>';
        echo '<p>' . $commentRow['content'] . '</p>';
        echo '<p>' . $commentRow['created_at'] . '</p>';

        // 显示评论的回复
        $commentId = $commentRow['id'];
        $repliesSql = "SELECT content, username, created_at FROM replies WHERE comment_id = $commentId";
        $repliesResult = mysqli_query($conn, $repliesSql);

        while ($replyRow = mysqli_fetch_assoc($repliesResult)) {
            echo '<div>';
            echo '<p>回复人：' . $replyRow['username'] . '</p>';
            echo '<p>' . $replyRow['content'] . '</p>';
            echo '<p>' . $replyRow['created_at'] . '</p>';
            echo '</div>';
        }

        // 显示回复表单
        echo '<form action="" method="post">';
        echo '<input type="hidden" name="comment_id" value="' . $commentRow['id'] . '" />';
        echo '<input type="text" name="reply_username" placeholder="回复人用户名">';
        echo '<textarea name="reply_content" placeholder="回复内容"></textarea>';
        echo '<input type="submit" value="回复" />';
        echo '</form>';
    }

    // 显示评论表单
    echo '<form action="" method="post">';
    echo '<input type="hidden" name="article_id" value="' . $articleId . '" />';
    echo '<input type="text" name="comment_username" placeholder="评论人用户名">';
    echo '<textarea name="comment_content" placeholder="评论内容"></textarea>';
    echo '<input type="submit" value="提交评论" />';
    echo '</form>';

    echo '<hr>';
}

// 关闭数据库连接
mysqli_close($conn);
?>



</body>
</html>

```



## article.html

```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <!-- 添加内部样式表 -->
    <style>
        /* 为body设置背景颜色 */
        body {
            background-color: #f2f2f2;
        }

        /* 设置表单样式 */
        form {
            width: 50%;
            margin: 0 auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.1);
        }

        h2 {
            font-size: 24px;
            /* 设置字体大小 */
            color: #333;
            /* 设置字体颜色，例如深灰色 */
            font-weight: bold;
            /* 设置字体粗细，使其加粗 */
            line-height: 1.5;
            /* 设置行高，影响行间距 */
            margin-top: 1em;
            /* 设置上边距 */
            margin-bottom: 0.5em;
            /* 设置下边距 */
            text-align: center;
            /* 设置文本居中对齐 */
        }

        /* 输入框和文本区域样式 */
        input[type="text"],
        textarea {
            display: block;
            width: 100%;
            margin-bottom: 10px;
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }

        /* 提交按钮样式 */
        input[type="submit"] {
            background-color: #007BFF;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            text-transform: uppercase;
        }

        /* 提交按钮悬停样式 */
        input[type="submit"]:hover {
            background-color: #0056b3;
        }

        /* 链接样式 */
        a {
            color: #007BFF;
            text-decoration: none;
        }

        /* 链接悬停样式 */
        a:hover {
            color: #0056b3;
        }
    </style>
</head>

<body>
    <h2>博客发表</h2>
    <form action="publish_article.php" method="POST">
        <label for="title">标题：</label>
        <input type="text" name="title" id="title" required=""><br>
        <label for="content">内容：</label>
        <textarea name="content" id="content" required=""></textarea><br>
        <label for="author">作者：</label>
        <input type="text" name="author" id="author" required=""><br>
        <input type="submit" value="发布文章">
        <br>
        <br><a href="homepage.php">点击返回主页</a></br>
    </form>
</body>

</html>
```



## change.php

```php
<?php
error_reporting(0);
// 连接数据库
include "connect.php";
session_start();

// 获取用户输入的用户名和密码
$user = $_SESSION['user'];
$pass = $_POST['pass'];
$newPassword = $_POST['password'];
$newPassword_2 = $_POST['password_2'];


// 准备查询语句
$stmt = mysqli_prepare($conn, "SELECT user, pass FROM user WHERE user=? AND pass=?");
mysqli_stmt_bind_param($stmt, "ss", $user, $pass);
mysqli_stmt_execute($stmt);

// 获取查询结果
$result = mysqli_stmt_get_result($stmt);

// 检查查询是否成功
if ($row = mysqli_fetch_assoc($result)) {
    // 用户存在，更新密码
    if ($newPassword != $newPassword_2) {
        $ERR = "两次输入的密码不一致";
    } else if (strlen($newPassword) >= 6) {
        // 新密码长度大于等于6位，更新密码
        // 更新密码时使用预处理语句
        $updateStmt = mysqli_prepare($conn, "UPDATE user SET pass=? WHERE user=? AND pass=?");
        mysqli_stmt_bind_param($updateStmt, "sss", $newPassword, $user, $pass);
        mysqli_stmt_execute($updateStmt);
        echo "<a href='index.html'>修改成功，返回登录</a>";
        header('location:index.html');
    } else {
        echo "密码长度不足六位<br>修改失败！";
    }
} else {
    echo "用户名或密码错误，返回登录</a>";
}

// 关闭预处理语句和数据库连接
mysqli_stmt_close($stmt);
mysqli_close($conn);

?>




<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>修改密码</title>
    <!-- 添加CSS样式 -->
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
        }

        div {
            width: 300px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ccc;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            background-color: white;
            text-align: center;
        }

        h2 {
            font-size: 24px;
            color: #333;
            font-weight: bold;
            line-height: 1.5;
            margin-top: 1em;
            margin-bottom: 0.5em;
            text-align: center;
        }

        form p {
            margin-bottom: 10px;
        }

        span {
            display: inline-block;
            width: 100px;
            text-align: right;
            margin-right: 10px;
        }

        input[type="text"] {
            width: 200px;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 3px;
        }

        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            margin-left: 10px;
        }

        input[type="submit"]:hover {
            background-color: #45a049;
        }

        a {
            display: block;
            text-align: center;
            color: #337ab7;
            text-decoration: none;
            margin-top: 15px;
        }

        a:hover {
            color: #23527c;
        }
    </style>
</head>

<body>
    <div>
        <h2>密码修改</h2>
        <form action="" method="post">
            <p><span>原始密码:</span><input type="text" name="pass" required=""></p>
            <p><span>新密码：</span><input type="text" name="password" required=""></p>
            <p><span>再次输入：</span><input type="text" name="password_2" required=""></p>
            <p><input type="reset" value="重置" />
                <input type="submit" name="" value="修改" />
            </p>
            <br></br>
            <a href="homepage.php">点击返回主页</a>
        </form>

    </div>

</body>

</html>
```



## homepage.php

```php
<?php
include "session.php";
?>

<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>页面标题</title>
    <!-- 引入CSS样式 -->
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
        }

        a {
            display: inline-block;
            padding: 10px 15px;
            text-decoration: none;
            color: #333;
            background-color: #e0e0e0;
            border-radius: 4px;
            margin-right: 10px;
        }

        a:hover {
            background-color: #ccc;
        }
    </style>
</head>

<body>
    <?php echo "<br></br>当前用户：" . $_SESSION['user'] . "<br></br>"; ?>
    <a href="person.php">个人信息</a>
    <a href="article.html">博客发表</a>
    <a href="article_list.php">博客浏览</a>
    <a href="upimg.html">头像修改</a>
    <a href="change.php">密码修改</a>
</body>

</html>
```



## index.html

```HTML
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>登入</title>
    <!-- 引入外部CSS文件 -->
    <link rel="stylesheet" href="styles.css">
    <!-- 或者直接在<style>标签内写入内部CSS -->
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
        }
        
        div {
            width: 300px;
            margin: 50px auto;
            padding: 20px;
            border: 1px solid #ccc;
            background-color: white;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        h2 {
            font-size: 24px;
            color: #333;
            font-weight: bold;
            line-height: 1.5;
            margin-top: 1em;
            margin-bottom: 0.5em;
            text-align: center;
        }
        
        form p {
            margin-bottom: 10px;
        }
        
        span {
            display: inline-block;
            width: 100px;
            text-align: right;
            margin-right: 10px;
        }
        
        input[type="text"] {
            width: 180px;
            padding: 5px;
            border: 1px solid #ccc;
        }
        
        input[type="submit"] {
            background-color: #4CAF50;
            color: white;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 16px;
            margin-top: 10px;
        }
        
        a {
            display: block;
            text-align: center;
            margin-top: 10px;
            color: #4CAF50;
            text-decoration: none;
        }
        
        a:hover {
            color: #2196F3;
        }
    </style>
</head>

<body>
    <div>
        <h2>登录</h2>
        <form action="login.php" method="post">
            <p><span>用户名: </span><input type="text" name="user" required=""></p>
            <p><span>密码:</span><input type="text" name="pass" required=""></p>
            <input type="submit" value="登录" />
        </form>
        <a href="register.php">新用户注册</a>
        <a href="#">忘记密码（待完善）</a>
    </div>
</body>

</html>
```



## login.php

```php
<?php
error_reporting(0);

include "connect.php";

// 获取用户输入的用户名和密码
$user = $_POST['user'];
$pass = $_POST['pass'];

// 准备查询语句
$stmt = mysqli_prepare($conn, "SELECT * FROM user WHERE user=? AND pass=?");
if (!$stmt) {
    die("查询语句准备失败：" . mysqli_error($conn));
}

// 绑定查询参数
mysqli_stmt_bind_param($stmt, "ss", $user, $pass);
if (mysqli_stmt_execute($stmt) === false) {
    die("查询执行失败：" . mysqli_error($conn));
}

// 获取查询结果
$result = mysqli_stmt_get_result($stmt);
if (!$result) {
    die("查询结果获取失败：" . mysqli_error($conn));
}

// 检查查询是否成功
$row = mysqli_fetch_array($result, MYSQLI_NUM);
if (!is_null($row)) {
    echo "登录成功";
    session_start();
    $_SESSION['login'] = "true";
    // 将用户名存储在sessionStorage中
    $_SESSION['user'] = $user;
    header('Location: homepage.php');
    exit;
} else {
    echo "账号或密码错误<br>登录失败！<br>";
    echo "<br><a href=index.html>点击返回</a></br>";
    $_SESSION['login'] = "false";
}

// 关闭数据库连接
mysqli_stmt_close($stmt);
mysqli_close($conn);
?>
```



## persion.php

```php
<?php
error_reporting(0);

// css样式
echo "<style>
    body { font-family: Arial, sans-serif; }
    a { color: #007BFF; text-decoration: none; }
    img { max-width: 100px; height: auto; border-radius: 50%; }
</style>";


// 假设用户已经登录并将用户ID存储在会话中
session_start();
$userId = $_SESSION['user'];
//echo $userId;

echo "<br><a href=homepage.php>点击返回主页</a></br>";
echo "<br></br>";

// 连接到数据库
include "connect.php";

// 执行查询操作获取登录用户的个人信息
$sql = "SELECT user, phone, photo FROM user WHERE user = '$userId'";
$result = $conn->query($sql);

// 输出登录用户的个人信息
if ($result->num_rows > 0) {
    $row = $result->fetch_assoc();

    if (isset($row["user"])) {
        echo "用户: " . htmlspecialchars($row["user"]) . "<br>";
    } else {
        echo "用户信息中缺少'user'字段。<br>";
    }

    if (isset($row["phone"])) {
        echo "电话: " . htmlspecialchars($row["phone"]) . "<br>";
    } else {
        echo "用户信息中缺少'phone'字段。<br>";
    }

    if (isset($row["photo"])) {
        if (is_file($row["photo"])) {
            echo '<img src="' . $row["photo"] . '" alt="头像">';
        } else {
            echo "头像文件不存在。";
        }
    } else {
        echo "用户信息中缺少'photo'字段。";
    }
} else {
    echo "没有找到登录用户的个人信息。";
}

// 关闭数据库连接

$conn->close();
?>
```



## publish_article.php

```php
<?php
error_reporting(0);

// 获取表单提交的文章信息
$title = $_POST['title'];
$content = $_POST['content'];
$author = $_POST['author'];

// 连接数据库
include "connect.php";
// $conn = mysqli_connect('localhost', 'root', 'root', 'test');

// 插入文章信息到数据库表
$sql = "INSERT INTO articles (title, content, author, publish_date) VALUES ('$title', '$content', '$author', NOW())";
mysqli_query($conn, $sql);

// 关闭数据库连接
mysqli_close($conn);

// 重定向到文章列表页面或显示发布成功消息
header('Location:  article_list.php');
exit;
?>
```



## register.php

```php
<?php
error_reporting(0);

include "connect.php";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    $name = $_POST['user'];
    $password1 = $_POST['password1'];
    $password2 = $_POST['password2'];
    $phone = $_POST['phone'];
    $tupian = $_FILES['tupian'];

    $ERR = "";
    $upload = false;

    if (empty($name) || empty($password1) || empty($password2) || empty($phone)) {
        $ERR = "用户、密码和手机号码不能为空";
    } else if (strlen($password1) < 6) {
        $ERR = "密码长度不足六位";
    } else if ($password1 != $password2) {
        $ERR = "两次输入的密码不一致";
    } else if (strlen($phone) != 11) {
        $ERR = "手机号码格式有问题";
    } else {
        if (getimagesize($_FILES["tupian"]["tmp_name"]) === false) {
            $ERR = "上传的文件不是图片";
        } else {
            $upload = move_uploaded_file($_FILES["tupian"]["tmp_name"], "tupian/" . $_FILES["tupian"]["name"]);
        }
    }

    if ($ERR === "") {
        if ($upload) {
            $sql = "INSERT INTO user (user, pass, phone, photo) VALUES (?, ?, ?, ?)";
            if ($stmt = $conn->prepare($sql)) {
                // 添加 "tupian/" 前缀到文件名中
                $profilePicturePath = 'tupian/' . $_FILES["tupian"]["name"];

                $stmt->bind_param("ssss", $name, $password1, $phone, $profilePicturePath);
                $stmt->execute();
                $ERR = "注册成功！";
            } else {
                $ERR = "注册失败";
            }
        } else {
            $ERR = "上传文件失败";
        }
    }
}

if (isset($_GET['return'])) {
    header('Location: index.html');
    exit;
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>注册</title>
    <!-- 添加内联样式 -->
    <style>
        /* 全局样式 */
        body {
            font-family: Arial, sans-serif;
            background-color: #f5f5f5;
            padding: 20px;
        }

        form {
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            background-color: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        h2 {
            font-size: 24px;
            color: #333;
            font-weight: bold;
            line-height: 1.5;
            margin-top: 1em;
            margin-bottom: 0.5em;
            text-align: center;
        }

        input[type="text"],
        input[type="password"],
        input[type="file"] {
            display: block;
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 3px;
            font-size: 16px;
        }

        input[type="submit"] {
            display: block;
            width: 100%;
            padding: 10px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 3px;
            cursor: pointer;
            text-transform: uppercase;
        }

        a {
            display: block;
            text-align: center;
            color: #337ab7;
            text-decoration: none;
            margin-top: 15px;
        }

        a:hover {
            color: #23527c;
        }

        img {
            max-width: 100%;
            height: auto;
            margin-bottom: 15px;
        }
    </style>
</head>

<body>
    <h2>新用户注册</h2>
    <?php if ($ERR) : ?>
        <p><?php echo htmlspecialchars($ERR); ?></p>
    <?php endif; ?>

    <form action="" method="POST" enctype="multipart/form-data">
        名字：<input type="text" name="user" required><br>
        密码：<input type="password" name="password1" required><br>
        重新输入密码：<input type="password" name="password2" required><br>
        请输入手机号码：<input type="text" name="phone" required><br>
        上传头像:<input type="file" name="tupian" accept="image/*" required></br>
        <label for="userImg">请确保头像命名为英文名，否则上传失败！</label><br></br>
        <input type="submit" value="提交">
    </form>

    <a href="index.html">点此返回登录页面</a><br>

    <?php if ($ERR) : ?>
        <p>你注册的用户为：<?php echo htmlspecialchars($name); ?></p>
        <p>你注册的手机号码为：<?php echo htmlspecialchars($phone); ?></p>
    <?php endif; ?>

    <?php if ($tupian && $upload) : ?>
        <img src="<?php echo htmlspecialchars("tupian/" . $_FILES["tupian"]["name"]); ?>" alt="上传的图片" />
    <?php endif; ?>
</body>

</html>
```



## session.php

```php
<?php
error_reporting(0);

session_start();
// echo $_SESSION['login'];
if ($_SESSION['login'] == true) {
    echo "您已登陆成功，<a href=zhuxiao.php>点击注销</a><br>";
} else {
    $_SESSION["login"] == false;
    die("您无权访问，<a href=index.html>点击此处登录</a><br>");
}
?>
```



## upimg.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>头像修改</title>
    <style>
        /* 通用样式 */
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            background-color: #f5f5f5;
            margin: 0;
            padding: 20px;
        }

        a {
            color: #007BFF;
            text-decoration: none;
        }
        
        a:hover {
            text-decoration: underline;
        }

        /* 表单样式 */
        form {
            display: inline-block;
            border: 1px solid #ccc;
            padding: 10px;
            border-radius: 5px;
            margin-bottom: 20px;
            background-color: #fff;
        }

        label {
            display: block;
            margin-bottom: 5px;
        }

        input[type="file"], input[type="submit"] {
            display: block;
            margin-bottom: 10px;
            width: 100%;
            /* padding: 10px; */
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }

        input[type="submit"] {
            background-color: #007BFF;
            color: #fff;
            cursor: pointer;
        }

        input[type="submit"]:hover {
            background-color: #0056b3;
        }
    </style>
</head>

<body>
    <a href="homepage.php">点击返回主页</a><br></br>
    <form method="post" action="upimg.php" enctype="multipart/form-data">
        <label for="userImg">选择需要上传或修改的头像图片：</label>
        <label for="userImg">请确保头像命名为英文名，否则上传失败！</label><br />
        <input type="file" name="userImg" id="userImg" /><br />
        <input type="submit" name="sub" value="提交" />
    </form>
</body>

</html>
```



## upimg.php

```php
<?php
error_reporting(0);

// 连接数据库
include "connect.php";
include "session.php";

// 获取上传的图片信息
if (isset($_FILES['userImg'])) {
    $img = $_FILES['userImg'];

    // 如果表单提交成功则执行
    if (isset($_POST['sub'])) {

        // 存储图片的目标目录
        $targetDirectory = 'tupian/';
        // 图片的完整路径

        // 文件同名处理
        $filename = pathinfo($img['name'], PATHINFO_FILENAME);
        $extension = pathinfo($img['name'], PATHINFO_EXTENSION);

        // 检查并生成不重复的文件名
        while (file_exists($targetDirectory . $filename . '.' . $extension)) {
            $filename .= '_';
        }
        $targetFile = $targetDirectory . $filename . '.' . $extension;

        // 检查文件是否有效
        if ($img['error'] !== UPLOAD_ERR_OK) {
            die('上传文件错误：' . $img['error']);
        }

        // 将图片移动到目标目录
        if (move_uploaded_file($img['tmp_name'], $targetFile)) {
            // 图片移动成功，将图片路径存储到数据库
            $userId = $_SESSION['user'];

            echo "<br>" . "当前用户：" . $_SESSION['user'] . "</br>";

            $sql = 'UPDATE user SET photo = ? WHERE user = ?';
            $stmt = mysqli_prepare($conn, $sql);
            $stmt->bind_param('ss', $targetFile, $userId);
            $stmt->execute();
            $stmt->close();

            if ($stmt->errno) {
                die('存储失败：' . $stmt->error);
            } else {
                echo "头像上传或者修改成功！";
                echo "e<br><a href=homepage.php>点击返回主页</a></br>";
            }
        } else {
            echo "头像上传失败！";
            echo "e<br><a href=homepage.php>点击返回主页</a></br>";
        }
    }
}

```



## zhuxiao.php

```php
<?php
error_reporting(0);

session_start();
$_SESSION['login'] = "false";
session_destroy();
header('location:index.html');
?>
```

