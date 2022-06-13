#### 同步订单接口及发货接口说明

> 1.第三方开发者需提供回调地址，联系平台配置服务码（state）、密钥（secret）
> 
> 2.调用同步、发货接口需要客户授权登录，授权地址：http://mms.pinduoduo.com/open.html?response_type=code&client_id=93cc920a6230466bae275f1743ea7793&redirect_uri=http://pdd.pddshuju.com/platform/pdd/return&state={服务码}，客户授权后跳转回第三方开发者地址，格式：{回调地址}/{ownuser}/{平台店铺名称}
> 
> 3.客户第一次授权登录需引导客户在平台（http://pdd.pddshuju.com/）上完成注册登录
> 
> 4.签名算法：
>> 
>> 1.所有参数进行按照首字母先后顺序排列
>> 
>> 2.把排序后的结果按照参数名+参数值的方式拼接，参数值如是数组，则转成json，拼接后格式：key1value1key2value2...
>> 
>> 3.拼装好的字符串尾部拼接密钥（secret）进行md5加密后转大写，拼接后格式：key1value1key2value2...secret

#### 同步订单接口

> 接口说明：该接口返回用户过去两天的未发货订单
> 
> 请求地址：http://pdd.pddshuju.com/gift_pdd_order_list_get
> 
> 请求方法：POST
> 
> 请求参数：
>> 
>> ownuser 必填  店铺标识码 c9b0e9a9e8282557fcc34cf20b48e94a
>> 
>> state	必填	服务码 sm5463-00s5
>> 
>> start	必填	开始时间 Y-m-d H:i:s
>> 
>> end	必填	结束时间 Y-m-d H:i:s
>> 
>> sign	必填	验证码	参考签名算法

> 返回参数：
>> 正常响应：
>>> msg	提示信息 success
>>> data	订单数组 数组
>>> 	buyer_memo	买家备注
>>> 	order_sn	订单号
>>> 	confirm_time	成交时间
>>> 	receiver_name	收件人姓名
>>> 	country	收件人国家
>>> 	province	省
>>> 	city		城市
>>> 	town		区
>>> 	address	详细地址
>>> 	receiver_phone收件人手机
>>> 	remark	卖家备注
>>> 	receiver_address详细地址不含省市区
>>> 	item_list	商品列表	数组
>>>>	outer_id	商品skuid
>>>>	goods_name	商品名称
>>>>	goods_count	购买数量
>>>>	goods_spec	商品规格

>> 异常响应：
>>> code	错误代码 500
>>> msg	错误信息 未绑定店铺，请绑定店铺之后再继续操作

#### 发货接口

> 请求地址：http://pdd.pddshuju.com/gift_send 
> 请求方法：POST 
> 请求参数：
>>  ownuser 必填 店铺标识码 c9b0e9a9e8282557fcc34cf20b48e94a 
>>  state	必填	服务码 sm5463-00s5
>> sign	必填	验证码	参考签名算法 
>> send_data	必填	数组 	
>>> order_sn 必填  拼多多订单号
>>> logistics_id	必填	物流公司代码(附件) 85 	
>>> tracking_number 必填	快递单号

> 返回参数： 
>> 正常响应：
>> msg	提示信息 success 
>> data	订单数组 数组 	
>> 发货失败 	
>>> code	错误代码 500 	
>>> msg	错误信息 未绑定店铺，请绑定店铺之后再继续操作 
>>> order_sn 订单号

>> 发货成功信息 	
>>> order_sn 订单号 
>>> msg	提示信息 success	 	

> 异常响应：
>>  code	错误代码 500 
>>  msg	错误信息 未绑定店铺，请绑定店铺之后再继续操作

####获取快递单号接口说明

> 在 http://pdd.pddshuju.com/ 完成注册后联系平台提供拼多多账号（pdd开头的账号）进行配置client_id、client_secret。
> access_token 是接口调用凭据，过期时间在获取时返回。
> 对于 GET 请求，请求参数应以 QueryString 的形式写在 URL 中。
> 对于 POST 请求，部分参数需以 QueryString 的形式写在 URL 中（一般只有 access_token，如有额外参数会在文档里的 URL 中体现），其他参数如无特殊说明均以 JSON 字符串格式写在 POST 请求的 body 中。


#### 获取access_token

> 请求地址：http://pdd.pddshuju.com/api/token?client_id={client_id}&client_secret={client_secret}

#### 获取快递单号接口

> 请求地址：http://pdd.pddshuju.com/api/free/entry/save_trade?access_token={access_token}
> 请求方法：POST
> 请求主体：
> ```json
>{"trade":{"receiver_zip":"string","receiver_name":"string","receiver_mobile":"string","receiver_phone":"string","receiver_state":"string","receiver_city":"string","receiver_district":"string","receiver_address":"string","tid":"string","payment":"string","seller_memo":"string","sender_name":"string","sender_mobile":"string","sender_state":"string","sender_city":"string","sender_district":"string","sender_address":"string"},"orders":[{"title":"string","order_sku_id":"string","order_sku":"string","num":"string","price":"string","youhui":"string","payment":"string"}],"express_print_data":{"ecode_no":"string","datoubi":"string","pc_code":"string","pc_name":"string","config":"string","package_id":"string","water_mark":"string","userid":"string","print_data":{"encryptedData":"string","signature":"string","templateUrl":"string","ver":"string"}},"add_address":0,"ptemp_id":0,"out_trade_no":"string","out_e_commerce_no":"string","taobao_oaid":"string","_partner":"string"}
> ```
> 部分参数说明：
>> _partner 第三方公司名称
>> out_trade_no 第三方订单编号，需保证唯一
>> out_e_commerce_no 电商单号，拼多多密文时必传
>> taobao_oaid 淘宝订单密文时必传
>> ptemp_id 快递模板编号

> 请求示例：
> ```json
> {
  "trade": {
    "receiver_zip": "000000",
    "receiver_name": "何小玲",
    "receiver_mobile": "18888888888",
    "receiver_phone": "18888888888",
    "receiver_state": "江苏省",
    "receiver_city": "常州市",
    "receiver_district": "天宁区",
    "receiver_address": "江苏省 常州市 天宁区 兰陵街道九州新家园5栋2单元904(213003)",
    "tid": "",
    "payment": "11",
    "seller_memo": "",
    "sender_name": "胖大熊",
    "sender_mobile": "15019712643",
    "sender_state": "上海市",
    "sender_city": "上海市",
    "sender_district": "青浦区",
    "sender_address": "1号仓库"
  },
  "orders": [
    {
      "title": "上海申通-梳子",
      "order_sku_id": "SHSTSZ",
      "order_sku": "1",
      "num": "1",
      "price": "1",
      "youhui": "0",
      "payment": "1"
    }
  ],
  "out_trade_no": "API2D5689783884752883565",
  "taobao_oaid": "",
  "out_e_commerce_no": "2234752237118646339",
  "add_address": "0",
  "ptemp_id": "1696",
  "_partner": "第三方公司名称"
}
> ```

> 返回示例：
> ```json
> {
  "code": 1,
  "msg": "success",
  "data": {
    "tid": "1638892904850",
    "out_trade_no": "API2D5435594703787256849",
    "ecode_no": "9885532483522",
    "express_print_data": {
      "ecode_no": "9885532483522",
      "datoubi": "",
      "pc_code": "",
      "pc_name": "",
      "config": "",
      "package_id": "1",
      "water_mark": "",
      "print_data": {
        "encryptedData": "AES:rU904rj6UH2oqfSUb43+Z5Cuo...2g==",
        "signature": "MD:VOBhHLyu7kj0cPT8jZhjmA==",
        "templateURL": "http://cloudprint.cainiao.com/template/standard/801/114",
        "ver": "waybill_print_secret_version_1"
      },
      "userid": "123906"
    }
  }
}
> ```
