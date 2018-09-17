问题一：访问链接打开空白

问题现象：

原因：签名错误

解决办法：签名所用到的参数

appSecret=?&md=?&nonce=?&timestamp=?

\(1\)appSecret会出现的问题有

a:   错把appKey当成appSecret

b：后台一个帐号下有多个媒体时，一个媒体的appSecret和另一个媒体的appSecret弄错了

c：appSecret前面加了&符

\(2\)md会出现的问题有

md用于签名的时候不要进行url\_enco de编码。只需要在请求活动的时候md进行url\_encode编码）

\(3\)nonce会出现的问题有

nonce随机数不是6位（正确是6位，并开头不是0）



问题二：素材链接或者广告位链接，拼好的链接会返回什么数据

回答：返回的是重定向到素材图片或者是H5的参与活动的页面。



问题三：如何验证md是否正确？

回答：反解md，检查是否正确。或者通过推啊提供的API验证小工具 [https://linode1.oilpi.com/](https://linode1.oilpi.com/) 



