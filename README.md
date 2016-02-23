1. 介绍
===
* 这是一个[dcloud](http://www.dcloud.io/)集成百度推送的教程（android端）。
* 这是一个离线打包配置的教程。
* 本项目用百度推送去替换dcloud默认集成的个推推送，使修改对于web层是完全透明的，也就是说js端的推送逻辑基本不用做修改。


2. 使用
===
* 按照[dcloud官方教程](http://ask.dcloud.net.cn/article/227)集成个推推送。
* 下载[百度推送sdk](http://push.baidu.com/sdk/push_client_sdk_for_android)，导入sdk中的jar包和so文件，so文件只需导入armeabi、armeabi-v7a、x86文件夹下的。
* 删除个推jar包GetuiSdk2.5.0.0.jar、GetuiExt-2.0.3.jar、aps-igexin.jar，导入本项目libs目录中的baidu-gexing.jar。
* 在AndroidManifest.xml中注释掉个推推送的所有配置，添加以下配置：
```html
<!-- baidu push开始 -->
        
<!-- 用于操作本地创建的通知 -->
<receiver android:name="io.dcloud.feature.aps.NotificationReceiver" >
    <!-- 如下配置用于处理推送消息 -->
    <intent-filter>
	<action android:name="android.intent.action.BOOT_COMPLETED" />
	<action android:name="cn.tomygo.app.v2.__CREATE_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__REMOVE_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__CLEAR_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__CLILK_NOTIFICATION" />
    </intent-filter>
</receiver>

<!-- 原本用于个推的配置，同样用于百度推送 -->
<meta-data
    android:name="PUSH_APPID"
    android:value="填写你的appid" />
<meta-data
    android:name="PUSH_APPKEY"
    android:value="填写你的appkey" />
<meta-data
    android:name="PUSH_APPSECRET"
    android:value="填写你的appsecret" />

<!-- Push service 运行需要的权限 -->
	 <uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.READ_PHONE_STATE" />
	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
	<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
	<uses-permission android:name="android.permission.WRITE_SETTINGS" />
	<uses-permission android:name="android.permission.VIBRATE" />
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
	<uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER"/>
	<uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
	<uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
	<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.EXPAND_STATUS_BAR" /> 
	
	<!-- 4.6版本新增的Activity声明，提升Push后台存活率 -->
	<activity
	    android:name="com.baidu.android.pushservice.PushKeepAlive"
	    android:theme="@android:style/Theme.Translucent.NoTitleBar"/>
	
	<!-- push service start -->
	<!-- 用于接收系统消息以保证PushService正常运行 -->
	<receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
	    android:process=":bdservice_v1" >
	    <intent-filter>
		<action android:name="android.intent.action.BOOT_COMPLETED" />
		<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
		<action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
		<action android:name="com.baidu.android.pushservice.action.media.CLICK" />
		<!-- 以下四项为可选的action声明，可大大提高service存活率和消息到达速度 -->
		<action android:name="android.intent.action.MEDIA_MOUNTED" />
		<action android:name="android.intent.action.USER_PRESENT" />
		<action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
		<action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
	    </intent-filter>
	</receiver>
	<!-- Push服务接收客户端发送的各种请求-->
	<receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
	    android:process=":bdservice_v1" >
	    <intent-filter>
		<action android:name="com.baidu.android.pushservice.action.METHOD" />
		<action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
	    </intent-filter>
	    <intent-filter>
		<action android:name="android.intent.action.PACKAGE_REMOVED" />
		<data android:scheme="package" />
	    </intent-filter>                   
	</receiver>
	<service android:name="com.baidu.android.pushservice.PushService" android:exported="true" 
	    android:process=":bdservice_v1" >
	    <intent-filter >
		    <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
	    </intent-filter>
	</service>
	<!-- 4.4版本新增的CommandService声明，提升小米和魅族手机上的实际推送到达率 -->
	<service android:name="com.baidu.android.pushservice.CommandService"
	    android:exported="true" />
	
	<!-- 此处Receiver名字修改为当前包名路径 -->
	<receiver android:name="io.dcloud.feature.apsGt.MyPushMessageReceiver">
	    <intent-filter>
		<!-- 接收push消息 --> 
		<action android:name="com.baidu.android.pushservice.action.MESSAGE" />
		<!-- 接收bind、setTags等method的返回结果--> 
		<action android:name="com.baidu.android.pushservice.action.RECEIVE" />
		<!-- 接收通知点击事件，和通知自定义内容 --> 
		<action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
	    </intent-filter>
	</receiver>
	<!-- baidu push结束 -->
```
*  配置完成。

3.说明
===
* 百度中推送的附加字段的内容放在消息对象的payload字段中：
```javascript
//js打印附加字段
plus.push.addEventListener( "click", function( msg ) {
	console.log("附加字段："+msg.payload);
}, false );
```
