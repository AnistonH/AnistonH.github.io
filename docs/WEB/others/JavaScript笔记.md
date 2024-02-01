## JavaScript笔记



1. **转换为字符串** （三种转换方式一般是使用第三种，这一种也成为'隐式转换'）

| 序号 | 方法              | 说明                         | 案例                              |
| ---- | :---------------- | :--------------------------- | --------------------------------- |
| 1    | toString()        | 转成字符串                   | var num=1; alert(num.toString()); |
| 2    | String() 强制转换 | 转成字符串                   | var num=1; alert(String(num));    |
| 3    | 加号拼接字符串    | 和字符串拼接的结果都是字符串 | var num=1; alert(num + ' ');      |



2. **转换为数字** （1和2 是重点）

| 序号 | 方式                    | 说明                         | 案例                |
| ---- | ----------------------- | ---------------------------- | ------------------- |
| 1    | parseInt(string) 函数   | 将string类型转成整数数值型   | parseInt('78')      |
| 2    | parseFloat(string) 函数 | 将string类型转成浮点数数值型 | parseFloat('78.12') |
| 3    | Number() 强制转换函数   | 将string类型转成数值型       | Number('12')        |
| 4    | js 隐式转换 ( -  *  / ) | 利用算术运算隐式转换为数值型 | '12' - 0            |



3. **转换为布尔型** 

| 序号 | 方式           | 说明               | 案例             |
| ---- | -------------- | ------------------ | ---------------- |
| 1    | Boolean() 函数 | 其他类型转成布尔值 | Boolean('true'); |

- 代表空、否定的值会被转换成false，如''、0、NaN、null、undefined
- 其余值都会被转换为true



4. **添加删除数组元素的方法**

| 方法名           | 说明                                                    | 返回值         |
| ---------------- | ------------------------------------------------------- | -------------- |
| push(参数1……)    | 末尾添加一个或多个元素，注意修改原数组                  | 新的长度       |
| pop()            | 删除数组最后一个元素，把数组长度减1，无参数，修改原数组 | 删除的元素的值 |
| unshift(参数1……) | 向数组的开头添加一个或多个元素，注意修改原数组          | 新的长度       |
| shift()          | 删除数组第一个元素，把数组长度减1，无参数，修改原数组   | 第一个元素的值 |



5. **数组转换为字符串**

| 方法名         | 说明                                       | 返回值         |
| -------------- | ------------------------------------------ | -------------- |
| toString()     | 把数组转换成字符串，逗号分隔每一项         | 返回一个字符串 |
| join('分隔符') | 方法用于把数组中的所有元素转换成一个字符串 | 返回一个字符串 |



6. **根据字符返回位置**（字符串的所有方法，都不会修改字符串本身（字符串是不可变的），操作完成会返回一个新的字符串）

| 方法名                             | 说明                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| indexOf('要查找的字符',开始的位置) | 返回指定内容在元字符串中的位置，如果返回不到就返回-1，开始的位置是index索引号 |
| lastIndexOf()                      | 从后往前找，只找第一个匹配的                                 |



7. **根据位置返回字符（重点）**

| 方法名            | 说明                                     | 使用                          |
| ----------------- | ---------------------------------------- | ----------------------------- |
| charAt(index)     | 返回指定位置的字符(index字符串的索引号)  | str.charAt(0)                 |
| charCodeAt(index) | 获取指定位置处字符的ASCII码(index索引号) | str.charCodeAt(0)             |
| str[index]        | 获取指定位置处字符                       | HTML5,IE8+支持 和charAt()等效 |



8. **字符串操作方法（重点，substr更是重点）**

| 方法名                    | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| concat(str1,str2,str3...) | concat()方法用于连接两个或多个字符串。拼接字符串，等效于+，+更常用 |
| substr(start,length)      | 从start位置开始(索引号)，length取的个数  **重点记住这个**    |
| slice(start,end)          | 从start位置开始，截取到end位置，end取不到(他们俩都是索引号)  |
| substring(start,end)      | 从start位置开始，截取到end位置，end取不到，基本和slice相同，但是不接受负值 |
| splice()                  | 数组删除splice(第几个开始,要删除个数)，返回被删除项目的新数组，注意:这个会影响原数组 |

（slice()和splice()目的基本相同，建议同学重点看下splice()）





# API



**常见的鼠标事件**

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedoen | 鼠标按下触发     |



### API终止学习



















```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
</head>

<body>
    <p>不管输入是否正确，输入框都会再输入后清空。</p>
    <p>请输入 5 ~ 10 之间的数字：</p>

    <input id="demo" type="text">
    <button type="button" onclick="myFunction()">点我</button>

    <p id="p01"></p>

    <script>
        function myFunction() {
            var message, x;
            message = document.getElementById("p01");
            message.innerHTML = "";
            x = document.getElementById("demo").value;
            try {
                if (x == "") throw "值是空的";
                if (isNaN(x)) throw "值不是一个数字";
                x = Number(x);
                if (x > 10) throw "太大";
                if (x < 5) throw "太小";
            }
            catch (err) {
                message.innerHTML = "错误: " + err + ".";
            }
            finally {
                document.getElementById("demo").value = "";
            }
        }
    </script>

</body>

</html>
```





```HTML
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
</head>

<body>

    <p>局部变量计数。</p>
    <button type="button" onclick="myFunction()">计数!</button>
    <p id="demo">0</p>

    <script>

        var add = (function () {
            var counter = 0;
            return function () { return counter += 1; }
        })();

        function myFunction() {
            document.getElementById("demo").innerHTML = add();
        }
        
    </script>

</body>

</html>

变量 add 指定了函数自我调用的返回字值。
自我调用函数只执行一次。设置计数器为 0。并返回函数表达式。
add变量可以作为一个函数使用。非常棒的部分是它可以访问函数上一层作用域的计数器。
这个叫作 JavaScript 闭包。它使得函数拥有私有变量变成可能。
计数器受匿名函数的作用域保护，只能通过 add 方法修改。
```





```js
class Site {
  constructor(name) {
    this.sitename = name;
  }
  present() {
    return '我喜欢' + this.sitename;
  }
}
 
class Runoob extends Site {
  constructor(name, age) {
    super(name);
    this.age = age;
  }
  show() {
    return this.present() + ', 它创建了 ' + this.age + ' 年。';
  }
}
 
let noob = new Runoob("菜鸟教程", 5);
document.getElementById("demo").innerHTML = noob.show();
```

