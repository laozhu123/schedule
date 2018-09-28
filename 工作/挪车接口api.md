# 齐车大圣挪车码API接入文档

[toc]

## 1 接入方式

## 2 名词解释

### 2.1 api_key
向齐车大圣API请求的时候，都需要带上本参数

### 2.2 secret_key
第三方应该严格保密本参数，只能在验证的时候使用

### 2.3 sign
签名，使用api_key和secret_key对参数进行签名后的签名数据

## 3 签名方法
### 3.1 生成base_string
将所有参数，额外增加api_key，随机字符串，组合在一起，按照key进行排 序，之后使用RFC3986 urlencode将数组map组合成字符串

### 3.2 加密
使用HMAC-SHA1方法和密钥对base_string进行加密，之后将加密的结果进行 base64编码，再放入请求的sign中

### 3.3 Sample(in python)
```
import urllib.parse
import hmac
import requests
import time
import base64
import random
import json

url5 = "http://open.mys4s.cn/openapi/code/query"

def urlencodeRFC3986(value):
    return urllib.parse.quote(value).replace("+", "%20").replace("%7E", "~")

def sign(param):
    kvs = []

    # sort
    lst = list(param.keys())
    lst.sort()

    # concat
    for k in lst:
	kvs.append("{}={}".format(k, urlencodeRFC3986(param[k])))

    baseStr = "&".join(kvs)
    # print(baseStr)

    # hmac_sha1
    my_sign = hmac.new(api_secret.encode(), baseStr.encode(), sha1).digest()
    return base64.b64encode(my_sign)

def test_openapi_code_query(key, code, uid):
    param = {
        "api_key": key,
        "code": code,
        "uid": uid
    }
    param["sign"] = sign(param).decode()

    print(json.dumps(param, ensure_ascii=False, indent=4))
    resp = requests.post(url, data=param)
    print(json.dumps(resp.json(), indent=4, ensure_ascii=False))
    
def main():
	test_openapi_code_query("100001", "201801161009206150285", "1")
    
if __name__ == '__main__':
    main()
```


## 4 挪车码接口

> 挪车码接口分为：创建、查询（查询单个挪车码、查询用户挪车码）、删除、通知挪车（短信、电话）等接口。


### 4.1 创建挪车码
#### 4.1.1 url
http://open.mys4s.cn/openapi/code/create

#### 4.1.2 请求方式
POST

#### 4.1.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     uid   |     string   | 用户id | | 1 |
|     tel   |     string   | 手机号 | | 17776767878 |
|     addr   |   string     | 地址 | | 浙江杭州西湖区 |
|     carno   |   string     | 车牌号 | | 浙A12345 |
|     qid   |    string    | 挪车码 | 扫描出来的挪车码 | 2018011610092061502 |
|     code   |   string     | 短信验证码 |  | 1234 |
|     name   |    string    | 姓名 | | 王瓦工 |
|     style   |   string     | 类型  | | - |

#### 4.1.4 返回数据格式

json(utf8)

#### 4.1.5 返回数据样例

1.创建成功

```
{
    "code": 0,
    "data": {
        "Id": 3,
        "UserId": 122,
        "Code": "20180116100938150",
        "CarNo": "浙A12378",
        "Tel": "15158027703",
        "Name": "",
        "Addr": "",
        "Style": "",
        "CreateTime": "2018-01-16 10:09:38",
        "Status": 0
    }
}

```

2.挪车码有误失败

```
{
    "code": 20103,
    "msg": "验证码错误"
}
```

3.用户不存在

```
{
    "code": 10000,
    "msg": "用户不存在"
}
```

#### 4.1.6 返回字段说明

| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     Id   |     int   | 挪车码id | | 3 |
|     UserId   |   int     | 用户id | | 122 |
|     Code   |   string     | 挪车码 | | 20180116100938150 |
|     CarNo   |    string    | 车牌号 |  | 浙A1237 |
|     Tel   |   string     | 手机号 |  | 15158027703 |
|     Name   |    string    | 姓名 | | 王瓦工 |
|     Addr   |   string     | 用户地址 | | 浙江杭州 |
|     Style   |   string     | 类型 | |  |
|     CreateTime   |   string     | 创建时间 | | 2018-01-16 10:09:38 |
|     Status   |     int   | 挪车码状态 | 0:有效 1:无效 | 1 |
|     Code   |     int   | 错误码 |  | 10000 |
|     msg   |     string   | 错误信息 |  | 用户不存在 |


### 4.2 查询单个挪车码（通过code）
#### 4.2.1 url
http://open.mys4s.cn/openapi/code/query

#### 4.2.2 请求方式
POST

#### 4.2.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     uid   |     int   | 用户id | | 1 |
|     code   |   string     | 挪车码 |  | 201801161009206150285 |

#### 4.2.4 返回数据格式

json(utf8)

#### 4.2.5 返回数据样例

1.获取成功
```
{
    "code": 0,
    "data": {
        "Code": {
            "Id": 2,
            "UserId": 5000232,
            "Code": "201801161009206150285",
            "CarNo": "闽123456",
            "Tel": "",
            "Name": "",
            "Addr": "",
            "Style": "",
            "CreateTime": "2018-01-16 10:09:20",
            "Status": 1
        },
        "IsOwner": false,
        "NeedBind": false
    }
}
```

2.获取失败
```
{
    "code": 0,
    "data": {
        "IsOwner": true,
        "NeedBind": true
    }
}
```
#### 4.2.6 返回字段说明

| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     Id   |     int   | 挪车码id | | 3 |
|     UserId   |   int     | 用户id | | 122 |
|     Code   |   string     | 挪车码 | | 20180116100938150 |
|     CarNo   |    string    | 车牌号 |  | 浙A1237 |
|     Tel   |   string     | 手机号 |  | 15158027703 |
|     Name   |    string    | 姓名 | | 王瓦工 |
|     Addr   |   string     | 用户地址 | | 浙江杭州 |
|     Style   |   string     | 类型 | |  |
|     CreateTime   |   string     | 创建时间 | | 2018-01-16 10:09:38 |
|     Status   |     int   | 挪车码状态 | 0:有效 1:无效 | 1 |
|     IsOwner   |     bool   | 用户是否拥有该挪车码 | true：拥有  false：不拥有 | true |
|     NeedBind   |     bool   | 该挪车码是否已绑定 | true：未绑定  false：已绑定 | true  |

### 4.3 查询用户挪车码列表
#### 4.3.1 url
http://open.mys4s.cn/openapi/code/query_list

#### 4.3.2 请求方式
POST

#### 4.3.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     uid   |     int   | 用户id |  | 5000232 |

#### 4.3.4 返回数据格式

json(utf8)

#### 4.3.5 返回数据样例
1.获取成功（有挪车码）
```
{
    "code": 0,
    "data": [
        {
            "Id": 1,
            "UserId": 5000232,
            "Code": "201801161008576150285",
            "CarNo": "皖123456",
            "Tel": "13702760463",
            "Name": "",
            "Addr": "",
            "Style": "",
            "CreateTime": "2018-01-16 10:08:57",
            "Status": 0
        },
        {
            "Id": 6,
            "UserId": 5000232,
            "Code": "201804021916496150285",
            "CarNo": "冀A123445",
            "Tel": "13702760463",
            "Name": "",
            "Addr": "",
            "Style": "",
            "CreateTime": "2018-04-02 19:16:49",
            "Status": 0
        }
    ]
}
```
2.无挪车码

```
{
    "code": 0,
    "data": []
}
```
#### 4.3.6 返回字段说明

| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     Id   |     int   | 挪车码id | | 3 |
|     UserId   |   int     | 用户id | | 122 |
|     Code   |   string     | 挪车码 | | 20180116100938150 |
|     CarNo   |    string    | 车牌号 |  | 浙A1237 |
|     Tel   |   string     | 手机号 |  | 15158027703 |
|     Name   |    string    | 姓名 | | 王瓦工 |
|     Addr   |   string     | 用户地址 | | 浙江杭州 |
|     Style   |   string     | 类型 | |  |
|     CreateTime   |   string     | 创建时间 | | 2018-01-16 10:09:38 |
|     Status   |     int   | 挪车码状态 | 0:有效 1:无效 | 1 |


### 4.4 删除挪车码
#### 4.4.1 url
http://open.mys4s.cn/openapi/code/delete

#### 4.4.2 请求方式
POST

#### 4.4.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     uid   |     int   | 用户id |  | 5000232 |
|     qid   |     string   | 挪车码 |  | 201801161009206150285 |

#### 4.4.4 返回数据格式

json(utf8)

#### 4.4.5 返回数据样例
1.删除成功
```
{
    "code": 0,
    "data": {
        "Id": 2,
        "UserId": 5000232,
        "Code": "201801161009206150285",
        "CarNo": "闽123456",
        "Tel": "13702760463",
        "Name": "",
        "Addr": "",
        "Style": "",
        "CreateTime": "2018-01-16 10:09:20",
        "Status": 1
    }
}
```

2.删除失败(挪车码不存在)
```
{
    "code": 10003,
    "msg": "服务器繁忙，请稍后重试"
}
```

3.删除失败（挪车码不属于用户）
```
{
    "code": 10000,
    "msg": "只能删除自己的挪车码"
}
```

#### 4.4.6 返回字段说明

| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     Id   |     int   | 挪车码id | | 3 |
|     UserId   |   int     | 用户id | | 122 |
|     Code   |   string     | 挪车码 | | 20180116100938150 |
|     CarNo   |    string    | 车牌号 |  | 浙A1237 |
|     Tel   |   string     | 手机号 |  | 15158027703 |
|     Name   |    string    | 姓名 | | 王瓦工 |
|     Addr   |   string     | 用户地址 | | 浙江杭州 |
|     Style   |   string     | 类型 | |  |
|     CreateTime   |   string     | 创建时间 | | 2018-01-16 10:09:38 |
|     Status   |     int   | 挪车码状态 | 0:有效 1:无效 | 1 |


### 4.5 挪车短信提醒
#### 4.5.1 url
http://open.mys4s.cn/openapi/code/sms

#### 4.5.2 请求方式
POST

#### 4.5.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     uid   |     int   | 用户id |  | 5000232 |
|     qid   |     string   | 挪车码 |  | 20180116100938150 |
|     loc   |     string   | 当前车辆所在地址 | 详细地址 | 某某街道 |
|     des   |     string   | 对需要挪车的描述 | 需要对方挪车的理由 | 挡住过往车辆了 |

#### 4.5.4 返回数据格式

json(utf8)

#### 4.5.5 返回数据样例
1.通知成功
```
{
    "code": 0,
    "msg": "已通知车主"
}
```

2.通知失败
```
{
    "code": 10000,
    "msg": "通知失败，请重试"
}
```

3.获取用户失败
```
{
    "code": 10000,
    "msg": "获取用户失败"
}
```


### 4.6 电话通知挪车
#### 4.6.1 url
http://open.mys4s.cn/openapi/code/call

#### 4.6.2 请求方式
POST

#### 4.6.3 请求参数
| Name | Type | meaning | ps | sample |
|--------|--------|--|--|--|
|     tel   |     string   | 当前用户手机号 |  | 13702760463 |
|     qid   |     string   | 挪车码 | | 20180116100938150 |

#### 4.6.4 返回数据格式

json(utf8)

#### 4.6.5 返回数据样例
1.获取手机号成功
```
{
    "code": 0,
    "msg": "18989090909"
}
```

2.获取用户失败
```
{
    "code": 10000,
    "msg": "获取用户失败"
}
```

3.不能呼叫自己
```
{
    "code": 10000,
    "msg": "不能呼叫自己"
}
```