# Users API 用户接口
##### 不同的接口有不同的调用权限，系统会根据传入的Access Token判断调用方的权限接口返回固定数据结构，其中code:200,message:ok 一般表示成功状态。

## 主要功能接口快速索引
### 注册板块
- [**POST  /users/create **](#userscreate) 用户注册

#####注册时需要做的验证
短信验证/图形验证等由调用方自行完成验证，非必须通过API调用,联盟有收费的短信验证接口

注册时如需要用同步验证用户输入用户名或者手机号是否已被占用可以调用如下接口：

- [**GET  /users/checkusername **](#userscheckusername) 检查用户名唯一性
- [**GET  /users/checkmobile **](#userscheckmobile) 检查手机号唯一性

提交用户注册信息调用如下接口:

- [**POST  /users/create **](#userscreate) 用户注册


#####注册完成默认事项
系统会为用户自动生成一个对应站点的openid,昵称会根据用户名自动生成,机构用户默认调查表已完成状态


### 登录板块
登出与API接口无关，登出只是登出调用方系统，无需经过API服务器，API服务器暂未对所有登出行为进行统计

- [**POST  /users/login **](#userslogin) 用户登录(web端调用接口,该接口无登录统计功能,APP调用建议选择其他接口)

### 用户信息修改板块
- [**POST  /users/update/password **](#usersupdatepassword) 修改密码
- [**POST  /users/forgetpwd **](#usersforgetpwd) 忘记密码
- [**POST  /users/update **](#usersupdate) 普通用户信息更新
- [**POST  /users/update/orgupdate **](#usersupdateorgupdate) 机构用户信息更新
- [**GET  /users/select/basic **](#usersselectbasic) 获取用户基本信息接口
- [**GET  /users/select/detail **](#usersselectdetail) 获取用户详细信息接口
- [**GET  /users/select/detail_full **](#usersselectdetailfull) 获取用户所有账户相关信息接口

### 信息验证板块

短信接口，用户注册，忘记密码，修改密码等需要通过短息验证的都由调用方自行完成，并在调用接口修改数据前使用短信验证码完成验证用户的合法性操作。接口本身只提供数据的修改功能，与合法性验证流程逻辑分离。类似还有七牛云图片上传等。


##所有接口按字母顺序排序

- [**GET /users/select/basic **](#usersselectbasic) 获取用户基本信息接口 
- [**GET /users/checkusername **](#userscheckusername) 检查用户名唯一性
- [**GET /users/checkmobile **](#userscheckmobile) 检查手机号唯一性
- [**POST /users/create **](#userscreate) 用户注册 
- [**GET /users/select/detail **](#usersselectdetail) 获取用户详细信息接口
- [**GET /users/select/detail_full **](#usersselectdetailfull) 获取用户所有账户相关信息接口
- [**POST /users/forgetpwd **](#usersforgetpwd) 忘记密码
- [**POST /users/login **](#userslogin) 用户登录
- [**POST /users/update **](#usersupdate) 普通用户信息更新
- [**POST /users/update/password **](#usersupdatepassword) 修改密码


#### <span id="userscheckusername"> GET /users/checkusername </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

判断用户名是否已经存在。

|参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|username|**required**|用户名||


|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|info| 展示信息|操作成功时才返回|
|status|状态码|操作成功时才返回(1表示存在，0表示不存在)|

###### 返回数据样例
```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "status": 0,
    "info": "数据不存在"
  }
}
 
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "status": 1,
    "info": "用户名被占用"
  }
}
```


#### <span id="userscheckmobile"> GET /users/checkmobile </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

判断手机号是否已经存在。

|参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|mobile|**required**|用户名||


|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|info| 展示信息|操作成功时才返回|
|status|状态码|操作成功时才返回(1表示存在，0表示不存在)|

###### 返回数据样例
```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "status": 0,
    "info": "数据不存在"
  }
}
 
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "status": 1,
    "info": "用户名被占用"
  }
}
```


#### <span id="userscreate"> POST /users/create </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;


创建新用户，根据传入的参数创建新用户，成功则返回用户注册并登录后信息。失败返回相应错误信息。

|参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|username|**optional**|用户名||
|password|**required**|密码||
|mobile|**required**|手机号|当用户名为空时默认手机号为用户名|
|state|**optional**|回调验证码|回传该参数至调用方,屏蔽无效请求|
|appid|**optional**|调用方的appid|如果不传则默认该注册用户为联盟所有|
|regip|**optional**|注册IP|用户注册的IP|
|usertype|**optional**|用户分类|默认为普通用户,机构用户传入字段值为’org'|
|regurl|**optional**|用户注册来源页面url||

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|type|用户类型|操作成功时才返回|
|bank_auth_status| 银行卡绑定状态|操作成功时才返回|
|agent_company_flag| 股权代持公司状态|操作成功时才返回|
|msgcache| 各类通知消息数量|1:系统消息,2评论消息,3私信消息,4项目消息|
|state|回调验证码|操作成功时才返回|
|openid| 用户注册所属平台的唯一的openid|操作成功时才返回|
|openid_uc| 用户针对用户中心唯一的openid|操作成功时才返回|

###### 模拟请求

curl -d "access_token=ACCESS_TOKEN&username=USERNAME&password=PASSWORD&mobile=MOBILE&state=STATE" "HOST_URL/v1/users/create"

###### 返回数据样例
```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "nickname": "an00009",
    "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/default_face8.jpg",
    "auth_status": "0",
    "qa_status": "0",
    "update_time": 1456883768,
    "type": "0",
    "bank_auth_status": "0",
    "agent_company_flag": "0",
    "msgcache": {
      "1": "0",
      "2": "0",
      "3": "0",
      "4": "0"
    },
    "state": "",
    "openid": "07c45e069c210b4f9f9034dab3e68e69",
    "openid_uc": "07c45e069c210b4f9f9034dab3e68e69"
  }
}
 
 {
  "code": 422,
  "message": "数据库验证错误",
  "errors": [
    {
      "message": "已存在",
      "field": "username"
    }
  ],
  "data": "{}"
}
```

#### <span id="usersforgepwd"> POST /users/forgetpwd </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
用户忘记登录密码找回功能(调用该接口前必须完成用户合法性验证)

|请求参数|是否必须 |说明  |备注|
|:--------|:--------:|:--------:|:--------:|
|access_token|**required**| 调用接口必须提供access_token||
|mobile|**required**| 用户注册的手机号||
|newpassword |**optional**|新密码||

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|message|错误信息|修改密码失败时才返回|
|field|错误字段|修改密码失败时才返回|

```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": null
}
```

###<span id="userslogin"> POST /users/login </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
提交用户名/手机/微信号，密码登陆用户
返回信息： 用户所属平台的唯一的openid，昵称，头像和更新时间等

|请求参数|是否必须|说明|备注|
|:----------- | :-----------: |:-----------: |
|access_token| **required**| 调用接口必须提供access_token
|username| **condtional required**| 用户名或手机号必须有
|mobile| **condtional required**| 手机号或用户名必须有
|wx_openid|**condtional required**|微信登录用户的微信open_id|微信登录时需要传入该参数
|password| **required**| 密码
|appid|**optional**|调用方的appid|如果不传则默认该用户从联盟主站登录|
|wx_nickname|**condtional**|微信登录用户的微信昵称|微信登录时同时传入用户名,密码,微信openid,且微信未绑定时则绑定该微信信息
|wx_headimgurl|**condtional**|微信登录用户的微信头像|微信登录时同时传入用户名,密码,微信openid,且微信未绑定时则绑定该微信信息
|wx_sex|**condtional**|微信登录用户的微信性别|微信登录时同时传入用户名,密码,微信openid,且微信未绑定时则绑定该微信信息

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|type|用户类型|操作成功时才返回|
|bank_auth_status| 银行卡绑定状态|操作成功时才返回|
|agent_company_flag| 股权代持公司状态|操作成功时才返回|
|msgcache| 各类通知消息数量|1:系统消息,2评论消息,3私信消息,4项目消息|
|state|回调验证码|操作成功时才返回|
|openid| 用户注册所属平台的唯一的openid|操作成功时才返回|
|openid_uc| 用户针对用户中心唯一的openid|操作成功时才返回|

```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "nickname": "nickname",
    "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/sample.jpeg",
    "auth_status": "9",
    "qa_status": "1",
    "update_time": 1456885640,
    "type": "0",
    "bank_auth_status": "0",
    "agent_company_flag": "0",
    "msgcache": {
      "1": "0",
      "2": "0",
      "3": "0",
      "4": "1"
    },
    "lastlogintime": "1456826312",
    "state": "",
    "openid": "a255e80f40adaa10807cacfceb0b6e8",
    "openid_uc": "a255e80f40adaa10807cacfceb0b6e8"
  }
}
```


#### <span id="usersselectdetail"> POST /users/select/detail </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

根据传入的用户openid获取用户详细信息。

| 参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|user_id|**required**|用户所属平台的唯一的openid||
|state|**optional**|回调函数验证码||


|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|focus| 关注领域|操作成功时才返回|
|birthday| 生日信息|操作成功时才返回|
|weibo| 微博信息|操作成功时才返回|
|province| 省份对应的ID信息|操作成功时才返回|
|city| 市对应的ID信息|操作成功时才返回|
|describe| 个人简介|操作成功时才返回|
|gender| 个人性别|操作成功时才返回|
|email| 用户邮箱|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|state|回调验证码|操作成功时才返回|

```
{
  "code": 200,
  "message": "nickname",
  "errors": [],
  "data": {
   "nickname": "2", 
   "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/2015sample.jpeg", 
   "focus": "2,7",
   "birthday": "1989-01-08", 
   "weibo": "http://weibo.com/22343380", 
   "province": "3", 
   "describe": "describe coentnt”, 
   "city": "75", 
   "email": "33333@163.com", 
   "gender": "0", 
   "auth_status": "9", 
   "qa_status": "1", 
   "update_time": 1456912540, 
   "state": “statecode”  
  }
}
```

#### <span id="usersselectdetailfull"> POST /users/select/detail_full </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

根据传入的用户openid获取用户所有相关详情信息。

| 参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|user_id|**required**|用户所属平台的唯一的openid||
|state|**optional**|回调函数验证码||


|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|focus| 关注领域|操作成功时才返回|
|birthday| 生日信息|操作成功时才返回|
|weibo| 微博信息|操作成功时才返回|
|province| 省份对应的ID信息|操作成功时才返回|
|city| 市对应的ID信息|操作成功时才返回|
|describe| 个人简介|操作成功时才返回|
|gender| 个人性别|操作成功时才返回|
|email| 用户邮箱|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|state|回调验证码|操作成功时才返回|

```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "subsite_id": "1",
    "open_id": "ab55e80f40adaa10807cacfceb0b6e8",
    "user_id": "3388",
    "id": "10800",
    "mobile": "99999999999",
    "username": "username",
    "status": "1",
    "user_updtime": "1456903633",
    "uid": "3388",
    "nickname": "adaman508",
    "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/2015.jpeg",
    "birthday": "1989-02-08",
    "gender": "0",
    "email": "33333@163.com",
    "type": "0",
    "reg_ip": "2147483647",
    "reg_time": "1425052833",
    "auth_status": "9",
    "qa_status": "1",
    "invest_amount": null,
    "focus": "2,7",
    "weibo": "http://weibo.com/212343380",
    "weixin": "weixinusername",
    "project_invest_amount_start": "0.00",
    "project_invest_amount_end": "0.00",
    "company": "companyname",
    "weixin_open_flag": "0",
    "linkin": null,
    "blog_url": null,
    "province": "3",
    "city": "75",
    "country": null,
    "address": "上城区南星街道凤凰山脚路7号C1",
    "position": "CCO",
    "income": null,
    "resume": null,
    "dess": "DESCRIBE",
    "invest_idea": "MY INVEST IDEA",
    "focus_city": "291",
    "bank_auth_status": "0",
    "agent_company_flag": "0",
    "userinfo_updtime": "1456971576",
    "legal_name": null,
    "card_no": null,
    "contact": null,
    "tel": null,
    "license_no": null,
    "license_url": null,
    "auth_error": null,
    "org_updtime": null,
    "real_name": "张三",
    "card_id": "3301241989020800111",
    "card_photo_url": null,
    "org_code": "",
    "tax_code": null,
    "card_type": "0",
    "authinfo_updtime": "1449568353"
  }
}
```


#### <span id="usersselectbasic"> POST /users/select/basic </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

根据传入的用户openid获取用户基本信息。

| 参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **required**|调用接口必须提供access_token||
|user_id|**required**|用户所属平台的唯一的openid||
|state|**optional**|回调函数验证码||


|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|type|用户类型|操作成功时才返回|
|bank_auth_status| 银行卡绑定状态|操作成功时才返回|
|agent_company_flag| 股权代持公司状态|操作成功时才返回|
|msgcache| 各类通知消息数量|1:系统消息,2评论消息,3私信消息,4项目消息|
|state|回调验证码|操作成功时才返回|
|openid| 用户注册所属平台的唯一的openid|操作成功时才返回|
|openid_uc| 用户针对用户中心唯一的openid|操作成功时才返回|

```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "nickname": "nickname",
    "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/sample.jpeg",
    "auth_status": "9",
    "qa_status": "1",
    "update_time": 1456885640,
    "type": "0",
    "bank_auth_status": "0",
    "agent_company_flag": "0",
    "msgcache": {
      "1": "0",
      "2": "0",
      "3": "0",
      "4": "1"
    },
    "lastlogintime": "1456826312",
    "state": "",
    "openid": "a255e80f40adaa10807cacfceb0b6e8",
    "openid_uc": "a255e80f40adaa10807cacfceb0b6e8"
  }
}
```

#### <span id="usersupdate"> POST /users/update </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
根据提供的参数更新用户信息
  
返回信息： 用户的更新以后的基本信息

|请求参数|是否必须 |说明  |备注|
|:--------|:--------:|:--------:|:--------:|
|access_token|**required**| 调用接口必须提供access_token||
|user_id|**required**| 用户针对该平台的唯一的openid||
|nickname |**optional**|   昵称||
|photo_url |**optional**|   头像url||
|focus |**optional**|   投资专注的领域||
|birthday |**optional**|   出生年月日||
|weibo |**optional**|  微博地址||
|province|**optional**|  省||
|city |**optional**|  市||
|country |**optional**|   区||
|describe |**optional**|   个人简介||
|mobile |**optional**|  手机||
|email |**optional**|  电子邮箱||
|gender |**optional**|  性别||
|resume |**optional**|   简历||
|describe |**optional**|  简介||
|address|**optional**| 详细地址||
|company |**optional**|  公司||
|position |**optional**|   职位||
|income |**optional**|   收入||
|invest_idea |**optional**|  投资理念||
|focus_city |**optional**|  关注城市||
|weixin |**optional**|  微信号||
|project_invest_amount_start |**optional**|   单项目最低投资额||
|project_invest_amount_end |**optional**|  单项目最高投资额||
|invest_amount|**optional**|  投资规模||
|weixin_open_flag |**optional**|  是否公开微信号||
|linkin |**optional**|   linkedin账号||
|blog_url |**optional**|   微博账号||

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|type|用户类型|操作成功时才返回|
|bank_auth_status| 银行卡绑定状态|操作成功时才返回|
|agent_company_flag| 股权代持公司状态|操作成功时才返回|
|msgcache| 各类通知消息数量|1:系统消息,2评论消息,3私信消息,4项目消息|
|state|回调验证码|操作成功时才返回|
|openid| 用户注册所属平台的唯一的openid|操作成功时才返回|
|openid_uc| 用户针对用户中心唯一的openid|操作成功时才返回|

```
{
    "code": 200,
    "message": "OK",
    "errors": [],
    "data": {
        "nickname": "nickname",
        "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/2015.jpeg",
        "auth_status": "9",
        "qa_status": "1",
        "update_time": 1456971576,
        "type": "0",
        "bank_auth_status": "0",
        "agent_company_flag": "0",
        "msgcache": {
            "1": "0",
            "2": "0",
            "3": "0",
            "4": "1"
        },
        "openid": "a255e80f40adaa10807cacfceb0b6e8"
    }
}
```

#### <span id="usersupdateorgupdate"> POST /users/update/orgupdate </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
根据提供的参数更新机构用户信息
  
返回信息： 机构用户的更新以后的基本信息

|请求参数|是否必须 |说明  |备注|
|:--------|:--------:|:--------:|:--------:|
|access_token|**required**| 调用接口必须提供access_token||
|user_id|**required**| 用户针对该平台的唯一的openid||
|REF:USERS/UPDATE |**optional**|参考普通用户|**除以下额外参数以外，其他与普通用户所能传的参数一致**|
|legal_name |**optional**|  法人姓名||
|contact |**optional**|   联系人姓名||
|license_no |**optional**|  营业执照号码||
|license_url|**optional**|  营业执照文件地址||
|tel |**optional**|  联系人电话||
|card_no |**optional**|   法人身份证号||
|auth_error |**optional**|   机构审核错误信息||

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|nickname| 用户昵称|操作成功时才返回|
|photo_url| 用户头像|操作成功时才返回|
|authstatus| 用户实名状态|操作成功时才返回|
|qastatus| 用户问卷调查状态|操作成功时才返回|
|type|用户类型|操作成功时才返回|
|bank_auth_status| 银行卡绑定状态|操作成功时才返回|
|agent_company_flag| 股权代持公司状态|操作成功时才返回|
|msgcache| 各类通知消息数量|1:系统消息,2评论消息,3私信消息,4项目消息|
|state|回调验证码|操作成功时才返回|
|openid| 用户注册所属平台的唯一的openid|操作成功时才返回|
|openid_uc| 用户针对用户中心唯一的openid|操作成功时才返回|

```
{
    "code": 200,
    "message": "OK",
    "errors": [],
    "data": {
        "nickname": "nickname",
        "photo_url": "http://7xkfhz.dl1.z0.glb.clouddn.com/2015.jpeg",
        "auth_status": "9",
        "qa_status": "1",
        "update_time": 1456971576,
        "type": "0",
        "bank_auth_status": "0",
        "agent_company_flag": "0",
        "msgcache": {
            "1": "0",
            "2": "0",
            "3": "0",
            "4": "1"
        },
        "openid": "a255e80f40adaa10807cacfceb0b6e8"
    }
}
```


#### <span id="usersupdatepassword"> POST /users/update/password </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
用户登录密码修改功能

|请求参数|是否必须 |说明  |备注|
|:--------|:--------:|:--------:|:--------:|
|access_token|**required**| 调用接口必须提供access_token||
|user_id|**required**| 针对该平台的唯一的openid||
|newpassword |**optional**|新密码||
|oldpassword |**optional**|原密码||

|返回data参数说明|说明|备注| 
|:-----------|:-----------:|:-----------|
|message|错误信息|修改密码失败时才返回|
|field|错误字段|修改密码失败时才返回|

```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": null
}

{
  "code": 1002,
  "message": "_ERR_CODE_DB_VALIDATE_",
  "errors": [
    {
      "message": "密码不正确",
      "field": "password"
    }
  ],
  "data": "{}"
}
```
