---
title: App Center Auth for Xamarin
description: Using Auth in App Center
keywords: sdk, auth
author: amchew
ms.author: achew
ms.date: 07/22/2019
ms.topic: article
ms.assetid: c9ph013b-2o49-69q3-8ca3-572b07z12y79
ms.service: vs-appcenter
ms.custom: sdk, auth
ms.tgt_pltfrm: xamarin
---

# App Center Auth

> [!div  class="op_single_selector"]
> * [Android](android.md)
> * [iOS](ios.md)
> * [React Native](react-native.md)
> * [Unity](unity.md)
> * [Xamarin](xamarin.md)

## Add the SDK to your app

App Center Auth is a cloud-based identity management service that enables developers to authenticate application users and manage user identities. The service integrates with other parts of App Center, enabling developers to leverage user identity to view user data in other services and even send push notifications to users instead of individual devices. App Center Auth is powered by [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/).

Please follow the [Getting Started](~/sdk/getting-started/xamarin.md) section if you haven't set up the SDK in your app.

### 1. Add the App Center Auth module

The App Center SDK is designed with a modular approach – a developer only needs to integrate the modules of the services that they're interested in.

#### Visual Studio for Mac

* Open Visual Studio for Mac.
* Click **File** > **Open** and choose your solution.
* In the solution navigator, right-click the **Packages** section, and choose **Add NuGet packages...**.
* Search for **App Center**, and install **App Center Auth**.
* Click **Add Packages**.

#### Visual Studio for Windows

* Open Visual Studio for Windows.
* Click **File** > **Open** and choose your solution.
* In the solution navigator, right-click **References** and choose **Manage NuGet Packages**.
* Search for **App Center**, and install **Microsoft.AppCenter.Auth**.

#### Package Manager Console

* Open the console in [Visual Studio](https://visualstudio.microsoft.com/vs/). To do this, choose **Tools** > **NuGet Package Manager** > **Package Manager Console**.
* If you're working in **Visual Studio for Mac**, make sure you have **NuGet Package Management Extensions** installed. For this, choose **Visual Studio** > **Extensions**, search for **NuGet** and install, if necessary.
* Type the following command in the console:

```shell
Install-Package Microsoft.AppCenter.Auth
```

> [!NOTE]
> If you use the App Center SDK in a portable project (such as **Xamarin.Forms**), you must install the packages in each of the projects: the portable, Android, and iOS ones. To do that, you should open each sub-project and follow the corresponding steps described in [Visual Studio for Mac](#visual-studio-for-mac) or [Visual Studio for Windows](#visual-studio-for-windows) sections.

### 2. Start App Center Auth

In order to use App Center, you must opt in to the module(s) that you want to use. By default no modules are started and you will have to explicitly call each of them when starting the SDK.

#### 2.1 Add App Center Auth imports

Add the App Center Auth imports before you get started with using Auth module:

* **Xamarin.iOS** - Open the project's **AppDelegate.cs** and add the following lines below the existing `using` statements
* **Xamarin.Android** - Open the project's **MainActivity.cs** and add the following lines below the existing `using` statements
* **Xamarin.Forms** - Open the project's **App.xaml.cs** and add the following lines below the existing `using` statements

```csharp
using Microsoft.AppCenter;
using Microsoft.AppCenter.Auth;
```

#### 2.2 Add the `Start()` method

To start App Center Auth, add the following code to your `Start()` method:

##### Xamarin.iOS

Open the project's **AppDelegate.cs** and add the `Start()` call inside the `FinishedLaunching()` method:

```csharp
AppCenter.Start("{Your Xamarin iOS App Secret}", typeof(Auth));
```

Be sure to replace `{Your App Secret}` in the code sample above with [your App Secret](~/dashboard/faq.md):
```csharp
AppCenter.Start("65dc3680-7325-4000-a0e7-dbd2276eafd1", typeof(Auth));
```

If you have already integrated other App Center modules, for example Data, it should look like the following code snippet:

```csharp
AppCenter.Start("65dc3680-7325-4000-a0e7-dbd2276eafd1", typeof(Analytics), typeof(Crashes), typeof(Auth), typeof(Data));
```

##### Xamarin.Android

Open the project's **MainActivity.cs** and add the `Start()` call inside the `OnCreate()` method:

```csharp
AppCenter.Start("{Your Xamarin Android App Secret}", typeof(Auth));
```

Be sure to replace `{Your App Secret}` in the code sample above with [your App Secret](~/dashboard/faq.md):
```csharp
AppCenter.Start("7433d0a8-3a21-49e4-8fca-f5eff43458df", typeof(Auth));
```

If you have already integrated other App Center modules, your call to start should look similar to the following code snippet.  This example integrates Analytics, Crashes, Auth, and Data:


```csharp
AppCenter.Start("7433d0a8-3a21-49e4-8fca-f5eff43458df", typeof(Analytics), typeof(Crashes), typeof(Auth), typeof(Data));
```

##### Xamarin.Forms

To create a Xamarin.Forms app targeting both Android and iOS platforms, you must create two apps in the App Center portal - one for each platform. Creating two apps will give you two App secrets - one for Android and another one for iOS. Open the project's **App.xaml.cs** (or your class that inherits from `Xamarin.Forms.Application`) in the shared or portable project and add the `Start()` call inside the `OnStart()` override method.

```csharp
AppCenter.Start("ios={Your Xamarin iOS App Secret};android={Your Xamarin Android App secret}", typeof(Auth));
```

Be sure to replace `{Your App Secret}` in the code sample above with [your App Secret](~/dashboard/faq.md):
```csharp
AppCenter.Start("ios=65dc3680-7325-4000-a0e7-dbd2276eafd1;android=7433d0a8-3a21-49e4-8fca-f5eff43458df", typeof(Auth));
```

If you have already integrated other App Center modules, for example Data, it should look like the following code snippet:

```csharp
AppCenter.Start("ios=65dc3680-7325-4000-a0e7-dbd2276eafd1;android=7433d0a8-3a21-49e4-8fca-f5eff43458df", typeof(Analytics), typeof(Crashes), typeof(Auth), typeof(Data));
```

### 3. Android additional steps

#### AndroidManifest.xml

To use the sign-in, you must add the following element to the project's **AndroidManifest.xml** file's `application` tag:

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data
            android:host="auth"
            android:scheme="msal{Your App Secret}" />
    </intent-filter>
</activity>
```

Make sure you replace `{Your App Secret}` in the code sample above with [your App Secret](~/dashboard/faq.md) (and remove the curly braces): `android:scheme="msal7433d0a8-3a21-49e4-8fca-f5eff43458df"`. The code snippet above would look like this: 

```xml
<activity android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />

        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <data
            android:host="auth"
            android:scheme="msal7433d0a8-3a21-49e4-8fca-f5eff43458df" />
    </intent-filter>
</activity>
```

#### Proguard

If you're using ProGuard, you must customize the project's configuration for Auth.

##### Create a ProGuard configuration file

> [!NOTE]
> If you don't use ProGuard or if you already have a ProGuard configuration file in your project, you may skip this section.

Add an empty file to your Xamarin.Android project named **proguard.cfg**. Set the build action to "ProguardConfiguration".

![proguard-configuration-build-action](../push/images/proguard-configuration-build-action.png)

##### Add customization to ProGuard configuration file

In your Xamarin.Android project, add the following line to the project's **proguard.cfg** file:

```text
-dontwarn com.nimbusds.jose.**
```

### 4. iOS additional steps

#### Modify the project's **Info.plist**

1. In the project's **Info.plist** file, right-click on the file and select **Open as...** > **Source code**.

> [!NOTE]
> If you are using the [VS for Mac IDE](https://visualstudio.microsoft.com/vs/mac/), right-clicking on the file and selecting **Open With...** does not show the option for **Source code**. In this case, you would have to open your **Info.plist** file in another text editor, for example, [Visual Studio Code](https://code.visualstudio.com/).

2. Copy and paste the following code:
 
```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal{APP_SECRET}</string>
        </array>
    </dict>
</array>
```

Replace `{APP_SECRET}` with [your actual App Secret](~/dashboard/faq.md). For example, if your app secret is `65dc3680-7325-4000-a0e7-dbd2276eafd1`, then it should look like `<string>msal65dc3680-7325-4000-a0e7-dbd2276eafd1</string>`.

Here's an example of the code snippet given that the app secret is `65dc3680-7325-4000-a0e7-dbd2276eafd1`:

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal65dc3680-7325-4000-a0e7-dbd2276eafd1</string>
        </array>
    </dict>
</array>
```

> [!NOTE]
> If you have already integrated other App Center SDKs, for example, the [Xamarin Distribute SDK](~/sdk/distribute/xamarin.md), then you may see an existing `string` for the `key`: `CFBundleURLSchemes`. For example, 
> ``` 
> <array>
>   <dict>
>       <key>CFBundleTypeRole</key>
>       <string>Editor</string>
>       <key>CFBundleURLName</key>
>       <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
>       <key>CFBundleURLSchemes</key>
>       <array>
>           <string>appcenter-889s4f4-9ac2-4e2e-ae54-dre54f2c6399</string>
>       </array>
>   </dict>
></array>
> ```
> 
> If so, add a new line `<string>msal{APP_SECRET}</string>` under the `key`: `CFBundleURLSchemes`. For example, given that your app secret is `65dc3680-7325-4000-a0e7-dbd2276eafd1`, then the code snippet will be:
> ``` 
> <array>
>   <dict>
>       <key>CFBundleTypeRole</key>
>       <string>Editor</string>
>       <key>CFBundleURLName</key>
>       <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
>       <key>CFBundleURLSchemes</key>
>       <array>
>           <string>msal65dc3680-7325-4000-a0e7-dbd2276eafd1</string>
>           <string>appcenter-889s4f4-9ac2-4e2e-ae54-dre54f2c6399</string>
>       </array>
>   </dict>
></array>
> ```

#### Add keychain sharing capability

> [!NOTE]
> You should enable Keychain sharing capability for your provisioning profile. See [Working with Capabilities in Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/deploy-test/provisioning/capabilities/?#devcenter) for a detailed guide.

App Center uses the `com.microsoft.adalcache` keychain access group to support single sign-on by enabling different apps to share keychain items (i.e. user secrets and credentials) with each other. We use this access group to securely share a user secret within a family of apps that rely on the same user secret. This way, logging into one of your apps will automatically grant the same user access to all of the family of apps.

You must add a new keychain group to your project Keychain Sharing Capabilities: `com.microsoft.adalcache`.

> [!NOTE]
> Please follow the [Working with entitlements in Xamarin.iOS](https://docs.microsoft.com/xamarin/ios/deploy-test/provisioning/entitlements) if you don't have an `Entitlements.plist` file for a step-by-step guide.

Open the project's `Entitlements.plist` and check **Enable Keychain Access Groups**. Then click **Add new Entry** and type `com.microsoft.adalcache` in the form.

![App Center Auth Keychain Capability](images/xamarin-entitlements.png)

## Sign users into the app

App Center provides a `SignInAsync()` method that triggers the sign in policy defined in your Azure AD B2C tenant.
To present the sign-in UI to the user, call the `SignInAsync` method:

```csharp
using Microsoft.AppCenter.Auth;

async Task SignInAsync()
{
    try
    {
        // Sign-in succeeded.
        UserInformation userInfo = await Auth.SignInAsync();
        string accountId = userInfo.AccountId;
    }
    catch (Exception e)
    {
        // Do something with sign-in failure.
    }
}
```

Please note the following:

* The Auth module must be started at the same time as other modules, in a `Start( ... )` call. If you decide to start the Auth service separately using `StartService()`, the app will not sign in automatically after the restart and `SignInAsync` must be called again.
* The app must initialize the App Center SDK using `AppCenter.Start` before calling `SignInAsync`.
* The app must be in the foreground before calling `SignInAsync`.
* App Center associates crash reports and handled errors with the signed in user after sign-in completes successfully.
* If sign-in fails, the crash reports and handled errors are not associated with any user.
* New push notifications targeting the signed-in user are received on the device that the user has signed into.
* Signing in on a device is not retroactive: the user does not receive push notifications that were sent to him prior to signing in on that device, and past error or crash reports are not updated with the new user information.
* The SDK automatically saves the signed in users' information so they do not have to sign in to your app again.
* If the app calls `SignInAsync` again, the SDK shows the sign-in UI again only if the saved sign-in information has expired or has been revoked by the authentication server.

## Get access token and ID token

When a user signs in to the application, the SDK exposes an ID token and an access token in the returned user information.

The tokens use the [JWT](https://jwt.io/) format.

An ID token represents the user information itself without any permission to call any other services' REST APIs.

The access token contains the same information as the ID token but also contains the scopes of what other services' REST APIs can be called on behalf of the user.

To access the tokens from the sign-in result:

```csharp
using Microsoft.AppCenter.Auth;

async Task SignInAsync()
{
    try
    {
        // Sign-in succeeded, UserInformation is not null.
        UserInformation userInfo = await Auth.SignInAsync();
        
        // Get tokens. They are not null.
        string idToken = userInfo.IdToken;
        string accessToken = userInfo.AccessToken;

        // Do work with either token.
    }
    catch (Exception e)
    {
        // Do something with sign-in failure.
    }
}
```

### Decoding tokens

You can decode user profile information such as the display name or the email address from the ID token or the access token. The SDK does not have APIs to directly expose user profile information, but this section will demonstrate how to decode the token.

Before decoding the token to get user profile information, the Azure AD B2C tenant must be configured to include the user profile fields in the tokens. By default, there is only metadata included in the token, and no user profile information.

To configure the list of user profile fields in the tokens, visit the tenant configuration on the Azure portal and select the user flow or custom policy that you've selected in the App Center Auth portal. If you are using a user flow, go to **Application claims** and select the user fields that need to be decoded, then click **Save** as illustrated in the following screenshot:

![Application Claims Settings](images/application-claims.png)

You also need to collect the user profile fields during the sign-up process so that they will be available in the tokens. On the user flow settings, go to **User attributes** and select the user fields, then click **Save** as illustrated in the following screenshot:

![User Attributes Settings](images/user-attributes.png)

If you are using a custom policy instead of a user flow, you can configure the claims as shown in the [XML configuration example](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-aad-custom#add-a-claims-provider) in the `OutputClaims` section.

> [!NOTE]
> Adding new user attributes will not update users that signed up before updating the settings.
> For existing users, the new selected fields will thus be missing from the tokens.

Once you have configured the tenant and the application has retrieved the ID token or the access token, you can decode the user profile information. Please see the example code snippets on how to decode the user profile information for **Display name** and **Email Addresses**:

```csharp
using System.Linq;
using System.IdentityModel.Tokens.Jwt;

// Decode the raw token string to read the claims.
var tokenHandler = new JwtSecurityTokenHandler();
try
{
    var jwToken = tokenHandler.ReadJwtToken(userInfo.IdToken);

    // Get display name.
    var displayName = jwToken.Claims.FirstOrDefault(t => t.Type == "name")?.Value;
    if (displayName != null)
    {
        // Do something with display name.
    }

    // Get first email address.
    var firstEmail = jwToken.Claims.FirstOrDefault(t => t.Type == "emails")?.Value;
    if (firstEmail != null)
    {
        // Do something with email.
    }
}
catch (ArgumentException)
{
    // Handle error.
}
```

The code sample requires the [JWT nuget package](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/). If your application uses Xamarin.Forms, the package needs to be installed in the .NET standard portable project and also the Android and iOS platform projects.

## Sign out

To sign out the user and clear all associated authentication tokens, call the `SignOut` method:

```csharp
using Microsoft.AppCenter.Auth;

Auth.SignOut();
```

## Enable or disable App Center Auth at runtime

As with every other App Center service, the app can disable the Auth service within the app. When disabled, the SDK signs the user out and ignores all other Auth method calls. In particular, `SignInAsync` will not do anything and no UI will be shown while Auth is disabled.

```csharp
Auth.SetEnabledAsync(false);
```

To enable App Center Auth again, use the same API but pass `true` as a parameter.

```csharp
Auth.SetEnabledAsync(true);
```

You don't have to await this call to make other API calls (such as `IsEnabledAsync`).

The module state is persisted across app launches.

> [!NOTE]
> This method must only be used after `Auth` has been started.

## Check if App Center Auth is enabled

You can also check whether App Center Auth is enabled or not:

```csharp
bool enabled = await Auth.IsEnabledAsync();
```

> [!NOTE]
> This method must only be used after `Auth` has been started, it will always return `false` before start.
