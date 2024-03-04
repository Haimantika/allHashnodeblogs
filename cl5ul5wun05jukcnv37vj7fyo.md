---
title: "Appwrite Autodesk OAuth Integration"
datePublished: Thu Jul 21 2022 05:21:30 GMT+0000 (Coordinated Universal Time)
cuid: cl5ul5wun05jukcnv37vj7fyo
slug: appwrite-autodesk-oauth-integration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1658380851300/p1IKpm6s2.png
tags: webdev, appwrite

---

Appwrite 0.15 is out now!🎉 We have released some amazing features, and also added a few more OAuth2 providers, one of which is Autodesk. This article will teach us to set up Autodesk authentication in our applications using Appwrite.

> **About Appwrite**
> Appwrite is a self-hosted backend-as-a-service platform that provides developers with all the core APIs required to build any application. Appwrite provides you with a set of APIs, tools, and a management console UI to help you build your apps a lot faster and in a much more secure way.

**📃Prerequisites**

To follow this tutorial, you’ll need access to an Appwrite project or permissions to create one. If you don’t already have an Appwrite server running, follow the official installation tutorial to set one up. Once you start a project on the Appwrite Console, you can head over to **Users →Settings** to find the list of the supported OAuth2 providers. This is where we will set up the Autodesk OAuth provider.

You will also need an account in the Autodesk Forge portal; if you don’t have one, you can easily create one free from here.

**🔐Configure Autodesk OAuth**

Once our Appwrite project is up and running, we must create an app in the Autodesk Forge portal. After signing in, you should see your name in the Profile menu on the top right, then click on My Apps. Click the CREATE APP button on the top-right of the My Apps page to create a new app.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380393721/IuW661vTs.png align="left")

You will be redirected to the next page where you will get to choose the required APIs, keep the default choice of APIs and then enter the required details, then click on CREATE APP. After the app creation, you will be redirected to the App Information page that contains the Client Id and Client Secret. Keep it ready at hand, as we will be using it in the following steps.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380529760/N0eNcd6l2.png align="left")

**🤝Enable Autodesk in Appwrite**

1. To enable Autodesk in Appwrite, visit the Appwrite Dashboard and head over to** Users →Settings.**


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380549889/v2doONATZ.png align="left")

2. From the list of providers, choose `Autodesk` and enter the App ID and App Secret from the previous step (i.e the `Client Id` and `Client Secret`).


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380579263/dzCf2qMxh.png align="left")


3. Make sure you copy the redirect URL from Appwrite’s Autodesk OAuth Setting dialog and paste it to your Autodesk app’s Callback URL


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380603826/_mYbBz3sg.png align="left")

**👨‍💻Implementing Sign-In With Autodesk In Your Project**

Once you have set up Autodesk OAuth credentials in the Appwrite console, you are ready to implement Autodesk Sign In in your project. Let's see how we can do it on various platforms.

You can use our client SDKs for various platforms to authenticate your users with OAuth2 providers. Before you can authenticate, you need to add our SDK as a dependency and configure it with an endpoint and project ID. To learn to configure our SDKs, you can follow the getting started guide for each platform. The appropriate links are provided in each section below. Once you have the SDK configured, you can instantiate and call the account service to create a session from the OAuth2 provider. Below are the examples for different platforms to initialize clients and perform OAuth2 login.

**🌐Web**

First, you need to add a web platform to your project from the Appwrite console. Adding a web platform allows Appwrite to validate the request it receives and prevent cross-origin errors on the web. On the project settings page, click on Add Platform button and select New Web App. In the dialog box that appears, give a recognizable name to your platform and add the hostname of your application.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380628198/a_n_-0oQK.png align="left")

Follow the Getting Started for Web guide for detailed instruction on how to use Appwrite with your web application.

```
const appwrite = new Appwrite();

appwrite
  .setEndpoint('[YOUR_END_POINT]')
  .setProject('[YOUR_PROJECT_ID]');

try {
    await appwrite.account.createOAuth2Session(
        "autodesk",
        "[YOUR_END_POINT]/auth/oauth2/success",
        "[YOUR_END_POINT]/auth/oauth2/failure",
    );
} catch (error) {
    throw error;
}
```

**📱Flutter**

For Flutter, in Android, to properly handle redirecting your users back to your mobile application after completion of the OAuth flow, you need to set the following in your AndroidManifest.xml file.

```xml
<manifest ...>
  ...
  <application ...>
    ...
    <!-- Add this inside the `<application>` tag, along side the existing `<activity>` tags -->
    <activity android:name="com.linusu.flutter_web_auth.CallbackActivity" android:exported="true">
      <intent-filter android:label="flutter_web_auth">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="appwrite-callback-[PROJECT_ID]" />
      </intent-filter>
    </activity>
  </application>
</manifest>
```

You also need to add the Flutter platform to your project from the Appwrite console. Adding Flutter platforms allows Appwrite to validate its request and prevents requests from unknown applications. On the project settings page, click on **Add Platform** button and select **New Flutter App**. In the dialog box that appears, select the appropriate Flutter platform, give a recognizable name to your platform and add the application ID or package name based on the platform. You need to follow this step for each Flutter platform you will build your application.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380663729/YpnrP989t.png align="left")

For more detailed instructions on getting started with Appwrite for Flutter developers, follow our official Getting Started for Flutter guide. Finally, you can call account.createOAuth2Session from your application as shown below.

```
import 'package:appwrite/appwrite.dart';

void main() async {
    final client = new Client();

    client
    .setEndpoint('YOUR_END_POINT')
    .setProject('YOUR_PROJECT_ID');

    final account = Account(client);

    try {
        await account.createOAuth2Session(
            provider: "autodesk"
        );
    } catch (error) {
        throw error;
    }
}
```

**🤖Android**

For Android, to properly handle redirecting your users back to your mobile application after completion of the OAuth flow, you need to set the following in your AndroidManifest.xml file.

```
<manifest ...>
  ...
  <application ...>
    ...
    <!-- Add this inside the `<application>` tag, along side the existing `<activity>` tags -->
    <activity android:name="io.appwrite.views.CallbackActivity" android:exported="true">
      <intent-filter android:label="android_web_auth">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="appwrite-callback-[PROJECT_ID]" />
      </intent-filter>
    </activity>
  </application>
</manifest>
```

You also need to add the Android platform to your project from the Appwrite console. Adding Android platforms allows Appwrite to validate the request it receives and also prevents requests from unknown applications. On the project settings page, click on **Add Platform** button and select **New Android App**. In the dialog box that appears, give your platform a recognizable name and add the package name of your application.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380739923/upODSwRHa.png align="left")

For more detailed instructions on getting started with Appwrite for Android developers, follow our official Getting Started for Android guide. Finally, you can call account.createOAuth2Session from your application as shown below.

```
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.launch
import io.appwrite.Client
import io.appwrite.services.Account

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val client = Client(applicationContext)
            .setEndpoint("https://[HOSTNAME_OR_IP]/v1") // Your API Endpoint
            .setProject("5df5acd0d48c2") // Your project ID

        val account = Account(client)

        GlobalScope.launch {
            account.createOAuth2Session(
                activity = this@MainActivity,
                provider = "autodesk"
            )

        }
    }
}
```

**🍎Apple**

To capture the Appwrite OAuth callback URL, the following URL scheme needs to add to your `Info.plist`.

```
<key>CFBundleURLTypes</key>
<array>
<dict>
    <key>CFBundleTypeRole</key>
    <string>Editor</string>
    <key>CFBundleURLName</key>
    <string>io.appwrite</string>
    <key>CFBundleURLSchemes</key>
    <array>
        <string>appwrite-callback-[PROJECT_ID]</string>
    </array>
</dict>
</array>
```
You also need to add the Apple platform to your project from the Appwrite console. Adding Apple platforms allows Appwrite to validate the request it receives and prevents requests from unknown applications. On the project settings page, click on the **Add Platform ** button and select **New Apple App**. In the dialog box that appears, select the appropriate Apple platform tab, give your platform a recognizable name, and add the package name of your application. For each supported Apple platform, you need to follow this process.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658380774433/hYRSJUVkK.png align="left")

For more detailed instructions on getting started with Appwrite for iOS developers, follow our official Getting Started for Apple guide. Finally, you can call account.createOAuth2Session from your application as shown below.

```
import Appwrite

let client = Client()
    .setEndpoint("[YOUR_ENDPOINT]")
    .setProject("[YOUR_PROJECT_ID]")

let account = Account(client)

account.createOAuth2Session(
    provider: "autodesk"
){ result in
    switch result {
    case .failure(let err):
        print(err.message)
    case .success:
        print("logged in")
    }
}
```

**🏁Conclusion**

Well, that’s all it takes to set up Autodesk OAuth-based authentication with Appwrite. The following resources can be handy if you want to explore Appwrite further.

- [Appwrite Docs](https://appwrite.io/docs)
- [Appwrite Discord](https://appwrite.io/discord)
- [Appwrite GitHub](https://github.com/appwrite/appwrite)