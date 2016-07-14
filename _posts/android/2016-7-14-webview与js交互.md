---
layout: post
title:  "webview与js交互"
comments: true
categories: android
---

* content
{:toc}

## 前言

刚入职一家新公司，目前该公司app使用的是前端技术，即webview里套html，里面涉及到了webview与javascript的交互，
这两天查了资料整理了一下andorid与js互相调用的过程。

下面直接看demo。


## webview与js交互Demo

1) 在mainfest.xml文件中添加网络权限。

	<uses-permission android:name="android.permission.INTERNET" />

2) 可以在本地写好html文件(也可以写好在服务器发布)，这里使用本地的html文件
	
	* 先在main目录下创建assets文件夹，注意assets文件与res文件同级。
	
	* 在assets文件下创建一个文件夹取名为www，在里面创建html文件。
	
	* 注意定义好andorid提供给js调用的对象和接口“AndroidFunction.showToast(testVal)”，
	
	其中showToast(String str)方法为android提供，参数str由js在html页面中获得，并调用该方法在android界面上弹出一个toast。
	
	html文件如下：

<!--	<!DOCTYPE >
<html xmlns="http://www.w3.org/1999/xhtml" debug="true">
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
		<meta name="apple-mobile-web-app-capable" content="yes">
		<meta name="viewport" content="target-densitydpi=device-dpi" />
		<script type="text/javascript">
			function init()
			{
				var testVal = document.getElementById('mytextId').value;
				AndroidFunction.showToast(testVal);
			}
		</script>
	</head>
	<body>
	<div style="float: left;width: 50%;">
		<input type="text" style="width: 180px;"
			name="myText" id="mytextId" />

	</div>
	<div style="clear: both;height: 3px;"> &lt/div>
		<div>
			<input value="submit" type="button" name="submit"
				id="btnSubmit" onclick="javascript:return init();" />
		</div>
	</div>
	</body>
</html>-->

	
3) 在android中写activity类，xml文件中就是一个WebView和一个TextView。

要实现webview的这些方法：

* 得到webview.getSettings()	对象，并设置webview的相关属性；
	
	//设置编码
	mwv.getSettings().setDefaultTextEncodingName("utf-8");
    //支持js
    mwv.getSettings().setJavaScriptEnabled(true);
    //设置背景颜色 透明
    mwv.setBackgroundColor(Color.argb(0, 0, 0, 0));
    //设置本地调用对象及接口 最重要的一个方法。
	//该方法有两个参数，前者为android中的javascript对象，后者为在html中javascript实例，必须与html中的命名一致，然后直接调用方法。
    mwv.addJavascriptInterface(javaScriptInterface, "AndroidFunction");
    //载入本地html文件
    mwv.loadUrl("file:///android_asset/www/index.html");
	
* JavaScript对象的编写

	public class JavaScriptInterface{

        Context mcontext;

        public JavaScriptInterface(Context context){
            this.mcontext = context;
        }

        @JavascriptInterface //当api>17时需要加该注解
        public void showToast(final String webMessage){
            final String msgeToast = webMessage;
            myHandler.post(new Runnable() {
                @Override
                public void run() {
                    mtv.setText(webMessage);
                }
            });
            Toast.makeText(mcontext, webMessage, Toast.LENGTH_SHORT).show();
        }
    }
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
