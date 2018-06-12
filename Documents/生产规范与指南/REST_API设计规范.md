## REST_API设计规范

[toc]

### Relationship between URL and HTTP methods
| Uniform Resource Locator (URL)                         | GET                                                                  | PUT                                                    | PATCH              | POST                                                                                                                          | DELETE                        |
| ------------------------------------------------------ | -------------------------------------------------------------------- | ------------------------------------------------------ | ------------------ | ----------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| Collection, such as https://api.example.com/resources/ | List the URIs and perhaps other details of the collection's members. | Replace the entire collection with another collection. | Not generally used | Create a new entry in the collection. The new entry's URI is assigned automatically and is usually returned by the operation. | Delete the entire collection. |
| Element, such as https://api.example.com/resources/item17 |           Retrieve a representation of the addressed member of the collection, expressed in an appropriate Internet media type. | Replace the addressed member of the collection, or if it does not exist, create it. | Update the addressed member of the collection. | Not generally used. Treat the addressed member as a collection in its own right and create a new entry within it. | Delete the addressed member of the collection. |

### HTTP状态码
| 状态码 | 状态码英文名称 | 中文描述 |
| ------ | -------------- | -------- |
| 100 |	Continue |	继续。客户端应继续其请求 |
| 101 |	Switching Protocols |	切换协议。服务器根据客户端的请求切换协议。只能切换到更高级的协议，例如，切换到HTTP的新版本协议 |
| 200 |	OK |	请求成功。一般用于GET与POST请求 |
| 201 |	Created |	已创建。成功请求并创建了新的资源 |
| 202 |	Accepted |	已接受。已经接受请求，但未处理完成 |
| 203 |	Non-Authoritative Information |	非授权信息。请求成功。但返回的meta信息不在原始的服务器，而是一个副本 |
| 204 |	No Content |	无内容。服务器成功处理，但未返回内容。在未更新网页的情况下，可确保浏览器继续显示当前文档 |
| 205 |	Reset Content |	重置内容。服务器处理成功，用户终端（例如：浏览器）应重置文档视图。可通过此返回码清除浏览器的表单域 |
| 206 |	Partial Content |	部分内容。服务器成功处理了部分GET请求 |
| 300 |	Multiple Choices |	多种选择。请求的资源可包括多个位置，相应可返回一个资源特征与地址的列表用于用户终端（例如：浏览器）选择 |
| 301 |	Moved Permanently |	永久移动。请求的资源已被永久的移动到新URI，返回信息会包括新的URI，浏览器会自动定向到新URI。今后任何新的请求都应使用新的URI代替 |
| 302 |	Found |	临时移动。与301类似。但资源只是临时被移动。客户端应继续使用原有URI |
| 303 |	See Other |	查看其它地址。与301类似。使用GET和POST请求查看 |
| 304 |	Not Modified |	未修改。所请求的资源未修改，服务器返回此状态码时，不会返回任何资源。客户端通常会缓存访问过的资源，通过提供一个头信息指出客户端希望只返回在指定日期之后修改的资源 |
| 306 |	Unused |	已经被废弃的HTTP状态码 |
| 305 |	Use Proxy |	使用代理。所请求的资源必须通过代理访问 |
| 307 |	Temporary Redirect |	临时重定向。与302类似。使用GET请求重定向 |
| 400 |	Bad Request |	客户端请求的语法错误，服务器无法理解 |
| 401 |	Unauthorized |	请求要求用户的身份认证 |
| 402 |	Payment Required |	保留，将来使用 |
| 403 |	Forbidden |	服务器理解请求客户端的请求，但是拒绝执行此请求 |
| 404 |	Not Found |	服务器无法根据客户端的请求找到资源（网页）。通过此代码，网站设计人员可设置"您所请求的资源无法找到"的个性页面 |
| 405 |	Method Not Allowed |	客户端请求中的方法被禁止 |
| 406 |	Not Acceptable |	服务器无法根据客户端请求的内容特性完成请求 |
| 407 |	Proxy Authentication Required |	请求要求代理的身份认证，与401类似，但请求者应当使用代理进行授权 |
| 408 |	Request Time-out |	服务器等待客户端发送的请求时间过长，超时 |
| 409 |	Conflict |	服务器完成客户端的PUT请求是可能返回此代码，服务器处理请求时发生了冲突 |
| 410 |	Gone |	客户端请求的资源已经不存在。410不同于404，如果资源以前有现在被永久删除了可使用410代码，网站设计人员可通过301代码指定资源的新位置 |
| 411 |	Length Required |	服务器无法处理客户端发送的不带Content-Length的请求信息 |
| 412 |	Precondition Failed |	客户端请求信息的先决条件错误 |
| 413 |	Request Entity Too Large |	由于请求的实体过大，服务器无法处理，因此拒绝请求。为防止客户端的连续请求，服务器可能会关闭连接。如果只是服务器暂时无法处理，则会包含一个Retry-After的响应信息 |
| 414 |	Request-URI Too Large |	请求的URI过长（URI通常为网址），服务器无法处理 |
| 415 |	Unsupported Media Type |	服务器无法处理请求附带的媒体格式 |
| 416 |	Requested range not satisfiable |	客户端请求的范围无效 |
| 417 |	Expectation Failed |	服务器无法满足Expect的请求头信息 |
| 500 |	Internal Server Error |	服务器内部错误，无法完成请求 |
| 501 |	Not Implemented |	服务器不支持请求的功能，无法完成请求 |
| 502 |	Bad Gateway |	充当网关或代理的服务器，从远端服务器接收到了一个无效的请求 |
| 503 |	Service Unavailable |	由于超载或系统维护，服务器暂时的无法处理客户端的请求。延时的长度可包含在服务器的Retry-After头信息中 |
| 504 |	Gateway Time-out |	充当网关或代理的服务器，未及时从远端服务器获取请求 |
| 505 |	HTTP Version not supported |	服务器不支持请求的HTTP协议的版本，无法完成处理 |

### GET，DELETE，PUT和POST的典型用法
| 重要方法 | 典型用法                                                                                                                     | 典型状态码                                                 | 安全 | 幂等 |
| -------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | ---- | ---- |
| GET      | 1. 获取表示; 2. 变更时获取表示（缓存）                                                                                       | 200，204，301，303，304，400，404，406，500，503           | 是   | 是   |
| DELETE   | 删除资源                                                                                                                     | 200，301,303，400，404，409，500，509                      | 否   | 是   |
| PUT      | 1. 用客户端管理的实例号创建一个资源; 2. 通过替换的方式更新资源; 3. 如果未被修改，则更新资源（乐观锁）                        | 200，201，301，303，400，404，406，409，412，415，500，503 | 否   | 是   |
| POST     | 1. 使用服务端管理的（自动产生）的实例号创建资源; 2. 创建子资源; 3. 部分更新资源; 4. 如果没有被修改，则不过更新资源（乐观锁） | 200，201,202,301,303，400,404,406,406,412,415，500,503     | 否   | 否   |



### PUT 和 POST的区别
对于一组资源的URI,如http://example.com/resources/， PUT方法的作用是使用给定的一组资源替换当前整组资源，而POST方法的作用是在本组资源中创建/追加一个新的资源。该操作往往返回新资源的URL。对于一个特定资源的URI，如http://example.com/resources/1， PUT方法的作用是替换/创建指定的资源,并将其追加到相应的资源组中，而POST方法的作用是把指定的资源当做一个资源组，并在其下创建/追加一个新的元素，使其隶属于当前资源。
而且PUT方法是幂等性的，POST方法不是，这也是它们最大的区别。

### Request HTTP方法
以下是HTTP方法通过API对资源进行CRUD的例子。
* GET /zoos：列出所有动物园
* POST /zoos：新建一个动物园
* GET /zoos/ID：获取某个指定动物园的信息
* PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
* DELETE /zoos/ID：删除某个动物园
* GET /zoos/ID/animals：列出某个指定动物园的所有动物
* DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物过滤信息

如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。下面是一些常见的参数。

* ?limit=5：指定返回数据的数量
* ?offset=5：指定返回数据的开始位置。
* ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
* ?animal_type_id=1：指定筛选条件

参数的设计允许存在冗余，即允许API路径和URL参数偶尔有重复。比如，GET /zoo/ID/animals 与 GET /animals?zoo_id=ID 的含义是相同的。

### Response
1. Response可以直接返回数据如以下所示：

2. HTTP方法处理资源成功返回的数据格式:

| HTTP方法 | Response 格式  |
| -------- | -------------- |
| GET      | 单个对象、集合 |
| POST     | 新增成功的对象 |
| PUT      | 更新成功的对象 |
| DELETE   | 空             |


### 参考资料
[HTTP幂等性](https://www.cnblogs.com/weidagang2046/archive/2011/06/04/2063696.html)

[RESTful API 维基百科](https://en.wikipedia.org/wiki/Representational_state_transfer)

[RESTful HTTP in practice 中文翻译](http://www.infoq.com/cn/articles/designing-restful-http-apps-roth?utm_source=infoq_en&utm_medium=link_on_en_item&utm_campaign=item_in_other_langs)

[RESTful API 设计规范博客](https://blog.csdn.net/yue7603835/article/details/52536497)
