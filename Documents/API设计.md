## API 设计

### API设计工具
根据老师的推荐，我们使用了apiary工具来进行我们的API设计，具体的API设计文档可点击[此处](https://eatouteorder.docs.apiary.io/#)查看。 apiary的tutorial可以通过这些[example](https://apiblueprint.org/documentation/examples/)去学习,以及相关的一些[资料](https://help.apiary.io/tools/)。具体的设计规范可以在[./Documents/生产规范与指南/REST_API设计规范.md](https://github.com/sysu-badass/Dashboard/blob/master/Documents/生产规范与指南/REST_API设计规范.md)文件中查看。

### URI设计

restaurant的后台管理的URI

| URI                                                           | 说明                                                      | HTTP方法         |
| ------------------------------------------------------------- | --------------------------------------------------------- | ---------------- |
| /restaurants/login                                            | 餐厅管理后台账号登录                                      | POST             |
| /restaurants/join                                             | 餐厅管理后台账号创建                                      | POST             |
| /restaurants/{restaurant_id}/menu                             | 餐厅的餐单资料                                            | GET, POST, DELETE        |
| /restaurants/{restaurant_id}/menu/{food_id}                   | 菜品的具体信息                                            | GET, PUT, DELETE |
| /restaurants/{restaurant_id}/orders                           | 餐厅的订单列表，包含待处理的和已处理的                    | GET, POST        |
| /restaurants/{restaurant_id}/orders?date={}&user={}&status={} | 通过订单的日期，下单user_id以及完成状态status进行检索     | GET              |
| /restaurants/{restaurant_id}/orders/{order_id}                | 通过order自身的id来查看订单数据                           | GET, PUT, DELETE |
| /restaurants/{restaurant_id}/orders/{order_id}/{food_id}      | 餐厅管理员查看订单里的菜品的信息，重定向到/menu/{food_id} | GET              |

user的信息管理URI，考虑到我们的用户仅仅需要查看订单，修改购物车以及支付，所以很多操作都只需要GET HTTP方法就可以了。支付方式暂时只支持微信支付。而且是扫码点餐，是在实体餐厅中扫码，所以所有的订单记录资源都可以作为当前餐厅的子资源。同时由于暂时不是很清楚微信小程序的工作原理，所以暂定顾客user的账号是其手机号码，密码是手机号码通过passlib库中的passlib.hash中的pbkdf2_sha256.hash('手机号码')来生成密码。(注：hash()函数需要unicode类型作为传入参数)

| URI                                                          | 说明                                                | HTTP方法 |
| ------------------------------------------------------------ | --------------------------------------------------- | -------- |
| /users/{user_id}/{restaurant_id}/orders                      | 顾客在当前餐厅的订单记录，包括待处理的与之前的历史  | GET      |
| /users/{user_id}/{restaurant_id}/orders/{order_id}           | 顾客查看具体订单的信息，包括未完成的订单的信息      | GET      |
| /users/{user_id}/{restaurant_id}/orders/{order_id}/{food_id} | 顾客查看订单里的菜品的信息，重定向到/menu/{food_id} | GET      |
| /users/{user_id}/{restaurant_id}/orders?limimt={}            | 顾客的订单记录查看数量受到limit限制                 | GET      |
| /users/{user_id}/{restaurant_id}/payment                     | 顾客选择支付方式                                    | POST      |
| /users/{user_id}/{restaurant_id}/menu                        | 餐厅的菜单                                          | GET      |
| /users/{user_id}/{restaurant_id}/menu/{food_id}              | 顾客查看菜单里菜品的信息                            | GET      |
| /users/login                                                 | 用于扫码登录                                        | POST     |

### 具体的接口设计
以下是我们项目在**apiary**上的文档源文件
________________________________________________

FORMAT: 1A

# EATOUT-EORDER API
This is the API design of EATOUT-EORDER project. You can get the detailed information [here](https://github.com/sysu-badass)

NOTE: This document is a **work in progress**.

# Group Useres

This section groups users resources.

## Users Login [/users/login]

### 客户端向服务端发送登录信息 [POST]
In the URL there should be a QR code and user can login with it.

+ Request (application/json)

    + Body

            {
              "user_id": "3062",
              "user_password": "123456",
              "restaurant_id": "9527"
            }

+ Response 200 (application/json)

    + Body

            {
              "URL": "/users/{user_id}/{restaurant_id}/menu"
            }


## Restaurant Menu [/users/{user_id}/{restaurant_id}/menu]

+ Parameters

    + user_id: 123 (int) - 用户的ID
    + restaurant_id: 9527 (int) - 餐厅的ID

### 获得客户端餐厅菜单界面的信息 [GET]
In this URL, the client can can the food infomation json

+ Response 200

    [Restaurants Food][]


## Orders List [/users/{user_id}/{restaurant_id}/orders]

顾客在当前餐厅的订单记录，包括待处理的与之前的历史

+ Parameters

    + user_id: 123 (int) - 用户的ID
    + restaurant_id: 234 (int) - 餐厅的ID

+ Model (application/json)

        {
          [
            {
              "order_history_id": 1,
              "date": "2018.6.18",
              "desk_number": 2,
              "total_price": 121,
              "restaurant_id": 9527,
              "user_id": 3062
            }
          ]
        }

### 客户度获得在当前餐厅订单列表信息 [GET]

+ Response 200

    [Orders List][]

## Order [/users/{user_id}/{restaurant_id}/orders/{order_id}]

+ Parameters

    + user_id: 123 (int) - 用户的ID
    + restaurant_id: 234 (int) - 餐厅的ID
    + order_id: 1 (int) - 查看的订单的ID

+ Model (application/json)

        {
          [
            {
              "order_history_item_id": 1,
              "number": 2,
              "name": "doufu",
              "description": "delicious",
              "image": "/image/doufu.png",
              "price": 12,
              "order_history_id": 34
            }
          ]
        }

### 客户端获得当前订单具体信息 [GET]

+ Response 200

    [Order][]

## Food Information of Order [/users/{user_id}/{restaurant_id}/orders/{order_id}/{food_id}]

+ Parameters

    + user_id: 123 (int) - 用户的ID
    + restaurant_id: 234 (int) - 餐厅的ID
    + order_id: 1 (int) - 查看的订单的ID
    + food_id: 1 (int) - 查看的订单里的菜品的ID

### 客户端获得当前订单中某菜品的具体信息 [GET]

+ Response 200

    [Restaurant Food][]

# Group Restaruants

This section groups restaurants resources.

## Restaurants Login [/restaurants/login]

The restaurant administrator login website. Because the database didn't need the information of the administrator to handle the order data and the menu data, I prefer just use the restaurant id to represent the URLs of the restaurant management website.

### 发送服务端管理账号登录信息 [POST]

+ Request (application/json)

    + Body

            {
              "restaurant_admin_id": 123,
              "restaurant_admin_password": 1234,
              "restaurant_id": 9527
            }

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu"
            }


## Restaurants Join [/restaurants/join]

### 发送服务端管理账号注册信息 [POST]

+ Request (application/json)

    + Body

            {
              "restaurant_id": 9527,
              "restaurant_admin_id": 123,
              "restaurant_admin_password": 1234,
              "restaurant_name": "Eorder",
              "restaurant_information": "小吃店"
            }
+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/"
            }

## Restaurant Menu [/restaurants/{restaurant_id}/menu]
展示餐单的菜品列表，同时可以批量删除其中的菜品。

+ Parameters

    + restaurant_id: 234 (int) - 餐厅的ID

### 服务端获得当前餐厅的菜单信息 [GET]

+ Response 200

    [Restaurants Food][]

### 服务端发送修改当前菜单的请求 [POST]

+ Request

    [Restaurants Food][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }

### 服务端发送批量删除菜单中菜品的请求 [DELETE]

+ Request

    [Restaurants Food][]

+ Response 204

## Restaurants Food [/restaurants/{restaurant_id}/menu/{food_id}]
查看，编辑，删减菜品的信息。

+ Parameters

    + restaurant_id: 234 (int) - 餐厅的ID
    + food_id: 1 (int) - 查看的订单里的菜品的ID

+ Model (application/json)
返回food类型的数组
    + Body

            {
              [
                {
                  "food_id": 1,
                  "name": "豆腐",
                  "price": 10,
                  "food_type": "素食",
                  "description": "美味",
                  "image": "/image/doufu.png",
                  "available": true
                  "restaurant_id": 9527
                }
              ]
            }

### 服务端获得当前餐厅菜单中特定菜品的信息 [GET]

+ Response 200

    [Restaurants Food][]

### 服务端发送修改当前餐厅菜单中特定菜品的信息的请求 [PUT]

修改菜品的信息

+ Request

    [Restaurants Food][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }

### 服务端发送删除当前餐厅菜单中特定菜品的信息的请求 [DELETE]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu"
            }

## Restaurants Orders List [/restaurants/{restaurant_id}/orders]
查看，编辑订单状态

+ Parameters

    + restaurant_id: 234 (int) - 餐厅的ID

+ Model (application/json)

    + Body

            {
              [
                {
                  'order_id': 123,
                  'date': '2018.6.18',
                  'desk_number': 2,
                  'total_price': 123.4,
                  'restaurant_id': 9527
                }
              ]
            }



### 服务端获得当前餐厅所有订单的列表 [GET]

+ Response 200

    [Restaurants Orders List][]

### 服务端发送在当前餐厅订单的列表创建订单的请求 [POST]
将详细的订单的信息发送到服务端

+ Request

    [Restaurants Order][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/orders/{order_id}"
            }

### 服务端发送删除在当前餐厅的订单列表中特定订单的请求 [DELETE]

+ Request

    [Restaurants Orders List][]

+ Response 204

## Restaurants Order [/restaurants/{restaurant_id}/orders/{order_id}]

+ Parameters

    + restaurant_id: 234 (int) - 餐厅的ID
    + order_id: 1 (int) - 查看的订单的ID

+ Model (application/json)

    + Body

            {
              [
                {
                  "order_item_id": 1,
                  "order_id": 2,
                  "number" : 2,
                  "name": "豆腐",
                  “price”: 10,
                  "description": "美味",
                  "image": "/image/doufu.png"
                }
              ]
            }

### 服务端获得在当前餐厅特定订单信息 [GET]

+ Response 200

    [Restaurants Order][]

### 服务端发送修改当前餐厅特定订单信息的请求 [PUT]

+ Request

    [Restaurants Order][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/orders/{order_id}"
            }

### 服务端发送删除当前餐厅特定订单信息的请求 [DELETE]

+ Response 204

## Restaurant Food Information in an Order [/restaurants/{restaurant_id}/orders/{order_id}/{food_id}]

+ Parameters

    + restaurant_id: 9527 (int) - 餐厅的ID
    + order_id: 1 (int) - 订单的ID
    + food_id: 1 (int) - 订单里菜品的ID

### 服务端获得当前餐厅特定订单中特定菜品的信息 [GET]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }
