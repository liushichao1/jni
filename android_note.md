### 6.设置apk应用权限

设置权限的代码
```
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```
完整的示例

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.sogo.map.ros2console">



    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

### 5.vlc插件的退出卡死问题

```
1.停止播放需要调先将回掉函数设为空，然后在调用MediaPlayer对象的release回掉函数，不要调用stop(),
    //mMediaPlayer.stop();
    mMediaPlayer.setVideoCallback(mByteBuffer,null);
    mMediaPlayer.release();
    
    mLibVLC.release();

2.MediaPlayer对象的release需要在其他的线程中调用，不要在onDisplay回掉函数中调用，不然会卡住。

```



### 4.安卓wifi调试

1.开启手机设备的端口
```
adb tcpip 5555
```

2.链接设备
```
adb connect 192.168.0.101:5555
```



### 3.设置安卓签名文件

1.在android studio中点击菜单栏中的build - Generate Signed Bundle /APK...

2.在弹出的对话框中选择APK，点击next

3.点击Create new...

4.选择要存放的密钥的位置，例如``/User/liushichao/dexxx.keystore``

5.然后填写密码，这一页的都要填满，要记住密码，后边要用，填写好后点ok，我这里会弹一个报错对话框，直接关了就行

6.将dexxx.keystore拷贝到工程中app模块目录下

7.修改app目录下的build.gradle,添加如下内容，然后正常编译就好了。

```
android {
    ...
    signingConfigs {
        release {
            storeFile file("./dexxx.keystore")
            storePassword "woshimima"
            keyAlias "sdfff"
            keyPassword "wohaishimima"
        }
        debug {
            storeFile file("./dexxx.keystore")
            storePassword "woshimima"
            keyAlias "sdfff"
            keyPassword "wohaishimima"
        }
    }
}
```

8.选择V1，V2有些机型可能不能用

8.生成的apk文件在app/debug目录下




### 2.android Timer中不能直接修改UI界面

要调用runOnUiThread

```
        status_timer = new Timer();
        status_timer.schedule(new TimerTask() {
            @Override
            public void run() {
                runOnUiThread(new Runnable() {
                    @Override
                    public void run() {
                        statusUpdateTimer();
                    }
                });
            }
        }, 0, STATUS_UPDATE_TIMER_VALUE * 1000);

```

也可以调用view类的post方法，直接将Runnable添加到ui线程的消息队列中


### 1.修改appId

```
android {
        defaultConfig {
            applicationId "com.example.myapp"
            minSdkVersion 15
            targetSdkVersion 24
            versionCode 1
            versionName "1.0"
        }
        ...
    }
    
```
    
    
### 4.adb命令

1.结束app进程

com.liushichao.myapp是appid，可以用adb shell top命令查看
``
adb shell am force-stop com.liushichao.myapp
``

### 5.android请求权限

```
1.需要手动添加androidmanifest.xml中
    <uses-permission android:name="android.permission.CAMERA"/>
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    
    WRITE_EXTERNAL_STORAGE包含READ_EXTERNAL_STORAGE权限，所以可以只写WRITE_EXTERNAL_STORAGE
    
2.   
    1.确认build.gradle中的compileSdkVersion是大于28的，如果不报错也没关系
    android {
        ....
        
        compileSdkVersion 28
        
        ....
    }
    2.在顶层中添加依赖
    
        dependencies {
          ...
          
          def activity_version = "1.2.0-alpha08"
          implementation "androidx.activity:activity:$activity_version"
          
          ...
        }

    3.在代码中需要请求权限的地方加入下边的代码
    // Permissions for Android 6+
    if (!ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.CAMERA) ||
            !ActivityCompat.shouldShowRequestPermissionRationale(this,Manifest.permission.WRITE_EXTERNAL_STORAGE)) {
      ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.CAMERA}, 1);
    } else {
      Toast.makeText(this,"已拒绝权限，请在应用管理中手动打开！！！", Toast.LENGTH_SHORT).show();
    }

    if (savedInstanceState != null) {
      isWorking = savedInstanceState.getBoolean(IS_WORKING);
    }
```
