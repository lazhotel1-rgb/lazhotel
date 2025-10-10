# LAZ Hotel Payment Deep Links

This repository contains payment success/failure pages that redirect users back to the LAZ Hotel app using deep links.

## 🚀 Setup Instructions

### Step 1: Enable GitHub Pages

1. Go to your repository: `https://github.com/lazhotel1-rgb/lazhotel`
2. Click on **Settings** tab
3. Scroll down to **Pages** section
4. Under **Source**, select **Deploy from a branch**
5. Choose **main** branch and **/ (root)** folder
6. Click **Save**

### Step 2: Upload Payment Pages

1. Create a folder called `payment-pages` in your repository root
2. Upload the following files:
   - `index.html` - Main payment processing page
   - `success.html` - Payment success page
   - `failed.html` - Payment failure page

### Step 3: Configure Your Flutter App

Update your `PayMongoService` to use these URLs:

```dart
'success_url': 'https://lazhotel1-rgb.github.io/lazhotel/payment-pages/success.html',
'cancel_url': 'https://lazhotel1-rgb.github.io/lazhotel/payment-pages/failed.html',
```

### Step 4: Add Deep Link Support to Flutter App

Add these dependencies to your `pubspec.yaml`:

```yaml
dependencies:
  app_links: ^3.4.5
  url_launcher: ^6.2.1
```

### Step 5: Configure Deep Links

#### For Android (`android/app/src/main/AndroidManifest.xml`):

```xml
<activity
    android:name=".MainActivity"
    android:exported="true"
    android:launchMode="singleTop"
    android:theme="@style/LaunchTheme">
    
    <!-- Deep link intent filter -->
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="lazhotel" />
    </intent-filter>
    
    <!-- Universal links -->
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="https"
              android:host="lazhotel1-rgb.github.io" />
    </intent-filter>
</activity>
```

#### For iOS (`ios/Runner/Info.plist`):

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLName</key>
        <string>lazhotel.deeplink</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>lazhotel</string>
        </array>
    </dict>
    <dict>
        <key>CFBundleURLName</key>
        <string>lazhotel.universal</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>https</string>
        </array>
    </dict>
</array>
```

## 🔗 Deep Link URLs

Your app will handle these deep link patterns:

- `lazhotel://payment-success?booking_id=123&amount=2500`
- `lazhotel://payment-failed?booking_id=123&error=insufficient_funds`
- `lazhotel://messages?payment_success=true`

## 📱 How It Works

1. **User initiates payment** → PayMongo checkout opens
2. **Payment completed** → Redirects to GitHub Pages success page
3. **GitHub Pages** → Shows success message and redirects to app via deep link
4. **App receives deep link** → Navigates to appropriate screen
5. **Booking created** → User sees confirmation

## 🎯 Benefits

- ✅ **Reliable**: GitHub Pages is stable and fast
- ✅ **Free**: No hosting costs
- ✅ **Customizable**: Full control over payment pages
- ✅ **Professional**: Branded payment experience
- ✅ **Deep Links**: Seamless app integration

## 🔧 Testing

Test your deep links:

1. **Android**: `adb shell am start -W -a android.intent.action.VIEW -d "lazhotel://payment-success?booking_id=test123" com.your.package.name`
2. **iOS**: Use Safari to navigate to `lazhotel://payment-success?booking_id=test123`

## 📞 Support

If you need help with the setup, check:
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Flutter Deep Links Guide](https://docs.flutter.dev/development/ui/navigation/deep-linking)
- [PayMongo Integration Docs](https://developers.paymongo.com/)
