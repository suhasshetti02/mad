# CIE-1, CIE-2, and CIE-Improvement — Complete Solved Answers

*(RVCE, IS266TEO — Mobile Application Development, Faculty: RBS, 2025-26 even sem)*

---

# 📄 PAPER 1: CIE-1 (16/06/2026)

## Part B — Main Questions

### Q1.a (6 Marks) — Challenges of Android App Development

**"Android platform provides rich functionality for application development; there are still a number of challenges to address" — Justify with examples.**

Even though Android gives developers powerful APIs, building a good Android app is genuinely hard because of these challenges:

1. **Device Fragmentation** — thousands of different phone models exist, with different screen sizes, resolutions, and hardware capabilities. *Example:* a layout that looks perfect on a 6.5" phone may overlap or look cramped on a 5" phone or a tablet.
2. **OS Version Fragmentation** — not every user updates their Android version. *Example:* a new Camera API added in Android 14 won't work on a device still running Android 10, so developers must write extra fallback code.
3. **Performance Constraints** — limited RAM, CPU, and battery, especially on low-end/budget devices. *Example:* a poorly optimized image-loading routine can crash a low-RAM device even though it runs fine on a flagship phone.
4. **Security Concerns** — Android's open ecosystem (sideloading, multiple app stores) exposes it to more malware risk than closed ecosystems. *Example:* apps must carefully request only needed permissions to avoid misuse of user data.
5. **Testing Complexity** — an app must be tested across many device/OS/screen combinations to guarantee it works everywhere. *Example:* a bug that only appears on a specific manufacturer's custom Android skin (e.g., a Samsung One UI quirk).
6. **UI Consistency** — the same layout may render differently on a tablet vs. a small phone vs. a foldable device, requiring responsive design (e.g., using `ConstraintLayout` and multiple resource folders like `layout-sw600dp`).

*(Comments to add, as your paper's note requires: mention that Google tries to reduce fragmentation via things like Jetpack libraries, App Bundles, and API-level checks — this shows deeper understanding.)*

---

### Q1.b (4 Marks) — Dalvik VM vs Java VM

| Aspect | Dalvik Virtual Machine (DVM) | Java Virtual Machine (JVM) |
|---|---|---|
| Platform | Built specifically for Android | General-purpose, runs anywhere Java runs |
| Architecture | Register-based | Stack-based |
| Executes | `.dex` (Dalvik Executable) files | `.class` bytecode files |
| Optimization | Optimized for low memory & battery use | Not optimized for mobile constraints |
| Instance model | Each Android app gets its own DVM instance (sandboxing) | A single JVM instance can run multiple applications |
| Successor | Replaced by ART (Android Runtime) from Android 5.0 onward | N/A — still current for desktop/server Java |

**Comment:** DVM (and now ART) was purpose-built because a phone can't afford the memory/CPU overhead a full desktop JVM needs.

---

### Q2.a (5 Marks) — Need and Structure of AndroidManifest.xml

**Justification of need:** `AndroidManifest.xml` is mandatory because it is the single file Android reads *before* running any of your code, to know:
- What components exist (Activities, Services, Broadcast Receivers, Content Providers) — undeclared components cannot be launched, even if the Java class is perfectly written.
- What permissions the app needs (camera, internet, location, etc.).
- The package name (unique app identity), version info, and minimum/target SDK.
- Which Activity is the launcher (entry point) when the user taps the icon.

Without it, Android has no way to safely install or run the app — think of it as the app's **ID card + permission form** submitted to the OS.

**Basic structure:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">

        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".SecondActivity" />
        <service android:name=".MyService" />
        <receiver android:name=".MyReceiver" />

    </application>
</manifest>
```
- `<manifest package="...">` — root tag, declares the unique package name.
- `<uses-permission>` — one line per permission needed.
- `<application>` — wraps icon, label, theme, and all component declarations.
- `<activity>` with `<intent-filter>` (`MAIN`+`LAUNCHER`) — marks the entry-point screen.
- `<service>`, `<receiver>` — declare background components the same way.

---

### Q2.b (5 Marks) — Demonstrate the Android Studio Debugger

The debugger lets you pause a running app at a specific line to inspect what's happening, instead of guessing from Logcat alone. **Steps:**

1. **Set a breakpoint** — click the left gutter next to a line of code; a red dot appears.
2. **Run in Debug mode** — click the bug-shaped 🐞 icon instead of the normal Run ▶ button.
3. **Execution pauses automatically** the moment that line is about to execute.
4. **Inspect variables** — the "Variables" pane (bottom of the IDE) shows live values of every local/instance variable at that paused moment.
5. **Step through code:**
   - *Step Over (F8)* — runs the current line, moves to the next line in the same method.
   - *Step Into (F7)* — jumps inside a method that's being called on the current line.
   - *Step Out (Shift+F8)* — finishes the current method and returns to its caller.
6. **Evaluate Expression** — type any expression/variable name to check its value on the fly without editing code.
7. **Resume Program (F9)** — continues execution until the next breakpoint or the program ends.

```java
int total = a + b;   // <-- set a breakpoint here to inspect 'a' and 'b' before they combine
```

**Comment:** the debugger is essential for finding logic errors (wrong values) that don't cause a crash but still produce incorrect output — Logcat alone can't show you this without manually adding print statements everywhere.

---

### Q3 (10 Marks) — Activity Lifecycle (Java code + Diagram)

**Diagram (draw this):**
```
        [Activity Created]
              |
          onCreate()
              |
           onStart()  <------------------+
              |                          |
          onResume()                     |
              |                          |
      (Activity Running)             onRestart()
              |                          |
           onPause()                     |
              |                          |
           onStop()  ---------------------+
              |
          onDestroy()
              |
       [Activity Destroyed]
```

**Java code:**
```java
package com.example.lifecycledemo;

import android.os.Bundle;
import android.util.Log;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private static final String TAG = "LifecycleDemo";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Log.i(TAG, "onCreate() - Activity is being created");
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.i(TAG, "onStart() - Activity is now visible");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i(TAG, "onResume() - Activity is in the foreground and interactive");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.i(TAG, "onPause() - Activity is losing focus");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i(TAG, "onStop() - Activity is no longer visible");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        Log.i(TAG, "onRestart() - Activity is restarting after being stopped");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.i(TAG, "onDestroy() - Activity is being destroyed");
    }
}
```

**Explanation of each method:**
- `onCreate()` — called once, when the Activity is first created; initialize UI here via `setContentView()`.
- `onStart()` — Activity becomes visible to the user, but not yet interactive.
- `onResume()` — Activity is now in the foreground and the user can interact with it.
- `onPause()` — another Activity is partially covering this one; this one is losing focus (still partly visible).
- `onStop()` — Activity is completely hidden from view.
- `onRestart()` — called just before `onStart()` when returning from the Stopped state.
- `onDestroy()` — Activity is being completely removed from memory.

**Comment:** always call `super.methodName()` first in every override, or the app crashes with a `SuperNotCalledException`.

---

### Q4 (10 Marks) — Android Software Stack (Neat Block Diagram)

**Diagram:**
```
 -----------------------------------------------------
|   System Apps          |     User Apps               |
 -----------------------------------------------------
|                Java API Framework                     |
|   (Activity Manager, Window Manager, Content          |
|    Providers, Notification Manager, View System)      |
 -----------------------------------------------------
| Native C/C++ Libraries   |  Android Runtime (ART)     |
| (SQLite, WebKit, OpenGL, |  (Core Libraries + ART/    |
|  Media Framework, SSL)   |   Dalvik executes bytecode)|
 -----------------------------------------------------
|          Hardware Abstraction Layer (HAL)              |
|   (Camera HAL, Audio HAL, Bluetooth HAL, Sensors HAL)   |
 -----------------------------------------------------
|                    Linux Kernel                         |
|  (Device Drivers, Power Management, Memory Management,  |
|   Process Management, Security)                          |
 -----------------------------------------------------
```

**Explanation, layer by layer (bottom to top):**
1. **Linux Kernel** — foundation layer; manages hardware directly through device drivers, handles memory allocation, process scheduling, and security (permissions, sandboxing).
2. **Hardware Abstraction Layer (HAL)** — provides standard interfaces that expose device hardware capabilities to the higher Java API Framework, without the framework needing to know exact hardware details.
3. **Native C/C++ Libraries** — perform heavy-lifting tasks fast (e.g., SQLite for local databases, WebKit for rendering web content, OpenGL for graphics).
4. **Android Runtime (ART)** — executes the compiled app bytecode; replaced the older Dalvik VM from Android 5.0 onward, with ahead-of-time (AOT) compilation for better performance.
5. **Java API Framework** — the layer developers interact with directly through Android SDK classes like `Activity`, `Service`, `ContentProvider`.
6. **System Apps & User Apps** — pre-installed apps (Phone, Contacts, Settings) and user-installed apps (like your own project), built on top of the Framework APIs.

---

### Q5 (10 Marks) — What are Intents? Categories with Example Code (Java + XML)

**Definition:** An Intent is a messaging object used to request an action from another app component — most commonly to start an Activity, Service, or deliver a Broadcast. It can carry data via `putExtra()`.

**Two categories of Intents:**

**1. Explicit Intent** — names the exact target component class; used when you know precisely which Activity/Service should handle it (typically within your own app).

```java
// MainActivity.java
Button btnNext = findViewById(R.id.btnNext);
btnNext.setOnClickListener(v -> {
    Intent intent = new Intent(MainActivity.this, SecondActivity.class);
    intent.putExtra("message", "Hello from MainActivity");
    startActivity(intent);
});
```
```java
// SecondActivity.java
String message = getIntent().getStringExtra("message");
TextView tv = findViewById(R.id.tvMessage);
tv.setText(message);
```
```xml
<!-- activity_main.xml -->
<Button
    android:id="@+id/btnNext"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Go to Second Activity" />
```
```xml
<!-- AndroidManifest.xml -->
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".SecondActivity" />
```

**2. Implicit Intent** — only describes the *action* to perform; Android finds any installed app capable of handling it (shows a chooser if more than one qualifies).

```java
// Open a webpage
Intent webIntent = new Intent(Intent.ACTION_VIEW);
webIntent.setData(Uri.parse("https://www.rvce.edu.in"));
startActivity(webIntent);

// Open the dialer with a number pre-filled
Intent callIntent = new Intent(Intent.ACTION_DIAL);
callIntent.setData(Uri.parse("tel:9538860055"));
startActivity(callIntent);
```
```xml
<!-- No special manifest entry needed for ACTION_DIAL/ACTION_VIEW; only ACTION_CALL
     (placing the call directly) needs this permission: -->
<uses-permission android:name="android.permission.CALL_PHONE" />
```

**Comparison:**

| Explicit Intent | Implicit Intent |
|---|---|
| Names the exact target class | Only describes the action |
| Used within the same app | Used to invoke another app/component |
| No chooser dialog | May show a chooser if multiple apps match |
| Example: `new Intent(this, X.class)` | Example: `new Intent(Intent.ACTION_VIEW)` |

---

## QUIZ

**Q1 (2M) — Full form of APK and its importance**

APK = **Android Package Kit**. It's the single installable, zipped file format Android uses to distribute and install apps — containing compiled code (`classes.dex`), resources, and the manifest. Its importance: without an APK (or its modern equivalent, the AAB), an app cannot be installed or distributed on any Android device or the Play Store.

**Q2 (1M) — Fill in the blank**

Edit the **`AndroidManifest.xml`** file to add features, components, and permissions to your Android app.

**Q3 (2M) — Illustrate creating a virtual device (AVD/emulator)**

1. Open **Tools → Device Manager** in Android Studio.
2. Click **Create Device**.
3. Choose a hardware profile (e.g., Pixel 7) → Next.
4. Select a system image (Android version) to download/use → Next.
5. Name the AVD and click **Finish**.
6. Select this AVD from the device dropdown and click **Run ▶** to launch the app on it.

**Q4 (1M) — Latest release of Android version**

As of mid-2026, **Android 16** is the latest stable public release (available since June 2025), with **Android 17** having entered public beta in early 2026 and moving toward stable rollout around mid-2026. *(Double-check on the day of your exam/viva, since this changes — this is genuinely a moving target and your faculty may expect whichever version was current when the syllabus was last taught.)*

**Q5 (2M) — Categorize the contents of the `res` directory**

| Folder | Contents |
|---|---|
| `drawable/` | Images and shape/gradient XML drawables |
| `layout/` | Screen layout XML files |
| `values/` | Strings, colors, dimensions, styles/themes |
| `mipmap/` | Launcher icons (different densities) |
| `menu/` | Menu XML definitions |
| `anim/` / `animator/` | Animation resource files |
| `raw/` | Raw asset files (audio, misc files) accessed by resource ID |

**Q6 (2M) — Options for testing Android apps**

Developers can test apps on: **the Android Emulator (AVD)**, **a physical Android device connected via USB**, **third-party emulators (e.g., Genymotion)**, and **cloud-based device labs (e.g., Firebase Test Lab)**.

---

# 📄 PAPER 2: CIE-2 (22/05/2026)

## Part A — QUIZ

**Q1 (2M) — What are Services in Android? Name a few.**

A Service is an app component that performs long-running operations **in the background, without a user interface**, even if the user switches to a different app. Examples: a **music playback service**, a **file download service**, a **location-tracking service**, and a **data-sync service**.

**Q2 (2M) — Two rules for implementing Android UI Threads**

1. **Do not block the UI thread** — never perform long-running operations (network calls, database queries, heavy computation) directly on the main/UI thread, or the app will freeze and may show an "Application Not Responding" (ANR) error.
2. **Do not access the UI toolkit from outside the UI thread** — Views can only be safely created, read, or modified from the main thread; updating a `TextView` from a background thread directly can crash the app or cause undefined behaviour.

**Q3 (2M) — What is a Loader? Mention two types.**

A Loader is a component that loads data asynchronously for an Activity/Fragment, and — unlike a plain AsyncTask — **survives configuration changes** (like screen rotation) without restarting the load, and automatically re-queries if the underlying data changes.
Two types: **`CursorLoader`** (loads data from a `ContentProvider`/database as a `Cursor`) and **`AsyncTaskLoader`** (a more general-purpose loader for any type of background-loaded data).

**Q4 (2M) — Two third-party HTTP Client Connection Libraries**

**Retrofit** and **Volley** (also acceptable: **OkHttp**) — all are popular libraries used in Android apps to simplify making HTTP requests to REST APIs, instead of manually using `HttpURLConnection`.

**Q5 (2M) — What are system Broadcasts? Give examples.**

System Broadcasts are messages the Android OS itself sends out automatically when a system-wide event occurs, which any registered app can listen for. Examples: `ACTION_BOOT_COMPLETED` (device finished booting), `ACTION_POWER_CONNECTED` (charger plugged in), `ACTION_BATTERY_LOW` (battery low), `ACTION_AIRPLANE_MODE_CHANGED`.

---

## Part B Questions

### Q1.a (6 Marks) — Check Network Availability: Mobile or Wi-Fi

```java
public boolean isNetworkAvailable() {
    ConnectivityManager cm = (ConnectivityManager)
            getSystemService(Context.CONNECTIVITY_SERVICE);

    Network network = cm.getActiveNetwork();
    if (network == null) {
        return false;
    }

    NetworkCapabilities capabilities = cm.getNetworkCapabilities(network);
    if (capabilities == null) {
        return false;
    }

    if (capabilities.hasTransport(NetworkCapabilities.TRANSPORT_WIFI)) {
        Log.i("Network", "Connected via WiFi");
    } else if (capabilities.hasTransport(NetworkCapabilities.TRANSPORT_CELLULAR)) {
        Log.i("Network", "Connected via Mobile Data");
    }
    return true;
}
```
```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
**Explanation:** `ConnectivityManager` is the system service that reports connection state. `getActiveNetwork()` returns the currently active network (or `null` if offline). `getNetworkCapabilities()` gives details about it, and `hasTransport()` checks whether it's specifically Wi-Fi or Cellular.

---

### Q1.b (4 Marks) — Intent Services and Their Limitations

**Definition:** `IntentService` is a base class for a Service that handles asynchronous requests (Intents) one at a time on a single background worker thread, and **automatically stops itself** once all queued requests are processed — so you don't have to manually call `stopSelf()`.

```java
public class MyIntentService extends IntentService {
    public MyIntentService() {
        super("MyIntentService");
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        // background work happens here, one request at a time
    }
}
```

**Limitations:**
- Handles requests **sequentially, one at a time** — cannot process multiple requests in parallel, so it's unsuitable for tasks needing concurrency.
- **Deprecated since API level 30** — Google now recommends `WorkManager` or `JobIntentService` instead.
- Since it always runs on a single worker thread, a long-running task **blocks all other queued requests** until it finishes.
- No built-in way to easily communicate progress back to the UI (needs additional Broadcast/callback mechanisms).

---

### Q2 (10 Marks) — AsyncTask: Syntax and Callback Prototypes

**Syntax:**
```java
private class MyTask extends AsyncTask<Params, Progress, Result> {

    @Override
    protected void onPreExecute() {
        // runs on UI thread, BEFORE background work starts
    }

    @Override
    protected Result doInBackground(Params... params) {
        // runs on a BACKGROUND thread
        return result;
    }

    @Override
    protected void onProgressUpdate(Progress... values) {
        // runs on UI thread, whenever publishProgress() is called
    }

    @Override
    protected void onPostExecute(Result result) {
        // runs on UI thread, AFTER doInBackground() completes
    }
}
```

**Concrete working example:**
```java
private class DownloadTask extends AsyncTask<String, Integer, String> {

    @Override
    protected void onPreExecute() {
        progressBar.setVisibility(View.VISIBLE);
    }

    @Override
    protected String doInBackground(String... urls) {
        for (int i = 0; i <= 100; i += 20) {
            publishProgress(i);
            try {
                Thread.sleep(300);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        return "Download complete";
    }

    @Override
    protected void onProgressUpdate(Integer... values) {
        progressBar.setProgress(values[0]);
    }

    @Override
    protected void onPostExecute(String result) {
        progressBar.setVisibility(View.GONE);
        Toast.makeText(getApplicationContext(), result, Toast.LENGTH_SHORT).show();
    }
}

// Execution:
new DownloadTask().execute("https://example.com/file");
```

**Callback prototypes, explained:**
- `AsyncTask<Params, Progress, Result>` — three generic types: **Params** (input type passed into `execute()`), **Progress** (type used for progress updates), **Result** (type returned to `onPostExecute`).
- `onPreExecute()` — no parameters, no return value; runs on UI thread before background work.
- `doInBackground(Params... params)` — takes a variable number of `Params`-type arguments, returns a `Result`; the ONLY method that runs off the main thread.
- `onProgressUpdate(Progress... values)` — takes variable `Progress`-type values, called via `publishProgress()`.
- `onPostExecute(Result result)` — takes the single `Result` returned by `doInBackground`.
- `.execute(Params...)` — starts the task, passing arguments matching the `Params` type.

---

### Q3 (10 Marks) — Implement Broadcast Receiver: "Power Connected"

```java
// PowerReceiver.java
public class PowerReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "Power connected!", Toast.LENGTH_SHORT).show();
        Log.i("PowerReceiver", "Device is now charging");
    }
}
```
```java
// MainActivity.java - dynamic (in-code) registration
public class MainActivity extends AppCompatActivity {

    PowerReceiver powerReceiver = new PowerReceiver();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        IntentFilter filter = new IntentFilter(Intent.ACTION_POWER_CONNECTED);
        registerReceiver(powerReceiver, filter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(powerReceiver);
    }
}
```
```xml
<!-- AndroidManifest.xml -->
<!-- Note: ACTION_POWER_CONNECTED requires dynamic registration (in code) since
     Android 8.0 (API 26) as a battery-saving restriction on implicit broadcasts.
     No manifest <receiver> entry is required/effective for this specific action;
     only the Activity registering it dynamically (as shown above) is needed. -->
```
**Output:** As soon as the charger is plugged in, a Toast "Power connected!" appears and Logcat logs the event — even without the user touching the app.

**Explanation:**
- `extends BroadcastReceiver` and overriding `onReceive(Context, Intent)` is all that's required to define a receiver.
- `IntentFilter` describes exactly which broadcast action this receiver listens for.
- `registerReceiver()`/`unregisterReceiver()` must be paired — register in `onCreate()`, unregister in `onDestroy()` to avoid memory leaks.

---

### Q4 (10 Marks) — Lifecycle of Different Forms of Services

Android has three broad forms of Services: **Started**, **Bound**, and **Foreground**.

**1. Started Service Lifecycle**
```
   startService()
        |
    onCreate()   (only if not already created)
        |
  onStartCommand()  <-----+
        |                 |  (each new startService() call)
   [Service Running]------+
        |
   stopSelf() / stopService()
        |
    onDestroy()
```
- Started via `startService()`; runs independently of whoever started it.
- `onStartCommand()` is called every time `startService()` is invoked, even if already running.
- Keeps running until it explicitly stops itself (`stopSelf()`) or is stopped externally (`stopService()`).

**2. Bound Service Lifecycle**
```
   bindService()
        |
    onCreate()
        |
     onBind()  --------> [Client(s) interact via IBinder]
        |
   (all clients call unbindService())
        |
    onUnbind()
        |
    onDestroy()
```
- Started via `bindService()`; provides a client-server interface (`IBinder`) for ongoing interaction.
- Runs only as long as at least one client remains bound; destroyed once all clients unbind.

**3. Foreground Service Lifecycle**
```
   startForegroundService()
        |
    onCreate()
        |
  onStartCommand()  --> startForeground(id, notification)
        |
  [Running with a persistent visible notification]
        |
   stopForeground() / stopSelf()
        |
    onDestroy()
```
- Similar to a started Service, but must display an ongoing notification (via `startForeground()`) so the user is aware it's running — required for tasks like music playback or navigation to avoid being killed by the system.

**Comparison table:**

| | Started | Bound | Foreground |
|---|---|---|---|
| Started by | `startService()` | `bindService()` | `startForegroundService()` |
| Runs until | Explicitly stopped | All clients unbind | Explicitly stopped (with visible notification) |
| Key callback | `onStartCommand()` | `onBind()` | `onStartCommand()` + `startForeground()` |
| Example use | One-off background download | Music player controls (client interacts) | Navigation/music apps that must stay alive |

---

### Q5 (10 Marks) — Short Notes

**a. Publish and Polish**

Before publishing, an app should be **"polished"**: tested thoroughly across devices/screen sizes, debug `Log` statements removed, images/resources optimized for size, crashes and edge cases handled gracefully, and Material Design guidelines followed for a professional look.
**Publishing steps:** generate a signed release build (APK/AAB) using a securely stored keystore → create a Google Play Console developer account → create the app listing (title, description, screenshots, icon) → set content rating and add a privacy policy → choose a release track (Internal → Closed → Open → Production) → set pricing/distribution → submit for Google's review → monitor crash reports and reviews post-launch.

**b. Google Services**

Google Play Services is a background framework of APIs bundled on most Android devices that gives apps access to Google's ecosystem without needing to bundle the logic themselves — including **Google Maps**, **Firebase** (Realtime Database, Authentication, Cloud Messaging), **Google Sign-In**, **Location Services (FusedLocationProviderClient)**, and **AdMob** for advertising. Apps declare a dependency on `com.google.android.gms:play-services-*` in Gradle to use these.

**c. Permissions and Securities**

**Permission types:** Normal (auto-granted, e.g. `INTERNET`), Dangerous (needs runtime user approval, e.g. `CAMERA`, `ACCESS_FINE_LOCATION`), Signature (only granted to apps signed with the same certificate).
**Security best practices:** use HTTPS for all network calls; store sensitive data with `EncryptedSharedPreferences`; request only the minimum permissions needed; validate all user input; never hardcode API keys in source code; use ProGuard/R8 to obfuscate release code; sign the APK/AAB securely; keep dependencies updated.

---

# 📄 PAPER 3: CIE-Improvement (19/06/2026)

## Part A — QUIZ

**Q1 (2M) — What are Broadcasts? Give examples.**

A Broadcast is a system-wide message that Android (or an app) sends to announce that an event has occurred, which any registered component can listen for and react to. Examples: `ACTION_BOOT_COMPLETED`, `ACTION_POWER_CONNECTED`, `ACTION_BATTERY_LOW`, `ACTION_AIRPLANE_MODE_CHANGED`.

**Q2 (2M) — Different ways of sending Broadcasts**

- **`sendBroadcast(intent)`** — a normal broadcast, delivered to all registered receivers in an undefined order, asynchronously.
- **`sendOrderedBroadcast(intent, receiverPermission)`** — delivers to receivers one at a time, in priority order; each receiver can modify the result or **abort** the broadcast, stopping it from reaching lower-priority receivers.
- **`LocalBroadcastManager`** *(legacy)* — sends a broadcast only within the same app process, more efficient and secure since it never leaves the app (now generally replaced by other patterns like `LiveData`/`Flow`, but still asked conceptually).

**Q3 (2M) — Two types of Hierarchical Navigation**

Android's "Up" (hierarchical/ancestral) navigation has two forms:
1. **Navigating Up within the app's own task** — moving to a parent screen defined by the app's own structure, staying within the same app.
2. **Navigating Up across apps/tasks** — when your Activity was launched by another app via an implicit Intent, "Up" takes the user back to that originating app instead of your own app's hierarchy, using a synthetic back stack.

**Q4 (2M) — Examples of Mobile Application based Services (at least 4)**

1. **Location-tracking service** (e.g., ride-sharing apps tracking a driver's position)
2. **Music/media playback service** (e.g., Spotify continuing to play while you use other apps)
3. **Push notification service** (e.g., Firebase Cloud Messaging delivering chat notifications)
4. **Data sync/backup service** (e.g., syncing contacts or photos in the background)
5. *(bonus)* **Download manager service** (e.g., downloading a large file while the app is minimized)

**Q5 (2M) — Are simulators different from emulators? List two differences.**

Yes, they are different.

| Simulator | Emulator |
|---|---|
| Mimics only the software behaviour/logic of a device, not the actual hardware | Replicates both the hardware and software of a real device (full virtual device) |
| Faster and lighter, but less accurate for hardware-dependent testing | Slower and heavier, but far more accurate — e.g., Android's AVD is a true emulator |

---

## Part B Questions

### Q1.a (5 Marks) — Toast: Complete Specification of Parameters and Methods

**Definition:** A Toast is a small pop-up message shown briefly on screen, which disappears automatically without requiring user interaction.

```java
// Standard creation - 3 parameters required
Toast toast = Toast.makeText(getApplicationContext(), "Hello Android!", Toast.LENGTH_SHORT);
toast.show();

// Overload using a string resource instead of a literal string
Toast.makeText(this, R.string.press_button, Toast.LENGTH_LONG).show();

// Custom position
toast.setGravity(Gravity.CENTER, 0, 0);

// Custom view (advanced, deprecated in latest APIs but still asked conceptually)
Toast customToast = new Toast(getApplicationContext());
customToast.setView(myCustomLayoutView);
customToast.setDuration(Toast.LENGTH_LONG);
customToast.show();

// Cancel a Toast early
toast.cancel();
```

**Parameter/method specification:**
- `Toast.makeText(Context context, CharSequence text, int duration)` — builds (but doesn't display) the Toast; `context` is usually `this`/`getApplicationContext()`.
- `duration` — either `Toast.LENGTH_SHORT` (~2 seconds) or `Toast.LENGTH_LONG` (~3.5 seconds); no custom duration is allowed.
- `.show()` — actually displays it; without this call, nothing appears.
- `.setGravity(int gravity, int xOffset, int yOffset)` — repositions the Toast (default is bottom-center).
- `.setView(View view)` — (older API) sets a fully custom layout for the Toast instead of plain text.
- `.cancel()` — dismisses the Toast before its duration expires.

---

### Q1.b (5 Marks) — Importance of Mobile Applications Today + Reasons to Develop for Android

**Importance of mobile apps in today's scenario:**

Mobile phones have become the primary computing device for most people worldwide — used for communication, banking, shopping, education, entertainment, and work. Apps let businesses reach customers directly, instantly, and personally (via notifications, location awareness, camera/sensor access) in ways a website cannot match. The mobile-app economy also drives major job creation and revenue (app stores, in-app purchases, advertising, subscriptions).

**Reasons to develop specifically for Android:**

- **Largest global market share** — Android runs on the majority of smartphones worldwide, especially in developing markets, giving apps the widest possible reach.
- **Open-source and flexible** — developers have more freedom to customize UI/behaviour compared to more restrictive platforms.
- **Lower barrier to publishing** — Play Store registration is a one-time low fee, with a faster/simpler review process than some competitors.
- **Familiar language ecosystem** — built using Java/Kotlin, which many developers already know, and supported by mature tooling (Android Studio).
- **Wide device price range** — Android apps can reach both budget and flagship users, unlike platforms tied to premium-only hardware.
- **Strong Google ecosystem integration** — Maps, Firebase, Play Services provide ready-made powerful features.

---

### Q2.a (4 Marks) — Differentiate Between the Two Types of Intents

| Explicit Intent | Implicit Intent |
|---|---|
| Specifies the exact target component class | Only declares the action to perform |
| Constructor: `new Intent(context, TargetActivity.class)` | Constructor: `new Intent(Intent.ACTION_VIEW)` + `setData()` |
| Used mainly within the same app | Used to request another app/component to handle the task |
| No chooser shown | Chooser dialog shown if multiple apps can handle it |
| Example: opening your app's SecondActivity | Example: opening a webpage, dialer, or share sheet |

---

### Q2.b (6 Marks) — Java Code for Different Types of Intents with Examples

```java
// 1. Explicit Intent - open a specific Activity within the same app, with data
Intent explicitIntent = new Intent(MainActivity.this, SecondActivity.class);
explicitIntent.putExtra("username", "Rekha");
startActivity(explicitIntent);
```
```java
// 2. Implicit Intent - open a webpage (any browser app can handle it)
Intent viewIntent = new Intent(Intent.ACTION_VIEW);
viewIntent.setData(Uri.parse("https://www.rvce.edu.in"));
startActivity(viewIntent);
```
```java
// 3. Implicit Intent - open the dialer with a number pre-filled
Intent dialIntent = new Intent(Intent.ACTION_DIAL);
dialIntent.setData(Uri.parse("tel:9538860055"));
startActivity(dialIntent);
```
```java
// 4. Implicit Intent - share text with any capable app (e.g., WhatsApp, Email)
Intent shareIntent = new Intent(Intent.ACTION_SEND);
shareIntent.setType("text/plain");
shareIntent.putExtra(Intent.EXTRA_TEXT, "Check out this app!");
startActivity(Intent.createChooser(shareIntent, "Share via"));
```
**Explanation:** the explicit Intent names `SecondActivity.class` directly and attaches data with `putExtra()`. The implicit Intents instead specify a generic `action` constant (`ACTION_VIEW`, `ACTION_DIAL`, `ACTION_SEND`) plus relevant data/type, letting Android's system resolve which installed app should handle each request — `Intent.createChooser()` forces a chooser dialog to always appear for the share action.

---

### Q3 (10 Marks) — Broadcast Receiver: Notification on Power Connected

```java
// PowerReceiver.java
public class PowerReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        String channelId = "power_channel";
        NotificationManager manager = context.getSystemService(NotificationManager.class);

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            NotificationChannel channel = new NotificationChannel(
                    channelId, "Power Alerts", NotificationManager.IMPORTANCE_DEFAULT);
            manager.createNotificationChannel(channel);
        }

        Notification notification = new NotificationCompat.Builder(context, channelId)
                .setSmallIcon(R.drawable.ic_notification)
                .setContentTitle("Power Connected")
                .setContentText("Your device is now charging.")
                .setAutoCancel(true)
                .build();

        manager.notify(1, notification);
    }
}
```
```java
// MainActivity.java - register/unregister dynamically
public class MainActivity extends AppCompatActivity {

    PowerReceiver powerReceiver = new PowerReceiver();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        IntentFilter filter = new IntentFilter(Intent.ACTION_POWER_CONNECTED);
        registerReceiver(powerReceiver, filter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(powerReceiver);
    }
}
```
```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
<!-- (required on Android 13+ for showing notifications; also request it at runtime) -->
```
**Output:** When the charger is plugged in, a notification titled "Power Connected" with the text "Your device is now charging." appears in the status bar/notification tray.

*(This is the same Broadcast Receiver concept as CIE-2 Q3, extended here to show a Notification instead of a Toast, as this paper specifically asks for a "notification/message.")*

---

### Q4 (10 Marks) — Lifecycle of Different Forms of Services

*(Identical expected answer as CIE-2, Q4 above — Started, Bound, and Foreground Service lifecycles with diagrams. Reproduce all three diagrams and the comparison table shown there in your answer script for full 10 marks.)*

**1. Started Service Lifecycle**
```
   startService()
        |
    onCreate()   (only if not already created)
        |
  onStartCommand()  <-----+
        |                 |  (each new startService() call)
   [Service Running]------+
        |
   stopSelf() / stopService()
        |
    onDestroy()
```

**2. Bound Service Lifecycle**
```
   bindService()
        |
    onCreate()
        |
     onBind()  --------> [Client(s) interact via IBinder]
        |
   (all clients call unbindService())
        |
    onUnbind()
        |
    onDestroy()
```

**3. Foreground Service Lifecycle**
```
   startForegroundService()
        |
    onCreate()
        |
  onStartCommand()  --> startForeground(id, notification)
        |
  [Running with a persistent visible notification]
        |
   stopForeground() / stopSelf()
        |
    onDestroy()
```

**Comparison table:**

| | Started | Bound | Foreground |
|---|---|---|---|
| Started by | `startService()` | `bindService()` | `startForegroundService()` |
| Runs until | Explicitly stopped | All clients unbind | Explicitly stopped (with visible notification) |
| Key callback | `onStartCommand()` | `onBind()` | `onStartCommand()` + `startForeground()` |
| Example use | One-off background download | Music player controls (client interacts) | Navigation/music apps that must stay alive |

---

### Q5 (10 Marks) — Explain in Detail

**i. Firebase and AdMob**

**Firebase** is Google's mobile/web backend-as-a-service platform, providing ready-made cloud infrastructure so developers don't need to build their own servers. Key features include:
- **Realtime Database** — a cloud-hosted NoSQL JSON database that syncs data instantly across all connected clients, and works offline (syncing once reconnected).
- **Authentication** — ready-made sign-in (email/password, Google, phone OTP, etc.).
- **Cloud Messaging (FCM)** — push notifications.
- **Crashlytics** — crash reporting and analytics.

```java
DatabaseReference dbRef = FirebaseDatabase.getInstance().getReference("students");
dbRef.child("s1").setValue(new Student("Rekha", 90));   // write

dbRef.addValueEventListener(new ValueEventListener() {
    @Override
    public void onDataChange(@NonNull DataSnapshot snapshot) {
        for (DataSnapshot child : snapshot.getChildren()) {
            Student s = child.getValue(Student.class);
            Log.i("Firebase", s.name + " scored " + s.marks);
        }
    }
    @Override
    public void onCancelled(@NonNull DatabaseError error) {}
});
```

**AdMob** is Google's mobile advertising platform, letting developers display ads (banner, interstitial, rewarded) inside their apps to generate revenue.

**Connectivity between Firebase and AdMob:** An AdMob account can be linked to a Firebase project through the Firebase console. This enables **Google Analytics for Firebase** events to inform ad targeting and measurement — giving developers a single unified dashboard showing ad revenue *alongside* user engagement, retention, and conversion data, instead of needing two disconnected tools.

```java
MobileAds.initialize(this, initializationStatus -> {});
AdView adView = findViewById(R.id.adView);
adView.loadAd(new AdRequest.Builder().build());
```
```xml
<com.google.android.gms.ads.AdView
    android:id="@+id/adView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    ads:adSize="BANNER"
    ads:adUnitId="ca-app-pub-3940256099942544/6300978111" />
```

**ii. Polish and Publish**

**Polish** (preparing the app for release):
- Test thoroughly on multiple real devices and screen sizes.
- Remove debug `Log` statements and test/dummy data.
- Optimize image and resource sizes to reduce app size.
- Handle crashes, empty states, and edge cases gracefully.
- Follow Material Design guidelines for a professional, consistent UI.

**Publish** (releasing the app):
1. Generate a **signed release AAB/APK** (Build → Generate Signed Bundle/APK) using a securely stored keystore.
2. Create a **Google Play Console** developer account.
3. Create the app's store listing — title, description, screenshots, icon, feature graphic.
4. Complete the **content rating questionnaire** and add a **privacy policy** URL if the app collects user data.
5. Choose a **release track** — Internal testing → Closed testing → Open testing → Production (staged rollout recommended).
6. Set **pricing and country distribution**.
7. **Submit for review** — Google reviews before the app goes live.
8. **Monitor post-launch** — crash reports, user reviews, and push updates as needed.

---

# 📌 Quick Cross-Reference: Repeated Questions Across These 3 Papers

| Topic | Appeared in |
|---|---|
| Broadcast Receiver — Power Connected | CIE-2 (Q3) **and** CIE-Improvement (Q3) — near-guaranteed to reappear |
| Services lifecycle with diagrams | CIE-2 (Q4) **and** CIE-Improvement (Q4) — near-guaranteed to reappear |
| Types/categories of Intents | CIE-1 (Q5), CIE-Improvement (Q2a, Q2b) |
| Toast — full specification | CIE-Improvement (Q1a) |
| Firebase + AdMob, Polish + Publish | CIE-2 (Q5) **and** CIE-Improvement (Q5) |
| Activity Lifecycle | CIE-1 (Q3) |
| Software Stack diagram | CIE-1 (Q4) |

If your actual exam follows this same faculty's pattern, **Broadcast Receivers and Service lifecycles are your highest-value topics** — they've been repeated in back-to-back papers within the same semester.

---

*Compiled directly from the uploaded CIE-1 (16/06/2026), CIE-2 (22/05/2026), and CIE-Improvement (19/06/2026) papers, Faculty: RBS, IS266TEO — Mobile Application Development, RVCE, 2025-26 even semester.*
