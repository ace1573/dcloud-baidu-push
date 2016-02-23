1. ����
===
* ����һ��[dcloud](http://www.dcloud.io/)���ɰٶ����͵Ľ̳̣�android�ˣ���
* ����һ�����ߴ�����õĽ̡̳�
* ����Ŀ�ðٶ�����ȥ�滻dcloudĬ�ϼ��ɵĸ������ͣ�ʹ�޸Ķ���web������ȫ͸���ģ�Ҳ����˵js�˵������߼������������޸ġ�


2. ʹ��
===
* ����[dcloud�ٷ��̳�](http://ask.dcloud.net.cn/article/227)���ɸ������͡�
* ����[�ٶ�����sdk](http://push.baidu.com/sdk/push_client_sdk_for_android)������sdk�е�jar����so�ļ���so�ļ�ֻ�赼��armeabi��armeabi-v7a��x86�ļ����µġ�
* ɾ������jar��GetuiSdk2.5.0.0.jar��GetuiExt-2.0.3.jar��aps-igexin.jar�����뱾��ĿlibsĿ¼�е�baidu-gexing.jar��
* ��AndroidManifest.xml��ע�͵��������͵��������ã�����������ã�
```html
<!-- baidu push��ʼ -->
        
<!-- ���ڲ������ش�����֪ͨ -->
<receiver android:name="io.dcloud.feature.aps.NotificationReceiver" >
    <!-- �����������ڴ���������Ϣ -->
    <intent-filter>
	<action android:name="android.intent.action.BOOT_COMPLETED" />
	<action android:name="cn.tomygo.app.v2.__CREATE_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__REMOVE_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__CLEAR_NOTIFICATION" />
	<action android:name="cn.tomygo.app.v2.__CLILK_NOTIFICATION" />
    </intent-filter>
</receiver>

<!-- ԭ�����ڸ��Ƶ����ã�ͬ�����ڰٶ����� -->
<meta-data
    android:name="PUSH_APPID"
    android:value="��д���appid" />
<meta-data
    android:name="PUSH_APPKEY"
    android:value="��д���appkey" />
<meta-data
    android:name="PUSH_APPSECRET"
    android:value="��д���appsecret" />

<!-- Push service ������Ҫ��Ȩ�� -->
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
	
	<!-- 4.6�汾������Activity����������Push��̨����� -->
	<activity
	    android:name="com.baidu.android.pushservice.PushKeepAlive"
	    android:theme="@android:style/Theme.Translucent.NoTitleBar"/>
	
	<!-- push service start -->
	<!-- ���ڽ���ϵͳ��Ϣ�Ա�֤PushService�������� -->
	<receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
	    android:process=":bdservice_v1" >
	    <intent-filter>
		<action android:name="android.intent.action.BOOT_COMPLETED" />
		<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
		<action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
		<action android:name="com.baidu.android.pushservice.action.media.CLICK" />
		<!-- ��������Ϊ��ѡ��action�������ɴ�����service����ʺ���Ϣ�����ٶ� -->
		<action android:name="android.intent.action.MEDIA_MOUNTED" />
		<action android:name="android.intent.action.USER_PRESENT" />
		<action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
		<action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
	    </intent-filter>
	</receiver>
	<!-- Push������տͻ��˷��͵ĸ�������-->
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
	<!-- 4.4�汾������CommandService����������С�׺������ֻ��ϵ�ʵ�����͵����� -->
	<service android:name="com.baidu.android.pushservice.CommandService"
	    android:exported="true" />
	
	<!-- �˴�Receiver�����޸�Ϊ��ǰ����·�� -->
	<receiver android:name="io.dcloud.feature.apsGt.MyPushMessageReceiver">
	    <intent-filter>
		<!-- ����push��Ϣ --> 
		<action android:name="com.baidu.android.pushservice.action.MESSAGE" />
		<!-- ����bind��setTags��method�ķ��ؽ��--> 
		<action android:name="com.baidu.android.pushservice.action.RECEIVE" />
		<!-- ����֪ͨ����¼�����֪ͨ�Զ������� --> 
		<action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
	    </intent-filter>
	</receiver>
	<!-- baidu push���� -->
```
*  ������ɡ�

3.˵��
===
* �ٶ������͵ĸ����ֶε����ݷ�����Ϣ�����payload�ֶ��У�
```javascript
//js��ӡ�����ֶ�
plus.push.addEventListener( "click", function( msg ) {
	console.log("�����ֶΣ�"+msg.payload);
}, false );
```
