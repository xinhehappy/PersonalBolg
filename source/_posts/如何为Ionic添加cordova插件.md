---
title: 如何为Ionic添加cordova插件
date: 2017-04-10 15:00:26
tags:
---
# 一、新建cordova plugin项目，项目工程结构如下所示
主要包括三个文件： src、www和plugin.xml。

![cordova 工程结构](http://upload-images.jianshu.io/upload_images/2327406-81a621d885563266.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 1、src文件主要编写不同平台下的native代码。Eg: android 和ios

```
/**
新建的类一定要继承自CordovaPlugin类，并重写execute方法
**/
public class LoadingPlugin extends CordovaPlugin {

    public ProgressDialog loadingDialog;
    public LayoutInflater inflater;
    public AlertDialog.Builder alertDialogBuilder;
    public AlertDialog alertDialog;
    private Handler handler = new Handler();
    private TextView text ;

    @Override
    public boolean execute(String action, JSONArray args, final CallbackContext callbackContext) throws JSONException {

        if ("showLoading".equals(action)) {
            this.createDialog(cordova.getActivity(), "loading...");
            callbackContext.success("showLoading success!");
            return true;
        } else if("dismissDialog".equals(action)) {
          this.dismissDialog();
          callbackContext.success("dismissDialog success!");
          return true;
        } else {
          callbackContext.error("get plugin error!");
          return false;
        }
    }
}
```

## 2、www文件中提供调用native代码的接口，以供JavaScript代码调用
  ```
var exec = require('cordova/exec');
exports.showLoading = function(msg, success, error) {
  //五个参数：成功的回调方法、失败的回调方法、service、action、传递的参数
  exec(success, error, "LoadingPlugin", "showLoading", [msg]);
}
```
## 3、plugin.xml文件主要作用是处理js文件与Android、ios等平台的相关配置
```
<?xml version='1.0' encoding='utf-8'?>
<plugin id="expense-plugin-loading" version="0.0.1" xmlns="http://apache.org/cordova/ns/plugins/1.0" xmlns:android="http://schemas.android.com/apk/res/android">
    <name>LoadingPlugin</name>
    <!-- js-module标签：和Java文件交互的js文件。属性src就是www下面的js文件-->
    <js-module name="LoadingPlugin" src="www/LoadingPlugin.js"><clobbers target="loadingManager"/></js-module>

    <!--platform标签：为plugin添加的平台，本文主要是开发Android平台上的代码-->
    <platform name="android">
      <!--config-file标签：将配置文件注入到Android工程中的config.xml中-->
        <config-file parent="/*" target="res/xml/config.xml">
            <feature name="LoadingPlugin">
            <param name="android-package" value="com.baiwangmaoyi.expense.LoadingPlugin"/>
            </feature>
        </config-file>
        <config-file parent="/*" target="AndroidManifest.xml">
        </config-file>
        <!--srource-file标签：将src下的文件复制到platform下相应的工程目录下-->
        <source-file src="src/android/LoadingPlugin.java" target-dir="src/com/baiwangmaoyi/expense"/>
        <source-file src="src/android/layout/loading_layout.xml" target-dir="res/layout" />
      </platform>
</plugin>

```

# 二、为Ionic添加所写的Plugin
## 1、cordova plugin add [path to your plugin]
## 2、编写调用native文件的js代码并将其封装为.ts

文件名：ShowLoading.js
```
'use strict';
var {Observable} = require('rxjs/Rx');
var ShowLoading = (function(){
    function ShowLoading(){};
    ShowLoading.prototype.showLoading = function(msg){
        var promise = new Promise(function(resolve, reject){
            /**loadingManager是add plugin 时系统根据js-module中clobbers中设置的
             ** target值而生成的全局变量
             **showLoading为plugin工程中www下的.js文件中导出的方法名 **/
            loadingManager.showLoading(msg, function(data){
                resolve(data);
            }, function(err){
                reject(err);
            });
            // resolve("success");
        });
        return promise;
    };
    ShowLoading.prototype.dismissLoading = function(msg){
        var promise = new Promise(function(resolve, reject){
            loadingManager.dismissLoading('', function(data){
                resolve(data);   
            }, function(err){
                resolve(err);
            });
            // resolve("success");
        });
        return promise;
    };
    return ShowLoading;
    
}());

exports.ShowLoading = ShowLoading;
```
文件名：ShowLoading.d.ts, 封装后的方法。
```
export declare class ShowLoading {
    showLoading(msg: string): Promise<any>;
    dismissLoading(msg: string): Promise<any>;
}
```
# 三、在Ionic中.ts 文件中使用封装后的方法：

```
 this.showLoading = new ShowLoading();
    this.showLoading.showLoading("loading").then(res => {
      this.showToast(res);
      console.log('success');
    }, err => {
      console.log('err',err);
      this.showToast(err);
    });

    setTimeout(() => {
      this.showLoading.dismissLoading('').then(res =>{
        this.showToast(res);
        console.log("dismiss",res);
      }, err => {
        this.showToast(err);
        console.log('dismiss',err);
      });
    },3000);
```