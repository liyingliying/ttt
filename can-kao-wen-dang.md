如何设置 APP 支持下载应用

1. 安卓环境 

a\) APP 内支持下载

第一步：WebView 设置下载监听

mWebView.setDownloadListener\(new MyDownload\(\)\);

第二步：MyDownload 实现下载监听的接口：

class    MyDownload    implements    DownloadListener    {  
                                 @Override  
                                 public     void     onDownloadStart\(String     url,     String     userAgent,     String     contentDisposition,  
String    mimetype,    long    contentLength\)    {  
                                                 String\[\]    urls    =    url.split\("\?"\);  
                                                 if    \(urls\[0\].endsWith\(".apk"\)\)    {  
                                                                 ProgressDialog    progressDialog    =    new    ProgressDialog\(mContext\);  
                                                                 //     设置进度条风格，风格为圆形，旋转的  
                                                                 progressDialog.setProgressStyle\(ProgressDialog.STYLE\_HORIZONTAL\);  
                                                                 //     设置 ProgressDialog     标题  
                                                                 progressDialog.setTitle\("正在下载"\);  
                                                                 //     设置 ProgressDialog 提示信息  
                                                                 progressDialog.setMessage\("完成进度..."\);  
                                                                 progressDialog.setProgressStyle\(ProgressDialog.STYLE\_HORIZONTAL\);  
                                                                 //     设置 ProgressDialog     是否可以按退回键取消  
                                                                 progressDialog.setCancelable\(true\);  
                                                                 final     DownLoadTask     downloadTask     =     new     DownLoadTask\(mContext,  
progressDialog\);  
                                                                 downloadTask.execute\(url\);  
                                                                 progressDialog.setOnCancelListener\(new    DialogInterface.OnCancelListener\(\)    {  
                                                                                 @Override  
                                                                                 public    void    onCancel\(DialogInterface    dialog\)    {  
                                                                                                 downloadTask.cancel\(true\);  
                                                                                 }  
                                                                 }\);  
                                                 }  
                                 }  
                 }

第三步：DownLoadTask 用异步任务实现下载 备注：webview 下载方式只要注册下载监听的接口，自己对下载链接做判断，最后网络 请求实现下载即可。

b\) 浏览器支持下载 点击跳转到浏览器下载，实现方式为在 MyDownLoad 里面隐式启动 activity:  
class    MyDownload    implements    DownloadListener    {  
                                 @Override  
                                 public     void     onDownloadStart\(String     url,     String     userAgent,     String     contentDisposition,  
String    mimetype,    long    contentLength\)    {  
                                             Uri    uri    =    Uri.parse\(url\);  
Intent    intent    =    new    Intent\(Intent.ACTION\_VIEW,    uri\);  
startActivity\(intent\);  
                                 }  
                 }



2. iOS 环境 iOS 环境下本身支持跳转至 AppStore 下载，无需开发者设置。

