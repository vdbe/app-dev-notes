# Android

## Security

- Multi-user Linux system
- Each app
    - Different user
    - Unique uid with permssiosn for own app
    - Own VM
    - Own Linux Process
    - Only access to components it requires (principle of least privilege)

## Aplication structure

### Android manifest
Is en XML based file names 'AndroidManifest.xml'.  
This is used by Google Play to check app compatibility

#### Content
- Package (app) name
- App Components (declerations)
    - Activities
    - Services
    - Broadcast Receivers
    - Content Providers
- Permissions
    - uses-permissions  
        Device features rquired, like Internet or SMS
- Hardware and software features needed
    - uses-feature  
        Hardware features required
    - uses-sdk  
        This will be overwritten by Grade
        - minSDK: Minimum API version
        - targetSDK: Minimum API version  
            Latest version by default


- Code for components
- Resources
- Grade build system

<details>
<summary> Example </summary>

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.appdev">

    <uses-feature android:name="android.hardware.camera.any"
                  android:required="true" />
    <uses-sdk android:minSdkVersion=“21"
              android:targetSdkVersion=“31" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:networkSecurityConfig="@xml/network_security_config"
        android:theme="@style/Theme.AppDev">
        <activity
            android:name=".SettingsActivity"
            android:exported="false" />
        <activity
            android:name=".EaterActivity"
            android:exported="false" />
        <activity
            android:name=".Factory"
            android:exported="false" />
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

</details>

### Application code

#### Android components
##### Activities
- Entry point for interaction with the user
- Single screen with a user interface
- Work together for cohesive user experience
- Independent of eacht other

###### Need a god title here
- Fundamental building block
- Almost all interact with the users
- Activity class creates window
- Lifecycle translates into a lot of methods in the activity

<img src="./assets/activity-structure.png" width="420"/>

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```
- onCreate()
    - Must be implemented
    - Basic application startup logic
    - Can restore previously saved state
- onStart()  
    Makes activity visible to user
- onResume()  
    Intializes components that must occur every time the activity is restarted  
    after being paused
- onPause()  
    Called first after user start leaving activity
    - used to release resourcces no longer necessary
    - Can stay in paused state until activity resume
- onStop()  
    Occurs when activity no longer visible to user
    - Used to release resources not needed when activity not visible
    - Perform CPU-intentsive shutdown operations
- onDestroy()  
    Called when activity destroyed
    - Used to release resources not released yet

##### Services
- Entry point for keepign app running in background
    - Long running operaitons
    - Work for remote processes
- No user interface  

##### Content provider
- Manges a shared set of data that can be stored
    - File system
    - SQLite database
    - online
    - ...
- Lets other apps query or modify data

##### Broadcast receiver
- Enables the sytem to deliver events to an app outside the reuglar user flow
- Allow app to respond to system-wide broadcast announcements
- Many are system events
    - Screen turned off
    - Battery low
    - Picture captured

## Resources
- Provided through xml files
- Visual presentation of the app
    - Images
    - Styles
    - Animations
    - ...
- Simple constants
    - Colors
    - Strings
    - Booleans
- Variety of devices
    - Screen size/density/orientation
- Handler through quoalifiers inthe dir names

### Layouts
Defined in XML

<details>
<summary> Example </summary>

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    tools:context="com.example.android.exampleapp.MainActivity">

    <EditText
        android:id="@+id/edit_text_name_input"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/colorAccent"
        android:hint="Enter your name"
        android:padding="4dp"
        android:textSize="24sp" />

    <TextView
        android:id="@+id/text_view_name_display"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="8dp"
        android:text="Your name appears here"
        android:textSize="30sp" />
</LinearLayout>
```

</details>

#### UI Compontents
| Class Name  | Description                                                 |
|-------------|-------------------------------------------------------------|
| TextView    | Creates text on the screen; generally non interactive text. |
| EditText    | Creates a text input on the screen                          |
| ImageView   | Creates an image on the screen                              |
| Button      | Creates a button on the screen                              |
| Chronometer | Creates a simple timer on screen                            |

#### Layout or container views
| Class Name       | Description                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------|
| LinearLayout     | Displays views in a single column or row.                                                                                 |
| RelativeLayout   | Displays views positioned relative to eacxh other and htis view.                                                          |
| FrameLayout      | A ViewGroup meant to contains a single child view.                                                                        |
| ScrollView       | A FrameLayout that is designed to let the user scroll through the content in the view.                                    |
| ConstraintLayout | This is a newer viewgroup; it postions views in a flexible way. We'll be exploring constraintslayout later in the lesson. |
| GridLayout       | A layout that places its children in a rectangular grid.                                                                  |

#### XML attributes
- Control the properties of a view
- Determine how a biew looks and interacts
- Examples: Text, Widh and height, Padding and Margin, Color

#### Layouts in java
Loading in the onCreate() method of the activity through setContentView()

<details>
<summary> Example </summary>

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // other code to setup the activity
    }
    // other code
}
```
</details>

## Gradle
- Open source build automation system  
    Not only for android
- Scription tool to build software in general  
    Compile code, Link to libraries, Run tests, Create packages, ...
- 3 steps
    1. Initialization  
        Set up environment for build
    2. Configuration  
        - Construct task graph
        - Determine tasks to run and in which order
    3. Execution
        Run the selected tasks
- Build Type 
    Debug, Release
- Manifest Entries
    Overrides value of the manifest: App Name, min API version, target API version

<img src="./assets/gradle-structure.png" width="250"/>

