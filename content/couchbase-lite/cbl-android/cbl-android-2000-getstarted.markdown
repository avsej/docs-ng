# Getting Started
This section contains the information you need to start developing Android apps with Couchbase Lite. 

If you want to play with a demonstration app, you can download and run [GrocerySync](https://github.com/couchbaselabs/GrocerySync-Android).  


## Prerequisites

1. Download and install [Android Studio](http://developer.android.com/sdk/installing/studio.html). 

2. Open the [Android SDK Manager](http://developer.android.com/tools/help/sdk-manager.html) and install **Extras/Google Repository** and **Extras/Android Support Repository** (future versions of Android Studio might make this step unnecessary).

## Adding Couchbase Lite to Your Project
Follow the steps in this section to create a new Android project that uses Couchbase Lite.

**Create a new project:** 

1. Open Android Studio.

2. In the Welcome to Android Studio screen, choose **New Project**.

	This example uses the name MyProject for the new project. 

3. In the New Project window, enter the application name, package name, and project location. 

4. Set the minimum required SDK to **API 11: Android 3.0 (Honeycomb)** or later.

5. Click **Next**, and move through the remaining setup screens.

6. Click **Finish**.

**Add Couchbase Lite Dependencies:**

<p style="border-style:solid;padding:10px;width:90%;margin:0 auto">
<strong>Note</strong>: If you are an advanced user and need to hack or debug Couchbase Lite, you should add the Couchbase Lite dependencies by following the steps in 
<a href="#adding-couchbase-lite-via-direct-code-dependency">Adding Couchbase Lite Via Direct Code Dependency</a> 
rather than the steps in this section.
</p>

1. Expand the **MyProject** folder, and then open the **build.gradle** file. 

	If the **build.gradle** file is empty, then you are looking at the wrong one. Make sure you open the one in the **MyProject** folder.

2. Add the following repositories section to the **build.gradle** file so it can resolve dependencies through Maven Central and the Couchbase Maven repository:

	```java
repositories {
    mavenCentral()
    maven {
        url "http://maven.hq.couchbase.com/nexus/content/repositories/releases/"
    }
    mavenLocal()
}
```

3. If it does not already exist, create a **libs** subdirectory in the **MyProject** directory.

	```bash
$ cd MyProject
$ mkdir libs
$ cd libs
```

4. Download [td_collator_so.jar](http://cl.ly/Pr1r/td_collator_so.jar) into the newly created **libs** directory.  

	```bash
$ wget http://cl.ly/Pr1r/td_collator_so.jar
or
$ curl -OL http://cl.ly/Pr1r/td_collator_so.jar
```

5. In the **build.gradle** file, add the following lines to the top-level dependencies section (not the one under the buildscript section).

	```groovy
dependencies {
    ...
    compile fileTree(dir: 'libs', include: 'td_collator_so.jar')  // hack to add .so objects (bit.ly/17pUlJ1)
    compile 'com.couchbase.cblite:CBLite:0.7'
}
```

6. Make sure that your dependency on the Android Support library looks like this:

	```
    compile 'com.android.support:support-v4:13.0.+'
```


**Build the empty project:**

1. Run the following command to make sure the code builds:

	```sh
	$./gradlew clean && ./gradlew build
	```

2. Restart Android Studio so it knows about the new dependencies and features such as autocomplete work.

**Add code and verify the app runs:**

1. Add the following code to your `onCreate` method in the **MainActivity.java** file:

	```java
String filesDir = getFilesDir().getAbsolutePath();
try {
    CBLServer server = new CBLServer(filesDir);
    server.getDatabaseNamed("hello-cblite");
} catch (IOException e) {
    Log.e("MainActivity", "Error starting TDServer", e);
}
Log.d("MainActivity", "Got this far, woohoo!");        
```

2. In the Android Studio UI, click **Play** to run the app.

	You should see a "Got this far, woohoo!" entry somewhere in the [logcat](http://developer.android.com/tools/help/logcat.html), and the app should not crash.


