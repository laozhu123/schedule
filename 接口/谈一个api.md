[TOC]

### Api for app

#### User(用户相关)

##### 发送短信验证码
```
/third/send_verify_code
参数
{"token":"6c8f11d661e248a6bc93ca58ee8baf3c","tsp":"1529493590","tel":"17764507393"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "msgId": "528907474593"
    }
}
备注
msgId手机号注册时需要再传给后台，客户端暂存下
```

##### 用户手机号注册
```
/user/tel_register
参数
{"device":0,"password":"12345","tel":"17764507393","msgId":"528907474593","code":"002440","cid":"fbce519333f74367b3dd0261a67441d3"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "user": {
            "id": 1,
            "loveNum": "",
            "tel": "17764507393",
            "imToken": "",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "",
            "pic": "",
            "picStatus": 0,
            "sex": 0,
            "birth": "",
            "constellation": "",
            "loveStatus": "",
            "education": "",
            "profession": "",
            "height": 0,
            "car": "",
            "house": "",
            "smoke": "",
            "drink": "",
            "nation": "",
            "address": "",
            "nativePlace": "",
            "marry": "",
            "tanBai": ""
        },
        "token":"01ec78cbea7c44eabd75f1eae2399186"
    }
}
备注
device设备类型，0=ios 1=android；cid设备的唯一标识id
```

##### 获取七牛token

```
/third/get_qnToken
参数
{"token":"6c8f11d661e248a6bc93ca58ee8baf3c","tsp":"1529493591"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "qnToken": "e468YSXjYJUkeUABfe1DdBDQL0Wgt4AlMnekaazp:lRczSFPACiPOLn4nyB6r3rozqOQ=:eyJzY29wZSI6ImppdWRpYW4iLCJkZWFkbGluZSI6MTUyOTc3NzA1MH0="
    }
}
```

##### 注册第二步：更新必要个人信息

```
/user/perfect_profile
参数
{"tsp":1529728299,"token":"01ec78cbea7c44eabd75f1eae2399186","pic":"03a191d265fc45d69c36b8be052632fa","nick":"过年的过","sex":0,"birth":"1998-08-08"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "user": {
            "id": 3,
            "loveNum": "this is loveNum.",
            "tel": "17764507393",
            "imToken": "5fjk02B/R6coqIAfV3pLC+WauDHLnVqG59I/unHxxtW6Fyih6OFz8yjbf9mTCLahHOSLH5x/SKxfQOq4kwNyUg==",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "过年的过",
            "pic": "03a191d265fc45d69c36b8be052632fa",
            "picStatus": 0,
            "sex": 0,
            "birth": "1998-08-08",
            "constellation": "",
            "loveStatus": "",
            "education": "",
            "profession": "",
            "height": 0,
            "car": "",
            "house": "",
            "smoke": "",
            "drink": "",
            "nation": "",
            "address": "",
            "nativePlace": "",
            "marry": "",
            "tanBai": ""
        }
    }
}
备注
sex 0=男 1=女	
```

##### 用户手机号登陆

```
/user/tel_login
参数
{"device":0,"password":"12345","tel":"17764507393","cid":"fbce519333f74367b3dd0261a67441d3"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "user": {
            "id": 1,
            "loveNum": "",
            "tel": "17764507393",
            "imToken": "",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "neOG",
            "pic": "",
            "picStatus": 0,
            "sex": 0,
            "birth": "1991-08-15",
            "constellation": "",
            "loveStatus": "",
            "education": "硕士",
            "profession": "",
            "height": 188,
            "car": "",
            "house": "",
            "smoke": "",
            "drink": "",
            "nation": "汉族",
            "address": "杭州市",
            "nativePlace": "绍兴市",
            "marry": "",
            "tanBai": "给些时间，才见真谛。"
        },
        "token":"01ec78cbea7c44eabd75f1eae2399186"
    }
```

##### 退出登陆

```
/user/logout
参数
{"tsp":1529728299,"token":"01ec78cbea7c44eabd75f1eae2399186"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {}
}
```

##### 获取个人择偶标准

```
/user/get_mChoice
参数
{"tsp":1529729989,"token":"01ec78cbea7c44eabd75f1eae2399186"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "mateChoice": {
            "id": 2,
            "userId": 0,
            "age": "不限",
            "address": "不限",
            "nativePlace": "不限",
            "education": "不限",
            "height": "不限",
            "income": "不限",
            "loveStatus": "不限",
            "house": "不限",
            "car": "不限",
            "otherReq": "不限"
        }
    }
}
备注
默认值 不限
```

##### 更新择偶标准
```
/user/update_mChoice
参数
{"tsp":1529730984,"token":"01ec78cbea7c44eabd75f1eae2399186","age":"25-30","address":"杭州市","nativePlace":"杭州市","education":"本科","height":188,"income":"10w-20w","loveStatus":"未婚","house":"已购房","car":"已购车","otherReq":"单纯善良"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "mateChoice": {
            "id": 2,
            "userId": 3,
            "age": "25-30",
            "address": "杭州市",
            "nativePlace": "杭州市",
            "education": "本科",
            "height": "188",
            "income": "10w-20w",
            "loveStatus": "未婚",
            "house": "已购房",
            "car": "已购车",
            "otherReq": "单纯善良"
        }
    }
}
```

##### 更新个人信息

```
/user/update_profile
参数
{"tsp":1529830987,"token":"e6af92478f3143d8a030b9ec89c2a318","nick":"过关","birth":"1991-12-12","pic":"fbce519333f74367b3dd0261a67441d3","sex":0,"height":188,"income":"10w-20w","loveStatus":"未婚","education":"硕士","car":"已购车","profession":"设计师","house":"已购房","smoke":"不抽烟","drink":"偶尔小酌","nation":"汉族","address":"杭州市","nativePlace":"绍兴市","marry":"三年内结婚","tanBai":"给些时间，才见真谛"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "user": {
            "id": 3,
            "loveNum": "this is loveNum.",
            "tel": "17764507393",
            "imToken": "5fjk02B/R6coqIAfV3pLC+WauDHLnVqG59I/unHxxtW6Fyih6OFz8yjbf9mTCLahHOSLH5x/SKxfQOq4kwNyUg==",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "过关",
            "pic": "fbce519333f74367b3dd0261a67441d3",
            "picStatus": 1,
            "sex": 0,
            "birth": "1991-12-12",
            "constellation": "",
            "loveStatus": "未婚",
            "education": "硕士",
            "profession": "设计师",
            "height": 188,
            "car": "已购车",
            "house": "已购房",
            "smoke": "不抽烟",
            "drink": "偶尔小酌",
            "nation": "汉族",
            "address": "杭州市",
            "nativePlace": "绍兴市",
            "marry": "三年内结婚",
            "tanBai": "给些时间，才见真谛"
        }
    }
}
```

##### 用户上传单张照片

```
/user/add_pic
参数
{"tsp":1529830988,"token":"e6af92478f3143d8a030b9ec89c2a318","pic":"fbce519333f74367b3dd0261a674apd3"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "pic": {
            "id": 1,
            "userId": 3,
            "proId": 0,
            "pic": "fbce519333f74367b3dd0261a674apd3",
            "type": 0,
            "status": 0,
            "createTime": "2018-06-23 15:50:12"
        }
    }
}
```

##### 删除单张照片

```
/user/delete_pic
参数
{"tsp":1529830990,"token":"e6af92478f3143d8a030b9ec89c2a318","pic":"fbce519333f74367b3dd0261a674apd3"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {}
}
```

##### 添加意见反馈

```
/user/add_feedbackInfo
参数
{"tsp":1529830995,"token":"e6af92478f3143d8a030b9ec89c2a318","picIds":"[\"1e62f8bec9044693bac211aa4717fe53\",\"c1d8bf01dd4341779b993c0b94813af6\"]","content":"棒棒"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {}
}
备注
照片list传json数组
```

##### 添加举报信息

```
/user/add_reportInfo
参数
{"tsp":1529830997,"token":"e6af92478f3143d8a030b9ec89c2a318","picIds":"[\"1e62f8bec9044693bac211aa4717fe53\",\"c1d8bf01dd4341779b993c0b94813af6\"]","content":"有毒","targetUid":1}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {}
}
备注
照片list传json数组
```

##### 获取个人信息（查看别人个人信息）

```
/user/get_userInfo
参数
{"tsp":1529830999,"token":"e6af92478f3143d8a030b9ec89c2a318","targetUid":1}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "userPics": [
            {
                "id": 5,
                "userId": 3,
                "proId": 2,
                "pic": "c1d8bf01dd4341779b993c0b94813af6",
                "type": 1,
                "status": 0,
                "createTime": "2018-06-23 18:42:13"
            }
        ],
        "user": {
            "id": 3,
            "loveNum": "this is loveNum.",
            "tel": "17764507393",
            "imToken": "5fjk02B/R6coqIAfV3pLC+WauDHLnVqG59I/unHxxtW6Fyih6OFz8yjbf9mTCLahHOSLH5x/SKxfQOq4kwNyUg==",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "过关",
            "pic": "fbce519333f74367b3dd0261a67441d3",
            "picStatus": 1,
            "sex": 0,
            "birth": "1991-12-12",
            "constellation": "",
            "loveStatus": "未婚",
            "education": "硕士",
            "profession": "设计师",
            "height": 188,
            "car": "已购车",
            "house": "已购房",
            "smoke": "不抽烟",
            "drink": "偶尔小酌",
            "nation": "汉族",
            "address": "杭州市",
            "nativePlace": "绍兴市",
            "marry": "三年内结婚",
            "tanBai": "给些时间，才见真谛"
        }
    }
}
备注
查看别人个人信息需传targetUid，查看自己的信息不需要targetUid。
```

##### 获取所有语音问题list

```
/user/get_ques_list
参数
{"tsp":1529831003,"token":"e6af92478f3143d8a030b9ec89c2a318"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "list": [
            {
                "id": 1,
                "content": "你喜欢什么星座，为什么？",
                "status": 0
            },
            {
                "id": 2,
                "content": "喜欢吃什么菜？",
                "status": 0
            }
        ]
    }
}
```

##### 回答语音问题

```
/user/ans_voiceQues
参数
{"tsp":1529831002,"token":"e6af92478f3143d8a030b9ec89c2a318","questionId":1,"voiceId":"e37f0b801f6a439da23548eb0d3c67e7","length":10}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "userVoice": {
            "id": 2,
            "userId": 3,
            "questionId": 1,
            "voiceId": "e37f0b801f6a439da23548eb0d3c67e7",
            "length": 10,
            "createTime": "2018-06-23 22:06:16"
        }
    }
}
```

##### 获取每日推荐用户

```
/match/get_match_user
参数
{"tsp":1529831005,"token":"e6af92478f3143d8a030b9ec89c2a318"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "dailyMatch": {
            "id": 1,
            "loveNum": "",
            "tel": "17764507394",
            "imToken": "",
            "device": 0,
            "cid": "fbce519333f74367b3dd0261a67441d3",
            "status": 0,
            "name": "",
            "nick": "neOG",
            "pic": "",
            "picStatus": 0,
            "sex": 0,
            "birth": "1991-08-15",
            "constellation": "",
            "loveStatus": "",
            "education": "硕士",
            "profession": "",
            "height": 188,
            "car": "",
            "house": "",
            "smoke": "",
            "drink": "",
            "nation": "汉族",
            "address": "杭州市",
            "nativePlace": "绍兴市",
            "marry": "",
            "tanBai": "给些时间，才见真谛。"
        }
    }
}
```

#### Auth(认证相关)

##### 身份证审核

```
/auth/verify_idCard
参数
{"tsp":1529831006,"token":"e6af92478f3143d8a030b9ec89c2a318","name":"过伟荣","idNum":"33068319910815285X","frontPic":"aabe4c0ad1944d8c8272369d56591a1a","frontDPic":"278d1e15addd47f49e46ce6ff426e44c"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "idCard": {
            "id": 1,
            "userId": 3,
            "realName": "过伟荣",
            "idNum": "33068319910815285X",
            "frontPic": "aabe4c0ad1944d8c8272369d56591a1a",
            "frontDPic": "278d1e15addd47f49e46ce6ff426e44c",
            "status": 1,
            "createTime": "2018-06-24 11:59:48"
        }
    }
}
备注
frontPic身份证正面照，frontDPic手持身份证照；status=1审核中
```

##### 学校学历审核

```
/auth/verify_education
参数
{"tsp":1529831007,"token":"e6af92478f3143d8a030b9ec89c2a318","schoolName":"浙江工业大学","education":"硕士","pic":"d4f21d8a0159464f9c8e700890c3d7bf"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "education": {
            "id": 1,
            "userId": 3,
            "schoolName": "浙江工业大学",
            "education": "硕士",
            "pic": "d4f21d8a0159464f9c8e700890c3d7bf",
            "status": 1,
            "createTime": "2018-06-24 12:04:25"
        }
    }
}
```

##### 车辆信息审核

```
/auth/verify_car
参数
{"tsp":1529831008,"token":"e6af92478f3143d8a030b9ec89c2a318","carName":"BMW","pic":"d4f21d8a0159464f9c8e700890c3d7bf"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "car": {
            "id": 1,
            "userId": 3,
            "carName": "BMW",
            "pic": "d4f21d8a0159464f9c8e700890c3d7bf",
            "status": 1,
            "createTime": "2018-06-24 12:06:12"
        }
    }
}
```

##### 房产信息审核

```
/auth/verify_house
参数
{"tsp":1529831009,"token":"e6af92478f3143d8a030b9ec89c2a318","address":"杭州市","pic":"d4f21d8a0159464f9c8e700890c3d7bf"}
返回
{
    "code": 200,
    "message": "Ok",
    "data": {
        "house": {
            "id": 1,
            "userId": 3,
            "address": "杭州市",
            "pic": "d4f21d8a0159464f9c8e700890c3d7bf",
            "status": 1,
            "createTime": "2018-06-24 12:07:24"
        }
    }
}
```

