# 实验4 Intent

## 实验环境
- 开发环境：Android 7.0
- 软件工具：Android Studio Koala 2024

## 实验目的

通过自定义 WebView 验证隐式 Intent 的使用。

## 实验内容

实验涉及两个独立的 Android 应用程序开发：

1. **第一个应用**: 获取 URL 地址并启动隐式 Intent。
2. **第二个应用**: 自定义 WebView 加载 URL。

## 实验步骤

### 1. 创建获取 URL 地址的应用

- **功能描述**: 用户输入 URL 地址，点击按钮后发起浏览网页的隐式 Intent。
- **实现步骤与代码**:
  1. 创建一个新工程。
  2. 在 `activity_main.xml` 文件中设计界面，包含一个 EditText 输入框和一个按钮：

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/editTextUrl"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="输入URL地址" />

    <Button
        android:id="@+id/buttonOpen"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="打开网页" />
</LinearLayout>
```

  3. 在 `MainActivity.java` 文件中编写逻辑代码：

```java
package com.example.intentexample;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText editTextUrl = findViewById(R.id.editTextUrl);
        Button buttonOpen = findViewById(R.id.buttonOpen);

        buttonOpen.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String url = editTextUrl.getText().toString();
                Intent intent = new Intent(Intent.ACTION_VIEW);
                intent.setData(Uri.parse(url));
                startActivity(intent);
            }
        });
    }
}
```

### 2. 创建自定义 WebView 的应用

- **功能描述**: 接收隐式 Intent，并使用自定义 WebView 加载网页。
- **实现步骤与代码**:
  1. 创建另一个新工程。
  2. 在 `activity_main.xml` 文件中设计界面，包含一个 WebView：

```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

  3. 在 `MainActivity.java` 文件中编写逻辑代码：

```java
package com.example.mybrowser;

import android.os.Bundle;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        WebView webView = findViewById(R.id.webView);
        webView.setWebViewClient(new WebViewClient());

        if (getIntent() != null && getIntent().getData() != null) {
            webView.loadUrl(getIntent().getData().toString());
        }
    }
}
```

  4. 配置 `AndroidManifest.xml` 文件，添加 Intent 过滤器：

```xml
<application
    android:allowBackup="true"
    android:label="@string/app_name"
    android:supportsRtl="true"
    android:theme="@style/Theme.MyBrowser">

    <activity android:name=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="http" />
            <data android:scheme="https" />
        </intent-filter>
    </activity>
</application>
```

## 实验结果

- **第一个应用**: 成功实现了输入 URL 并触发隐式 Intent 的功能。
- **第二个应用**: 成功使用自定义 WebView 加载指定的 URL，并在跳转后可以选择使用自定义浏览器。

## 实验总结

通过本次实验，我们熟悉了以下内容：

1. **隐式 Intent 的使用**: 学会如何通过 Intent 在应用间传递数据。
2. **自定义 WebView 的实现**: 了解了如何创建自定义 WebView 并加载指定的 URL。
3. **Intent 过滤器配置**: 学习了如何通过配置过滤器来响应隐式 Intent。

## 参考文献

- [Android 官方文档: Intents and Intent Filters](https://developer.android.google.cn/guide/components/intents-filters)