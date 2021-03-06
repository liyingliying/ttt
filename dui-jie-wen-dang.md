虚拟奖品定义

应用场景

对接准备

（1）需要在推啊后台获取广告位链接

（2）需要在广告位链接后面&userId=xxxxx

\(3\)   信息收集表（表格联系推啊运营）

![](/assets/信息收集.png)（4）表格信息填写需要的充值接口文档参考以下内容

```
 虚拟商品API充值URL，需要媒体方提供，媒体方必须提供完全匹配企业应用可信域名，推啊测试可用后可在线上使用。
```

对接文档

参数说明

请求方式是：POST

| 参数 | 类型 | 是否必须 | 解释 |
| :--- | :--- | :--- | :--- |
| userId | String | 是 | 用户id |
| timestamp | Long | 是 | 时间戳，系统当前毫秒数 |
| prizeFlag | String | 是 | 虚拟商品标识 |
| account | String | 否 | 用户帐号，多帐号用逗号隔开 |
| orderId | String | 是 | 推啊订单号，具有唯一性，幂等由媒体保障 |
| appkey | String | 是 | 媒体信息 |
| sign | String | 是 | 签名 |
| score | Integer | 否 | 数值类型的虚拟商品，对应实际数值得100倍，媒体在充值操作时除以100 |
| reason | String | 是 | 充值理由 |

响应说明

| 参数 | 类型 | 是否必须 | 解释 |
| :--- | :--- | :--- | :--- |
| code | String | 是 | 0：成功，-1：重填，其他：充值异常 |
| msg | String | 是 | 充值失败信息 |
| orderId | String | 是 | 推啊订单号 |

签名说明

签名源数据及顺序:

timestamp，prizeFlag，orderId，appKey，appSecret

其中，appSecret为媒体秘钥（推啊后台获取路径-流量合作-我的媒体）

签名算法为MD5，以下是例子：
StringBuilder sb = new StringBuilder(); sb.append(timestamp);//时间戳 sb.append(prizeFlag);//虚拟商品标识 sb.append(orderId);//订单号 sb.append(appKey);//媒体信息
sb.append(appSecret);//媒体密钥 try {
return MD5.md5(sb.toString());
} catch (NoSuchAlgorithmException | UnsupportedEncodingException e) {
log.warn("虚拟商品充值签名失败 msg={}",e.getMessage(),e);
   return null;
}


验证请求
请求验证分为时效性验证和签名验证，验证通过后在充值。
时效性验证：5分钟失效 
Date date = null; try {
date = new Date(Long.valueOf(timestamp)); } catch (Exception e) {
   return false;
}
//5 分钟失效
return date.after(new Date(new Date().getTime()-5*60*1000L));

签名验证
StringBuilder sb = new StringBuilder(); sb.append(timestamp);//时间戳
sb.append(prizeFlag);//虚拟商品标识 sb.append(orderId);//订单号 sb.append(appKey);//媒体信息 sb.append(appSecret);//媒体密钥 try {
String sign = MD5.md5(sb.toString()); if(map.get("sign").equals(sign)){
       return true;
   }
} catch (NoSuchAlgorithmException | UnsupportedEncodingException e) { log.warn("虚拟商品充值签名失败 msg={}",e.getMessage(),e);
}
return false;

安全策略

连续积累 300 个订单请求无响应，将停止为该媒体充值服务; 补偿策略减少无响应订单数量后，自动开启充值服务;

补偿策略
2 秒超时无响应订单，进入重试补偿机制，重试间隔策略为 30s，60s，120s， 重试次数为 3 次，否则进入人工补偿;


