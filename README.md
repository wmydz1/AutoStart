# AutoStart

当Android启动时，会发出一个系统广播，内容为ACTION_BOOT_COMPLETED，它的字符串常量表示为android.intent.action.BOOT_COMPLETED。
只要在程序中“捕捉”到这个消息，再启动之即可。记住，Android框架说：Don't call me, I'll call you back。
我们要做的是等到接收这个消息，而实现的手段就是实现一个BroadcastReceiver。

```
package com.example;
 
import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.*;
import android.widget.Toast;
 
public class MainActivity extends Activity {   // 收到消息启动此Activity
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        super.setIntegerProperty("splashscreen", R.drawable.wandapad_splash);
        TextView tv = new TextView(this);
        tv.setText("Hello这是MainActivity");
        setContentView(tv);
    }

```

BootBroadcastReceiver.java 该类派生自BroadcastReceiver，覆载方法onReceive中，检测接收到的Intent是否符合BOOT_COMPLETED，如果符合，则启动MainActivity。

```
package com.example;
 
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
 

public class BootBroadcastReceiver extends BroadcastReceiver {
    static final String ACTION = "android.intent.action.BOOT_COMPLETED";
 
    @Override
    public void onReceive(Context context, Intent intent) {
        if (intent.getAction().equals(ACTION)) {
            Intent mainActivityIntent = new Intent(context, MainActivity.class);  // 要启动的Activity
            mainActivityIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.startActivity(mainActivityIntent);
        }
    }
}

```

```
<receiver android:name=".BootBroadcastReceiver">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED" />
   </intent-filter>
  </receiver>
  

```

```
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"></uses-permission>

```

在多数情况下，要自动运行的不是有界面的程序，而是在后台运行的service。此时，就要用startService来启动相应的service了。
