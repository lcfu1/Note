# LitePal

目录：

- [介绍](#介绍)
- [使用](#使用)
- [更多查询操作](#更多查询操作)

## 介绍

1. 开源的Android数据库框架，采用了对象关系映射(ORM)的模式。
2. github地址：[https://github.com/LitePalFramework/LitePal](https://github.com/LitePalFramework/LitePal)。

## 使用

1. 添加以下依赖：

   ```
   implementation 'org.litepal.android:core:2.0.0'
   ```

2. 切换到Project模式，右击app/src/main目录，New/Directory，创建一个assets目录，在此目录下新建一个litepal.xml文件，如下：

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <litepal>
       <dbname value="DB" />
       <version value="1" />
       <list>
           <mapping class="com.example.androidtest.AB"/>
       </list>
   </litepal>
   ```

3. 在AndroidManifest.xml中加上`android:name="org.litepal.LitePalApplication"`，如下：

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
       package="com.example.androidtest">
   
       <uses-permission android:name="android.permission.INTERNET" />
   
       <application
           android:name="org.litepal.LitePalApplication"
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

4. 根据你需要创建的表和表中包含的列，用面向对象的思维，定义一个AB类(AB相当于表名，A、B为列)，如下：

   ```
   package com.example.androidtest;
   
   import org.litepal.crud.LitePalSupport;
   
   public class AB extends LitePalSupport {
       private String A;
       private String B;
   
       public String getA() {
           return A;
       }
   
       public void setA(String a) {
           A = a;
       }
   
       public String getB() {
           return B;
       }
   
       public void setB(String b) {
           B = b;
       }
   }
   ```

5. 创建数据库、增加数据、更新数据、查询数据、删除数据，如下：

   LitePalActivity.java

   ```
   package com.example.androidtest;
   
   import android.support.v7.app.AppCompatActivity;
   import android.os.Bundle;
   import android.view.View;
   import android.widget.Button;
   
   import com.example.androidtest.Util.LogUtil;
   
   import org.litepal.LitePal;
   
   import java.util.List;
   
   public class LitePalActivity extends AppCompatActivity implements View.OnClickListener {
       private Button createDatabase;
       private Button addData;
       private Button updataData;
       private Button queryData;
       private Button deleteData;
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_lite_pal);
   
           createDatabase = (Button) findViewById(R.id.create_database);
           createDatabase.setOnClickListener(this);
           addData = (Button) findViewById(R.id.add_data);
   
           addData.setOnClickListener(this);
   
           updataData = (Button) findViewById(R.id.updata_data);
           updataData.setOnClickListener(this);
   
           queryData = (Button) findViewById(R.id.query_data);
           queryData.setOnClickListener(this);
   
           deleteData = (Button) findViewById(R.id.delete_data);
           deleteData.setOnClickListener(this);
       }
   
       @Override
       public void onClick(View v) {
           int id = v.getId();
           AB ab = new AB();
           if (id == R.id.create_database) {
               LitePal.getDatabase();
           } else if (id == R.id.add_data) {
               ab.setA("a");
               ab.setB("c");
               ab.save();
           } else if (id == R.id.updata_data) {
               ab.setB("b");
               ab.updateAll("A=?", "a");
           } else if (id == R.id.query_data) {
               List<AB> abs = LitePal.findAll(AB.class);
               for (AB ab1 : abs) {
                   LogUtil.d("A----------", ab1.getA());
                   LogUtil.d("B----------", ab1.getB());
               }
           } else if (id == R.id.delete_data) {
               LitePal.deleteAll(AB.class, "A=?", "a");
           }
       }
   }
   ```

   activity_lite_pal.xml

   ```
   <?xml version="1.0" encoding="utf-8"?>
   <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:orientation="vertical">
   
       <Button
           android:id="@+id/create_database"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:text="createDatabase"
           android:textAllCaps="false"/>
       <Button
           android:id="@+id/add_data"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:text="addData"
           android:textAllCaps="false"/>
       <Button
           android:id="@+id/updata_data"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:text="updataData"
           android:textAllCaps="false"/>
       <Button
           android:id="@+id/query_data"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:text="queryData"
           android:textAllCaps="false"/>
       <Button
           android:id="@+id/delete_data"
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:text="deleteData"
           android:textAllCaps="false"/>
   </LinearLayout>
   ```

## 更多查询操作

