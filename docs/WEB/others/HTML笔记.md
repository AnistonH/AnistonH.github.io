> 
>
> <img decoding="async" src="" width="258" height="39" />
>
> decoding
> 标签的 decoding 属性用于告诉浏览器使用何种方式解析图像数据。
>
> 该属性可以取以下三个值：
>
> sync: 同步码图像，保证与其他内容一起显示。
> async: 异步解码图像，加快显示其他内容。
> auto: 默认模式，表示不偏好解码模式。由浏览器决定哪种方式更适合用户。
> 此属性类似于在script标签上使用 async 属性。加载图像所需的时间不会改变，但其“解码”的方式由解码属性决定。decoding 属性可以控制是否允许浏览器尝试异步加载图像。异步加载对元素很有用，对屏幕外的图像对象可能会更有用。



> `<a href="https://www.baidu.com/" target="_blank" rel="noopener noreferrer">访问baidu</a>`  这段代码中的noopener和noreferrer分别是什么意思  这段代码中的noopener和noreferrer分别是什么意思：
>
> 这段代码是一段HTML代码，其中`noopener`和`noreferrer`是链接（`<a>`标签）的属性。
>
> - `noopener`：当链接（`<a>`标签）使用`noopener`属性时，在打开新页面或链接时，不会将原页面的`window.opener`属性设置为新页面的引用。这意味着新页面无法通过`window.opener`属性访问原页面的window对象。这个属性可以帮助减少跨站脚本攻击（XSS）的风险。
> - `noreferrer`：当链接（`<a>`标签）使用`noreferrer`属性时，表示在打开新页面或链接时，不会在原页面和新页面之间创建HTTP Referer（引荐来源页面）的关联。这意味着在访问新页面时，不会将原页面的URL作为引荐来源记录在服务器日志或统计系统中。这个属性可以帮助保护用户的隐私，避免泄露用户的浏览记录。
>
> 在这段代码中，`noopener`和`noreferrer`属性一起使用，可以增强链接的安全性和隐私保护。

