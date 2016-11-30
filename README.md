# Camera Material

Download
--------

```groovy
dependencies {
  compile 'com.horsesdeveloper:camera:0.1.3'
}
```
Configuration
--------------------
### Android Manifest

First, you have to register a library Activity from your app's `AndroidManifest.xml` file:

```xml
<activity
    android:name="app.horses.camera.ui.activity.TakeActivity" />
```

### Init configuration

```java
public class App extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        /** Simple Usage (default values) */
        CameraManager.init(this);

        /** Complete usage */
        CameraManager.init(
                new CameraManager.Builder(this)
                    .setPrimaryColor(R.color.colorPrimary)
                    .build());

        ...
    }
}
```

### Add callback and open camera

```java
public class MainActivity extends AppCompatActivity implements CallbackView {

    private CallbackManager callbackManager = new CallbackManager();
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
        /** Open Camera*/
        CameraManager.openCamera(this);
        
        /** Open Camera and set folder path to save images*/
        File root = Environment.getExternalStorageDirectory();
        File dirBase=new File(root, "saved_images");
        CameraManager.openCamera(this,dirBase.getPath());    
            
        /** Set Callback*/
        callbackManager.setCallback(this);
    }
    
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        callbackManager.onActivityResult(requestCode, resultCode, data);
    }
    
    @Override
    public void successCamera(String path) {
        Log.i(TAG, "successCamera: " + path);
    }

    @Override
    public void errorCamera() {
        Log.i(TAG, "errorCamera");
    }

    @Override
    public void cancelCamera() {
        Log.i(TAG, "cancelCamera");
    }
}
```
