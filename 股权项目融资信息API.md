##股权项目融资信息API

- [**POST  /ProjectFunds/create **](#ProjectFunds-create)
- [**POST  /ProjectFunds/update **](#ProjectFunds-update)

#### <span id="ProjectFunds-create"> POST /ProjectFunds/create </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
添加股权项目融资信息。
  
返回信息：添加成功的股权项目融资信息ID

|参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **必须**|调用接口必须提供access_token||
|id|**必须**|项目ID||
|need_fund|**必须**|融资金额|float型|
|real_fund|**可选**|实际融资额|float型|
|project_valuation|**必须**|项目方估值|float型|
|final_valuation|**可选**|最终估值|float型|
|min_lead_fund|**必须**|最低领投金额|float型|
|min_follow_fund|**必须**|最低跟投金额|float型|
|total_fund|**必须**|认投总额|float型|
|agree_total_fund|**可选**|已接受投资额|float型|
|leader_id|**可选**|领投人||
|extra_source|**可选**|附加资源||
|purpose|**可选**|资金用途||
|charge_type|**必须**|到账方式||
|shareholder_right|**可选**|股东特权||
|end_time|**可选**|结束时间|int型|
|user_id|**必须**|操作者ID||

|返回参数|说明|备注|
|:----------- |:-----------:|:-----------:|
|code| API执行结果代码|参照API共通说明|
|message | API错误信息内容|参照API共通说明|
|errors | API错误信息详细内容|参照API共通说明|
|data | 业务数据|项目融资信息ID|

###### Example Request
curl  --post --include  ‘HOST_URL/ProjectFunds/create’

###### 返回值样例（执行成功时）
```
{"code":200,"message":"OK","errors":[],"data":"{"id":"1"}"}
```

#### <span id="ProjectFunds-update"> POST /ProjectFunds/update </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
根据股权项目ID更新指定的股权项目融资信息。
  
返回信息：更新件数

| 参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **必须**|调用接口必须提供access_token||
|id|**必须**|项目ID||
|need_fund|**可选**|融资金额|float型|
|real_fund|**可选**|实际融资额|float型|
|project_valuation|**可选**|项目方估值|float型|
|final_valuation|**可选**|最终估值|float型|
|min_lead_fund|**可选**|最低领投金额|float型|
|min_follow_fund|**可选**|最低跟投金额|float型|
|total_fund|**可选**|认投总额|float型|
|agree_total_fund|**可选**|已接受投资额|float型|
|leader_id|**可选**|领投人||
|extra_source|**可选**|附加资源||
|purpose|**可选**|资金用途||
|charge_type|**可选**|到账方式||
|shareholder_right|**可选**|股东特权||
|end_time|**可选**|结束时间|int型|
|user_id|**必须**|操作者ID||

|返回参数|说明|备注|
|:----------- |:-----------:|:-----------:|
|code| API执行结果代码|参照API共通说明|
|message | API错误信息内容|参照API共通说明|
|errors | API错误信息详细内容|参照API共通说明|
|data | 业务数据|更新件数|


###### Example Request
curl  --post --include  ‘HOST_URL/ProjectFunds/update'

###### 返回值样例
```
{"code":200,"message":"OK","errors":[],"data":{"count":1}}
```

#### <span id="ProjectFunds-select"> GET /ProjectFunds/select </span> &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
***
根据股权项目ID获取指定的股权项目融资信息。
  
传入信息：
| 参数|是否必须|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|access_token| **必须**|调用接口必须提供access_token||
|project_id|**必须**|项目ID||
|user_id|**必须**|用户的openid||

|返回参数|说明|备注|
|:----------- |:-----------:|:-----------:|
|code| API执行结果代码|参照API共通说明|
|message | API错误信息内容|参照API共通说明|
|errors | API错误信息详细内容|参照API共通说明|
|data | 业务数据|更新件数|

| 参数|说明|备注|
|:--------|:--------:|:--------:|:--------:|
|project_id|项目ID||
|need_fund|融资金额|float型|
|real_fund|实际融资额|float型|
|project_valuation|项目方估值|float型|
|final_valuation|最终估值|float型|
|min_lead_fund|最低领投金额|float型|
|min_follow_fund|最低跟投金额|float型|
|max_follow_fund|最高跟投金额|float型|
|fund_step|加价幅度|float型|
|total_fund|认投总额|float型|
|agree_total_fund|已接受投资额|float型|
|leader_flag|是否需要领投||
|leader_id|领投人||
|extra_source|附加资源||
|purpose|资金用途||
|can_over_fund|是否允许超募||
|over_fund|允许超募金额||
|charge_type|到账方式||
|shareholder_right|股东特权||
|end_time|结束时间|int型|
|update_time|更新时间||

###### Example Request
curl  --get --include  ‘HOST_URL/ProjectFunds/select'

###### 返回值样例
```
{
  "code": 200,
  "message": "OK",
  "errors": [],
  "data": {
    "id": "377",
    "project_id": "GQ14483328750011916",
    "need_fund": "220000.00",
    "real_fund": "0.00",
    "project_valuation": "1110000.00",
    "final_valuation": "0.00",
    "min_lead_fund": "50000.00",
    "min_follow_fund": "30000.00",
    "max_follow_fund": "30000.00",
    "fund_step": "10000.00",
    "total_fund": "0.00",
    "agree_total_fund": "0.00",
    "leader_flag": "1",
    "leader_id": null,
    "extra_source": null,
    "purpose": "AAFASDF",
    "can_over_fund": "0",
    "over_fund": "0.00",
    "charge_type": null,
    "shareholder_right": "REDTE",
    "risk_max_fund": "0.00",
    "end_time": "0",
    "create_time": "1448336082",
    "create_id": "3088",
    "update_time": "1448336082",
    "update_id": "3088"
  }
}
```
