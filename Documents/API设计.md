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

user的信息管理URI，考虑到我们的用户仅仅需要查看订单，修改购物车以及支付，所以很多操作都只需要GET HTTP方法就可以了。支付方式暂时只支持微信支付。而且是扫码点餐，是在实体餐厅中扫码，所以所有的订单记录资源都可以作为当前餐厅的子资源。

| URI                                                          | 说明                                                | HTTP方法 |
| ------------------------------------------------------------ | --------------------------------------------------- | -------- |
| /users/{user_id}/{restaurant_id}/orders                      | 顾客在当前餐厅的订单记录，包括待处理的与之前的历史  | GET      |
| /users/{user_id}/{restaurant_id}/orders/{order_id}           | 顾客查看具体订单的信息，包括未完成的订单的信息      | GET      |
| /users/{user_id}/{restaurant_id}/orders/{order_id}/{food_id} | 顾客查看订单里的菜品的信息，重定向到/menu/{food_id} | GET      |
| /users/{user_id}/{restaurant_id}/orders?limimt={}            | 顾客的订单记录查看数量受到limit限制                 | GET      |
| /users/{user_id}/{restaurant_id}/payment                     | 顾客选择支付方式                                    | GET, POST      |
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

### Post the user information [POST]
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

### GET [GET]
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

### Get the history of orders [GET]

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

### Get the order information [GET]

+ Response 200

    [Order][]

## Food Information of Order [/users/{user_id}/{restaurant_id}/orders/{order_id}/{food_id}]

+ Parameters

    + user_id: 123 (int) - 用户的ID
    + restaurant_id: 234 (int) - 餐厅的ID
    + order_id: 1 (int) - 查看的订单的ID
    + food_id: 1 (int) - 查看的订单里的菜品的ID

### Get the food information in the order [GET]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaruants/{restaurant_id}/menu/{food_id}"
            }

# Group Restaruants

This section groups restaurants resources.

## Restaurants Login [/restaurants/login]

The restaurant administrator login website.

### Post the restaurant administrator account information [POST]

+ Request (application/json)

    + Body

            {
              "restaurant_id": 9527,
              "restaurant_password": 1234
            }

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu"
            }


## Restaurants Join [/restaurants/join]

### Post the restaurant administrator account information [POST]

+ Request (application/json)

    + Body

            {
              "restaurant_id": 9527,
              "restaurant_password": 1234
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

### Get the restaurant food list [GET]

+ Response 200

    [Restaurants Food][]

### Post food entry to the restaurant food list [POST]

+ Request

    [Restaurants Food][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }

### Delete food entry from the restaurant list [DELETE]

+ Request

    [Restaurants Food][]

+ Response 204

## Restaurants Food [/restaurants/{restaurant_id}/menu/{food_id}]
查看，编辑，删减菜品的信息。

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

### Get the restaurant food information [GET]

+ Response 200

    [Restaurants Food][]

### Post the new food entry [POST]

修改菜品的信息

+ Request

    [Restaurants Food][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }

### Delete food entry [DELETE]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu"
            }

## Restaurants Orders List [/restaurants/{restaurant_id}/orders]
查看，编辑订单状态

### Get the restaurant orders list [GET]

+ Response 200

    [Restaurants Order][]

### Post order entry to the restaurant orders list [POST]

+ Request

    [Restaurants Order][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/orders/{order_id}"
            }

### Delete order entry from the restaurant orders lit [DELETE]

+ Request

    [Restaurants Order][]

+ Response 204

## Restaurants Order [/restaurants/{restaurant_id}/orders/{order_id}]

+ Model (application/json)

    + Body

            {
              [
                {
                  "food_id": 1,
                  "order_id": 2,
                  "number" : 2,
                  "name": "豆腐",
                  “price”: 10,
                  "description": "美味",
                  "image": "/image/doufu.png"
                }
              ]
            }

+ Parameters

    + restaurant_id: 9527 (int) - 餐厅的ID
    + order_id: 1 (int) - 订单的ID

### Get the restaurant order information [GET]

+ Response 200

    [Restaurants Order][]

### Put the new order entry [PUT]

+ Request

    [Restaurants Order][]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/orders/{order_id}"
            }

### Delete order entry [DELETE]

+ Response 204

## Restaurant Food Information in an Order [/restaurants/{restaurant_id}/orders/{order_id}/{food_id}]

+ Parameters

    + restaurant_id: 9527 (int) - 餐厅的ID
    + order_id: 1 (int) - 订单的ID
    + food_id: 1 (int) - 订单里菜品的ID

### Get the food information [GET]

+ Response 200 (application/json)

    + Body

            {
              "URL": "/restaurants/{restaurant_id}/menu/{food_id}"
            }
