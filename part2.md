# What are the two types of Intents?

- Explicit Intents
  - specify the exact component (class) to start
  - e.g. Intent(context, TargetActivity::class.java)
  - the system knows exactly which app and which activity is being launched.
- Implicit Intents
  - specify a general action and data (e.g. ACTION_VIEW).
  - The Android system asks the user or checks which apps are registered to handle that action

# Which type is generally more secure?

- Explicit Intents are significantly more secure. Because they target a specific component within your own application, there is no risk of a malicious third-party app intercepting the data or 'hijacking' the intent.

# What type of Intent is used in SecondFragment.kt?

```java
var intent = Intent(Intent.ACTION_VIEW)
intent.type = "text/giftcards_browse"
intent.data = Uri.parse("https://appsec.moyix.net/api/index")
```

- This is an implicit intent.
- You are asking the Android OS to find any app that claims it can handle ACTION_VIEW for the type text/giftcards_browse. A malicious app could easily register itself to intercept this request, steal the loggedInUser object (which likely contains your authentication token), and potentially log in as the user.

to fix (SecondFragment.kt):

```java
// Replace the old Intent block with this:
var intent = Intent(activity, ProductScrollingActivity::class.java)
intent.putExtra("User", loggedInUser)
startActivity(intent)
```

to fix (AndroidManifest.xml):

```xml
<activity
    android:name=".ProductScrollingActivity"
    android:exported="false" /> <activity
    android:name=".MainActivity"
    android:exported="true"> <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

you must "lock down" your Activities to prevent external apps from launching them. Since you now know that you only want your own code to launch these activities, you should set exported="false" for everything that isn't the main entry point.

# What type is used in ThirdFragment.kt?

```java
var intent = Intent(activity, ProductScrollingActivity::class.java)
```

- This is an Explicit Intent.
- You are telling Android exactly which class (ProductScrollingActivity) in your own app should handle this. No other app can intercept this request.

# Which Intent is considered the "proper" implementation?

- The "proper" way is Explicit Intents. In secure Android development, this means targeting your own components directly to ensure that sensitive data (like loggedInUser) is never handed off to an unknown, third-party component.

# What does "proper" even mean in the context of secure Android development?

- If you don't need to communicate with the outside world, you should use an Explicit Intent so that only your code can resolve the intent.

# Shutting out the world

```XML
        <activity
            android:name=".UseCard"
            android:exported="false" />

        <activity
            android:name=".GetCard"
            android:exported="false" />

        <activity
            android:name=".ProductScrollingActivity"
            android:exported="false" />

        <activity
            android:name=".CardScrollingActivity"
            android:exported="false" />

        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

- If exported="true": Any app on the device can start this activity. If that activity handles sensitive data (like a User object or a specific card ID), an attacker can craft an intent to launch it directly, potentially causing your app to crash or perform unauthorized actions.

- If exported="false": Only your app's package (and the Android system itself) can launch the activity. This effectively isolates your internal logic from malicious interference.
