# ZxHwSDK-Android

SDK-Android 接入文档

# 环境准备

+ 在以下环境通过测试  
Android Studio 2020.3  
gradle 7.0.4
minSdk 19   
targetSdk 32   

+ 物料清单说明(单独提供)   
  -   Applovin_APIKey, 替换build.gradle对应的值   
  -   Applovin_SdkKey,替换AndroidManifest.xml对应的值  
  -   Admob_PubKey,替换AndroidManifest.xml对应的值  
  -   FB_App_ID,替换strings.xml对应的值  
  -   FB_Login_Protocol_Scheme,替换strings.xml对应的值   
  -   FB_Client_Token,替换strings.xml对应的值   
  -   google-services.json, 配置到app目录  
  -   tenjin.aar,配置到app的lib目录   
  -   zxhwsdk.aar,配置到app的lib目录   



# Project层级配置
+ gradle.properties 添加
  ```
  android.useAndroidX=true
  android.enableJetifier=true
  ```


+ build.gradle 添加（app conf 标识行）
  ```
  buildscript {
      repositories {

          //app conf
          google()
          mavenCentral()

          //app conf: applovin ad review
          maven {url 'https://artifacts.applovin.com/android'}
      }
      dependencies {

          //app conf: Google services
          classpath 'com.google.gms:google-services:4.3.13'

          //app conf: applovin ad review
          classpath "com.applovin.quality:AppLovinQualityServiceGradlePlugin:+"

          //app conf:  Add the Crashlytics Gradle plugin
          classpath 'com.google.firebase:firebase-crashlytics-gradle:2.9.1'

      }
  }
  ```


+ settings.gradle （app conf 标识行）
  ```
  repositories {
      //app conf: pangle
      maven {url 'https://artifact.bytedance.com/repository/pangle'}
  }
  ```
  <table><tr><td bgcolor=LightSeaGreen>注意：高版本gradle 的settings.gradle可配置maven，如果gradle版本较低，不可以配置maven，可以直接在app层级的build.gradle中配置。</td></tr></table>


# App层级配置
+ build.gradle（app conf 标识行）

  ```
  //app conf: google services
  apply plugin: 'com.google.gms.google-services'
  apply plugin: 'com.google.firebase.crashlytics'
  //app conf: applovin ad review
  apply plugin: 'applovin-quality-service'
  applovin {
      apiKey "Applovin_APIKey"
  }

  dependencies {

      //app conf: add admob SDK
      implementation 'com.google.android.gms:play-services-ads:21.0.0'

      //app conf: add gson
      implementation 'com.google.code.gson:gson:2.9.0'

      //app conf: add google pay
      def billing_version = "5.0.0"
      implementation "com.android.billingclient:billing:$billing_version"

      //app conf: Add firebase SDK
      implementation platform('com.google.firebase:firebase-bom:30.3.1')
      implementation 'com.google.firebase:firebase-analytics'
      implementation 'com.google.firebase:firebase-auth'
      implementation 'com.google.firebase:firebase-firestore'
      implementation 'com.google.firebase:firebase-config'

      //app conf: applovin SDK
      implementation 'com.applovin:applovin-sdk:+'

      //app conf: Google Advertising ID
      implementation 'com.google.android.gms:play-services-appset:16.0.2'
      implementation 'com.google.android.gms:play-services-ads-identifier:18.0.1'
      implementation 'com.google.android.gms:play-services-basement:18.1.0'

      //app conf: tenjin
      implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
      implementation files('libs/tenjin.aar')
      implementation 'com.android.installreferrer:installreferrer:2.2'

      //app conf: crashlytics SDK
      implementation 'com.google.firebase:firebase-crashlytics'

      //app conf: fb sdk
      implementation 'com.facebook.android:facebook-android-sdk:14.1.1'
      //implementation 'com.facebook.android:facebook-login:latest.release'

      //app conf: fb audience-network SDK
      implementation 'com.facebook.android:audience-network-sdk:6.+'

      //app conf: aliyun sdk
      implementation 'com.aliyun.dpa:oss-android-sdk:2.9.11'
      implementation 'com.squareup.okhttp3:okhttp:4.10.0'
      implementation 'com.squareup.okio:okio:3.2.0'

      //app conf: pangle ,adapter-for-admob
      implementation 'com.pangle.global:ads-sdk:4.6.0.4'
      implementation 'com.pangle.global:adapter-for-admob:1.4.0'

      //app conf: Vungle SDK
      implementation 'com.vungle:publisher-sdk-android:6.12.0'
      implementation 'com.vungle:publisher-sdk-android:6.12.0'

      //app conf: adcolony SDK
      implementation 'com.adcolony:sdk:4.8.0'

      //app conf: inmobi SDK
      implementation 'com.inmobi.monetization:inmobi-ads:10.0.8'
      implementation 'androidx.browser:browser:1.3.0'
      implementation 'com.squareup.picasso:picasso:2.71828'
      //implementation 'com.android.support:support-v4:28.0.0'
      implementation 'androidx.recyclerview:recyclerview:1.1.0'

      //Admob for Adapter: applovin,facebook,vungle,adcolony,inmobi
      implementation 'com.google.ads.mediation:applovin:11.4.4.0'
      implementation 'com.google.ads.mediation:facebook:6.11.0.1'
      implementation 'com.google.ads.mediation:vungle:6.9.1.0'
      implementation 'com.google.ads.mediation:adcolony:4.8.0.0'
      implementation 'com.google.ads.mediation:inmobi:10.0.7.0'

      //Applovin for Adapter: admob,facebook,pangle,vungle,adcolony,inmobi
      implementation 'com.applovin.mediation:google-adapter:+'
      implementation 'com.applovin.mediation:facebook-adapter:+'
      implementation 'com.applovin.mediation:bytedance-adapter:+'
      implementation 'com.applovin.mediation:vungle-adapter:+'
      implementation 'com.applovin.mediation:adcolony-adapter:+'
      implementation 'com.applovin.mediation:inmobi-adapter:+'

      //app conf: zxhwsdk
      implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])
      implementation files('libs/zxhwsdk.aar')

  }
  ```


+ AndroidManifest.xml 配置
   - 在application内配置

    ```
    <application

        <!--==============================================-->
        <!--app conf: admob ID-->
        <meta-data
            android:name="com.google.android.gms.ads.APPLICATION_ID"
            android:value="Admob_PubKey"/>

        <!--app conf: applovin key-->
        <meta-data android:name="applovin.sdk.key"
            android:value="Applovin_SdkKey"/>

        <meta-data
            android:name="TENJIN_APP_STORE"
            android:value="googleplay" />

        <!--app conf: facebook-->
        <meta-data android:name="com.facebook.sdk.ApplicationId" android:value="@string/facebook_app_id"/>
        <meta-data android:name="com.facebook.sdk.ClientToken" android:value="@string/facebook_client_token"/>

        <activity android:name="com.facebook.FacebookActivity"
            android:configChanges=
                "keyboard|keyboardHidden|screenLayout|screenSize|orientation"
            android:label="@string/app_name" />
        <activity
            android:name="com.facebook.CustomTabActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="@string/fb_login_protocol_scheme" />
            </intent-filter>
        </activity>

        <!--app conf: AdColony-->
        <activity android:name="com.adcolony.sdk.AdColonyInterstitialActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen"
            android:hardwareAccelerated="true"/>
        <activity android:name="com.adcolony.sdk.AdColonyAdViewActivity"
            android:configChanges="keyboardHidden|orientation|screenSize"
            android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen"
            android:hardwareAccelerated="true"/>
        <!--==============================================-->

    </application>
    ```   

   - 配置权限 permission
    ```
    <!--===========================app conf: permission===========================-->
    <uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <!--===========================app conf: permission===========================-->
    ```

+ res/values/strings.xml 配置
  ```
  <resources>
    <string name="facebook_app_id">FB_App_ID</string>
    <string name="fb_login_protocol_scheme">FB_Login_Protocol_Scheme"</string>
    <string name="facebook_client_token">FB_Client_Token</string>
  </resources>
  ```

# ProGuard Rules
  ```

  ################################---app---##################################
  # ----------------------------------------------------------------------------

  #medtion sdk
  -keep class com.bytedance.sdk.** { *; }

  -keepclassmembers class * {
      @android.webkit.JavascriptInterface <methods>;
  }

  -keepattributes SourceFile,LineNumberTable
  -keep class com.inmobi.** { *; }
  -keep public class com.google.android.gms.**
  -dontwarn com.google.android.gms.**
  -dontwarn com.squareup.picasso.**
  -keep class com.google.android.gms.ads.identifier.AdvertisingIdClient{
       public *;
  }
  -keep class com.google.android.gms.ads.identifier.AdvertisingIdClient$Info{
       public *;
  }
  # skip the Picasso library classes
  -keep class com.squareup.picasso.** {*;}
  -dontwarn com.squareup.okhttp.**
  # skip Moat classes
  -keep class com.moat.** {*;}
  -dontwarn com.moat.**
  # skip IAB classes
  -keep class com.iab.** {*;}
  -dontwarn com.iab.**


  # SDK
  -keep class com.ldxyx.zx.hw.xsdk.* {
      public static <methods>;
      public <methods>;
  }
  ################################---app---##################################
  ```

# 接入示例
  + Activity 配置SDK   
   ```
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ZxHwSDK.onCreate(this, new ZxHwSDKHandler() {
            @Override
            public void handlePaymentResult(String productId,String orderId,String token) {
                /*
                    AppActivity app = (AppActivity)activity;
                    app.runOnGLThread(new Runnable() {
                        @Override
                        public void run() {
                            Cocos2dxJavascriptJavaBridge.evalString(jsfun);
                        }
                    });
                 */
            }

            @Override
            public void handleAdEarnedReward(String adUnit, String rewardType, int rewardAmount) {
                //激励视频完成，发放奖励回调
                /*
                    AppActivity app = (AppActivity)activity;
                    app.runOnGLThread(new Runnable() {
                        @Override
                        public void run() {
                            Cocos2dxJavascriptJavaBridge.evalString(jsfun);
                        }
                    });
                 */
            }

            @Override
            public void handleFacebookLoginResult(boolean status, String token, String userId, String name,String pictureUrl) {
            }

            @Override
            public void handleFacebookDownloadPictureResult(boolean status, String userId,String file) {
            }

            @Override
            public void handleReadUserStoreResult(boolean status, int accountType, String userId, String userData) {
            }
        });
    }

    @Override
    protected void onPause() {
        super.onPause();
        ZxHwSDK.onPause(this);
    }

    @Override
    protected void onResume() {
        super.onResume();
        ZxHwSDK.onResume(this);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        ZxHwSDK.onActivityResult(this,requestCode,resultCode,data);
        super.onActivityResult(requestCode, resultCode, data);
    }
   ```
   
   
+ 广告展示操作      
     strategy定义： 广告策略组，策略组定义在广告配置中。没有特殊需求，该字段可以指定为strategy=""   
     ad_type定义：  （AD_Type_None = 0; AD_Type_Banner = 1; AD_Type_Interstitial = 2; AD_Type_Rewarded = 3;)   
     
  -   查询广告状态
   ```
    String strategy = "";
    int ad_type = AD_Type_Rewarded;
    if(ZxHwSDK.isAdReady(strategy,ad_type)){
        //do something
    }
   ```
   
  -   展示广告   
   激励视频的奖励，通过ZxHwSDKHandler 的handleAdEarnedReward()接口回调给业务层。   
   ```
    String strategy = "";
    int ad_type = AD_Type_Rewarded;
    if(ZxHwSDK.adShow(strategy, AD_Type_Rewarded)){
        //do something
    }
   ```
