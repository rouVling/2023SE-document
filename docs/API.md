# API 规范文档

地址是前端请求后端的地址~<br>
内容多了的话可以考虑使用多个页面<br>
[需求文档链接](https://docs.qq.com/doc/DWkNSa0xEeE51eGti)

## **账户页面**

- ### 登录  `<url>/login` 

	仅登录，以 `post` 请求<br>
	和小作业差不多，除了 md5 之外。不过登录 demo 用不用 md5 都可以

	=== "请求体"

		```JSON
		{
			"userName": "rouVling",
			"password": "de56ep24dark86fant53145asy"    //  MD5 值
		}
		```

	=== "成功响应"

		```JSON
		{
			"code": 0,
			"info": "Succeed",
			"token": "***.***.***" // JWT
		}
		```
	=== "错误响应"

		```JSON
		{
			"code": *,
			"info": "[Some message]"
		}
		```

	???question "可能需要的功能"
		- 实际密码 md5 计算的时候应该再和用户名结合
		- 医生端每次登录都更新token，实现三天不登录才需要重新登录
		- [x] ~~支持用户名登录/工号登录~~ 使用唯一id即可

- ### 注册 `<url>/register`

	格式同上 `登录` 部分

## **患者病历**

- ### 创建病历 `<url>/medRec`

	该 API 用于创建一个id尚不存在的病历。

	请求头<br>
	需要将 `Authorization` 字段设置为 JWT 令牌<br>
	使用 `POST` 请求

	=== "请求体"

		前端看应该是一个问卷的样子
	
		```json
		{
			"id": "由字母和数字组成的就诊卡号",
			"name": "Agile",	//	string, 姓名
			"type": 1,	//	int, 0->正常人，1->患者
			"coordination": 1,	//	int, 0->欠配合，1->配合
			"coordinationDesc": "good coordination with doctor",	//	string, 配合描述
			"gender": 1,	//	int, 0->男，1->女
			"IDnumber": "身份证号",	//	string, 身份证号
			"birth": "2023.7.4", //	string, 格式无所谓，统一就好，后端只储存字符串
			"actBirth": "2023.7.5",	//	string, 实际出生日期
			"appletId": "9961",	//	string, 小程序病人id
			"hand": 1,	//	int, 0->左利手，1->右利手，2->双利手
			"educated": 3,		//	int, 0->无，1->小学，2->初中，3->高中，4->本科及以上
			"educatedYears": 10,//	int, 教育年限
			"livingEnv": 1,	//	int, 0->城市，1->农村，2->乡镇
			"yearEducatedFift": 7,	//	int, 15岁之前受教育年限
			"height": 170,	//	int, 身高
			"weight": 100,	//	int, 体重
			"stressImpact": 20,	//	int, 应激事件影响个数
			"stressDesc": "string",	//	string, 应激事件描述
			"dementiaHistoryID": 1,	//	int, 痴呆家族史，0->无，1->有，2->不详
			"dementiaHistoryDesc": "string",	//	string, 痴呆家族史描述
			"psychiatricHistoryID": 2,	//	int, 神经科其他疾病家族史, 0->无，1->有，2->不详
			"psychiatricHistoryDesc": "string",	//	string, 神经科其他疾病家族史描述
			"address": "string",	//	string, 居住地址
			"telephone": "110",	//	string, 联系电话
			"tcmDiagnosis": "string",	//	string, 中医辩证
			"medicNum": "string" 	//	string, 病案号
		}
		```

	=== "成功响应"

		```json
		{
			"code": 0,
			"info": "Post succeed",
		}
		```
		> 后续可能会在响应内容中增加其他内容来配合跳转等功能？

	=== "错误响应"

		```json
		{
			"code": *,
			"info": "[Some message]"
		}
		```

	???todo "后端实现"
		后端需要解析成 json 然后在对应的表里面进行存储<br>
		ERD 图暂时没有更新，请以当前 json 为准

	???todo "前端实现"
		整体上就是一个问卷，每个问题对应一个字段<br>
		更具体内容可以看[需求文档](https://docs.qq.com/doc/DWkNSa0xEeE51eGti)

	???question "可能需要的功能"
		- 后端检查是否有病历提醒是否覆盖
		- 设置必填字段和可选字段

- ### 获取/修改病历 `<url>/medRec/{id}`

	该 API 用于操作由id确定的一张具体病历，包括获取病历、修改病历。

	#### GET

	请求头<br>
	需要将 `Authorization` 字段设置为 JWT 令牌<br>
	使用 `GET` 请求

	=== "请求体"

		本方法不需要提供任何请求体

	=== "成功响应"

		```JSON
		{
			"code": 0,
			"info": "Get succeed",
			"id": "由字母和数字组成的就诊卡号",
			"name": "Agile",	//	string, 姓名
			"type": 1,	//	int, 0->正常人，1->患者
			"coordination": 1,	//	int, 0->欠配合，1->配合
			"coordinationDesc": "good coordination with doctor",	//	string, 配合描述
			"gender": 1,	//	int, 0->男，1->女
			"IDnumber": "身份证号",	//	string, 身份证号
			"birth": "2023.7.4", //	string, 格式无所谓，统一就好，后端只储存字符串
			"actBirth": "2023.7.5",	//	string, 实际出生日期
			"appletId": "9961",	//	string, 小程序病人id
			"hand": 1,	//	int, 0->左利手，1->右利手，2->双利手
			"educated": 3,		//	int, 0->无，1->小学，2->初中，3->高中，4->本科及以上
			"educatedYears": 10,//	int, 教育年限
			"livingEnv": 1,	//	int, 0->城市，1->农村，2->乡镇
			"yearEducatedFift": 7,	//	int, 15岁之前受教育年限
			"height": 170,	//	int, 身高
			"weight": 100,	//	int, 体重
			"stressImpact": 20,	//	int, 应激事件影响个数
			"stressDesc": "string",	//	string, 应激事件描述
			"dementiaHistoryID": 1,	//	int, 痴呆家族史，0->无，1->有，2->不详
			"dementiaHistoryDesc": "string",	//	string, 痴呆家族史描述
			"psychiatricHistoryID": 2,	//	int, 神经科其他疾病家族史, 0->无，1->有，2->不详
			"psychiatricHistoryDesc": "string",	//	string, 神经科其他疾病家族史描述
			"address": "string",	//	string, 居住地址
			"telephone": "110",	//	string, 联系电话
			"tcmDiagnosis": "string",	//	string, 中医辩证
			"medicNum": "string" 	//	string, 病案号
		}
		```
		
	=== "错误响应"

		```JSON
		{
			"code": *,
			"info": "[Some message]"
		}
		```
  
	#### POST
	
	请求头<br>
	需要将 `Authorization` 字段设置为 JWT 令牌<br>
	使用 `POST` 请求

	=== "请求体"
  
		```JSON
		{
			"id": "由字母和数字组成的就诊卡号",
			"name": "Agile",	//	string, 姓名
			"type": 1,	//	int, 0->正常人，1->患者
			"coordination": 1,	//	int, 0->欠配合，1->配合
			"coordinationDesc": "good coordination with doctor",	//	string, 配合描述
			"gender": 1,	//	int, 0->男，1->女
			"IDnumber": "身份证号",	//	string, 身份证号
			"birth": "2023.7.4", //	string, 格式无所谓，统一就好，后端只储存字符串
			"actBirth": "2023.7.5",	//	string, 实际出生日期
			"appletId": "9961",	//	string, 小程序病人id
			"hand": 1,	//	int, 0->左利手，1->右利手，2->双利手
			"educated": 3,		//	int, 0->无，1->小学，2->初中，3->高中，4->本科及以上
			"educatedYears": 10,//	int, 教育年限
			"livingEnv": 1,	//	int, 0->城市，1->农村，2->乡镇
			"yearEducatedFift": 7,	//	int, 15岁之前受教育年限
			"height": 170,	//	int, 身高
			"weight": 100,	//	int, 体重
			"stressImpact": 20,	//	int, 应激事件影响个数
			"stressDesc": "string",	//	string, 应激事件描述
			"dementiaHistoryID": 1,	//	int, 痴呆家族史，0->无，1->有，2->不详
			"dementiaHistoryDesc": "string",	//	string, 痴呆家族史描述
			"psychiatricHistoryID": 2,	//	int, 神经科其他疾病家族史, 0->无，1->有，2->不详
			"psychiatricHistoryDesc": "string",	//	string, 神经科其他疾病家族史描述
			"address": "string",	//	string, 居住地址
			"telephone": "110",	//	string, 联系电话
			"tcmDiagnosis": "string",	//	string, 中医辩证
			"medicNum": "string" 	//	string, 病案号
		}
		```
		
	=== "成功响应"
	
		```JSON
		{
			"code": 0,
			"info": "Post succeed",
		}
		```
	=== "错误响应"
	
		```JSON
		{
			"code": *,
			"info": "[Some message]"
		}
		```

	???todo "后端实现"
		后端需要解析成 json 然后在对应的表里面进行存储<br>
		ERD 图暂时没有更新，请以当前 json 为准
	
	???todo "前端实现"
		整体上就是一个问卷，每个问题对应一个字段<br>
		更具体内容可以看[需求文档](https://docs.qq.com/doc/DWkNSa0xEeE51eGti)
	
	???question "可能需要的功能"
		- 后端检查是否有病历提醒是否覆盖
		- 设置必填字段和可选字段

## **患者列表**
患者列表需要做

1. 列表页面<br>
	显示具有查看权限的患者列表，按照**最近就诊时间**排序（即最近就诊的排在最前面）。
	列表显示患者就诊卡号、姓名、性别、年龄、病历创建日期、最近就诊日期、主治医生ID和操作按钮，操作按钮包括：诊疗、编辑、删除。
2. 患者筛选<br>
	在患者列表页面，可以根据病历中的基本信息（单选和数值类型信息），对患者进行筛选。
3. 患者查询<br>
	可以根据患者姓名或就诊卡号进行查询。

摘录自[需求文档](https://docs.qq.com/doc/DWkNSa0xEeE51eGti)

诊疗按钮功能大概是跳转页面，目前不需要填充<br>
编辑按钮为跳转到修改病历页面，需要实现<br>
删除按钮.....医生真的可以删除患者吗？？？？<br>


- ### 患者查询与筛选 `<url>/usr/patient`

	请使用 `GET` 进行请求

	===	"请求头"

		需要将 `Authorization` 字段设置为 JWT 令牌

	=== "请求体"

		```json
		{
			"userName": "doctor's name",
			"filter": 
			{
				"id": "卡号",
				"type": 1,	//	int, 0->正常人，1->患者
				"coordination": 1,	//	int, 0->欠配合，1->配合
				"gender": 1,	//	int, 0->男，1->女
				"hand": 1,	//	int, 0->左利手，1->右利手，2->双利手
				"educated": 3,		//	int, 0->无，1->小学，2->初中，3->高中，4->本科及以上
				"educatedYears": 10,//	int, 教育年限
				"livingEnv": 1,	//	int, 0->城市，1->农村，2->乡镇
				"stressImpact": 20,	//	int, 应激事件影响个数
				"dementiaHistoryID": 1,	//	int, 痴呆家族史，0->无，1->有，2->不详
				"psychiatricHistoryID": 2,	//	int, 神经科其他疾病家族史, 0->无，1->有，2->不详
			}
		}
		```

		!!!note "发送请求请注意"
			- `filter` 不是所有都是必填字段，请按照用户的输入决定是否有
			- `filter` 中，`id` 和 `其他字段` 不要同时出现，`id` 为查询使用，`其他字段`为筛选时候使用
			- 如果筛选条件太多，输入框太多不好看的话，可以加一个展开更多的选项，将一些不常用的筛选条件放里面
		
		???note "协商余地"
			前端大概做成这样：<br>
			输入框 + 查询按钮 + 其他条件 + 筛选按钮<br>
			如果前端觉得 `id` 和其他字段不同时出现难度大的话，可以协商加一个字段表示后端是否采用 `id` 忽略其他<br>


	=== "成功响应"

		返回 `就诊卡号、姓名、性别、年龄、病历创建日期、最近就诊日期、主治医生ID`

		```json
		{
			"code": 0,
			"info": "Succeed",
			"userName": "rho",
			"patients": [
				{
					"id": "就诊卡号",
					"name": "姓名",
					"gender": 0,	// 0->男 1->女
					"age": -100,
					"createdAt": "894659854",	//	timestamp,同小作业
					"lastDiagnosed": "51846513",	//	timestamp
					"doctorID": "主治医生ID"
				},
				//	...
			]
		}
		```

	=== "失败响应"

		```JSON
		{
			"code": *,
			"info": "[Some message]"
		}
		```

	???note "前端实现"
		需要搭好查询页面的框架，内部表格直接解析后端发来的 json 即可

	???note "后端实现"
		将前端的 `filter` 直接加入查询条件。必要的时候进行 key 的转化

	???warning "后续开发注意"
		- 患者页面的诊疗按钮功能尚未实现
	
	???question "可能需要的功能"
		- 删除功能尚未完成，但是真的需要删除吗？删除医生对病人病历的读取权限？？

## **诊疗页面**

4.1.4.1 查看信息（1‘）
- 可以查看患者的病历页面、门诊记录、各项检查结果。
- 可以将整个患者的查看权限分享给其他医生，也可以仅分享部分信息（如仅分享病历、仅分享某项检查的结果）。

4.1.4.2 发起检查（0.25’）
- 可以发起检查，并指定医生（可以是自己），如果已有过此项检查，则自动更新。

4.1.4.3 诊疗（0.25‘）
- 可以写下诊疗结果和依据。

前端大概是，列出人名，旁边加上 `查看病人` 按钮切换到病历页面，再加上 `分享` 按钮，选择医生并分享，

### 查看诊疗 `<url>/diagnose`

#### POST

#### GET

### 选择医生 `<url>/diagnose/doctors`

使用 `GET` 请求

=== "请求体"

	不需要请求体

=== "成功响应"

	```json
	{
		"code": 0,
		"info": "Succeed",
		"list":	[
			{
				"name": "rho",
				"id": "93oijfafwa"
			},
			...
		]
	}
	```

=== "失败响应"

	```json
	{
		"code": *,
		"info": "[Some message]"
	}
	```

???question "可以加的功能"
	- 按照最近访问排序

### 发放查看权限 `<url>/diagnose/grant`

请求方法为 `GET`

需要将 `Authorization` 字段设置为 JWT 令牌<br>

=== "请求体"

	```json
	{
		"doctor":"a123dcz3",			//	医生 id
		"patient":"oijlkaw390fask"		//	病人 id
	}
	```

=== "成功响应"

	和平常正常响应一样，前端只需要显示 `成功` 就好

=== "失败响应"

	和前面一样

