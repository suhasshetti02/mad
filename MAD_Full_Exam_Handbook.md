# Mobile Application Development — Complete Exam Preparation Handbook
### RVCE Syllabus, Units 1–5 | Coding Question Bank with Full Explanations

Compiled from CIE-1/2/3 papers, Test papers, Quizzes, Open-Book papers, and the Unit 1–5 possible-coding-questions notes (2018–2026, including the most recent CIE_26 / IS266TEO pattern).

**How the star rating works:**

| Stars | Meaning |
|---|---|
| ⭐⭐⭐⭐⭐ | Must Study — appears almost every year, in multiple paper types |
| ⭐⭐⭐⭐ | Very Important — appears in most years |
| ⭐⭐⭐ | Important — appears regularly, sometimes as a sub-part |
| ⭐⭐ | Good to Know — appears occasionally, quiz/short-answer style |

---

# UNIT 1: Introduction to Android — Basics, Resources, Manifest, Debugging

*(Syllabus: Smartphone OS, Installing Android Studio, creating a project, UI Design, Activities and Intents, Activity Lifecycle, Debugger, Testing, Android Support Library)*

## Q1.1 — Activity Lifecycle (Java code + diagram)
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
This is the single most repeated question across every CIE-1 paper from 2018 to 2025-26 (RBS faculty paper, 18MIT3E1 papers, 18G6E10 papers, 12IS6D4 papers — all of them). It is asked almost every semester, worth 5–10 marks, and is also a common viva question.

**Concept explanation in simple words:**
An Android Activity is a single screen of your app. As the user opens, minimizes, or closes your app, the Activity moves through different "states" — like a traffic light changing colours. Android calls specific methods automatically at each state change so you can respond (save data, release resources, restart something). You don't call these methods yourself — the Android system calls them for you.

**Complete answer:**
The seven lifecycle methods, in the order they normally happen:

1. `onCreate()` — called once when the Activity is first created. Initialize UI here.
2. `onStart()` — Activity becomes visible to the user.
3. `onResume()` — Activity comes to the foreground and gets user focus (interactive now).
4. `onPause()` — another Activity is partially covering this one; Activity is losing focus.
5. `onStop()` — Activity is no longer visible at all.
6. `onRestart()` — called just before `onStart()` when coming back from a stopped state.
7. `onDestroy()` — Activity is being completely removed from memory.

**Complete code:**
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
        Log.i(TAG, "onCreate() called - Activity is being created");
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.i(TAG, "onStart() called - Activity is now visible");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.i(TAG, "onResume() called - Activity is now in the foreground");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.i(TAG, "onPause() called - Activity is losing focus");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.i(TAG, "onStop() called - Activity is no longer visible");
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        Log.i(TAG, "onRestart() called - Activity is restarting after being stopped");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.i(TAG, "onDestroy() called - Activity is being destroyed");
    }
}
```

**XML code (if needed):**
```xml
<!-- res/layout/activity_main.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Check Logcat for Lifecycle logs"
        android:textSize="18sp" />
</LinearLayout>
```

**AndroidManifest code:** Not required beyond the default `<activity android:name=".MainActivity">` entry that Android Studio creates automatically.

**Output / result:**
When you run the app, open **Logcat**, filter by tag `LifecycleDemo`. You will see:
```
onCreate() called - Activity is being created
onStart() called - Activity is now visible
onResume() called - Activity is now in the foreground
```
Press the Home button, and you'll see `onPause()` then `onStop()`. Reopen the app, and you'll see `onRestart()` → `onStart()` → `onResume()`. Press Back, and you'll see `onPause()` → `onStop()` → `onDestroy()`.

**Line-by-line explanation of the code:**
- `extends AppCompatActivity` — makes this class an Activity so it can use lifecycle methods and show a UI.
- `private static final String TAG = "LifecycleDemo";` — a constant string used to filter logs easily in Logcat.
- `@Override` — tells Java we are intentionally replacing (overriding) a method already defined in the parent `AppCompatActivity` class.
- `super.onCreate(savedInstanceState);` — **always call the super method first** so Android's own internal setup still happens; skipping this crashes the app.
- `setContentView(R.layout.activity_main);` — connects this Activity's Java code to its XML layout file so the UI appears on screen. This line only belongs in `onCreate()`.
- `Log.i(TAG, "message")` — prints an "info" level log message tagged with `TAG`, visible in the Logcat window; used here just to prove which method ran.
- Each of the six other overridden methods follows the exact same pattern: call `super.method()` first, then log a message.

**Viva / oral exam questions:**
- Why must you always call `super.onCreate()` first?
- What is the difference between `onPause()` and `onStop()`?
- Which method is the best place to release a camera or GPS resource, and why?
- What happens to an Activity's memory if the system needs resources while it is in the Stopped state?
- Is `onRestart()` called the first time an app opens? Why or why not?

**Common mistakes students make:**
- Forgetting to call `super.methodName()` — this causes a runtime crash (`SuperNotCalledException`).
- Putting `setContentView()` inside a method other than `onCreate()`.
- Confusing `onPause()` (still partially visible) with `onStop()` (completely invisible).
- Thinking `onDestroy()` is always guaranteed to be called (it may not be, if the system kills the process abruptly).

**Memory trick / quick revision point:**
Remember the order as: **"Create – Start – Resume – Pause – Stop – Restart – Destroy"** → first letters **C-S-R-P-S-R-D**. Think of it like a movie: **C**reate the set, **S**tart the show, **R**esume when the camera rolls, **P**ause for a break, **S**top the show, **R**estart if reopened, **D**estroy the set when done forever.

---

## Q1.2 — AndroidManifest.xml: Need & Basic Structure
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
Asked directly in the 2025-26 CIE-1 (RBS) paper as a 5-mark question, and in nearly every quiz as a fill-in-the-blank ("Edit the ______ file to add permissions...").

**Concept explanation in simple words:**
Think of `AndroidManifest.xml` as your app's **ID card + permission slip**, submitted to the Android operating system before your app is even allowed to run. It tells Android: what the app is called, what screens (Activities) it has, what permissions it needs (camera, internet, etc.), and which screen should open first.

**Complete answer:**
The manifest is essential because:
- It declares all app components: Activities, Services, Broadcast Receivers, Content Providers.
- It declares permissions the app needs (Internet, Camera, Location, etc.).
- It defines the app's package name (unique identity).
- It sets the minimum and target SDK versions.
- Without a correctly declared component in the manifest, Android will refuse to launch it — you'll get a runtime crash (`ActivityNotFoundException`) even if the Java/Kotlin code is perfect.

**Complete code / structure:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

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

        <receiver android:name=".MyBroadcastReceiver" />

    </application>
</manifest>
```

**Output / result:** There is no visual "output" — this file is metadata read by the Android OS at install time and app-launch time. If it is missing a declared Activity, launching that Activity throws:
`android.content.ActivityNotFoundException: Unable to find explicit activity class`

**Line-by-line explanation of the code:**
- `<manifest ... package="com.example.myapp">` — root element; `package` is the unique app identifier used on the Play Store.
- `<uses-permission android:name="..."/>` — one line per permission the app needs; must be declared here even before asking the user at runtime.
- `<application ...>` — wraps all components; `android:icon`, `android:label`, `android:theme` control the app's launcher icon, name, and visual style.
- `<activity android:name=".MainActivity">` — declares an Activity class; the leading dot means "in this app's package".
- `<intent-filter>` with `action MAIN` and `category LAUNCHER` — marks this Activity as the one that opens when the user taps the app icon (the "entry point").
- `<service>` and `<receiver>` — declare a background Service and a BroadcastReceiver the same way Activities are declared.

**Viva / oral exam questions:**
- What happens if you forget to declare an Activity in the manifest?
- Where do you declare a permission your app needs?
- What is the purpose of the `intent-filter` with `MAIN`/`LAUNCHER`?
- What is the difference between `minSdkVersion` and `targetSdkVersion`?

**Common mistakes students make:**
- Forgetting to add a new Activity to the manifest after creating it in Java (very common — Android Studio does this automatically only if you use the "New > Activity" wizard).
- Misspelling the permission string (must match exactly, e.g. `android.permission.CAMERA`).
- Declaring two Activities with `MAIN`/`LAUNCHER` intent filters (causes two icons or confusion).

**Memory trick / quick revision point:**
**"Manifest = Menu card of the app"** — just like a restaurant menu lists every dish (Activities/Services), ingredients needed (permissions), and the "chef's special" (launcher Activity), the manifest lists everything Android needs to know before serving your app.

---

## Q1.3 — Android Software Stack / Architecture (Neat Block Diagram)
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
Asked as a full 10-mark question in the 2025-26 CIE-1, the 2021-22 CIE-1, and the 18MIT3E1 Test-1 papers — always with "neat diagram" explicitly demanded.

**Concept explanation in simple words:**
Android is built like a **5-layer cake**. Each layer only talks to the layer directly below or above it. The bottom layer touches the hardware; the top layer is what the user actually sees and taps.

**Complete answer (layers bottom to top):**

| Layer | Contains | Example |
|---|---|---|
| 1. Linux Kernel | Device drivers, memory & process management, security | Camera driver, WiFi driver |
| 2. Hardware Abstraction Layer (HAL) | Standard interfaces exposing device hardware to the framework | Camera HAL, Audio HAL |
| 3. Native C/C++ Libraries & Android Runtime | Core libraries + ART/Dalvik runtime that executes app bytecode | SQLite, WebKit, OpenGL, ART |
| 4. Java API Framework | Managers that developers directly use via SDK | Activity Manager, Window Manager, Notification Manager |
| 5. System Apps & User Apps | Pre-installed and user-installed apps | Phone, Contacts, your app |

**Diagram to draw (describe as boxes stacked bottom-to-top):**
```
 ---------------------------------------------------
|  System Apps          |   User Apps               |
 ---------------------------------------------------
|              Java API Framework                    |
 ---------------------------------------------------
| Native C/C++ Libraries |   Android Runtime (ART)   |
 ---------------------------------------------------
|         Hardware Abstraction Layer (HAL)            |
 ---------------------------------------------------
|                 Linux Kernel                        |
 ---------------------------------------------------
```

**Output / result:** N/A (theory diagram question) — draw the 5 boxes exactly as above and label 2 examples per layer for full marks.

**Line-by-line explanation (of the diagram logic, since there's no code):**
- The **Linux Kernel** is the foundation — it directly manages hardware and provides core OS services like memory allocation and process scheduling.
- The **HAL** sits right above the kernel, converting hardware-specific instructions into a standard format the upper layers can use, without upper layers needing to know the exact hardware model.
- **Native Libraries** (written in C/C++) do heavy-lifting tasks (graphics, database, web rendering) fast; the **Android Runtime** next to it executes your compiled app code.
- The **Java API Framework** is what you, the developer, interact with directly through classes like `Activity`, `Service`, `ContentProvider`.
- **Apps** sit on top, built using the Framework layer's APIs.

**Viva / oral exam questions:**
- Why is HAL needed between the Kernel and the Native Libraries?
- Name two examples of Native Libraries and what each does.
- What replaced Dalvik in newer Android versions, and why?
- Which layer would you say `Activity Manager` belongs to?

**Common mistakes students make:**
- Drawing the layers in the wrong order (many students put Framework below Native Libraries).
- Forgetting to label examples inside each layer (loses marks even if the diagram shape is right).
- Confusing HAL with the Kernel — they are separate layers.

**Memory trick / quick revision point:**
**"LHNFA"** — **L**inux Kernel, **H**AL, **N**ative libraries+runtime, **F**ramework, **A**pps — read bottom to top like climbing a ladder.

---

## Q1.4 — Dalvik Virtual Machine (DVM) vs Java Virtual Machine (JVM)
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
Appears as a direct 4-mark comparison question in the 2025-26 CIE-1, the 2022 CIE-1, and multiple quizzes ("What was the main reason for replacing Java VM with Dalvik VM?").

**Concept explanation in simple words:**
JVM runs regular Java programs on desktops/servers. DVM (and its successor ART) is Android's own special version, redesigned to run efficiently on phones with limited battery and memory.

**Complete answer (comparison table):**

| Aspect | Dalvik Virtual Machine (DVM) | Java Virtual Machine (JVM) |
|---|---|---|
| Platform | Designed specifically for Android | General-purpose Java applications |
| Architecture | Register-based | Stack-based |
| Executes | `.dex` (Dalvik Executable) files | `.class` (bytecode) files |
| Optimization | Optimized for low memory & battery | Not mobile-optimized |
| Performance | Faster (fewer instructions) | Comparatively slower |
| Memory usage | More efficient | Higher memory consumption |
| Instance handling | Each Android app runs its own DVM instance | JVM can run multiple applications in a single instance |

**Complete code:** Not applicable — this is a conceptual/comparison question, no code is required. (If your paper asks "what converts Java bytecode to Dalvik bytecode?" the answer is the **`dx` / d8 compiler**, which produces the `.dex` file from `.class` files.)

**Output / result:** N/A.

**Line-by-line explanation:** N/A (theory).

**Viva / oral exam questions:**
- Why was Dalvik VM introduced instead of using JVM directly?
- What replaced Dalvik in Android 5.0+ and what's the key improvement?
- What is a `.dex` file?

**Common mistakes students make:**
- Saying DVM and ART are the same thing (ART = Android Runtime, the newer replacement for Dalvik from Lollipop onward — mention this if your syllabus year covers it).
- Not knowing that each app gets its own separate DVM instance (for sandboxing/security).

**Memory trick / quick revision point:**
**"D for Device, J for Just-anywhere"** — DVM is built specifically for the Android **D**evice; JVM runs **J**ust about anywhere Java is installed.

---

## Q1.5 — Display a Toast Message
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
A Toast is the simplest possible "show something on screen" code and appears embedded inside almost every application-level question (scoring apps, login apps, etc.), plus is directly asked ("illustrate the working of toast with complete specification of parameters and methods").

**Concept explanation in simple words:**
A Toast is a small pop-up message that appears briefly at the bottom of the screen and disappears on its own — used to show quick feedback like "Login successful" without needing the user to press OK.

**Complete answer:**
`Toast.makeText(context, message, duration).show()` is the standard one-line way. `context` is usually `this` or `getApplicationContext()`; `duration` is either `Toast.LENGTH_SHORT` (~2 sec) or `Toast.LENGTH_LONG` (~3.5 sec).

**Complete code:**
```java
package com.example.toastdemo;

import android.os.Bundle;
import android.view.Gravity;
import android.widget.Button;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btnToast = findViewById(R.id.btnToast);
        btnToast.setOnClickListener(v -> {
            Toast toast = Toast.makeText(getApplicationContext(),
                    "Hello Android!", Toast.LENGTH_SHORT);
            toast.setGravity(Gravity.CENTER, 0, 0);
            toast.show();
        });
    }
}
```

**XML code:**
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <Button
        android:id="@+id/btnToast"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show Toast" />
</LinearLayout>
```

**Output / result:** When the button is tapped, a small grey rounded pop-up saying "Hello Android!" appears in the centre of the screen for about 2 seconds, then fades away automatically.

**Line-by-line explanation of the code:**
- `Button btnToast = findViewById(R.id.btnToast);` — connects the Java variable to the XML button using its `id`.
- `btnToast.setOnClickListener(v -> {...});` — registers a lambda function to run when the button is clicked.
- `Toast.makeText(context, message, duration)` — creates (but does not yet show) the Toast object; three parameters required.
- `toast.setGravity(Gravity.CENTER, 0, 0);` — optional: repositions the Toast (default position is bottom of screen); the two `0,0` are x/y pixel offsets.
- `toast.show();` — actually displays the Toast on screen; forgetting this line means nothing appears even though the object was created.

**Viva / oral exam questions:**
- What are the two duration constants for Toast, and roughly how long is each?
- Can a Toast be clicked or interacted with?
- What's the difference between using `this` and `getApplicationContext()` as the context?

**Common mistakes students make:**
- Forgetting to call `.show()` after `makeText()` — the most common Toast mistake.
- Passing a `null` context.
- Trying to update the same Toast object repeatedly without calling `show()` again — causes stacked/laggy toasts (fix: cancel or reuse one Toast variable).

**Memory trick / quick revision point:**
**"Make, then Show"** — always remember Toast has 2 steps: `makeText()` builds it, `.show()` displays it. No show, no toast!

---

## Q1.6 — `findViewById()` and `Log.i()` Usage
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
Directly asked as fill-in-the-blank in almost every CIE-1 Part-A ("______ method is used to access a view element..."), and required as a supporting skill in nearly every coding question in the paper.

**Concept explanation in simple words:**
`findViewById()` is how your Java code "finds" a UI element you drew in the XML layout, using its unique `id`, so you can read or change it. `Log.i()` prints debug messages you can see in the Logcat window while testing.

**Complete answer + code:**
```java
TextView tv = findViewById(R.id.textView1);
tv.setText("Updated from Java code");

Log.i("MainActivity", "MainActivity layout is complete");
Log.d("MainActivity", "This is a debug-level message");
Log.e("MainActivity", "This is an error-level message");
```

**XML code:**
```xml
<TextView
    android:id="@+id/textView1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Original Text" />
```

**Output / result:** The TextView on screen changes from "Original Text" to "Updated from Java code"; three lines appear in Logcat tagged `MainActivity` at info/debug/error levels (info is white, debug is blue, error is red in Logcat's default colour scheme).

**Line-by-line explanation of the code:**
- `TextView tv = findViewById(R.id.textView1);` — `R.id.textView1` is an auto-generated constant referring to the XML element with `android:id="@+id/textView1"`; the method returns a `View` which we store as `TextView` type.
- `tv.setText(...)` — changes the visible text of that TextView at runtime.
- `Log.i(tag, message)` — `i` = info level; `tag` is usually the class name, used to filter logs.

**Viva / oral exam questions:**
- What does the `@+id/` prefix mean in XML, versus `@id/`?
- What are the different Log levels and their typical colour codes in Logcat?
- What happens if you call `findViewById()` before `setContentView()`?

**Common mistakes students make:**
- Calling `findViewById()` before `setContentView()` — returns `null`, causing a `NullPointerException`.
- Misspelling the `id` between XML and Java.
- Forgetting to cast to the correct View type (mismatched types cause `ClassCastException`).

**Memory trick / quick revision point:**
**"Find before you Bind"** — always call `setContentView()` first, THEN `findViewById()`, because Java can only find a view that's already inflated.

---

## Q1.7 — Create and Reference a String Resource (`strings.xml`)
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
Repeatedly asked ("Justify why string resources are recommended instead of hardcoding" + "add a new string named X with value Y") in almost every CIE-1 Part-A across all years.

**Concept explanation in simple words:**
Instead of typing text directly inside a layout (`android:text="Submit"`), you store it once in a central file called `strings.xml` and refer to it by name everywhere. This makes translation to other languages and app-wide text changes much easier.

**Complete answer:**
"It is recommended to create string resources instead of hardcoding text directly into layouts" because string resources are reusable across multiple screens/activities, support easy localization (translation) without touching layout files, and centralize all text for easy editing.

**Complete code:**
```xml
<!-- res/values/strings.xml -->
<resources>
    <string name="app_name">MyApp</string>
    <string name="press_button">Android ISE!</string>
    <string name="edit_message">Enter a message</string>
</resources>
```
```xml
<!-- Reference inside a layout XML -->
<Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/press_button" />
```
```java
// Reference inside Java code
String message = getString(R.string.press_button);
```

**Output / result:** The Button displays "Android ISE!" on screen; `message` in Java holds the string `"Android ISE!"`.

**Line-by-line explanation of the code:**
- `<string name="press_button">Android ISE!</string>` — declares a string resource with key `press_button` and value `Android ISE!`.
- `android:text="@string/press_button"` — the `@string/` prefix tells the XML parser to look up the value from `strings.xml` instead of using literal text.
- `getString(R.string.press_button)` — the Java-side equivalent lookup, returning a `String` object.

**Viva / oral exam questions:**
- Why shouldn't you hardcode text directly in a layout XML?
- What is the file path where `strings.xml` is stored?
- How would you support multiple languages using string resources?

**Common mistakes students make:**
- Typo in the resource `name` attribute causing `R.string.xxx` to fail to resolve (red underline in Android Studio).
- Forgetting the `@string/` prefix and typing plain text instead.
- Using `getResources().getString()` when just `getString()` (inherited from Context/Activity) is enough.

**Memory trick / quick revision point:**
**"@string/ = @ means 'look elsewhere'"** — the `@` symbol in Android XML always means "go fetch this value from a resource file", not use it literally.

---

## Q1.8 — Demonstrate the Android Studio Debugger
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Directly asked as a 5-mark question in the 2025-26 CIE-1 (RBS) paper: "Demonstrate the Android studio debugger."

**Concept explanation in simple words:**
The debugger lets you "pause" your running app at a specific line of code and inspect what's happening inside (variable values, which method is executing) instead of guessing from Logcat messages alone.

**Complete answer (steps):**
1. **Set a breakpoint** — click the left gutter next to a code line; a red dot appears.
2. **Run in Debug mode** — click the bug-shaped icon instead of the normal Run (▶) button.
3. **Execution pauses** at the breakpoint automatically when that line is about to run.
4. **Inspect variables** — the Variables window shows live values of all local/instance variables.
5. **Step through code** — use *Step Over (F8)* to run the current line and move to the next; *Step Into (F7)* to go inside a called method; *Step Out (Shift+F8)* to exit the current method.
6. **Evaluate Expression** — type any expression to check its value on the fly, without changing code.
7. **Resume Program (F9)** — continues running until the next breakpoint or program end.

**Complete code:** No code required — this is a tool-usage/IDE question, but you can mention adding a breakpoint inside a method like:
```java
int total = a + b;   // <-- put breakpoint here to inspect 'a' and 'b'
```

**Output / result:** The app pauses mid-execution at the breakpoint line; the Debug tool window at the bottom shows the call stack and current variable values.

**Line-by-line explanation:** N/A — this is a step-by-step procedure, not code, so explain each step as shown above.

**Viva / oral exam questions:**
- What is the difference between Step Over and Step Into?
- Why would you use "Evaluate Expression" instead of adding a `Log.i()` line?
- How do you remove a breakpoint?

**Common mistakes students make:**
- Running the app normally (▶) instead of in Debug mode (🐞), so breakpoints are ignored.
- Confusing Step Into (goes inside a method call) with Step Over (skips over it).
- Forgetting to click Resume, leaving the app stuck/frozen at the breakpoint during a demo.

**Memory trick / quick revision point:**
**"Bug icon = Pause button for code"** — remember the debugger's icon is a literal insect (🐞) because you're "catching bugs" by pausing execution.

---

## Q1.9 — APK Components & Challenges of Android App Development
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
"What is APK? List its components" and "list the challenges of Android app development" are recurring quiz/short-answer questions across almost every paper set (2025-26 CIE-1, older Test-1 papers).

**Concept explanation in simple words:**
An APK is the single installable file for an Android app — like a zip folder containing everything needed to run it. The "challenges" question asks you to explain why building apps for so many different Android phones is genuinely hard.

**Complete answer:**
**APK components:**
- `classes.dex` — compiled Java/Kotlin bytecode
- `AndroidManifest.xml` — compiled manifest
- `res/` — compiled resources (images, layouts, values)
- `assets/` — raw files bundled as-is
- `lib/` — native (.so) libraries
- `META-INF/` — signing certificate info

**Challenges of Android development (with examples):**
- **Device fragmentation** — many screen sizes/resolutions (an app may look perfect on one phone, broken on another).
- **OS version fragmentation** — not all users update their Android version, so newer APIs may not work everywhere.
- **Performance constraints** — limited RAM/CPU/battery on low-end devices.
- **Security concerns** — Android's openness makes it more exposed to malware if data isn't handled carefully.
- **Testing complexity** — need to test across many device/OS combinations.
- **UI consistency** — same layout may look different on a tablet vs. a small phone.

**Complete code:** Not applicable (theory question).

**Output / result:** N/A.

**Line-by-line explanation:** N/A.

**Viva / oral exam questions:**
- What is inside a `.dex` file?
- Give one real example of a problem caused by device fragmentation.
- Why is Android considered "more open" than iOS, and how does that create a security challenge?

**Common mistakes students make:**
- Only naming 2–3 challenges instead of the expected 5–6 with examples (loses marks on a 6-mark question).
- Confusing APK components with Manifest contents.

**Memory trick / quick revision point:**
**"DAPS-T-U"** for challenges: **D**evice fragmentation, **A**pp compatibility/UI, **P**erformance, **S**ecurity, **T**esting complexity, **U**pdates/OS fragmentation.

---

# UNIT 2: User Experience — UI Widgets, Buttons, Dialogs, Intents, Activity Interaction

*(Syllabus: User interaction, Input Controls, Menus, Screen Navigation, RecyclerView intro, Delightful UX, Drawables, Styles/Themes, Material Design)*

## Q2.1 — Two Techniques for Handling Button Click Events
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
Directly asked for exactly "2 techniques" worth 10 marks in the CIE-2 (Nov 2019) paper, and used inside almost every application-level question in every paper.

**Concept explanation in simple words:**
A button by itself does nothing until you tell Android what code to run when it's tapped. There are two common ways: writing the method's name directly in XML, or attaching a listener object in Java.

**Complete answer:**
**Technique 1 — `android:onClick` XML attribute:** Simplest for a single button with a dedicated method.
**Technique 2 — `setOnClickListener()` in Java:** More flexible; works well when handling multiple buttons or dynamically created views.

**Complete code:**
```xml
<!-- Technique 1: XML attribute -->
<Button
    android:id="@+id/button_send"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Send"
    android:onClick="sendMessage" />
```
```java
// Technique 1: corresponding method in the Activity (must be public, return void,
// and take exactly one View parameter)
public void sendMessage(View view) {
    Toast.makeText(this, "Button clicked via XML!", Toast.LENGTH_SHORT).show();
}

// Technique 2: OnClickListener set in Java
Button button = findViewById(R.id.button_send);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Toast.makeText(MainActivity.this, "Clicked via listener!", Toast.LENGTH_SHORT).show();
    }
});
```

**Output / result:** Tapping the button shows a Toast — "Button clicked via XML!" if only Technique 1's XML attribute is wired, or "Clicked via listener!" if Technique 2's listener is attached (use only one technique per button in practice, shown together here for comparison).

**Line-by-line explanation of the code:**
- `android:onClick="sendMessage"` — tells Android to look for a public method called `sendMessage` in the hosting Activity when this button is tapped.
- `public void sendMessage(View view)` — the method signature MUST match exactly: public, void return type, single `View` parameter — otherwise a crash occurs at runtime.
- `button.setOnClickListener(new View.OnClickListener() {...})` — creates an anonymous class implementing the `OnClickListener` interface and registers it as the click handler.
- `public void onClick(View v)` — the single method required by the `OnClickListener` interface; `v` refers to the view that was clicked.

**Viva / oral exam questions:**
- What are the exact method signature requirements for the XML `onClick` technique?
- Which technique is better when you have 10 buttons doing similar things, and why?
- Can you use both techniques on the same button at once?

**Common mistakes students make:**
- Wrong method signature for the XML `onClick` technique (e.g., missing the `View` parameter) — causes an `IllegalStateException` at runtime, not compile time.
- Forgetting `@Override` on `onClick()`.
- Not calling `findViewById()` before `setOnClickListener()`.

**Memory trick / quick revision point:**
**"XML says the name, Java gives the logic (technique 2), or XML method IS the logic (technique 1)"** — Technique 1 (XML+method) is a shortcut; Technique 2 (listener) is the standard, more powerful pattern used in real apps.

---

## Q2.2 — Explicit Intent: Two-Activity App with Data Passing (Welcome Message App)
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
This exact case study — "launcher activity displays a WELCOME message for 10 seconds, then opens a second activity that takes a username and returns it" — has appeared verbatim or near-verbatim in **at least 4 different years** of CIE-1 papers (2016, 2018, 2019, and repeated pattern in 2025-26 style). It is the highest-value application question in the whole syllabus.

**Concept explanation in simple words:**
An **explicit Intent** is like writing a courier's exact house address — you specify precisely which Activity class should open next. You can also attach extra data (like a note inside the courier package) using `putExtra()`, and get a reply back using the Activity Result API.

**Complete answer:**
Use `Intent(context, TargetActivity.class)`, `putExtra()` to send data, `startActivityForResult`/`ActivityResultLauncher` to receive a reply, and `setResult()` + `finish()` in the second Activity to send data back.

**Complete code:**
```java
// Activity1.java (launcher activity)
package com.example.welcomeapp;

import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.widget.TextView;
import androidx.activity.result.ActivityResultLauncher;
import androidx.activity.result.contract.ActivityResultContracts;
import androidx.appcompat.app.AppCompatActivity;

public class Activity1 extends AppCompatActivity {

    TextView tvWelcome;
    ActivityResultLauncher<Intent> launcher;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity1);

        tvWelcome = findViewById(R.id.tvWelcome);
        tvWelcome.setText("WELCOME TO MOBILE APP DEVELOPMENT");

        launcher = registerForActivityResult(
            new ActivityResultContracts.StartActivityForResult(),
            result -> {
                if (result.getResultCode() == RESULT_OK && result.getData() != null) {
                    String username = result.getData().getStringExtra("username");
                    tvWelcome.setText("WELCOME " + username);
                }
            });

        new Handler().postDelayed(() -> {
            Intent intent = new Intent(Activity1.this, Activity2.class);
            launcher.launch(intent);
        }, 10000); // 10-second delay
    }
}
```
```java
// Activity2.java (second activity - takes username, sends it back)
package com.example.welcomeapp;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;

public class Activity2 extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity2);

        EditText etUsername = findViewById(R.id.etUsername);
        Button btnSubmit = findViewById(R.id.btnSubmit);

        btnSubmit.setOnClickListener(v -> {
            Intent resultIntent = new Intent();
            resultIntent.putExtra("username", etUsername.getText().toString());
            setResult(RESULT_OK, resultIntent);
            finish();
        });
    }
}
```

**XML code:**
```xml
<!-- res/layout/activity1.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvWelcome"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="12dp"
        android:textSize="20sp"
        android:gravity="center" />
</LinearLayout>
```
```xml
<!-- res/layout/activity2.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/etUsername"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="10dp"
        android:hint="Enter username" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="submit" />
</LinearLayout>
```

**AndroidManifest code:**
```xml
<activity android:name=".Activity1">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity android:name=".Activity2" />
```

**Output / result:**
The app opens showing "WELCOME TO MOBILE APP DEVELOPMENT". After 10 seconds it automatically switches to a screen with a text box and a "submit" button. Typing a name (e.g. "Rekha") and tapping submit returns to the first screen, now showing "WELCOME Rekha".

**Line-by-line explanation of the code:**
- `new Handler().postDelayed(() -> {...}, 10000);` — schedules the lambda to run 10000 milliseconds (10 seconds) later on the main thread.
- `Intent intent = new Intent(Activity1.this, Activity2.class);` — an **explicit** Intent: names both the current context and the exact target Activity class.
- `launcher.launch(intent);` — starts `Activity2` and waits for a result, using the modern `ActivityResultLauncher` API (replaces the deprecated `startActivityForResult`).
- `registerForActivityResult(new ActivityResultContracts.StartActivityForResult(), result -> {...})` — registers a callback that fires automatically once `Activity2` finishes and returns.
- `resultIntent.putExtra("username", ...)` — packs a key-value pair of data into the Intent to send back.
- `setResult(RESULT_OK, resultIntent);` — tells the calling Activity "I succeeded, and here's my data".
- `finish();` — closes `Activity2` and returns control (and the result) to `Activity1`.

**Viva / oral exam questions:**
- What is the difference between an explicit and an implicit Intent?
- Why do we use `ActivityResultLauncher` instead of the older `startActivityForResult()`?
- What happens if the user presses Back on Activity2 without pressing submit — what result code is returned?
- Where exactly should `putExtra()` be called — before or after `startActivity()`?

**Common mistakes students make:**
- Forgetting to call `finish()` in Activity2 after `setResult()` — the app doesn't return automatically.
- Using `Intent()` with only one argument (that's for implicit Intents, not explicit).
- Mismatched key names in `putExtra("username", ...)` vs `getStringExtra("username")` (typo mismatch returns null).

**Memory trick / quick revision point:**
**"Explicit = Exact address, Implicit = General request"** — remember explicit Intents always take a `.class` reference; implicit Intents only describe an action (like `ACTION_VIEW`) and let Android pick the app.

---

## Q2.3 — Implicit Intent (Open Web Page / Dialer)
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"Design the java code to illustrate the working of implicit intent" is a direct 6-mark question repeated across multiple CIE-1 papers (2021-22, and earlier years), and always paired with the explicit-vs-implicit theory question.

**Concept explanation in simple words:**
An implicit Intent doesn't say exactly which app should handle the request — it just describes *what* needs to be done (e.g., "view this URL"), and Android shows the user a chooser of apps that can handle it (browser, dialer, etc.).

**Complete answer + code:**
```java
// Open a web page (implicit intent)
Intent webIntent = new Intent(Intent.ACTION_VIEW);
webIntent.setData(Uri.parse("https://www.rvce.edu.in"));
startActivity(webIntent);

// Open the phone dialer (implicit intent)
Intent callIntent = new Intent(Intent.ACTION_DIAL);
callIntent.setData(Uri.parse("tel:9538860055"));
startActivity(callIntent);
```

**XML code:** Not needed for this snippet — assume a Button whose `onClick` calls the above code.

**AndroidManifest code:** No special permission is required for `ACTION_DIAL` (it just opens the dialer, doesn't place the call automatically); `ACTION_CALL` (which places the call directly) *would* require:
```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```

**Output / result:** Tapping the "open webpage" button opens the default browser (or a chooser dialog if multiple browsers are installed) showing rvce.edu.in. Tapping the "dialer" button opens the Phone app with `9538860055` pre-filled, ready for the user to press Call manually.

**Line-by-line explanation of the code:**
- `new Intent(Intent.ACTION_VIEW)` — creates an Intent describing the generic action "view this data"; Android decides which app is capable of handling it.
- `webIntent.setData(Uri.parse("https://..."))` — attaches the actual data (a URI) the action should operate on.
- `startActivity(webIntent);` — hands the Intent to the Android system to resolve and launch the matching app.
- `Intent.ACTION_DIAL` vs `Intent.ACTION_CALL` — `DIAL` just opens the dialer (safe, no permission needed); `CALL` places the call immediately (needs the dangerous `CALL_PHONE` permission).

**Viva / oral exam questions:**
- What's the difference between `ACTION_DIAL` and `ACTION_CALL`?
- What happens if no app on the device can handle an implicit Intent?
- Why doesn't `ACTION_VIEW` need you to specify a browser app by name?

**Common mistakes students make:**
- Using `ACTION_CALL` without adding the `CALL_PHONE` permission (crashes with `SecurityException`).
- Forgetting `setData()`, so the Intent has no target to act on.
- Confusing implicit Intent creation (single-argument constructor + action string) with explicit Intent (two-argument constructor + class).

**Memory trick / quick revision point:**
**"DIAL is safe, CALL needs permission"** — remember D comes before C alphabetically, and Dial is the "softer/safer" one.

---

## Q2.4 — AlertDialog with Positive and Negative Buttons
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
Dialog classes are a recurring quiz topic ("Dialog classes in android? AlertDialog, ProgressDialog, DatePickerDialog...") and AlertDialog specifically appears in application-style questions needing confirmation before an action (e.g., exit confirmation, delete confirmation).

**Concept explanation in simple words:**
An AlertDialog is a pop-up box that interrupts the user and forces a decision — usually a Yes/No or OK/Cancel choice — before the app continues.

**Complete code:**
```java
new AlertDialog.Builder(this)
    .setTitle("Confirm Exit")
    .setMessage("Do you want to exit the app?")
    .setPositiveButton("Yes", (dialog, which) -> finish())
    .setNegativeButton("No", (dialog, which) -> dialog.dismiss())
    .setCancelable(false)
    .show();
```

**XML code:** Not required — AlertDialog is built entirely in Java, no separate layout XML needed for the basic version.

**Output / result:** A pop-up box titled "Confirm Exit" appears with the message and two buttons, "Yes" and "No". Tapping "Yes" closes the app; tapping "No" dismisses the dialog and the app continues normally.

**Line-by-line explanation of the code:**
- `new AlertDialog.Builder(this)` — starts building a dialog attached to the current Activity's context.
- `.setTitle(...)` / `.setMessage(...)` — set the header and body text.
- `.setPositiveButton("Yes", (dialog, which) -> finish())` — defines the "affirmative" button and its click action (a lambda implementing `DialogInterface.OnClickListener`).
- `.setNegativeButton("No", (dialog, which) -> dialog.dismiss())` — defines the "cancel" button; `dialog.dismiss()` closes the pop-up without further action.
- `.setCancelable(false)` — prevents the user from dismissing the dialog by tapping outside it or pressing Back, forcing an explicit choice.
- `.show();` — actually displays the built dialog.

**Viva / oral exam questions:**
- What is the difference between `dialog.dismiss()` and `dialog.cancel()`?
- How would you add a third, "neutral" button?
- What does `setCancelable(false)` do, and when would you use it?

**Common mistakes students make:**
- Forgetting `.show()` at the end — dialog is built but never appears.
- Confusing `AlertDialog` with `Toast` (Toast is non-interactive and auto-dismisses; AlertDialog blocks interaction until answered).
- Not handling the "outside tap" dismiss behavior when it isn't wanted.

**Memory trick / quick revision point:**
**"Builder pattern = Build, then Show"** — almost every Dialog class in Android uses a `.Builder()` chain ending in `.show()`; memorize this pattern once and it applies everywhere.

---

## Q2.5 — DatePickerDialog and TimePickerDialog
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
"Demonstrate working of DatePicker dialog subclass and necessary code to create TimePicker" is a full 10-mark question in the RVCE Model Question Paper and appears again in the 2020-21 Closed Book Test papers.

**Concept explanation in simple words:**
These are ready-made pop-up pickers so users can select a date or time using a familiar calendar/clock UI instead of typing it manually.

**Complete code:**
```java
import java.util.Calendar;
import android.app.DatePickerDialog;
import android.app.TimePickerDialog;
import android.widget.Toast;

// DatePickerDialog
Calendar c = Calendar.getInstance();
int year = c.get(Calendar.YEAR);
int month = c.get(Calendar.MONTH);
int day = c.get(Calendar.DAY_OF_MONTH);

DatePickerDialog dateDialog = new DatePickerDialog(this,
    (view, selectedYear, selectedMonth, selectedDay) -> {
        String date = selectedDay + "/" + (selectedMonth + 1) + "/" + selectedYear;
        Toast.makeText(this, "Selected date: " + date, Toast.LENGTH_SHORT).show();
    }, year, month, day);
dateDialog.show();

// TimePickerDialog
int hour = c.get(Calendar.HOUR_OF_DAY);
int minute = c.get(Calendar.MINUTE);

TimePickerDialog timeDialog = new TimePickerDialog(this,
    (view, selectedHour, selectedMinute) -> {
        String time = selectedHour + ":" + selectedMinute;
        Toast.makeText(this, "Selected time: " + time, Toast.LENGTH_SHORT).show();
    }, hour, minute, true);
timeDialog.show();
```

**XML code:** A simple button to trigger each dialog is enough — no special layout XML is required for the pickers themselves.

**Output / result:** Tapping "Pick Date" opens a calendar-style pop-up; after choosing a date, a Toast shows e.g. "Selected date: 29/7/2026". Tapping "Pick Time" opens a clock-style pop-up; after choosing, a Toast shows e.g. "Selected time: 14:30".

**Line-by-line explanation of the code:**
- `Calendar.getInstance()` — gets the device's current date/time as a starting default for the picker.
- `new DatePickerDialog(context, listener, year, month, day)` — the 3 numeric parameters at the end set which date the calendar opens showing initially.
- `(view, selectedYear, selectedMonth, selectedDay) -> {...}` — a lambda implementing `DatePickerDialog.OnDateSetListener`, called once the user confirms a date.
- `(selectedMonth + 1)` — Android's `Calendar.MONTH` is zero-indexed (January = 0), so we add 1 for display.
- `new TimePickerDialog(context, listener, hour, minute, true)` — the last boolean parameter `true` means 24-hour format; `false` would show AM/PM format.

**Viva / oral exam questions:**
- Why do we add 1 to the month value from `DatePickerDialog`?
- What does the boolean parameter in `TimePickerDialog`'s constructor control?
- How would you restrict the DatePicker to not allow past dates?

**Common mistakes students make:**
- Forgetting `Calendar.MONTH` is zero-based, displaying "0" instead of "1" for January.
- Mixing up the listener interfaces (`OnDateSetListener` vs `OnTimeSetListener`).
- Not calling `.show()`.

**Memory trick / quick revision point:**
**"Month starts at 0, like an index"** — Calendar months behave like array indices (Jan=0 ... Dec=11); always +1 when displaying to a user.

---

## Q2.6 — Create an Options Menu
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"With code sample enumerate step by step procedure to create a menu" — asked in CIE-1 (2019) for 4 marks, and closely tied to the "menu" quiz questions seen in Quiz-1 papers.

**Concept explanation in simple words:**
An options menu is the 3-dot (⋮) menu at the top of many apps. You first design what items it should have in an XML file, then write Java code to load that XML and respond to taps.

**Complete code:**
```java
@Override
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.main_menu, menu);
    return true;
}

@Override
public boolean onOptionsItemSelected(MenuItem item) {
    int id = item.getItemId();
    if (id == R.id.action_settings) {
        Toast.makeText(this, "Settings clicked", Toast.LENGTH_SHORT).show();
        return true;
    } else if (id == R.id.action_about) {
        Toast.makeText(this, "About clicked", Toast.LENGTH_SHORT).show();
        return true;
    }
    return super.onOptionsItemSelected(item);
}
```

**XML code:**
```xml
<!-- res/menu/main_menu.xml -->
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/action_settings"
        android:title="Settings" />
    <item
        android:id="@+id/action_about"
        android:title="About" />
</menu>
```

**Output / result:** A 3-dot menu icon appears in the app bar. Tapping it shows "Settings" and "About" options; tapping either shows a corresponding Toast message.

**Line-by-line explanation of the code:**
- `onCreateOptionsMenu(Menu menu)` — called once when the Activity's menu is first created.
- `getMenuInflater().inflate(R.menu.main_menu, menu);` — reads the XML menu resource and "inflates" it into actual menu item objects.
- `return true;` — tells Android the menu should be displayed (returning `false` would hide it).
- `onOptionsItemSelected(MenuItem item)` — called whenever any menu item is tapped; `item.getItemId()` tells you which one.
- `return super.onOptionsItemSelected(item);` — default behaviour fallback if none of your `if` conditions matched.

**Viva / oral exam questions:**
- What is the difference between `onCreateOptionsMenu()` and `onOptionsItemSelected()`?
- What does returning `true` vs `false` mean in these methods?
- Where are menu XML files stored in the project structure?

**Common mistakes students make:**
- Forgetting to `return true;` in `onCreateOptionsMenu()`.
- Comparing `item.getItemId()` using `==` incorrectly with boxed Integer objects instead of `int` (a subtle bug in some cases) — using primitive `int id` avoids this.
- Forgetting `break`/`return` after handling a menu item, causing fallthrough logic errors.

**Memory trick / quick revision point:**
**"Create then Select"** — two-step pattern: `onCreateOptionsMenu` builds the menu once, `onOptionsItemSelected` fires every time an item is tapped.

---

## Q2.7 — Switch Control with Toast Messages for ON/OFF
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"With code segment demonstrate the steps how to add an input control (Switch) which lets the user change a setting between on and off. Toast messages On and Off to be displayed" — asked for 6 marks in the RVCE Model Question Paper.

**Concept explanation in simple words:**
A Switch is a toggle button (like a light switch) representing an on/off setting. You attach a listener that fires every time its state changes.

**Complete code:**
```java
Switch mySwitch = findViewById(R.id.mySwitch);
mySwitch.setOnCheckedChangeListener((buttonView, isChecked) -> {
    if (isChecked) {
        Toast.makeText(MainActivity.this, "Switch is ON", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(MainActivity.this, "Switch is OFF", Toast.LENGTH_SHORT).show();
    }
});
```

**XML code:**
```xml
<Switch
    android:id="@+id/mySwitch"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Enable Notifications" />
```

**Output / result:** Sliding the Switch to the right (ON) shows a Toast "Switch is ON"; sliding it back (OFF) shows "Switch is OFF".

**Line-by-line explanation of the code:**
- `Switch mySwitch = findViewById(R.id.mySwitch);` — binds the Java object to the XML Switch view.
- `setOnCheckedChangeListener((buttonView, isChecked) -> {...})` — registers a callback that fires whenever the checked state changes; `isChecked` is a boolean representing the new state.
- The `if (isChecked)` / `else` block simply picks which Toast message to show based on the new state.

**Viva / oral exam questions:**
- What's the difference between a `Switch` and a `CheckBox`?
- What listener interface does `setOnCheckedChangeListener` implement?
- How would you set the Switch's initial state programmatically?

**Common mistakes students make:**
- Confusing `setOnClickListener` (fires on any tap) with `setOnCheckedChangeListener` (fires specifically when checked state changes).
- Forgetting the boolean logic and showing the wrong message for each state.

**Memory trick / quick revision point:**
**"Switch = light switch, Checked = light ON"** — `isChecked == true` always means "ON"/toggled position.

---

## Q2.8 — Application-Level: Login/Email Validation App
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Appeared as a 15-mark full application question in the 2020-21 Closed Book Online Test-1 with an Email App diagram (views A–E, SIGNIN → LOGIN screen with validation).

**Concept explanation in simple words:**
This combines input validation (checking a field isn't empty or too short) with conditional Toast feedback — a very common "real app" pattern examiners like to test.

**Complete code:**
```java
public class LoginActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        EditText etUser = findViewById(R.id.etUsername);
        EditText etPass = findViewById(R.id.etPassword);
        Button btnLogin = findViewById(R.id.btnLogin);

        btnLogin.setOnClickListener(v -> {
            String user = etUser.getText().toString().trim();
            String pass = etPass.getText().toString().trim();

            if (user.isEmpty()) {
                etUser.setError("Username required");
                return;
            }
            if (pass.length() < 6) {
                etPass.setError("Password must be at least 6 characters");
                return;
            }
            if (user.equals("admin") && pass.equals("admin123")) {
                Toast.makeText(this, "Logged in successfully", Toast.LENGTH_SHORT).show();
            } else {
                Toast.makeText(this, "Cannot login", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

**XML code:**
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="20dp">

    <EditText
        android:id="@+id/etUsername"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="example@email.com" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="textPassword"
        android:hint="Password" />

    <Button
        android:id="@+id/btnLogin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="LOGIN" />
</LinearLayout>
```

**Output / result:** Leaving username empty shows an inline error "Username required" under that field. A short password shows "Password must be at least 6 characters". Correct credentials ("admin"/"admin123") show "Logged in successfully"; anything else shows "Cannot login".

**Line-by-line explanation of the code:**
- `etUser.getText().toString().trim()` — reads the current text, converts it to a plain `String`, and removes leading/trailing spaces.
- `etUser.setError("...")` — shows a small red error icon + message directly attached to that field, and also stops further validation via `return;`.
- The final `if/else` checks hardcoded demo credentials — in a real app this would call a server or database instead.

**Viva / oral exam questions:**
- Why do we call `.trim()` on the input text?
- What does `etUser.setError()` do visually, and how is it different from a Toast?
- How would you extend this to check credentials against a real database instead of hardcoded values?

**Common mistakes students make:**
- Not trimming input, so a lone space passes the "not empty" check.
- Forgetting `return;` after `setError()`, so validation continues and shows multiple conflicting messages.
- Comparing passwords with `==` instead of `.equals()`.

**Memory trick / quick revision point:**
**"Validate top-down, return early"** — check each field one at a time, and `return` immediately when one fails, so the user only sees one error at a time.

---

## Q2.9 — Application-Level: Team Sport Scoring App
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Appeared as a full Open-Book programming exam question (2018): "Create a scoring app... Use string resources and convert layout dimensions to resources... Implement click handler methods."

**Concept explanation in simple words:**
Two counters (one per team) increase every time their button is tapped; a "Finish" button compares the two scores and announces a winner via Toast.

**Complete code:**
```java
public class ScoringActivity extends AppCompatActivity {

    int scoreA = 0, scoreB = 0;
    TextView tvScoreA, tvScoreB;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_scoring);

        tvScoreA = findViewById(R.id.tvScoreA);
        tvScoreB = findViewById(R.id.tvScoreB);

        findViewById(R.id.btnScoreA).setOnClickListener(v ->
            tvScoreA.setText(String.valueOf(++scoreA)));

        findViewById(R.id.btnScoreB).setOnClickListener(v ->
            tvScoreB.setText(String.valueOf(++scoreB)));

        findViewById(R.id.btnFinish).setOnClickListener(v -> {
            String winner;
            if (scoreA > scoreB) winner = getString(R.string.team_a);
            else if (scoreB > scoreA) winner = getString(R.string.team_b);
            else winner = "It's a tie";
            Toast.makeText(this, "Congratulations " + winner + "!", Toast.LENGTH_LONG).show();
        });
    }
}
```

**XML code:**
```xml
<!-- res/values/strings.xml additions -->
<string name="team_a">Team A</string>
<string name="team_b">Team B</string>
<string name="finish">Finish</string>

<!-- res/values/dimens.xml -->
<dimen name="btn_padding">12dp</dimen>
```
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@drawable/sport_bg">

    <TextView
        android:id="@+id/tvScoreA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0" />

    <Button
        android:id="@+id/btnScoreA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="@dimen/btn_padding"
        android:text="@string/team_a" />

    <!-- Similar TextView + Button pair for Team B -->

    <Button
        android:id="@+id/btnFinish"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/finish" />
</RelativeLayout>
```

**Output / result:** Each team button tap increments and displays that team's running score; tapping Finish shows a Toast declaring the winning team (or a tie).

**Line-by-line explanation of the code:**
- `int scoreA = 0, scoreB = 0;` — instance variables holding the running scores.
- `tvScoreA.setText(String.valueOf(++scoreA));` — the **pre-increment** operator `++scoreA` increases the value *before* it's used, then converts it to a `String` for display.
- The `if / else if / else` chain in the Finish handler compares final scores to decide the winner text.

**Viva / oral exam questions:**
- What is the difference between `++scoreA` and `scoreA++`?
- Why is `String.valueOf()` needed before calling `setText()` on an int?
- How would you reset both scores back to zero after Finish is tapped?

**Common mistakes students make:**
- Trying to pass an `int` directly into `setText()` (compiles but sets text to a resource ID, not the number!) — must use `String.valueOf()` or `""+ scoreA`.
- Off-by-one errors confusing `scoreA++` vs `++scoreA` when also using the value in the same line.

**Memory trick / quick revision point:**
**"setText needs a String, not a number"** — always wrap numeric values with `String.valueOf()` before calling `setText()`, or Android will interpret the int as a resource ID and crash.

---

# UNIT 3: Working in the Background & All About Data — Storage, SQLite, Fragments, RecyclerView

*(Syllabus: AsyncTask/AsyncTaskLoader basics, Storing Data, SharedPreferences, SQLite Database, Content Providers, Internet/Maps/SMS intro, Sensors)*

## Q3.1 — SQLite CRUD Operations (SQLiteOpenHelper + Insert/Query/Update/Delete)
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"Illustrate with sample code how to send queries to the SQLite database" and full CRUD SQLite questions appear in almost every CIE-2/CIE-3 paper and multiple Test papers — the single most-tested storage topic.

**Concept explanation in simple words:**
SQLite is a tiny built-in database stored as a file on the device. `SQLiteOpenHelper` is a helper class Android gives you to create and upgrade that database easily. Once created, you Insert, Query (read), Update, and Delete rows using simple method calls — this is called **CRUD**.

**Complete answer + code:**
```java
// DBHelper.java
public class DBHelper extends SQLiteOpenHelper {
    private static final String DB_NAME = "StudentDB";
    private static final int DB_VERSION = 1;
    public static final String TABLE_NAME = "students";

    public DBHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String query = "CREATE TABLE " + TABLE_NAME +
            " (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, marks INTEGER)";
        db.execSQL(query);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }

    // INSERT
    public long insertStudent(String name, int marks) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("name", name);
        values.put("marks", marks);
        return db.insert(TABLE_NAME, null, values);
    }

    // READ (query all)
    public Cursor getAllStudents() {
        SQLiteDatabase db = this.getReadableDatabase();
        return db.rawQuery("SELECT * FROM " + TABLE_NAME, null);
    }

    // UPDATE
    public int updateMarks(String name, int newMarks) {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put("marks", newMarks);
        return db.update(TABLE_NAME, values, "name = ?", new String[]{name});
    }

    // DELETE
    public int deleteStudent(String name) {
        SQLiteDatabase db = this.getWritableDatabase();
        return db.delete(TABLE_NAME, "name = ?", new String[]{name});
    }
}
```
```java
// Using it in an Activity
DBHelper dbHelper = new DBHelper(this);
dbHelper.insertStudent("Rekha", 90);

Cursor cursor = dbHelper.getAllStudents();
if (cursor.moveToFirst()) {
    do {
        int id = cursor.getInt(cursor.getColumnIndexOrThrow("id"));
        String name = cursor.getString(cursor.getColumnIndexOrThrow("name"));
        int marks = cursor.getInt(cursor.getColumnIndexOrThrow("marks"));
        Log.i("DB", id + " " + name + " " + marks);
    } while (cursor.moveToNext());
}
cursor.close();

dbHelper.updateMarks("Rekha", 95);
dbHelper.deleteStudent("Rekha");
```

**XML code:** Not required for the database logic itself; a simple layout with EditTexts for name/marks and 4 buttons (Insert/View/Update/Delete) would host this in an app.

**Output / result:** After inserting "Rekha, 90", Logcat prints `1 Rekha 90` when queried. After `updateMarks`, a subsequent query would show `1 Rekha 95`. After `deleteStudent`, the query returns zero rows.

**Line-by-line explanation of the code:**
- `extends SQLiteOpenHelper` — inherits Android's built-in database-creation/versioning helper class.
- `super(context, DB_NAME, null, DB_VERSION);` — passes the database name and version number to the parent constructor; the `null` is for a custom Cursor factory (rarely used, so left null).
- `onCreate(SQLiteDatabase db)` — called only the very first time the database is created; this is where you run `CREATE TABLE`.
- `onUpgrade(...)` — called when `DB_VERSION` increases; here it simply drops and recreates the table (simplest but destroys old data — fine for exam purposes).
- `ContentValues values = new ContentValues(); values.put("name", name);` — a key-value container matching column names to new values, used for both insert and update.
- `db.insert(TABLE_NAME, null, values);` — inserts a new row; returns the new row's ID, or -1 on failure.
- `db.rawQuery("SELECT * FROM students", null);` — runs raw SQL and returns a `Cursor` (a pointer into the result set, one row at a time).
- `cursor.moveToFirst()` / `cursor.moveToNext()` — move the cursor's position; the `do-while` loop reads each row until there are no more.
- `cursor.getColumnIndexOrThrow("name")` — safely finds the column's index by name, throwing an exception (rather than silently failing) if it doesn't exist.
- `db.update(TABLE_NAME, values, "name = ?", new String[]{name})` — updates rows matching the WHERE clause; the `?` placeholder is safely replaced by the array value (prevents SQL injection).
- `db.delete(TABLE_NAME, "name = ?", new String[]{name})` — deletes matching rows the same safe way.
- `cursor.close();` — always close the Cursor when done to free memory/resources.

**Viva / oral exam questions:**
- Why do we use `?` placeholders instead of directly concatenating strings into the WHERE clause?
- What's the difference between `getReadableDatabase()` and `getWritableDatabase()`?
- What triggers `onUpgrade()` to run?
- Why must you close a Cursor after use?

**Common mistakes students make:**
- Forgetting `cursor.moveToFirst()` before reading — accessing an empty cursor position crashes.
- Using string concatenation for WHERE clauses instead of `?` placeholders (a security/SQL-injection risk, and marks are deducted).
- Forgetting to close the Cursor or the database connection.
- Mismatched column name spelling between `CREATE TABLE` and later `ContentValues`/`Cursor` calls.

**Memory trick / quick revision point:**
**"CRUD = Create, Read, Update, Delete"** — and in SQLite Java code that maps to: `insert()`, `rawQuery()`/`query()`, `update()`, `delete()` — memorize these four method names as a set.

---

## Q3.2 — SharedPreferences: Save and Retrieve Values
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
Directly asked ("Differentiate between Shared Preferences and Saved Instance State", "Shared preferences, explain its functionality and classification") in multiple Test/Quiz papers, and used inside many application-level questions (remembering a setting, storing a login flag).

**Concept explanation in simple words:**
SharedPreferences is a small private storage area for simple key-value data (like a settings file) that persists even after the app is closed and reopened — perfect for remembering things like "dark mode: on" or "username: Rekha".

**Complete code:**
```java
// Save data
SharedPreferences prefs = getSharedPreferences("MyPrefs", MODE_PRIVATE);
SharedPreferences.Editor editor = prefs.edit();
editor.putString("username", "Rekha");
editor.putInt("score", 95);
editor.putBoolean("isLoggedIn", true);
editor.apply();

// Retrieve data
SharedPreferences prefs = getSharedPreferences("MyPrefs", MODE_PRIVATE);
String username = prefs.getString("username", "Guest");
int score = prefs.getInt("score", 0);
boolean isLoggedIn = prefs.getBoolean("isLoggedIn", false);
```

**XML code:** Not applicable — SharedPreferences is a pure Java/Kotlin API, no XML layout needed.

**Output / result:** On first run before saving, `getString("username", "Guest")` returns the default `"Guest"`. After calling `editor.putString("username", "Rekha"); editor.apply();`, every future read (even after closing and reopening the app) returns `"Rekha"` until changed again.

**Line-by-line explanation of the code:**
- `getSharedPreferences("MyPrefs", MODE_PRIVATE)` — opens (or creates) a preferences file named "MyPrefs", accessible only by this app (`MODE_PRIVATE`).
- `prefs.edit()` — returns an `Editor` object, needed to make any changes (you can't write directly to `SharedPreferences`).
- `editor.putString(key, value)` / `putInt` / `putBoolean` — stage changes in memory; nothing is saved to disk yet.
- `editor.apply();` — asynchronously commits all staged changes to disk (`commit()` is the synchronous alternative, blocking until saved — `apply()` is preferred in exams for its simplicity/performance).
- `prefs.getString("username", "Guest")` — the second argument is the **default value** returned if the key doesn't exist yet.

**Viva / oral exam questions:**
- What's the difference between SharedPreferences and Saved Instance State (`onSaveInstanceState`)?
- What's the difference between `apply()` and `commit()`?
- What data types can SharedPreferences store?

**Common mistakes students make:**
- Forgetting `editor.apply()`/`commit()` — changes are made in the Editor object but never actually saved.
- Confusing `SharedPreferences` (persists across app restarts, small key-value data) with `onSaveInstanceState` (survives configuration changes like rotation only, cleared when the app is fully closed).
- Not providing a sensible default value in the `get___()` calls.

**Memory trick / quick revision point:**
**"Edit, then Apply"** — just like AlertDialog's Build-then-Show pattern, SharedPreferences always follows Edit-then-Apply.

---

## Q3.3 — Create and Host a Basic Fragment
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
"Discuss the following with examples: a. Fragments b. Room database" is a direct 10-mark question in the CIE-2 (Nov 2019) paper, and Fragment concepts appear in multiple quiz questions ("Which one is NOT related to fragment class?").

**Concept explanation in simple words:**
A Fragment is like a mini-Activity — a reusable piece of UI with its own lifecycle, that must live *inside* a host Activity. It's useful when you want the same screen section to be reused across different Activities, or to build flexible layouts for tablets vs phones.

**Complete code:**
```java
// MyFragment.java
public class MyFragment extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container,
                              @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_my, container, false);
    }
}
```
```java
// Hosting the Fragment inside an Activity
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                .replace(R.id.fragment_container, new MyFragment())
                .commit();
        }
    }
}
```

**XML code:**
```xml
<!-- res/layout/fragment_my.xml -->
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="I am a Fragment!" />
</LinearLayout>
```
```xml
<!-- res/layout/activity_main.xml -->
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/fragment_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

**Output / result:** The Activity's screen shows the text "I am a Fragment!", even though that UI is defined and controlled by a completely separate Fragment class.

**Line-by-line explanation of the code:**
- `extends Fragment` — makes this class a Fragment with its own lifecycle (`onCreateView`, `onStart`, `onPause`, etc., similar to an Activity but slightly different).
- `onCreateView(inflater, container, savedInstanceState)` — must be overridden; this is where the Fragment's own layout is "inflated" (converted from XML into real View objects) and returned.
- `inflater.inflate(R.layout.fragment_my, container, false)` — the `false` means "don't attach this view to the container yet" (the FragmentManager handles attaching).
- `getSupportFragmentManager()` — gets the manager responsible for adding/removing/replacing Fragments within this Activity.
- `.beginTransaction().replace(R.id.fragment_container, new MyFragment()).commit();` — a three-step pattern: begin a transaction, replace whatever is in the container view with a new Fragment instance, then commit (apply) the transaction.
- `if (savedInstanceState == null)` — ensures the Fragment is only added once (on first creation), not duplicated every time the Activity recreates (e.g., on rotation).

**Viva / oral exam questions:**
- Why do we check `if (savedInstanceState == null)` before adding a Fragment?
- What is the minimum method a Fragment subclass must override?
- What is the difference between `add()` and `replace()` in a FragmentTransaction?

**Common mistakes students make:**
- Forgetting the `if (savedInstanceState == null)` check, causing duplicate Fragments stacking on top of each other after rotation.
- Passing `true` instead of `false` as the third argument to `inflate()`.
- Forgetting to call `.commit()` after `beginTransaction()`.

**Memory trick / quick revision point:**
**"Fragment = Activity's roommate"** — it lives inside an Activity, shares the space, but has its own separate lifecycle and behaviour.

---

## Q3.4 — RecyclerView Adapter and ViewHolder
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
RecyclerView is explicitly named in the syllabus ("Recycler View, Delightful user experience") and appears as a supporting requirement in master-detail application questions from the possible-coding-questions notes.

**Concept explanation in simple words:**
RecyclerView efficiently displays a scrolling list of items (like a list of students or contacts) by "recycling" the views that scroll off-screen instead of creating new ones for every item — much more efficient than a plain ListView for long lists.

**Complete code:**
```java
// StudentAdapter.java
public class StudentAdapter extends RecyclerView.Adapter<StudentAdapter.ViewHolder> {

    List<String> studentList;

    public StudentAdapter(List<String> list) {
        this.studentList = list;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(parent.getContext())
                .inflate(android.R.layout.simple_list_item_1, parent, false);
        return new ViewHolder(v);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        holder.textView.setText(studentList.get(position));
    }

    @Override
    public int getItemCount() {
        return studentList.size();
    }

    static class ViewHolder extends RecyclerView.ViewHolder {
        TextView textView;
        ViewHolder(View itemView) {
            super(itemView);
            textView = itemView.findViewById(android.R.id.text1);
        }
    }
}
```
```java
// Setting it up in an Activity
RecyclerView recyclerView = findViewById(R.id.recyclerView);
recyclerView.setLayoutManager(new LinearLayoutManager(this));
List<String> students = Arrays.asList("Rekha", "Kavitha", "Geetha");
recyclerView.setAdapter(new StudentAdapter(students));
```

**XML code:**
```xml
<androidx.recyclerview.widget.RecyclerView
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

**Output / result:** A vertically scrolling list appears showing "Rekha", "Kavitha", "Geetha" — one per row, using Android's built-in simple list row layout.

**Line-by-line explanation of the code:**
- `extends RecyclerView.Adapter<StudentAdapter.ViewHolder>` — an Adapter connects your data (a `List<String>`) to the RecyclerView, using a generic `ViewHolder` type.
- `onCreateViewHolder(...)` — called only when a brand-new row view is needed (not for every item — this is the "recycling" efficiency); inflates one row's layout.
- `onBindViewHolder(holder, position)` — called every time a row needs to display data for a specific list position; sets the text here.
- `getItemCount()` — tells the RecyclerView how many total items exist.
- `static class ViewHolder extends RecyclerView.ViewHolder` — holds references to a single row's views (here just one TextView) so `findViewById()` isn't called repeatedly (performance optimization).
- `recyclerView.setLayoutManager(new LinearLayoutManager(this));` — required; tells RecyclerView to arrange items in a vertical (default) list.
- `recyclerView.setAdapter(...)` — connects the adapter (and therefore the data) to the RecyclerView.

**Viva / oral exam questions:**
- Why is RecyclerView considered more efficient than a plain ListView?
- What's the purpose of the ViewHolder pattern?
- What happens if you forget to call `setLayoutManager()`?

**Common mistakes students make:**
- Forgetting `setLayoutManager()` — the RecyclerView will show nothing at all, with no error message.
- Doing `findViewById()` inside `onBindViewHolder()` instead of storing references in the ViewHolder (defeats the performance purpose).
- Returning the wrong count from `getItemCount()` (e.g., hardcoding it instead of `list.size()`).

**Memory trick / quick revision point:**
**"3 Must-Haves: LayoutManager + Adapter + ViewHolder"** — a RecyclerView is broken/blank if any one of these three pieces is missing.

---

## Q3.5 — `onSaveInstanceState()` — Preserve UI State Across Rotation
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
Directly compared against SharedPreferences in Quiz papers ("Differentiate between Shared Preferences and Saved Instance State"), and is a classic "why does my counter reset on rotation" real-world bug students must understand.

**Concept explanation in simple words:**
When you rotate your phone, Android actually **destroys and recreates** the Activity from scratch (to reload a layout suited to the new orientation). `onSaveInstanceState()` lets you save small bits of temporary UI state (like a counter value) right before that happens, and restore it right after.

**Complete code:**
```java
public class MainActivity extends AppCompatActivity {
    int counter = 0;
    TextView tvCounter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        tvCounter = findViewById(R.id.tvCounter);

        if (savedInstanceState != null) {
            counter = savedInstanceState.getInt("counter", 0);
            tvCounter.setText(String.valueOf(counter));
        }

        findViewById(R.id.btnIncrement).setOnClickListener(v -> {
            counter++;
            tvCounter.setText(String.valueOf(counter));
        });
    }

    @Override
    protected void onSaveInstanceState(@NonNull Bundle outState) {
        super.onSaveInstanceState(outState);
        outState.putInt("counter", counter);
    }
}
```

**XML code:**
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:id="@+id/tvCounter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        android:textSize="24sp" />

    <Button
        android:id="@+id/btnIncrement"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Increment" />
</LinearLayout>
```

**Output / result:** Tapping Increment several times raises the counter (e.g., to 5). Rotating the phone would normally reset it to 0 — but with this code, it correctly still shows 5 after rotation.

**Line-by-line explanation of the code:**
- `onSaveInstanceState(Bundle outState)` — called automatically by Android right before the Activity is destroyed for a config change; `outState` is where you stash data.
- `outState.putInt("counter", counter);` — saves the current counter value under the key `"counter"`.
- In `onCreate`, `if (savedInstanceState != null)` — checks if this is a fresh launch (`null`) or a recreation after rotation (`not null`, containing the saved Bundle).
- `savedInstanceState.getInt("counter", 0)` — retrieves the previously saved value, defaulting to 0 if not found.

**Viva / oral exam questions:**
- Why does rotating the screen destroy and recreate the Activity?
- What's the difference between `onSaveInstanceState` and `SharedPreferences` in terms of how long the data survives?
- Is `onSaveInstanceState` guaranteed to be called when the user presses the Back button?

**Common mistakes students make:**
- Confusing this with permanent storage — `onSaveInstanceState` data is lost once the app is fully closed/swiped away, unlike SharedPreferences.
- Forgetting to check `savedInstanceState != null` in `onCreate`, so the restore logic runs (and crashes on null) even on first launch.
- Not calling `super.onSaveInstanceState(outState)`.

**Memory trick / quick revision point:**
**"Rotation-only memory, not close-app memory"** — `onSaveInstanceState` survives rotation but NOT a full app close; SharedPreferences survives both.

---

## Q3.6 — Room Persistence Library Basics
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
Explicitly asked alongside Fragments in the CIE-2 (2019) paper ("Discuss ... b. Room database"), and mentioned in the syllabus as a modern alternative to raw SQLite.

**Concept explanation in simple words:**
Room sits "on top of" SQLite and removes a lot of repetitive boilerplate code (like writing raw SQL strings and manually managing Cursors). You define your table as a simple Java class (`@Entity`), your queries as an interface (`@Dao`), and Room generates all the SQLite code for you behind the scenes.

**Complete code:**
```java
// Student.java - the "table" (Entity)
@Entity
public class Student {
    @PrimaryKey(autoGenerate = true)
    public int id;
    public String name;
    public int marks;
}
```
```java
// StudentDao.java - the queries (Data Access Object)
@Dao
public interface StudentDao {
    @Insert
    void insert(Student student);

    @Query("SELECT * FROM Student")
    List<Student> getAll();

    @Delete
    void delete(Student student);
}
```
```java
// AppDatabase.java - the database itself
@Database(entities = {Student.class}, version = 1)
public abstract class AppDatabase extends RoomDatabase {
    public abstract StudentDao studentDao();
}
```
```java
// Using it (typically inside a background thread, not shown here for simplicity)
AppDatabase db = Room.databaseBuilder(getApplicationContext(),
        AppDatabase.class, "student-db").allowMainThreadQueries().build();

db.studentDao().insert(new Student());
List<Student> all = db.studentDao().getAll();
```

**Output / result:** Room automatically creates the underlying SQLite table matching the `Student` class fields; calling `getAll()` returns a ready-made `List<Student>` object — no manual Cursor parsing needed.

**Line-by-line explanation of the code:**
- `@Entity` — marks a Java class as a database table; each public field becomes a column.
- `@PrimaryKey(autoGenerate = true)` — marks `id` as the primary key that auto-increments.
- `@Dao` (Data Access Object) — an interface where you declare query methods using annotations instead of writing SQL manually (except for custom `@Query` strings).
- `@Insert`, `@Query`, `@Delete` — annotations Room recognizes to auto-generate the actual SQL and Java implementation at compile time.
- `@Database(entities = {Student.class}, version = 1)` — declares which Entities belong to this database and its version number.
- `Room.databaseBuilder(...).build();` — constructs the actual database instance at runtime.
- `.allowMainThreadQueries()` — only used for quick demos/exams; in real apps, database work should run on a background thread.

**Viva / oral exam questions:**
- What are the three main annotated components of Room (Entity, DAO, Database)?
- Why is Room preferred over raw `SQLiteOpenHelper` in modern apps?
- Why shouldn't `allowMainThreadQueries()` be used in production apps?

**Common mistakes students make:**
- Forgetting `@PrimaryKey` on the Entity class (Room requires exactly one).
- Running Room queries directly on the main thread in a real app (causes janky UI or crashes with `IllegalStateException` unless explicitly allowed for demos).
- Confusing `@Dao` (an interface, no direct SQL implementation needed for basic operations) with a regular class.

**Memory trick / quick revision point:**
**"Room = SQLite's polished twin"** — same underlying engine, but Entity+DAO+Database annotations do the boilerplate work for you.

---

## Q3.7 — Internal Storage vs External Storage
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
"All Android devices have two file storage areas — briefly discuss the facts about each storage space" is a recurring 3–4 mark sub-question in CIE-2 papers.

**Concept explanation in simple words:**
Internal storage is a private locker only your app can open; external storage is more like a shared shelf other apps (and sometimes the user via USB) can also access.

**Complete answer (comparison table):**

| Internal Storage | External Storage |
|---|---|
| Always available | Not always available (can be removed/unmounted) |
| Private — only your app can access it | World-readable by other apps |
| Deleted automatically when app is uninstalled | Deleted on uninstall only if saved via `getExternalFilesDir()` |
| Best when data must stay private/secure | Best for files meant to be shared or accessed via computer |

**Complete code:** Not required for a pure theory comparison, but a quick example:
```java
// Internal storage
FileOutputStream fos = openFileOutput("myfile.txt", MODE_PRIVATE);

// External storage (app-specific, auto-cleaned on uninstall)
File file = new File(getExternalFilesDir(null), "myfile.txt");
```

**Output / result:** N/A (conceptual).

**Line-by-line explanation:**
- `openFileOutput("myfile.txt", MODE_PRIVATE)` — creates/opens a file in the app's private internal storage directory.
- `getExternalFilesDir(null)` — returns the app-specific directory on external storage (still removed on uninstall, unlike the shared public directories).

**Viva / oral exam questions:**
- Which storage type should you use for a user's private diary entries, and why?
- What happens to external storage files when the SD card is removed?
- Is external storage always an SD card?

**Common mistakes students make:**
- Assuming external storage always means a removable SD card (modern devices often have "external" storage built-in but still logically separate).
- Forgetting that files saved to the *public* external directories (not app-specific ones) persist even after the app is uninstalled.

**Memory trick / quick revision point:**
**"Internal = Private locker, External = Shared shelf"**.

---

# UNIT 4: Working in the Background — AsyncTask, Loaders, Services, Broadcast Receivers

*(Syllabus: Async Task and AsyncTaskLoader, Connect to Internet, Broadcast Receivers and Services, Scheduling background tasks, Notifications, Alarms, Transferring Data Efficiently)*

## Q4.1 — AsyncTask with All Four Callback Methods
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"With execution steps, explain the Asynch task, task usage and task parameters" (Model QP, 8 marks) and "Demonstrate the AsyncTask class usage for background task" (Open-Book, 20 marks with the Grade Calculator app) are both major recurring questions.

**Concept explanation in simple words:**
Some work (like downloading a file, or waiting 10 seconds) takes too long to run directly on the main/UI thread — doing so would freeze the app. `AsyncTask` lets you run that slow work on a separate background thread, while still being able to safely update the UI before, during, and after.

**Complete code:**
```java
private class DownloadTask extends AsyncTask<String, Integer, String> {

    @Override
    protected void onPreExecute() {
        // Runs on UI thread, BEFORE background work starts
        progressBar.setVisibility(View.VISIBLE);
    }

    @Override
    protected String doInBackground(String... urls) {
        // Runs on a BACKGROUND thread - never touch UI views here directly
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
        // Runs on UI thread, called whenever publishProgress() is used
        progressBar.setProgress(values[0]);
    }

    @Override
    protected void onPostExecute(String result) {
        // Runs on UI thread, AFTER doInBackground() finishes
        progressBar.setVisibility(View.GONE);
        Toast.makeText(getApplicationContext(), result, Toast.LENGTH_SHORT).show();
    }
}

// Executing the task
new DownloadTask().execute("https://example.com/file");
```

**XML code:**
```xml
<ProgressBar
    android:id="@+id/progressBar"
    style="?android:attr/progressBarStyleHorizontal"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:max="100"
    android:visibility="gone" />
```

**Output / result:** When executed, the ProgressBar appears and fills up in steps (0 → 20 → 40 → ... → 100) over about 1.8 seconds, then disappears and a Toast reads "Download complete".

**Line-by-line explanation of the code:**
- `class DownloadTask extends AsyncTask<String, Integer, String>` — the three generic types are: **Params** (input type to `execute()`, here `String`), **Progress** (type passed to `onProgressUpdate`, here `Integer`), **Result** (type returned by `doInBackground` and received by `onPostExecute`, here `String`).
- `onPreExecute()` — always runs first, on the UI thread; good place for showing a loading indicator.
- `doInBackground(String... urls)` — the only method that runs off the main thread; `urls` is a vararg array matching whatever was passed to `.execute(...)`.
- `publishProgress(i);` — sends a progress value from the background thread back to the UI thread, triggering `onProgressUpdate`.
- `onProgressUpdate(Integer... values)` — runs on the UI thread each time `publishProgress()` is called; safe to update UI here.
- `onPostExecute(String result)` — runs on the UI thread once `doInBackground` returns; receives that method's return value as `result`.
- `new DownloadTask().execute("https://...");` — creates the task and starts it, passing the String argument into `doInBackground`.

**Viva / oral exam questions:**
- Why can't you update UI views directly inside `doInBackground()`?
- What are the three generic type parameters of AsyncTask, in order?
- What method would you override to show a progress bar update mid-task?
- Is AsyncTask still recommended in the very latest Android versions? (Mention it is now deprecated in favor of `Executor`/`Thread` + `Handler`, or Kotlin Coroutines — but for exam purposes, AsyncTask is still the expected syllabus answer.)

**Common mistakes students make:**
- Trying to update a TextView or Toast directly inside `doInBackground()` — will crash or silently fail since that's a background thread.
- Getting the order of the three generic types wrong (Params, Progress, Result — easy to mix up).
- Forgetting `.execute()` at the end — the task class is defined but never actually run.

**Memory trick / quick revision point:**
**"PPPP = Pre, Progress, Post, Params"** — Or remember the acronym **"PDPP"**: `onPreExecute → doInBackground → onProgressUpdate (during) → onPostExecute`.

---

## Q4.2 — BroadcastReceiver for `ACTION_POWER_CONNECTED`
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"Implement a Broadcast receiver – 'Power connected'. Design the corresponding java file and AndroidManifest.xml file" is asked verbatim in the 2025-26 CIE-2 AND CIE-Improvement papers — a guaranteed-repeat question.

**Concept explanation in simple words:**
A BroadcastReceiver "listens" for system-wide announcements (like "phone is now charging", "WiFi turned on", "battery low") and runs your code automatically in response, even if your app isn't currently open.

**Complete code:**
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
// Registering it dynamically inside an Activity (required for this specific action
// on modern Android, since ACTION_POWER_CONNECTED cannot be registered statically
// in the manifest from API 26 onward)
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

**XML code:** Not required beyond the Activity's own layout.

**AndroidManifest code:**
```xml
<!-- If your exam expects a manifest-registered (static) receiver for demonstration,
     use this form; note that for ACTION_POWER_CONNECTED specifically, Android
     requires dynamic (in-code) registration since API 26+ -->
<receiver android:name=".PowerReceiver">
    <intent-filter>
        <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
    </intent-filter>
</receiver>
```

**Output / result:** As soon as the charger is plugged into the test device/emulator, a Toast pops up saying "Power connected!" and a Logcat entry confirms it — even if you weren't actively touching the app.

**Line-by-line explanation of the code:**
- `extends BroadcastReceiver` — makes this class capable of receiving system broadcasts.
- `onReceive(Context context, Intent intent)` — the single method you must implement; called automatically by Android when a matching broadcast arrives.
- `IntentFilter filter = new IntentFilter(Intent.ACTION_POWER_CONNECTED);` — describes exactly which broadcast action this receiver cares about.
- `registerReceiver(powerReceiver, filter);` — dynamically registers the receiver while the Activity is alive.
- `unregisterReceiver(powerReceiver);` — must be called in `onDestroy()` (or `onPause()`/`onStop()` depending on design) to avoid memory leaks once the Activity is no longer active.

**Viva / oral exam questions:**
- What is the difference between a statically registered (manifest) and dynamically registered (code) BroadcastReceiver?
- Why must you call `unregisterReceiver()`?
- Name two other common system broadcast actions besides power-connected.

**Common mistakes students make:**
- Forgetting `unregisterReceiver()`, causing a memory leak or a crash if the receiver tries to register twice.
- Trying to register `ACTION_POWER_CONNECTED` purely in the manifest on modern Android — many implicit broadcasts require dynamic registration since Android 8.0 (API 26) as a battery-saving measure.
- Misspelling the action string constant.

**Memory trick / quick revision point:**
**"Register in onCreate, Unregister in onDestroy"** — always pair these two calls like opening and closing a door.

---

## Q4.3 — Check Network Availability: Mobile vs Wi-Fi
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
"Design Java file in Android to check if Network is available and if exists check if it is of type Mobile or Wifi" is a direct 6-mark question in the 2025-26 CIE-2 (RBS) paper.

**Concept explanation in simple words:**
Before your app tries to fetch data from the internet, it's good practice to first check if the device actually has a working connection, and whether that connection is Wi-Fi (usually free/fast) or Mobile Data (may cost the user money).

**Complete code:**
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

**XML code:** Not applicable (pure logic).

**AndroidManifest code:**
```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

**Output / result:** Calling `isNetworkAvailable()` returns `true`/`false`; Logcat prints "Connected via WiFi" or "Connected via Mobile Data" depending on the current connection type when true.

**Line-by-line explanation of the code:**
- `ConnectivityManager cm = (ConnectivityManager) getSystemService(...)` — gets the system service responsible for network state information.
- `cm.getActiveNetwork();` — returns the currently active `Network` object, or `null` if there is none.
- `cm.getNetworkCapabilities(network);` — returns detailed info about that network's capabilities (transport type, bandwidth, etc.).
- `capabilities.hasTransport(NetworkCapabilities.TRANSPORT_WIFI)` — checks if the connection is specifically over Wi-Fi.
- `capabilities.hasTransport(NetworkCapabilities.TRANSPORT_CELLULAR)` — checks if it's over mobile data instead.

**Viva / oral exam questions:**
- What permission is required to check network state?
- What does `getActiveNetwork()` return if there's no connection at all?
- What other transport types exist besides WiFi and Cellular?

**Common mistakes students make:**
- Forgetting the `ACCESS_NETWORK_STATE` permission in the manifest.
- Using the older deprecated `NetworkInfo`/`getActiveNetworkInfo()` API in newer code without realizing it's deprecated (both are acceptable for exam purposes, but `NetworkCapabilities` is the modern recommended approach).
- Not null-checking `network` or `capabilities` before using them (causes `NullPointerException`).

**Memory trick / quick revision point:**
**"3 Steps: Manager → Network → Capabilities"** — always drill down through these three objects to check connection type.

---

## Q4.4 — Display a Notification
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
"Demonstrate the android app with .java & .xml code for notifications" is an 8-mark question in the RVCE Model Question Paper, and notifications are a commonly tested Unit-4 topic.

**Concept explanation in simple words:**
A notification is the message that appears in the phone's status bar/notification tray (like "You have a new message"), even when your app isn't open on screen.

**Complete code:**
```java
public void showNotification() {
    String channelId = "my_channel";
    NotificationManager manager = getSystemService(NotificationManager.class);

    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        NotificationChannel channel = new NotificationChannel(
                channelId, "My Channel", NotificationManager.IMPORTANCE_DEFAULT);
        manager.createNotificationChannel(channel);
    }

    Notification notification = new NotificationCompat.Builder(this, channelId)
            .setSmallIcon(R.drawable.ic_notification)
            .setContentTitle("New Message")
            .setContentText("You have a new message")
            .setPriority(NotificationCompat.PRIORITY_DEFAULT)
            .setAutoCancel(true)
            .build();

    manager.notify(1, notification);
}
```

**XML code:** Not required beyond a Button to trigger `showNotification()`.

**AndroidManifest code:** For Android 13+ (API 33+), a runtime permission is additionally required:
```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

**Output / result:** A notification with the title "New Message" and body "You have a new message" appears in the status bar/notification tray, disappearing on tap because of `setAutoCancel(true)`.

**Line-by-line explanation of the code:**
- `NotificationManager manager = getSystemService(NotificationManager.class);` — gets the system service that actually posts notifications.
- `if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O)` — Notification Channels are required from Android 8.0 (Oreo) onward; older versions don't need this block.
- `new NotificationChannel(channelId, "My Channel", IMPORTANCE_DEFAULT)` — creates a channel (a category of notifications the user can control independently in system settings).
- `manager.createNotificationChannel(channel);` — registers the channel with the system (safe to call repeatedly — it won't recreate an identical channel).
- `new NotificationCompat.Builder(this, channelId)` — starts building the actual notification, tied to the channel created above.
- `.setSmallIcon(...)` — **required**; a notification without a small icon will crash or silently fail.
- `.setAutoCancel(true)` — automatically dismisses the notification once the user taps it.
- `manager.notify(1, notification);` — actually posts the notification; the `1` is a unique ID used to update or cancel this specific notification later.

**Viva / oral exam questions:**
- Why are Notification Channels required from Android 8.0 onward?
- What does the numeric ID in `manager.notify(1, ...)` do?
- What's the difference between `setContentTitle()` and `setContentText()`?

**Common mistakes students make:**
- Forgetting `setSmallIcon()` — the single most common reason a notification silently fails to show.
- Not creating a NotificationChannel on Android 8.0+ devices, causing the notification to not appear at all.
- Reusing the same notify ID unintentionally, overwriting a different notification.

**Memory trick / quick revision point:**
**"Channel first (Oreo+), then Builder, then Notify"** — three-step mental checklist for every notification.

---

## Q4.5 — AsyncTaskLoader with Callback Methods
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"Discuss in detail with the help of necessary block diagram the implementation of Async Task Loader. Give details of its subclass and call back methods" is a full 10-mark question in the 2021-22 CIE Re-Test paper.

**Concept explanation in simple words:**
A Loader is similar to AsyncTask but specifically designed to survive configuration changes (like screen rotation) without restarting the background work, and it automatically monitors its data source for changes.

**Complete code:**
```java
public class MyLoader extends AsyncTaskLoader<List<String>> {

    public MyLoader(Context context) {
        super(context);
    }

    @Override
    protected void onStartLoading() {
        forceLoad(); // triggers loadInBackground()
    }

    @Override
    public List<String> loadInBackground() {
        List<String> data = new ArrayList<>();
        // simulate fetching data
        data.add("Item 1");
        data.add("Item 2");
        return data;
    }
}
```
```java
// Implementing LoaderManager.LoaderCallbacks in an Activity/Fragment
public class MainActivity extends AppCompatActivity
        implements LoaderManager.LoaderCallbacks<List<String>> {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getSupportLoaderManager().initLoader(0, null, this);
    }

    @NonNull
    @Override
    public Loader<List<String>> onCreateLoader(int id, Bundle args) {
        return new MyLoader(this);
    }

    @Override
    public void onLoadFinished(@NonNull Loader<List<String>> loader, List<String> data) {
        // update UI with loaded data here
        Log.i("Loader", "Data loaded: " + data.toString());
    }

    @Override
    public void onLoaderReset(@NonNull Loader<List<String>> loader) {
        // clean up references to the old data
    }
}
```

**Output / result:** Logcat prints `Data loaded: [Item 1, Item 2]` once loading finishes; rotating the screen while loading does NOT restart the load from scratch (unlike a plain AsyncTask would).

**Line-by-line explanation of the code:**
- `extends AsyncTaskLoader<List<String>>` — the generic type is the kind of data this Loader produces.
- `onStartLoading()` — called when the Loader becomes active; `forceLoad()` here triggers the actual background work.
- `loadInBackground()` — runs on a background thread, similar to AsyncTask's `doInBackground`; returns the loaded data.
- `getSupportLoaderManager().initLoader(0, null, this);` — starts (or reconnects to an existing) Loader with ID `0`.
- `onCreateLoader(int id, Bundle args)` — must return a new instance of your Loader subclass.
- `onLoadFinished(Loader loader, List<String> data)` — called on the UI thread once loading completes; safe to update UI here.
- `onLoaderReset(Loader loader)` — called when the Loader is being reset/destroyed; used to release any references to the old data to avoid memory leaks.

**Viva / oral exam questions:**
- What is the key advantage of Loader over a plain AsyncTask regarding configuration changes?
- What are the three `LoaderManager.LoaderCallbacks` methods?
- What does `forceLoad()` do?

**Common mistakes students make:**
- Confusing `onLoadFinished` (safe to touch UI) with `loadInBackground` (must not touch UI).
- Forgetting to call `initLoader()` to actually kick off the process.
- Not implementing all three required callback methods.

**Memory trick / quick revision point:**
**"Loader survives rotation, AsyncTask doesn't"** — the single biggest reason to prefer Loader for long-running data-loading tasks tied to UI.

---

## Q4.6 — Service: Started vs Bound (with Lifecycle)
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"With appropriate examples and service life cycle clarify when to use service – android app component. Compare started services vs bound services" is a direct 6-mark question in the CIE-2 (Nov 2019) paper.

**Concept explanation in simple words:**
A Service is a component that runs in the background **without a visible UI** — like playing music while you use other apps. A "started" service runs independently until told to stop; a "bound" service is tied to whichever component connected to it and stops when nobody's connected anymore.

**Complete code:**
```java
public class MyService extends Service {

    @Override
    public void onCreate() {
        super.onCreate();
        // Service is being created
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // Called when startService() is used - do background work here
        return START_STICKY;
    }

    @Override
    public IBinder onBind(Intent intent) {
        // Called when bindService() is used - return a Binder for bound services
        return null;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        // Service is no longer used and is being destroyed
    }
}
```

**AndroidManifest code:**
```xml
<service android:name=".MyService" />
```

**Output / result:** Calling `startService(new Intent(this, MyService.class));` runs the Service's `onStartCommand()`; it keeps running even after the calling Activity is closed, until `stopService()`/`stopSelf()` is called.

**Line-by-line explanation of the code / comparison:**

| | Started Service | Bound Service |
|---|---|---|
| Started by | `startService()` | `bindService()` |
| Stops when | Explicitly stopped via `stopService()`/`stopSelf()` | All clients unbind |
| Use case | One-off background task, independent of any UI component | Ongoing interaction/IPC with a component (e.g., music controls) |
| Key method | `onStartCommand()` | `onBind()` |

- `onCreate()` — called once, when the Service is first created (either way it's started).
- `onStartCommand(Intent, flags, startId)` — called every time `startService()` is invoked; `START_STICKY` tells Android to recreate the Service if it's killed to free memory, without redelivering the last Intent.
- `onBind(Intent intent)` — must return an `IBinder` object for bound services to communicate with; returning `null` means this Service cannot be bound to.
- `onDestroy()` — cleanup when the Service is finally being removed.

**Viva / oral exam questions:**
- When would you use a bound Service instead of a started Service?
- What does `START_STICKY` mean?
- What's returned from `onBind()` if a Service is meant to be started-only, never bound?

**Common mistakes students make:**
- Forgetting to declare the Service in the manifest.
- Confusing `onStartCommand` with `onBind` — using the wrong one for the wrong service type.
- Not calling `stopSelf()`/`stopService()`, causing a started service to run forever in the background, draining battery.

**Memory trick / quick revision point:**
**"Started = independent, runs till stopped. Bound = dependent, runs till abandoned."**

---

## Q4.7 — Send SMS Using SmsManager
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Explicitly listed in the possible-coding-questions notes ("Write code to send an SMS using SmsManager") and matches the syllabus's "communicating with SMS and emails" topic.

**Concept explanation in simple words:**
`SmsManager` lets your app send an SMS text message programmatically, without opening the default Messages app.

**Complete code:**
```java
SmsManager smsManager = SmsManager.getDefault();
smsManager.sendTextMessage("9538860055", null, "Hello from my app!", null, null);
```

**AndroidManifest code:**
```xml
<uses-permission android:name="android.permission.SEND_SMS" />
```

**Output / result:** An SMS reading "Hello from my app!" is silently sent to the number 9538860055, with no visible UI (this needs the runtime permission granted first on Android 6.0+).

**Line-by-line explanation of the code:**
- `SmsManager.getDefault();` — gets the system's default SMS manager instance.
- `sendTextMessage(destinationAddress, scAddress, text, sentIntent, deliveryIntent)` — the 5 parameters are: phone number, service center address (usually `null` to use the default), the message text, and two optional `PendingIntent`s for tracking sent/delivered status (both `null` here since we don't need tracking).

**Viva / oral exam questions:**
- What permission is required to send SMS, and is it a normal or dangerous permission?
- What do the last two `null` parameters in `sendTextMessage()` represent, and when would you use them?

**Common mistakes students make:**
- Forgetting the `SEND_SMS` permission (a **dangerous** permission requiring runtime request on Android 6.0+, not just a manifest declaration).
- Wrong parameter order in `sendTextMessage()`.

**Memory trick / quick revision point:**
**"SEND_SMS is a dangerous permission"** — always pair the manifest declaration with a runtime permission request (see Unit 5, Q5.1).

---

## Q4.8 — WebView, Last Known Location & Alarms (Grouped Quick Reference)
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Each of these appears at least once across the possible-coding-questions notes and older Closed Book Test papers ("Write in detail about different map types", "With code samples demonstrate how to get the last known location", "Explain elapsed real-time alarms and Real-time clock alarms").

**Concept explanation in simple words:**
- **WebView** shows a web page inside your own app (instead of opening a separate browser).
- **Last known location** quickly retrieves the device's most recently recorded GPS/network position without waiting for a brand-new fix.
- **Alarms** schedule code to run at a future time, even if the app isn't open.

**Complete code:**
```java
// WebView
WebView webView = findViewById(R.id.webView);
webView.setWebViewClient(new WebViewClient());
webView.getSettings().setJavaScriptEnabled(true);
webView.loadUrl("https://www.rvce.edu.in");
```
```java
// Last known location
FusedLocationProviderClient client = LocationServices.getFusedLocationProviderClient(this);
if (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION)
        == PackageManager.PERMISSION_GRANTED) {
    client.getLastLocation().addOnSuccessListener(location -> {
        if (location != null) {
            Log.i("Location", location.getLatitude() + ", " + location.getLongitude());
        }
    });
}
```
```java
// Elapsed Real-time Alarm and RTC Alarm
AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
Intent intent = new Intent(this, AlarmReceiver.class);
PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 0, intent,
        PendingIntent.FLAG_IMMUTABLE);

// Fires 60 seconds after now, measured from device boot; survives sleep
alarmManager.set(AlarmManager.ELAPSED_REALTIME_WAKEUP,
        SystemClock.elapsedRealtime() + 60000, pendingIntent);

// Fires at a specific wall-clock time
alarmManager.set(AlarmManager.RTC_WAKEUP,
        System.currentTimeMillis() + 60000, pendingIntent);
```

**XML code:**
```xml
<WebView
    android:id="@+id/webView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

**AndroidManifest code:**
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

**Output / result:** The WebView displays the RVCE website inside the app itself. The location code logs the device's last known coordinates. The alarm code triggers `AlarmReceiver.onReceive()` 60 seconds later.

**Line-by-line explanation of the code:**
- `webView.setWebViewClient(new WebViewClient());` — ensures links open *inside* the WebView instead of launching an external browser.
- `getSettings().setJavaScriptEnabled(true);` — required if the web page uses JavaScript (off by default for security).
- `getLastLocation()` — returns a cached location instantly (may be `null` if never fetched before), much faster than requesting a brand-new GPS fix.
- `AlarmManager.ELAPSED_REALTIME_WAKEUP` vs `AlarmManager.RTC_WAKEUP` — the first counts time since device boot (good for "X minutes from now" timers); the second uses actual wall-clock date/time (good for "at 6 PM" alarms). The `_WAKEUP` suffix means it can wake a sleeping device.

**Viva / oral exam questions:**
- Why must `setJavaScriptEnabled(true)` be set explicitly?
- What's the difference between Elapsed Real-time and RTC alarms?
- Why might `getLastLocation()` return `null`?

**Common mistakes students make:**
- Forgetting the `INTERNET` permission for WebView.
- Not checking location permission before calling `getLastLocation()` (crashes with `SecurityException`).
- Confusing elapsed-time-based alarms with wall-clock-based alarms.

**Memory trick / quick revision point:**
**"Elapsed = stopwatch since boot, RTC = wall clock"**.

---

# UNIT 5: Hardware Support & Devices — Permissions, Firebase, Maps, Security, Publishing

*(Syllabus: Permissions and Libraries, Performance and Security, Firebase and AdMob, Publish and Polish, Multiple Form Factors, Using Google Services)*

## Q5.1 — Request a Runtime Permission
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"List and explain the types of permissions in android and three best practices" and "Explain the types of permission in android with respect to Grant and revoke / Types of permissions / Best practices" are asked as full 10-mark questions in multiple papers (2020-21 Re-Test, Model QP), and the coding part is required to support Units 4 and 5 both.

**Concept explanation in simple words:**
Some Android features are considered sensitive (camera, location, SMS, contacts) — the app must explicitly ask the user for permission **at runtime** (while the app is running), not just declare it in the manifest, starting from Android 6.0.

**Complete answer:**
**Types of permissions:**
- **Normal** — auto-granted at install time, low risk (e.g., `INTERNET`, `ACCESS_NETWORK_STATE`).
- **Dangerous** — needs explicit user approval at runtime (e.g., `CAMERA`, `ACCESS_FINE_LOCATION`, `SEND_SMS`, `READ_CONTACTS`).
- **Signature** — automatically granted only to apps signed with the same certificate as the app that defined the permission.

**Grant and revoke:** The user grants a dangerous permission via a system dialog; they can **revoke** it any time later from Settings → Apps → Permissions — so your app must always re-check before using the protected feature, never assume a past grant still holds.

**Best practices:** Request only when needed (just-in-time, not all at app launch), explain *why* you need it (a rationale dialog) before asking, request the minimum necessary permissions, and handle denial gracefully instead of crashing.

**Complete code:**
```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.CAMERA)
        != PackageManager.PERMISSION_GRANTED) {

    if (ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.CAMERA)) {
        Toast.makeText(this, "Camera is needed to scan documents", Toast.LENGTH_LONG).show();
    }

    ActivityCompat.requestPermissions(this,
            new String[]{Manifest.permission.CAMERA}, 100);
} else {
    // permission already granted - proceed
    openCamera();
}

@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                        @NonNull int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    if (requestCode == 100) {
        if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            Toast.makeText(this, "Camera permission granted", Toast.LENGTH_SHORT).show();
            openCamera();
        } else {
            Toast.makeText(this, "Permission denied", Toast.LENGTH_SHORT).show();
        }
    }
}
```

**XML code:** Not applicable.

**AndroidManifest code:**
```xml
<uses-permission android:name="android.permission.CAMERA" />
```

**Output / result:** On first use, a system dialog pops up asking "Allow App to take pictures and record video?" with Allow/Deny options. Choosing Allow shows a Toast "Camera permission granted" and opens the camera; choosing Deny shows "Permission denied".

**Line-by-line explanation of the code:**
- `ContextCompat.checkSelfPermission(this, permission)` — checks if the permission is already granted, returning either `PackageManager.PERMISSION_GRANTED` or `PERMISSION_DENIED`.
- `ActivityCompat.shouldShowRequestPermissionRationale(...)` — returns `true` if the user previously denied this permission once (a good moment to explain *why* you need it before asking again).
- `ActivityCompat.requestPermissions(this, permissionsArray, requestCode)` — actually triggers the system permission dialog; `requestCode` is a number you choose to identify this particular request later.
- `onRequestPermissionsResult(...)` — a callback automatically invoked once the user responds to the dialog; you check `requestCode` to know which request this result belongs to, and `grantResults[0]` to know if it was approved.

**Viva / oral exam questions:**
- What's the difference between a Normal and a Dangerous permission?
- Why must you re-check permission status even if it was granted before?
- What does `shouldShowRequestPermissionRationale()` help you do?

**Common mistakes students make:**
- Forgetting to check `grantResults.length > 0` before accessing `grantResults[0]` (can throw `ArrayIndexOutOfBoundsException` if the request was cancelled).
- Assuming a permission granted once stays granted forever (user can revoke it anytime from Settings).
- Not declaring the permission in the manifest at all before requesting it at runtime (runtime request fails silently without the manifest entry).

**Memory trick / quick revision point:**
**"Check → Explain (if needed) → Request → Handle Result"** — a repeatable 4-step pattern for every dangerous permission.

---

## Q5.2 — Firebase Realtime Database: Read and Write
**Importance:** ⭐⭐⭐⭐⭐ Must Study

**Why this question is important:**
"What is FireBase? Explain firebase realtime database with example" is asked as a full 10-mark question in at least 3 different Online Test papers (2020-21 series) and again in the CIE-Improvement 2025-26 paper.

**Concept explanation in simple words:**
Firebase Realtime Database is a cloud-hosted database from Google that stores data as JSON and automatically syncs it to every connected device in real time — if one user updates data, every other connected app instantly sees the change, even while offline (it syncs once reconnected).

**Complete code:**
```java
// build.gradle (Module): implementation 'com.google.firebase:firebase-database:20.x.x'

public class Student {
    public String name;
    public int marks;
    public Student() {} // required empty constructor for Firebase
    public Student(String name, int marks) {
        this.name = name;
        this.marks = marks;
    }
}
```
```java
DatabaseReference dbRef = FirebaseDatabase.getInstance().getReference("students");

// WRITE
dbRef.child("s1").setValue(new Student("Rekha", 90));

// READ (real-time listener - fires immediately and again on every future change)
dbRef.addValueEventListener(new ValueEventListener() {
    @Override
    public void onDataChange(@NonNull DataSnapshot snapshot) {
        for (DataSnapshot child : snapshot.getChildren()) {
            Student s = child.getValue(Student.class);
            Log.i("Firebase", s.name + " scored " + s.marks);
        }
    }

    @Override
    public void onCancelled(@NonNull DatabaseError error) {
        Log.e("Firebase", "Read failed: " + error.getMessage());
    }
});
```

**AndroidManifest code:** No special manifest entry is needed beyond internet permission:
```xml
<uses-permission android:name="android.permission.INTERNET" />
```

**Output / result:** In the Firebase Console, a new JSON node appears: `students/s1 = {name: "Rekha", marks: 90}`. Logcat prints "Rekha scored 90" immediately, and would print again automatically if that data changes from any other connected device.

**Line-by-line explanation of the code:**
- `public Student() {}` — Firebase requires an empty (no-argument) constructor to be able to reconstruct objects from JSON automatically.
- `FirebaseDatabase.getInstance().getReference("students")` — gets a reference pointing to the `students` node in the database tree.
- `dbRef.child("s1").setValue(new Student(...))` — writes a new Student object as JSON under the child key `"s1"`.
- `addValueEventListener(...)` — attaches a **real-time** listener; unlike a one-time read, this callback fires again automatically whenever the underlying data changes.
- `onDataChange(DataSnapshot snapshot)` — receives the current state of the data as a `DataSnapshot`; `snapshot.getChildren()` iterates over each child node.
- `child.getValue(Student.class)` — automatically converts the JSON child back into a `Student` Java object.
- `onCancelled(DatabaseError error)` — called if the read is cancelled/fails (e.g., due to permission rules), letting you handle errors gracefully.

**Viva / oral exam questions:**
- Why does the `Student` class need an empty constructor?
- What is the difference between `addValueEventListener` and a one-time read (`addListenerForSingleValueEvent`)?
- What happens to Firebase writes made while the device is offline?

**Common mistakes students make:**
- Forgetting the empty constructor on the data model class, causing a runtime exception when Firebase tries to deserialize data.
- Using `addValueEventListener` when only a single read was intended (keeps listening forever unless removed, wasting resources).
- Forgetting the `INTERNET` permission.

**Memory trick / quick revision point:**
**"Firebase = shared live notebook"** — anyone connected sees updates instantly, like everyone writing in the same notebook at once.

---

## Q5.3 — Google Maps: Setup and Configure Initial State
**Importance:** ⭐⭐⭐⭐ Very Important

**Why this question is important:**
"Write in detail about different map types offered by Google Maps Android API. With code segments explain how to configure the initial state of the map" is a full 10-mark question in the 2020-21 Closed Book Test-3 paper.

**Concept explanation in simple words:**
Google Maps in an app shows an interactive map; you configure things like which spot it centers on when it first opens, how zoomed-in it is, and what visual style (normal/satellite/terrain) to use.

**Complete code:**
```java
public class MapsActivity extends AppCompatActivity implements OnMapReadyCallback {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_maps);
        SupportMapFragment mapFragment = (SupportMapFragment)
                getSupportFragmentManager().findFragmentById(R.id.map);
        mapFragment.getMapAsync(this);
    }

    @Override
    public void onMapReady(GoogleMap googleMap) {
        LatLng rvce = new LatLng(12.9237, 77.4986);
        googleMap.addMarker(new MarkerOptions().position(rvce).title("RVCE"));
        googleMap.moveCamera(CameraUpdateFactory.newLatLngZoom(rvce, 15));
        googleMap.setMapType(GoogleMap.MAP_TYPE_NORMAL);
        googleMap.getUiSettings().setZoomControlsEnabled(true);
    }
}
```

**XML code:**
```xml
<!-- res/layout/activity_maps.xml -->
<fragment
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/map"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:name="com.google.android.gms.maps.SupportMapFragment" />
```

**AndroidManifest code:**
```xml
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<meta-data
    android:name="com.google.android.geo.API_KEY"
    android:value="YOUR_API_KEY_HERE" />
```

**Output / result:** The app opens showing a Google Map centered on RVCE with a marker labeled "RVCE", zoomed in at level 15 (street-level detail), using the standard road map style.

**Line-by-line explanation of the code:**
- `implements OnMapReadyCallback` — required so `onMapReady()` is called automatically once the map has finished loading.
- `getSupportFragmentManager().findFragmentById(R.id.map)` — locates the map Fragment declared in the XML.
- `mapFragment.getMapAsync(this);` — asynchronously loads the map and calls back to `onMapReady()` on this Activity.
- `new LatLng(12.9237, 77.4986)` — a coordinate object (latitude, longitude) for RVCE's location.
- `googleMap.addMarker(new MarkerOptions().position(rvce).title("RVCE"));` — places a pin at that location with a label.
- `googleMap.moveCamera(CameraUpdateFactory.newLatLngZoom(rvce, 15));` — instantly moves the visible map view (camera) to center on that point, zoomed to level 15 (higher number = more zoomed in).
- `googleMap.setMapType(GoogleMap.MAP_TYPE_NORMAL);` — other options include `MAP_TYPE_SATELLITE`, `MAP_TYPE_TERRAIN`, `MAP_TYPE_HYBRID`.
- `googleMap.getUiSettings().setZoomControlsEnabled(true);` — shows the +/- zoom buttons on screen.

**Map types table:**

| Map Type | Description |
|---|---|
| `MAP_TYPE_NORMAL` | Standard road map |
| `MAP_TYPE_SATELLITE` | Satellite photography, no labels |
| `MAP_TYPE_TERRAIN` | Topographic detail (elevation, vegetation) |
| `MAP_TYPE_HYBRID` | Satellite photography WITH road labels |

**Viva / oral exam questions:**
- What is the purpose of `OnMapReadyCallback`?
- Name all four map types and briefly describe each.
- What does the second parameter in `newLatLngZoom(latLng, zoom)` control?

**Common mistakes students make:**
- Forgetting to add the API key in the manifest — the map shows a blank grey grid instead.
- Confusing `moveCamera()` (instant jump) with `animateCamera()` (smooth animated transition).
- Forgetting `ACCESS_FINE_LOCATION` permission when trying to show the user's current position on the map.

**Memory trick / quick revision point:**
**"NSTH = Normal, Satellite, Terrain, Hybrid"** — the four map types, remembered together as one acronym.

---

## Q5.4 — Publish an Android App to the Google Play Store
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"Explain the high level tasks for publishing android app to the Google Play store" and "Discuss the steps in detail of Polish and Publishing of the Android App developed" are both full 10-mark questions across multiple papers.

**Concept explanation in simple words:**
Publishing isn't just uploading a file — it involves preparing (polishing) the app so it looks professional and works reliably, then going through Google's store submission process.

**Complete answer (steps):**
1. **Polish the app first:** test thoroughly on multiple devices/screen sizes, remove debug `Log` statements, optimize image/resource sizes, handle crashes and edge cases gracefully, follow Material Design guidelines for a professional look.
2. **Generate a signed release build** — Build → Generate Signed Bundle/APK, using a securely stored keystore file (losing this keystore means you can never update the app again under the same listing!).
3. **Create a Google Play Console developer account** (one-time registration fee).
4. **Create a new app entry** — fill in title, short/full description, screenshots, feature graphic, app icon.
5. **Set content rating** via Google's content rating questionnaire.
6. **Add a privacy policy URL** (mandatory if the app collects any user data).
7. **Choose a release track** — Internal testing → Closed testing → Open testing → Production (staged rollout recommended).
8. **Set pricing and country distribution.**
9. **Submit for review** — Google reviews the app (can take hours to a few days) before it goes live.
10. **Monitor post-launch** — check crash reports, user reviews, and update as needed.

**Complete code:** Not applicable — this is a process/procedure question, no code required.

**Output / result:** N/A (procedural).

**Line-by-line explanation:** N/A — see numbered steps above; explain each briefly in your answer script.

**Viva / oral exam questions:**
- Why is losing your app-signing keystore a serious problem?
- What is a "staged rollout" and why would a developer use one?
- What must be provided if your app collects personal user data?

**Common mistakes students make:**
- Listing only 3-4 steps instead of the full process (loses marks on a 10-mark question — always aim for at least 6-8 clear steps).
- Forgetting to mention the signed build / keystore step, which is often specifically tested.
- Not distinguishing "polish" activities from "publish" activities when the question asks for both.

**Memory trick / quick revision point:**
**"Polish, Package, Publish"** — three P's: polish the app quality, package it (signed build), publish it (Play Console submission).

---

## Q5.5 — Security Best Practices in Android App Development
**Importance:** ⭐⭐⭐ Important

**Why this question is important:**
"List and explain best practices for security" and "Clarify about privacy best practices in android apps with respect to permissions" are recurring 10-mark questions across the Online Test series.

**Concept explanation in simple words:**
Security best practices are simple habits that prevent your app's data (and the user's data) from being stolen, leaked, or misused.

**Complete answer:**
- Use **HTTPS** for all network communication, never plain HTTP.
- Store sensitive data using `EncryptedSharedPreferences`, never plain `SharedPreferences`, for things like tokens or passwords.
- Request the **minimum permissions** necessary — don't ask for Camera access if you only need to pick an existing photo.
- **Validate all user input** before using it (especially before database queries) to avoid injection attacks.
- **Never hardcode API keys/secrets** directly in Java/Kotlin source code (they can be extracted from the APK).
- Use **ProGuard/R8** to obfuscate and shrink code in release builds, making reverse-engineering harder.
- **Sign your APK/AAB** with a securely stored keystore; never share or lose it.
- Keep all dependencies/libraries **updated** to patch known security vulnerabilities.

**Complete code:** Not applicable (theory question), but one illustrative example:
```java
// Bad practice: storing a token in plain SharedPreferences
prefs.edit().putString("auth_token", token).apply();

// Better practice: use EncryptedSharedPreferences (androidx.security library)
MasterKey masterKey = new MasterKey.Builder(context)
        .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
        .build();
SharedPreferences securePrefs = EncryptedSharedPreferences.create(
        context, "secure_prefs", masterKey,
        EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
        EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM);
securePrefs.edit().putString("auth_token", token).apply();
```

**Output / result:** N/A (both approaches store the token, but the second is unreadable if the device storage is extracted or rooted, while the first is stored as readable plain text).

**Line-by-line explanation of the code:**
- `MasterKey.Builder(context).setKeyScheme(AES256_GCM).build();` — generates/retrieves a securely stored master encryption key.
- `EncryptedSharedPreferences.create(...)` — creates a SharedPreferences instance that automatically encrypts/decrypts every key and value transparently.

**Viva / oral exam questions:**
- Why is storing an auth token in plain SharedPreferences risky?
- What does ProGuard/R8 do for security?
- Why should permission requests be minimized?

**Common mistakes students make:**
- Only mentioning 2-3 practices instead of a comprehensive list for a full 10-mark answer.
- Confusing HTTPS (network security) with local storage security (different layers of the same overall topic).

**Memory trick / quick revision point:**
**"HTTPS, Encrypt, Minimum Permissions, No Hardcoded Secrets"** — the four most-important, most-quotable practices to lead your answer with.

---

## Q5.6 — Geocoding and Reverse Geocoding
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
"Briefly discuss about the class and its methods which is used for geocoding and reverse geocoding in android apps" is a direct 4-mark question in the 2020-21 Closed Book Test-3 paper.

**Concept explanation in simple words:**
**Geocoding** converts a human-readable address into GPS coordinates (e.g., "RVCE Bengaluru" → 12.9237, 77.4986). **Reverse geocoding** does the opposite — coordinates into a readable address.

**Complete code:**
```java
Geocoder geocoder = new Geocoder(this, Locale.getDefault());

// Forward geocoding: address -> lat/lng
try {
    List<Address> addresses = geocoder.getFromLocationName("RVCE Bengaluru", 1);
    if (!addresses.isEmpty()) {
        double lat = addresses.get(0).getLatitude();
        double lng = addresses.get(0).getLongitude();
        Log.i("Geocoder", lat + ", " + lng);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// Reverse geocoding: lat/lng -> address
try {
    List<Address> results = geocoder.getFromLocation(12.9237, 77.4986, 1);
    if (!results.isEmpty()) {
        String addressLine = results.get(0).getAddressLine(0);
        Log.i("Geocoder", addressLine);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

**Output / result:** The first block logs coordinates close to `12.9237, 77.4986`; the second logs a readable address string for that location.

**Line-by-line explanation of the code:**
- `new Geocoder(this, Locale.getDefault());` — creates a Geocoder tied to the device's current language/locale for formatting results.
- `getFromLocationName("RVCE Bengaluru", 1)` — the `1` means "return at most 1 matching result"; returns a `List<Address>`.
- `getFromLocation(12.9237, 77.4986, 1)` — same idea in reverse, taking coordinates and returning nearby address matches.
- `.getAddressLine(0)` — the first formatted line of the resulting address (e.g., street + city).

**Viva / oral exam questions:**
- What is the difference between geocoding and reverse geocoding?
- What class is used for both operations?
- Why should Geocoder calls be wrapped in a try-catch?

**Common mistakes students make:**
- Not handling the case where the returned list is empty (`IndexOutOfBoundsException` if you call `.get(0)` without checking).
- Forgetting the `try-catch` — these methods throw `IOException` since they may need network access.

**Memory trick / quick revision point:**
**"Geocode = words to coordinates, Reverse = coordinates to words"** — same direction relationship as "encode/decode".

---

## Q5.7 — Firebase + AdMob Connectivity
**Importance:** ⭐⭐ Good to Know

**Why this question is important:**
Directly asked ("Show the connectivity between Firebase and AdMob") in the RVCE Model Question Paper Part-A, and "Explain in detail: Firebase and Admob" in the 2025-26 CIE-Improvement paper.

**Concept explanation in simple words:**
AdMob is Google's mobile advertising platform (shows ads in your app to earn money). When linked with Firebase, you get combined analytics — you can see not just how much money ads made, but also connect that to user behavior data (like which users saw ads before making a purchase).

**Complete answer:**
Firebase and AdMob link through the Firebase console: an AdMob account is connected to a Firebase project, enabling **Google Analytics for Firebase** events to inform ad targeting and measurement. This lets a developer see unified reporting — ad revenue alongside user engagement, retention, and conversion metrics — all inside one Firebase dashboard, rather than needing two separate disconnected tools.

**Complete code:** Not applicable (conceptual/connectivity question) — but a basic AdMob banner setup for reference:
```java
MobileAds.initialize(this, initializationStatus -> {});

AdView adView = findViewById(R.id.adView);
AdRequest adRequest = new AdRequest.Builder().build();
adView.loadAd(adRequest);
```

**XML code:**
```xml
<com.google.android.gms.ads.AdView
    android:id="@+id/adView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    ads:adSize="BANNER"
    ads:adUnitId="ca-app-pub-3940256099942544/6300978111" />
```

**AndroidManifest code:**
```xml
<meta-data
    android:name="com.google.android.gms.ads.APPLICATION_ID"
    android:value="ca-app-pub-3940256099942544~3347511713" />
```

**Output / result:** A banner ad appears where the `AdView` is placed in the layout; Firebase Analytics (if linked) automatically logs ad impression/click events alongside your app's normal user events.

**Line-by-line explanation of the code:**
- `MobileAds.initialize(this, ...);` — must be called once, usually in `onCreate()`, before loading any ads.
- `AdView` (XML) — the actual ad container view; `ads:adUnitId` identifies which ad slot/account this ad belongs to (the example ID shown is Google's public test ad unit).
- `adView.loadAd(new AdRequest.Builder().build());` — requests and loads an ad into that view.

**Viva / oral exam questions:**
- What is the benefit of linking AdMob with Firebase instead of using AdMob alone?
- What does the `APPLICATION_ID` meta-data entry in the manifest do?
- Name one Firebase feature (besides Analytics) useful alongside AdMob.

**Common mistakes students make:**
- Treating Firebase and AdMob as unrelated tools instead of explaining their linked-analytics relationship (the actual point of this question).
- Forgetting `MobileAds.initialize()` before loading ads.

**Memory trick / quick revision point:**
**"AdMob earns, Firebase explains"** — AdMob generates ad revenue; Firebase Analytics explains the "why" behind the numbers by connecting it to user behavior.

---

# Final Revision Checklist

| Unit | Top 3 "Must Study" Questions |
|---|---|
| Unit 1 | Activity Lifecycle code, AndroidManifest structure, Software Stack diagram |
| Unit 2 | Button click techniques, Explicit Intent (Welcome app), Implicit Intent |
| Unit 3 | SQLite CRUD, SharedPreferences, Fragment + RecyclerView |
| Unit 4 | AsyncTask (4 methods), BroadcastReceiver (Power Connected), Network check |
| Unit 5 | Runtime Permissions, Firebase Realtime Database, Google Maps setup |

**General exam-writing tips (apply to every unit):**
- Always draw the diagram when a question says "with a neat diagram/sketch" — even a rough hand-drawn version earns marks a text-only answer won't.
- For every code answer, write the **XML layout too** if a UI element is involved — many students lose marks for skipping XML even when it wasn't explicitly demanded.
- Comment your code lightly (`// this handles the click`) — several papers explicitly say "students need to add comments to their answers wherever required."
- For "compare X vs Y" questions, use a table — it's faster to write and easier for evaluators to award full marks.

---

*This handbook was compiled by cross-referencing every CIE, Test, Quiz, Open-Book, and Model Question Paper you uploaded, including the most recent 2025-26 CIE-1/CIE-2/CIE-Improvement papers (Faculty: RBS). Treat it as a structured practice base — always cross-check specific theory phrasing and diagrams against your own class notes, since faculty wording can vary by section.*
