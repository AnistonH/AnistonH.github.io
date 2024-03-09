# HTTP 报文

## HTTP 协议请求头

| 名称                    | 名称                                                         |
| ----------------------- | ------------------------------------------------------------ |
| ==**Accept**==          | 指定客户端能够接收的内容类型                                 |
| Accept-Charset          | Accept-Charset                                               |
| ==**Accept-Encoding**== | **指定浏览器可以支持的web服务器返回内容压缩编码类型**        |
| ==**Accept-Language**== | **浏览器可接受的语言**                                       |
| Accept-Ranges           | 可以请求网页实体的一个或者多个子范围字段                     |
| AuthorizationHTTP       | 授权的授权证书                                               |
| Cache-Control           | 指定请求和响应遵循的缓存机制                                 |
| ==**Connection**==      | **表示是否需要持久连接。（HTTP 1.1默认进行持久连接）**       |
| ==**Cookie**==          | **表示服务端给客户端传的http请求状态,也是多个key=value形式组合，比如登录后的令牌等** |
| CookieHTTP              | 请求发送时，会把保存在该请求域名下的所有cookie值一起发送给web服务器 |
| ==**Content-Length**==  | **请求的内容长度**                                           |
| ==**Content-Type**==    | **请求的与实体对应的MIME信息**                               |
| Date                    | 请求发送的日期和时间                                         |
| Expect                  | 请求的特定的服务器行为                                       |
| From                    | 发出请求的用户的Email                                        |
| ==**Host**==            | **指定请求的服务器的域名和端口号**                           |
| If-Match                | 只有请求内容与实体相匹配才有效                               |
| If-Modified-Since       | 如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304代码 |
| If-None-Match           | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 |
| If-Range                | 如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体 |
| If-Unmodified-Since     | 只在实体在指定时间之后未被修改才请求成功                     |
| Max-Forwards            | 限制信息通过代理和网关传送的时间                             |
| ==**Origin**==          | **告诉服务器请求从哪里发起的，仅包括协议和域名 CORS跨域请求中可以看到response有对应的header，Access-Control-Allow-Origin** |
| Pragma                  | 用来包含实现特定的指令                                       |
| Proxy-Authorization     | 连接到代理的授权证书                                         |
| Range                   | 只请求实体的一部分，指定范围                                 |
| ==**Referer**==         | **先前网页的地址，当前请求网页紧随其后,即来路**              |
| TE                      | 客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息     |
| Upgrade                 | 向服务器指定某种传输协议以便服务器进行转换                   |
| ==**User-Agent**==      | **请求的用户信息**                                           |
| Via                     | 通知中间网关或代理服务器地址，通信协议                       |
| ==**Warning**==         | **关于消息实体的警告信息**                                   |

**接下来是 Content-Type 的具体类型**  

1. text/html：HTML 格式
2. text/plain：纯文本格式
3. text/xml： XML 格式  
4. image/gif：gif 图片格式
5. image/jpeg：jpg 图片格式
6. image/png：png 图片格式  
7. application/json：JSON 数据格式
8. application/pdf：pdf 格式
9. application/octet-stream：二进制流数据，一般是文件下载  
10. application/x-www-form-urlencoded：form 表单默认的提交数据的格式，会编码成 key=value 格式  
11. multipart/form-data： 表单中需要上传文件的文件格式类型



## HTTP 协议响应头

| 名称                                | 意义                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| Accept-Ranges                       | 表明服务器是否支持指定范围请求及哪种类型的分段请求           |
| ==**Access-Control-Allow-Origin**== | **定哪些站点可以参与跨站资源共享**                           |
| Age                                 | 从原始服务器到代理缓存形成的估算时间（以秒计，非负）         |
| ==**Allow**==                       | **对某网络资源的有效的请求行为，不允许则返回405**            |
| ==**Cache-Control**==               | **告诉所有的缓存机制是否可以缓存及哪种类型**                 |
| ==**Content-Encoding**==            | **服务器支持的返回内容压缩编码类型**                         |
| ==**Content-Language**==            | **响应体的语言**                                             |
| Content-Length                      | 响应体的长度                                                 |
| Content-Location                    | 请求资源可替代的备用的另一地址                               |
| Content-MD5                         | 返回资源的MD5校验值                                          |
| Content-Range                       | 在整个返回体中本部分的字节位置                               |
| ==**Content-Type**==                | **返回内容的MIME类型**                                       |
| ==**Date**==                        | **原始服务器消息发出的时间**                                 |
| ETag                                | 请求变量的实体标签的当前值                                   |
| ==**Expires**==                     | **响应过期的日期和时间**                                     |
| Last-Modified                       | 请求资源的最后修改时间                                       |
| ==**Location**==                    | **用来重定向接收方到非请求URL的位置来完成请求或标识新的资源** |
| Pragma                              | 包括实现特定的指令，它可应用到响应链上的任何接收方           |
| Proxy-Authenticate                  | 它指出认证方案和可应用到代理的该URL上的参数                  |
| refresh                             | 应用于重定向或一个新的资源被创造，在5秒之后重定向（由网景提出，被大部分浏览器支持） |
| Retry-After                         | 如果实体暂时不可取，通知客户端在指定时间之后再次尝试         |
| ==**Server**==                      | **服务器软件名称**                                           |
| Set-Cookie                          | 设置Http Cookie                                              |
| Trailer                             | 指出头域在分块传输编码的尾部存在                             |
| ==**Transfer-Encoding**==           | **文件传输编码**                                             |
| Vary                                | 告诉下游代理是使用缓存响应还是从原始服务器请求               |
| Via                                 | 告知代理客户端响应是通过哪里发送的                           |
| Warning                             | 警告实体可能存在的问题                                       |
| WWW-Authenticate                    | 表明客户端请求实体应该使用的授权方案                         |

