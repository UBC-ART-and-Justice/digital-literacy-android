# android-app
Android app for the project, to run the course content on the tablets.

## Creating a new version of the app with updated content

<!-- TODO: create a script to do this process -->

1. Build the new version of the course content by running `mkdocs build` in the [course content](https://github.com/UBC-ART-and-Justice/course-content) repository.
2. Copy the contents of the `site` folder in the course content repository to the `app/src/main/assets/site` folder in this repository.
3. Delete the `sitemap.xml.gz` file from the `site` folder in this repository, or else this will cause a file conflict when building the app. It is not needed for the app.
4. **OPTIONAL** Test the app on the device to ensure everything is working as expected. 
    - Make sure the device has gone through the [tablet configuration](https://github.com/UBC-ART-and-Justice/tablet-configuration) process.
    - Connect the device to the computer
    - In Android Studio, see if `Lenovo TB-8506F` is listed in the `Connected Devices` dropdown in the top center of the window.
    - Click the green play button and wait for the app to be installed on the device.
5. Build the app by clicking `Build > Build Bundle(s) / APK(s) > Build APK(s)` in the top menu of Android Studio. You can find the APK file in the `app/build/outputs/apk/debug` folder.






## Setting up the project from scratch

This is included in case the app needs to be recreated from scratch.


**Initial setup:**

1. Install Android Studio
2. Clone this repository and clear the contents, or create a new repository
3. Open Android Studio and create a new project
    - Select `Empty Activity` and click `Next`
    - Enter a **Name** (ex. `Digital Literacy`) and leave the **Package name** as is
    - Swap the **Save location** to the repository folder
    - Leave the **Minimum SDK** as is (our tablets are on Android 11 so don't pick anything higher)
    - Ensure that the **Build configuration language** is set to `Kotlin DSL`
    - Click `Finish`
4. Wait for some time for the system to set everything up. You can check the progress in the bottom right corner of the window.

**Configuring WebView:**

1. Once everything is set up, use the menu on the left to swap from `Android` to `Project`. This will let you see the file structure of the project.
2. Navigate to `app > src > main > java > [...]` and open the `MainActivity.kt` file. Then do the following:

    In the **imports** section at the top of the file, add the following line:
    ```kotlin
    import android.webkit.WebView
    import android.webkit.WebViewClient
    import androidx.compose.ui.viewinterop.AndroidView
    ```

    In the `MainActivity` class, swap the contents of the `setContent` function `WebViewScreen()` like this
    ```kotlin
    class MainActivity : ComponentActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContent {
                WebViewScreen()
            }
        }
    }
    ```

    Delete the other functions in the `MainActivity` class (ex. `Greeting()` and `GreetingPreview()`).

    Add the following composable function to the file. This will create a full-screen WebView.
    ```kotlin
    @Composable
    fun WebViewScreen() {
        AndroidView(
            factory = { context ->
                WebView(context).apply {
                    webViewClient = WebViewClient()     // Prevents opening an external browser
                    loadUrl("file:///android_asset/site/index.html") // Load local content
                }
            },
            modifier = Modifier.fillMaxSize() // Make WebView fill the screen
        )
    }
    ```
3. Go to the `Creating a new version of the app with updated content` section to continue.


##  Add a custom icon to the app

While this is not necessary, it can be helpful to differentiate the app from others on the device. See [this guide](https://developer.android.com/codelabs/basic-android-kotlin-compose-training-change-app-icon#0) for more information.

1. Navigate to `app > src > main > res`. The various `mipmap` folders contain the icons for the app.
2. Delete the old drawable resources that contain the Android icon and green grid background. You should **uncheck** the `Safe delete` option when deleting these files, since we are replacing them. We don't want to change any code that references these files.
    ```
    # Delete these files
    drawable/ic_launcher_background.xml
    drawable/ic_launcher_foreground.xml

    # Delete these folders
    mipmap-anydpi-v26/
    mipmap-hdpi/
    mipmap-mdpi/
    mipmap-xhdpi/
    mipmap-xxhdpi/
    mipmap-xxxhdpi/
    ```
3. Create a new Image Asset. You can either right-click on the res directory and choose New > Image Asset, or you can click on the Resource Manager tab, click the + icon, then select Image Asset from the dropdown.
    - Wait for the tool to open
    - Leave most of the default settings as is
    - With `Foreground Layer` selected, click on the `Path` and select the file you want to use. This should be a square image with a transparent background. You can toggle the `Trim` option to see what looks better in the preview.
    - You can do the same for the `Background Layer` if you want to add a background to the icon.
    - Click `Next` and `Finish` to save the icon.
4. Test the app on the device to ensure everything is working as expected.

If you want to see the image files that were used for this app, go to `~/icons/`