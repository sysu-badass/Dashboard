## API 设计

### API设计工具
根据老师的推荐，我们使用了apiary工具来进行我们的API设计，具体的API设计文档可点击[此处](https://eatouteorder.docs.apiary.io/#)查看。 apiary的tutorial可以通过这些[example](https://apiblueprint.org/documentation/examples/)去学习,以及相关的一些[资料](https://help.apiary.io/tools/)。具体的设计规范可以在[./Documents/生产规范与指南/REST_API设计规范.md](https://github.com/sysu-badass/Dashboard/blob/master/Documents/生产规范与指南/REST_API设计规范.md)文件中查看。

### URI设计

restaurant的后台管理的URI
| URI                                                           | 说明                                                  | HTTP方法         |
| ------------------------------------------------------------- | ----------------------------------------------------- | ---------------- |
| /restaurants/login                                            | 餐厅管理后台账号登录                                  | POST             |
| /restaurants/join                                             | 餐厅管理后台账号创建                                  | POST             |
| /restaurants/{restaurant_id}/menu                             | 餐厅的餐单资料                                        | GET, POST        |
| /restaurants/{restaurant_id}/menu/{food_id}                   | 菜品的具体信息                                        | GET, PUT, DELETE |
| /restaurants/{restaurant_id}/orders                           | 餐厅的订单列表，包含待处理的和已处理的                | GET, POST        |
| /restaurants/{restaurant_id}/orders?date={}&user={}&status={} | 通过订单的日期，下单user_id以及完成状态status进行检索 | GET, PUT, DELETE |
| /restaurants/{restaurant_id}/orders/{order_id}                | 通过order自身的id来查看订单数据                       | GET, PUT, DELETE |

user的信息管理URI，考虑到我们的用户仅仅需要查看订单，修改购物车以及支付，所以很多操作都只需要GET HTTP方法就可以了。支付方式暂时只支持微信支付。而且是扫码点餐，是在实体餐厅中扫码，所以所有的订单记录资源都可以作为当前餐厅的子资源。
| URI                                                     | 说明                                       | HTTP方法  |
| ------------------------------------------------------- | ------------------------------------------ | --------- |
| /users/{user_id}/{restaurant_id}/orders                                 | 顾客在当前餐厅的订单记录，包括待处理的与之前的历史   | GET       |
| /users/{user_id}/{restaurant_id}/orders/?limimt={}                      | 顾客的订单记录查看数量收到limit限制        | GET       |
| /users/{user_id}/{restaurant_id}shoppingcart/           | 顾客在当前餐厅的购物车，由餐厅的id进行限制 | GET       |
| /users/{user_id}/{restaurant_id}/shoppingcart/{food_id} | 顾客在当前餐厅购物车中查看具体的菜品信息           | GET       |
| /users/{user_id}/{restaurant_id}/payment                              | 顾客选择支付方式                           | GET, POST |

### 各种对象大概的数据结构
由于food和order都是restraurants URI的子资源，所以不需要restaurant_id去检索。
```
food {
  int restaurant_id
  int food_id;
  string food_name;
  string food_image_location; #例如URL
  int food_price;
}
```

```
date {
  int order_id;
  int year;
  int month;
  int day;
  int hour;
  int minute;
}

order {
  int order_id;  #order自身的id
  int user_id; #下单顾客的id
  int restaurant_id;  #餐厅的id
  #这三个id方便数据库进行检索
  food foods[]; #点餐的food数组
  date order_date;
  int desk_number;
  int price;
}
```

```
payment {
  int payment_id; #选择支付方式用id进行表示。
  int user_id;
}
```
