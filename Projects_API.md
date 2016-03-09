# Porject API 项目接口
##### 
## 主要功能接口快速索引

### 股权项目提交审核
- [**POST  /projects/apply **](#projectsapply) 股权项目提交

##所有接口按字母顺序排序
- [**POST  /projects/apply **](#projectsapply) 股权项目提交


#### <span id="projectsapply"> POST /projects/apply </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***

判断用户名是否已经存在。

| 传入参数|是否必须|说明|备注|
|:----------- | :-----------: |:-----------: ||
|access_token| **required**|调用接口必须提供access_token||
|username|**required**|用户名||


| 返回参数|说明|备注|
|:----------- |:-----------: |
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
