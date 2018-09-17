如何设置 APP 支持下载应用



1. 安卓环境 

a\) APP 内支持下载

第一步：WebView 设置下载监听 

mWebView.setDownloadListener\(new MyDownload\(\)\);

第二步：MyDownload 实现下载监听的接口：

class	MyDownload	implements	DownloadListener	{
								 @Override
								 public	 void	 onDownloadStart(String	 url,	 String	 userAgent,	 String	 contentDisposition,	
String	mimetype,	long	contentLength)	{
												 String[]	urls	=	url.split("\\?");
												 if	(urls[0].endsWith(".apk"))	{
																 ProgressDialog	progressDialog	=	new	ProgressDialog(mContext);
																 //	 设置进度条风格，风格为圆形，旋转的
																 progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
																 //	 设置 ProgressDialog	 标题
																 progressDialog.setTitle("正在下载");
																 //	 设置 ProgressDialog 提示信息
																 progressDialog.setMessage("完成进度...");
																 progressDialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);
																 //	 设置 ProgressDialog	 是否可以按退回键取消
																 progressDialog.setCancelable(true);
																 final	 DownLoadTask	 downloadTask	 =	 new	 DownLoadTask(mContext,	
progressDialog);
																 downloadTask.execute(url);
																 progressDialog.setOnCancelListener(new	DialogInterface.OnCancelListener()	{
																				 @Override
																				 public	void	onCancel(DialogInterface	dialog)	{
																								 downloadTask.cancel(true);
																				 }
																 });
												 }
								 }
				 }

																 										 											 
																 																					 

 

