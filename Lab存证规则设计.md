
# 1 数据结构

- **数据类型DataType**

服务端约定的数据类型,目前做一个简单的区间划分

| 暂定取值范文 | 说明 |
| :-----: | :-----:|
| 1001-1999  | 用户身份信息相关操作 |


- **存证标签DataTag**

链端用以标记该存证的标签，标签为多级，用`.`进行划分。
	
对应服务端约定的数据类型如下表：
	
| 数据类型DataType | 存证标签DataTag | 说明 |
| :-----: | :----- | :----- |
| 1001  | behavior.user.save | 用户数字身份上链 |

- **Body**

一点知道APP端 调取合约函数时设置接收者的请求正文数据
	
```
	{
		"contract_id": 2, # 合约id
		"func": "newEvidence", # 合约函数名称
		"params": {
			"signer": "0x085154d302b49577252c17c9fd7fad354347b596", # 签名人，如果是存证合约，签名人是`init_params`中的人
			"evidence": [Payload] # 存证内容
		}
	}
	
```
	
- **Payload**
	
	Lab试验台 用户行为轨迹的存证内容 Json格式
	
	根据「主谓宾」拆解为`json`。
	如「某weid 学习 某资源」：
	
	```
	{
		"app_id": 1, # appid 链端提供
		"operator": [weid], # 用户的数字身份
		"action": [存证标签DataTag], # 例："behavior.user.save" 用户身份上链,
		"resource": [schoolId md5转换], # 用户学校ID做MD5转换
	}
	```
	
# 2 调取合约函数

Lab书烟台 按约定的数据类型`DataType` 调取合约函数

Path: `/api/v1/contract/func`

Method: `POST`

Params: `app_id `, `secret_key`

Body: `[Body Data]` ***<数据结构 Body部分说明>***

Successful Response:

```
{
    "contract_id" = 1;
    description = "<null>";
    id = 22;
    "inserted_at" = "2021-03-05T08:35:39";
    key = 0xdc7ea03c0cc93023a424aa5d4a5f4f27cb294817;
    owners =     (
        0x085154d302b49577252c17c9fd7fad354347b596
    );
    signers =     (
        0x085154d302b49577252c17c9fd7fad354347b596
    );
    "tx_id" = 0x11e8ddcceefd20035f820c5f1500bbb303ee03da52feb2d4b82bbd474903b3ff;
    "updated_at" = "2021-03-05T08:35:39";
    value = "{\"resource\":\"6ea9ab1baa0efb9e19094440c317e21b\",\"app_id\":\"1\",\"operator\":\"did:weid:1:0x06eb5c56dff200a92434679c8eb1325d857e7052\",\"action\":\"behavior.user.save\"}";
}
```


Mock URL: [https://1a7a8fe3-01de-42fc-ba79-cdb9d4c42734.mock.pstmn.io/api/v1/weid/create?app_id=1&secret_key=fff](https://1a7a8fe3-01de-42fc-ba79-cdb9d4c42734.mock.pstmn.io/api/v1/weid/create?app_id=1&secret_key=fff)

 
# 3 保存存证信息

Lba试验台 根据调取微芒链合约函数返回的结果保存存证创建信息

Path: `内部接口`

Method: `POST`

Params: 

```
{
	"dataType": [dataType], # 数据类型DataType
	"weid": [weid], # 用户的数字身份
	"key": "", # 资源ID (存证数据返回字段)
	"value": {}, # (请求存证时传递的数据)
	"owners": [
		"0x085154d302b49577252c17c9fd7fad354347b596"
    ], # (存证数据返回字段)
	"signers": [
    	"0x085154d302b49577252c17c9fd7fad354347b596"
    ], # (存证数据返回字段)
	"tx_id": "0x855f072a0eae73e89352d91db62c4d1eed16dc57361c3b28a8b1c807faae0951",
	"createTime": "" # 数据存储时的时间戳
}
```


Successful Response:

```
{
	"error_code": 0,
	"data":true
}
```
