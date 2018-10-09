## 1.1查询用户余额

用户标识符 email : test1@qq.com

```
Curl:

curl -X GET --header 'Accept: application/json' 'http://IP:3040/api/User/test1%40qq.com'


GET Responses：

{
    "$class": "token.Company",
    "email": " test1@qq.com",
    "accountBalance": 2296.5
  }
```





## 1.2用户查看已购买服务记录

1.列表：按照时间先后 ，显示"消费服务ID" 和"时间"。

使用query，请求url 为GetUserConsumeServiceU。

```
Parameters：

user的值是 resource:token.User#user1@email.com  //注意这里的参数需要加上class!!

Curl:

curl -X GET --header 'Accept: application/json' 'http://IP:3040/api/queries/GetUserConsumeServiceU?user=resource%3Atoken.User%23user1%40email.com'

GET Response（按照timestamp从前到后升序排列）：

[
  {
    "$class": "token.UserConsumeService",
    "logs": [],
    "serviceID": "resource:token.Service#service2@mooc",
    "user": "resource:token.User#user1@email.com",
    "transactionId": "f9ba97e35628f077810ce2979c5a6c742020c1d3e1d1476ac9dd72f669568fb8",
    "timestamp": "2018-09-05T04:30:00.447Z"
  }
]
```



2.点开某一个"消费服务ID" 和"时间"，可以看到"消费服务ID"的具体内容。

服务标识符 serviceID:  service2@mooc

```
Curl:

curl -X GET --header 'Accept: application/json' 'http://IP:3040/api/Service/service2%40mooc'

GET Responses：

{
  "$class": "token.Service",
  "serviceID": "service2@mooc",
  "serviceName": "高数课程",
  "servicePrice": 35.5,
  "company": "resource:token.Company#mooc@email.com"
}
```

"消费服务ID"的具体内容包括：服务名称、服务价格、服务提供商



## 2.1用户充值：用户提出一个充值请求

功能描述：输入充值金额，提交，alert 充值成功。

需要提交的参数：

用户ID email :  user1@email.com
充值金额 tokenNum:  150
充值记录的ID rechargeID :  recharge4

```
POST Jason Parameters：
{
  "$class": "token.UserRecharge",
  "tokenNum": 150,  
  "rechargeID": "recharge4",   
  "user": "resource:token.User#user1@email.com"  
}

Curl:

curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
   "$class": "token.UserRecharge", \ 
   "tokenNum": 150,   \ 
   "rechargeID": "recharge4",    \ 
   "user": "resource:token.User#user1%40email.com"   \ 
 }' 'http://IP:3040/api/UserRecharge'
```



## 2.2用户查看充值记录

1.查看用户**成功充值**的记录



1.1 列表：按照"充值ID "的升序 ，显示"充值ID" 、"充值金额"。

使用query，请求url 为GetUserTokenRechargeUserY     ///TODO

```
Parameters：

user的值是 resource:token.User#user1@email.com  //注意这里的参数需要加上class!!

Curl:

curl -X GET --header 'Accept: application/json' 'http://IP:3040/api/queries/GetUserTokenRechargeUserY?user=resource%3Atoken.User%23user1%40email.com'

GET Response（按照"充值ID "从字符顺序小到大升序排列）：

[                 ///TODO
{
    "$class": "token.UserTokenRecharge",
    "tokenRechargeID": "recharge1",
    "confirmBank": "Y",
    "tokenNum": 222,
    "user": "resource:token.User#user1@email.com"
  }
]
```



1.2  点开某一个"充值ID" ，查看"充值时间"。

使用了filter :  {"where": {"rechargeID": "recharge1"}}

上面得到的 "tokenRechargeID": "recharge1"，作为rechargeID的请求参数。

```
Curl:

http://IP:3040/api/UserRecharge?filter=%7B%22where%22%3A%20%7B%22rechargeID%22%3A%20%22recharge1%22%7D%7D


GET Responses：

  {
    "$class": "token.UserRecharge",
    "logs": [],
    "tokenNum": 222,
    "rechargeID": "recharge1",
    "user": "resource:token.User#user1@email.com",
    "transactionId": "b8b7a68172e439aa172c4d91f4a8583475f9949a69881afcca74843b73a65fcd",
    "timestamp": "2018-09-05T04:11:04.819Z"
  }
```

 "timestamp"代表了"充值时间"。



///TODO 使用query完成该操作。





