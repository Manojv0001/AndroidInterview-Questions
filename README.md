
# AndroidInterview-Questions

# How to share data between two applications?
To share data betweem two apps we can use content provider, in that we can use content resolver to perform CRUD Operation.
and we can take value using content values. please read arictle for details. <br>
http://codetheory.in/android-sharing-application-data-with-content-provider-and-content-resolver/

# How to allow to share data between few apps android?
To restrict app from sharing data, we can make custom permission for provider to give access to another app.<br>
https://www.codementor.io/maker-abhi/share-data-between-your-android-apps-only-securely-a0pa8ea0j

# How to find rooted device?
Rooting detection is a cat and mouse game and it is hard to make rooting detection that will work on all devices for all cases.
```
If you still need some basic rooting detection check the code below:

  /**
   * Checks if the device is rooted.
   *
   * @return <code>true</code> if the device is rooted, <code>false</code> otherwise.
   */
  public static boolean isRooted() {

    // get from build info
    String buildTags = android.os.Build.TAGS;
    if (buildTags != null && buildTags.contains("test-keys")) {
      return true;
    }

    // check if /system/app/Superuser.apk is present
    try {
      File file = new File("/system/app/Superuser.apk");
      if (file.exists()) {
        return true;
      }
    } catch (Exception e1) {
      // ignore
    }

    // try executing commands
    return canExecuteCommand("/system/xbin/which su")
        || canExecuteCommand("/system/bin/which su") || canExecuteCommand("which su");
  }

  // executes a command on the system
  private static boolean canExecuteCommand(String command) {
    boolean executedSuccesfully;
    try {
      Runtime.getRuntime().exec(command);
      executedSuccesfully = true;
    } catch (Exception e) {
      executedSuccesfully = false;
    }

    return executedSuccesfully;
  }

```
# How to upload image in sever?
```
 AsyncTask<Void, Void, HttpEntity> editProfileTask = new AsyncTask<Void, Void, HttpEntity>() {

  @Override
    protected HttpEntity doInBackground(Void... params) {          
     HttpClient httpclient = new DefaultHttpClient();
    HttpPost httppost = new HttpPost("Your url"); // your url

  try {                     
    MultipartEntity reqEntity = new MultipartEntity();
    reqEntity.addPart("firstname",new StringBody(firstnameEV.getText().toString(),
    "text/plain",Charset.forName("UTF-8")));
               "text/plain",Charset.forName("UTF-8")));
   if (file != null) {
       reqEntity.addPart("image",new FileBody(((File) file),"application/zip"));
      }         
      httppost.setEntity(reqEntity);
       HttpResponse resp = httpclient.execute(httppost);          
     HttpEntity resEntity = resp.getEntity();
     return resEntity;
    } catch (ClientProtocolException e) {
      e.printStackTrace();
     } catch (IOException e) {
     e.printStackTrace();
     }return null;
     }
    @Override
  protected void onPostExecute(HttpEntity resEntity) {
 if (resEntity != null) {             
   try {
     JSONObject responseJsonObject = new JSONObject(EntityUtils.toString(resEntity));
      String status = responseJsonObject.getString("status");             
       if (status.equals("success")) {
            Toast.makeText(activity, "Your Profile is updated", Toast.LENGTH_LONG).show();
             String data = responseJsonObject.getString("data");              
             isUpdatedSuccessfully=true;            
       } else {
          Toast.makeText(activity, "Profile not updated", Toast.LENGTH_LONG).show();
     }
    } catch (Exception e) {
   e.printStackTrace();
  }
    }            
  }
   };
  editProfileTask.execute(null, null, null)
  ```

# Type of broadcast reciever?
 Types of broadcast :Local,Normal,Ordered and Sticky <br>
 
* <b> Normal Broadcast </b> <br>
:- use sendBroadcast() <br>
:- asynchronous broadcast <br>
:- any receiver receives broadcast not any particular order <br>
* <b> Ordered Broadcast </b>  <br>
:- use sendOrderedBroadcast() <br>
:- synchronous broadcast <br>
:- receiver receives broadcast in priority base <br>
:- we can also simply abort broadcast in this type <br>
* <b> Local Broadcast </b>  <br>
:- use only when broadcast is used only inside application <br>
* <b> Sticky Broadcast </b>  <br>
:- normal broadcast intent is not available any more after is was send and processed by the system. <br>
:- use sendStickyBroadcast(Intent) <br>
:- the corresponding intent is sticky, meaning the intent you are sending stays around after the broadcast is complete. <br>
:- because of this others can quickly retrieve that data through the return value of registerReceiver(BroadcastReceiver, IntentFilter). <br>
:- apart from this same as sendBroadcast(Intent). <br>
	
# How to Integrate Payment Gateway?
https://www.rishabhsoft.com/blog/payment-gateway-integration-android-ios
1. Add PayPal Android SDK dependency to your build.gradle file as shown in README.md

2. Create a PayPalConfiguration object

private static PayPalConfiguration config = new PayPalConfiguration()
```
        // Start with mock environment.  When ready, switch to sandbox (ENVIRONMENT_SANDBOX)
        // or live (ENVIRONMENT_PRODUCTION)
        .environment(PayPalConfiguration.ENVIRONMENT_NO_NETWORK)

        .clientId("<YOUR_CLIENT_ID>");
```
3. Start PayPalService when your activity is created and stop it upon destruction:
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Intent intent = new Intent(this, PayPalService.class);

    intent.putExtra(PayPalService.EXTRA_PAYPAL_CONFIGURATION, config);

    startService(intent);
}

@Override
public void onDestroy() {
    stopService(new Intent(this, PayPalService.class));
    super.onDestroy();
}
```
4. Create the payment and launch the payment intent, for example, when a button is pressed:
```
public void onBuyPressed(View pressed) {

    // PAYMENT_INTENT_SALE will cause the payment to complete immediately.
    // Change PAYMENT_INTENT_SALE to 
    //   - PAYMENT_INTENT_AUTHORIZE to only authorize payment and capture funds later.
    //   - PAYMENT_INTENT_ORDER to create a payment for authorization and capture
    //     later via calls from your server.

    PayPalPayment payment = new PayPalPayment(new BigDecimal("1.75"), "USD", "sample item",
            PayPalPayment.PAYMENT_INTENT_SALE);

    Intent intent = new Intent(this, PaymentActivity.class);

    // send the same configuration for restart resiliency
    intent.putExtra(PayPalService.EXTRA_PAYPAL_CONFIGURATION, config);

    intent.putExtra(PaymentActivity.EXTRA_PAYMENT, payment);

    startActivityForResult(intent, 0);
}
```
Note: To provide a shipping address for the payment, see addAppProvidedShippingAddress(...) in the sample app. To enable retrieval of shipping address from the user's PayPal account, see enableShippingAddressRetrieval(...) in the sample app.

5.Implement onActivityResult():
```
@Override
protected void onActivityResult (int requestCode, int resultCode, Intent data) {
    if (resultCode == Activity.RESULT_OK) {
        PaymentConfirmation confirm = data.getParcelableExtra(PaymentActivity.EXTRA_RESULT_CONFIRMATION);
        if (confirm != null) {
            try {
                Log.i("paymentExample", confirm.toJSONObject().toString(4));

                // TODO: send 'confirm' to your server for verification.
                // see https://developer.paypal.com/webapps/developer/docs/integration/mobile/verify-mobile-payment/
                // for more details.

            } catch (JSONException e) {
                Log.e("paymentExample", "an extremely unlikely failure occurred: ", e);
            }
        }
    }
    else if (resultCode == Activity.RESULT_CANCELED) {
        Log.i("paymentExample", "The user canceled.");
    }
    else if (resultCode == PaymentActivity.RESULT_EXTRAS_INVALID) {
        Log.i("paymentExample", "An invalid Payment or PayPalConfiguration was submitted. Please see the docs.");
    }
}
```
6. Send the proof of payment to your servers for verification, as well as any other processing required for your business, such as fulfillment.

# How to Handle Configuration change when dont want to re-create activity?
Handling the Configuration Change Yourself
If your application doesn't need to update resources during a specific configuration change and you have a performance limitation that requires you to avoid the activity restart, then you can declare that your activity handles the configuration change itself, which prevents the system from restarting your activity.

To declare that your activity handles a configuration change, edit the appropriate <activity> element in your manifest file to include the android:configChanges attribute with a value that represents the configuration you want to handle. Possible values are listed in the documentation for the android:configChanges attribute (the most commonly used values are "orientation" to prevent restarts when the screen orientation changes and "keyboardHidden" to prevent restarts when the keyboard availability changes). You can declare multiple configuration values in the attribute by separating them with a pipe | character.

For example, the following manifest code declares an activity that handles both the screen orientation change and keyboard availability change:
```
<activity android:name=".MyActivity"
          android:configChanges="orientation|keyboardHidden"
          android:label="@string/app_name">
```
Now, when one of these configurations change, MyActivity does not restart. Instead, the MyActivity receives a call to onConfigurationChanged(). This method is passed a Configuration object that specifies the new device configuration. By reading fields in the Configuration, you can determine the new configuration and make appropriate changes by updating the resources used in your interface. At the time this method is called, your activity's Resources object is updated to return resources based on the new configuration, so you can easily reset elements of your UI without the system restarting your activity.
```
@Override
public void onConfigurationChanged(Configuration newConfig) {
    super.onConfigurationChanged(newConfig);

    // Checks the orientation of the screen
    if (newConfig.orientation == Configuration.ORIENTATION_LANDSCAPE) {
        Toast.makeText(this, "landscape", Toast.LENGTH_SHORT).show();
    } else if (newConfig.orientation == Configuration.ORIENTATION_PORTRAIT){
        Toast.makeText(this, "portrait", Toast.LENGTH_SHORT).show();
    }
}
```

### What is Intent Filter?
Intent filter declares the capabilities of its parent component — what an activity or service can do and what types of broadcasts a receiver can handle.<br>
```
<activity
    android:name=".ActivityTest"
    android:label="@string/app_name" >
    <intent-filter>
      <action android:name="android.intent.action.SEND" />

      <category android:name="android.intent.category.DEFAULT" />

      <data android:mimeType="text/plain" />

    </intent-filter>

</activity>
```
# What is Thread?
https://github.com/siddhpatil6/Java/wiki/Thread-Interview-Questions

# What is Executor?
Android has an Executor framework using which you can automatically manage a pool of threads with a task queue. Multiple threads will run in parellel executing tasks from the queue (BlockingQueue usually).

https://github.com/siddhpatil6/Kotlin-Executor

# What is Pool Thread?
# Thread Pools
A thread pool manages a pool of worker threads (the exact number varies depending upon how it’s implementation).

A task queue holds tasks waiting to be executed by any one of the idle threads in the pool. Tasks are added to the queue by producers, whereas the worker threads act as consumers by consuming the tasks from the queue whenever there’s an idle thread ready to perform a new background execution.

# What is Thread pool Executor?
The ThreadPoolExecutor executes a given task using one of its threads from the thread pool.
https://blog.mindorks.com/threadpoolexecutor-in-android-8e9d22330ee3


# How to done Image cache Manuelly?
https://developer.android.com/topic/performance/graphics/cache-bitmap.html

# Intent Flags?

1) FLAG_ACTIVITY_NEW_TASK - If set, this activity will become the start of a new task on this history stack. A task (from the activity that started it to the next task activity) defines an atomic group of activities that the user can move to. Tasks can be moved to the foreground and background; all of the activities inside of a particular task always remain in the same order.

2) FLAG_ACTIVITY_CLEAR_TOP - If set, and the activity being launched is already running in the current task, then instead of launching a new instance of that activity, all of the other activities on top of it will be closed and this Intent will be delivered to the (now on top) old activity as a new Intent.

3) FLAG_ACTIVITY_SINGLE_TOP - If set, the activity will not be launched if it is already running at the top of the history stack.


# How to make request Manually Android?
```
class RetrieveFeedTask extends AsyncTask<Void, Void, String> {

        private Exception exception;

        protected void onPreExecute() {
            progressBar.setVisibility(View.VISIBLE);
            responseView.setText("");
        }

        protected String doInBackground(Void... urls) {
            String email = emailText.getText().toString();
            // Do some validation here

            try {
                URL url = new URL(API_URL + "email=" + email + "&apiKey=" + API_KEY);
                HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();
                try {
                    BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(urlConnection.getInputStream()));
                    StringBuilder stringBuilder = new StringBuilder();
                    String line;
                    while ((line = bufferedReader.readLine()) != null) {
                        stringBuilder.append(line).append("\n");
                    }
                    bufferedReader.close();
                    return stringBuilder.toString();
                }
                finally{
                    urlConnection.disconnect();
                }
            }
            catch(Exception e) {
                Log.e("ERROR", e.getMessage(), e);
                return null;
            }
        }

        protected void onPostExecute(String response) {
            if(response == null) {
                response = "THERE WAS AN ERROR";
            }
            progressBar.setVisibility(View.GONE);
            Log.i("INFO", response);
            responseView.setText(response);
        }
    }
```

# How to share Image between activity?
```
String bitmapPath = MediaStore.Images.Media.insertImage(getContentResolver(), bitmap,"title", null);
    Uri bitmapUri = Uri.parse(bitmapPath);

        Intent intent = new Intent(Intent.ACTION_SEND);
        intent.setType("image/png");
        intent.putExtra(Intent.EXTRA_STREAM, bitmapUri);
        startActivity(Intent.createChooser(intent, "Share"));
 ```
 # How to Store Bitmap image in SQLite?
 https://stackoverflow.com/questions/11790104/how-to-storebitmap-image-and-retrieve-image-from-sqlite-database-in-android
 
 # What is use of Loop and Handler?
 https://blog.mindorks.com/android-core-looper-handler-and-handlerthread-bd54d69fe91a
 
 # What is START_STICKY and START_NOT_STICKY Service?
* Both codes are only relevant when the phone runs out of memory and kills the service before it finishes executing.  START_STICKY tells the OS to recreate the service after it has enough memory and call onStartCommand() again with a null intent. START_NOT_STICKY tells the OS to not bother recreating the service again. There is also a third code START_REDELIVER_INTENT that tells the OS to recreate the service and redeliver the same intent to onStartCommand().

# what is ServiceConnection and why to use?
**ServiceConnection**
android.content.ServiceConnection is an interface which is used to monitor the state of service. We need to override following methods. <br>
onServiceConnected(ComponentName name, IBinder service) : This is called when service is connected to the application. <br>
onServiceDisconnected(ComponentName name) : This is called when service is disconnected.<br>

# How to handle screen rotation?
When you rotate your device, your present activity gets completely destroyed, ie goes through 
* onSaveInstanceState() 
* onPause() 
* onStop() 
* onDestroy() 
and a new activity is created completely which goes through 
* onCreate() 
* onStart()
* onRestoreInstanceState() <br>
The Two Methods in the bold, onSaveInstanceState() saves the instance of the present activity which is going to be destroyed. onRestoreInstanceState This method restores the saved state of the previous activity. This way you don't lose your previous state of the app.<br>
Here is how you use these methods.<br>
`@Override  `
    `public void onSaveInstanceState(Bundle outState, PersistableBundle outPersistentState) {`
        `super.onSaveInstanceState(outState, outPersistentState);`

        `outState.putString("theWord", theWord); // Saving the Variable theWord`
        `outState.putStringArrayList("fiveDefns", fiveDefns); // Saving the ArrayList fiveDefns`
    `} `

    `@Override`
    `public void onRestoreInstanceState(Bundle savedInstanceState, PersistableBundle persistentState) {`
        `super.onRestoreInstanceState(savedInstanceState, persistentState);`

        `theWord = savedInstanceState.getString("theWord"); // Restoring theWord`
        `fiveDefns = savedInstanceState.getStringArrayList("fiveDefns"); //Restoring fiveDefns`
    `}`<br>
# What are these final, finally and finalize keywords?

final is a keyword in the java language. It is used to apply restrictions on class, method and variable. Final class can't be inherited, final method can't be overridden and final variable value can't be changed.

```
class FinalExample {
	public static void main(String[] args) {  
		final int x=100;  
		x=200;//Compile Time Error because x is final
	}
}
finally is a code block and is used to place important code, it will be executed whether exception is handled or not.
class FinallyExample {  
	public static void main(String[] args) {  
		try {  
			int x=300;  
		}catch(Exception e) {
			System.out.println(e);
		}  
		finally {
			System.out.println("finally block is executed");
		}  
	}
}  
Finalize is a method used to perform clean up processing just before object is garbage collected.
class FinalizeExample {  
	public void finalize() {
		System.out.println("finalize called");
	}  
	
	public static void main(String[] args) {  
		FinalizeExample f1=new FinalizeExample();  
		FinalizeExample f2=new FinalizeExample();  
		f1=null;  
		f2=null;  
		System.gc();  
	}
}
```
# What if two icons created of Main?
If we have mentioned launcher to two activity it will create two icons which will make different launcher screen.

# What is Serialization and Parcelable differences

Parcelable and Serialization are used for marshaling and unmarshaling Java objects. Differences between the two are often cited around implementation techniques and performance results. From my experience, I have come to identify the following differences in both the approaches:

Parcelable is well documented in the Android SDK; serialization on the other hand is available in Java. It is for this very reason that Android developers prefer Parcelable over the Serialization technique.

In Parcelable, developers write custom code for marshaling and unmarshaling so it creates less garbage objects in comparison to Serialization. The performance of Parcelable over Serialization dramatically improves (around two times faster), because of this custom implementation.

Serialization is a marker interface, which implies the user cannot marshal the data according to their requirements. In Serialization, a marshaling operation is performed on a Java Virtual Machine (JVM) using the Java reflection API. This helps identify the Java objects member and behavior, but also ends up creating a lot of garbage objects. Due to this, the Serialization process is slow in comparison to Parcelable

# What is Job Scheduler?
If you have a repetitive task in your Android app, you need to consider that activities and services can be terminated by the Android system to free up resources. Therefore you can not rely on standard Java schedule like the TimerTasks class.

The Android system currently has two main means to schedule tasks:

* the (outdated) AlarmManager
* the JobScheduler API.

Modern Android applications should use the JobScheduler API. Apps can schedule jobs while letting the system optimize based on memory, power, and connectivity conditions.

Read this article if you have time : -
http://www.vogella.com/tutorials/AndroidTaskScheduling/article.html

# What is Job Dispatcher?

# How we can read data from database?
```
public void getData() {
    public String[] getAppCategorydetail() {
        String Table_Name="cytaty";

        String selectQuery = "SELECT  * FROM " + Table_Name;
              SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.rawQuery(selectQuery, null);
                 String[] data = null;
        if (cursor.moveToFirst()) {
            do {
               // get  the  data into array,or class variable
            } while (cursor.moveToNext());
        }
        db.close();
        return data;
    }

}
```

# What is difference between Apply() and Commit() in android <br>
<br>
apply() was added in 2.3, it commits without returning a boolean indicating success or failure.<br>
commit() returns true if the save works, false otherwise.<br>

# What is difference between FragmentPagerAdapter and FragmentStatePagerAdapter?
 <b> FragmentPagerAdapter </b> stores the whole fragment in memory, and could increase a memory overhead if a large amount of fragments are used in ViewPager.
In contrary its sibling, <b> FragmentStatePagerAdapter </b> only stores the savedInstanceState of fragments, and destroys all the fragments when they lose focus

# How to communicate if both fragments on same screen and want to communicate?
It can be achieve by
1.Interface
2.Broadcast Reciever

# How to show notification?
```
NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this, CHANNEL_ID)
        .setSmallIcon(R.drawable.notification_icon)
        .setContentTitle("My notification")
        .setContentText("Much longer text that cannot fit one line...")
        .setStyle(new NotificationCompat.BigTextStyle()
                .bigText("Much longer text that cannot fit one line..."))
        .setPriority(NotificationCompat.PRIORITY_DEFAULT);
```


### What are launching mode of Activity?
https://android.jlelse.eu/android-activity-launch-mode-e0df1aa72242
https://inthecheesefactory.com/blog/understand-android-activity-launchmode/en

### What are the four pillers of android?
Android Components:Four pillars of every Android app
1. Activity.
1. Service.
1. Content providers.
1. Broadcast Receivers.

### What is AsyncTask and tell me about its methods

## Definition -
Updating UI from the separate thread is the most encountered scenario.
for such scenarios, Android has provided us API which is called as AsyncTask.

## AsyncTask Have four methods -
1. PreExecute
1. DoInBackground (must override)
1. ProgressUpdate
1. PostExecute

## Explanation -
# doInBackgrond - 
Android runs long running task in doInBackground, android runs doInBackground in Separate Thread.<br>
Android Itself creates the separate thread for doInBackground.<br>
# PreExecute - <br>
It is initialised before doInBackground<br>
Anything initialisation is to be done before performing any task in the background is to be done in PreExecute.<br>
<br>
# PostExecute<br>
it is called after DoInBackground<br>
It executes on UI Thread.<br>
after performing task in background to update result on UIThread PostExecute Method get called.<br>
<br>
# ProgressUpdate<br>
Prgressupdate is called when You want to continually update UI of UIThread from doInBackground and also wants to come back and perform task in doInBackground.<br>
to do that you have to call<br>
**publishProgress** in doInbackground<br>

# Explain UI Thread/ Main Thread
Each App is executed by different process </br>

for example </br>
Proccess 1 Executes App1</br>
Process 2 Executes App2</br>
</br>
Each Process Contains Multiple Threads</br>
For Example- </br>
Process 1- Thread1, Thread2, Thread3</br>
</br>
There is <b> Main Thread </b> where we all UI related Task we Perform</br>
For Example - </br>
    Button Click, Button Rendering, All task related to UI</br>
    
If the Task is very <b> Resource Intensive Task </b> which requires more than 5 Seconds to execute, it not good idea to execute it on UI Thread.It will give ANR.</br>
    So We Can use seprate thread to execute such task.</br>
   </br>
https://www.youtube.com/watch?v=kpFwxJFYnOo</br>
# Reason for OutOfMemory
In order to prevent it, first we need to know the reasons of "Out of Memory" exception:
* The biggest reason is memory leak i.e, Context leaking or can say Activity Leaking, a Service has the same problems as Activity in this regard.
* You are doing the process that demands continuous memory and at a point, it goes beyond max memory limit of a process.
* When you are dealing with large Bitmap and load all of them at runtime.

# How to Handle OutOfMemory issue?
https://www.youtube.com/watch?v=HY9aaXHx8yA<br>
https://stackoverflow.com/questions/477572/strange-out-of-memory-issue-while-loading-an-image-to-a-bitmap-object <br>

# what is JobScheduler and JobInfo?<br>
## Job Scheduler <br>
This is an API for scheduling various types of jobs against the framework that will be executed in your application's own process.<br>
<br>
## Job Info <br>
JobInfo for more description of the types of jobs that can be run and how to construct them. You will construct these JobInfo objects and pass them to the JobScheduler with schedule(JobInfo). When the criteria declared are met, the system will execute this job on your application's JobService. You identify the service component that implements the logic for your job when you construct the JobInfo using JobInfo.Builder(int, android.content.ComponentName).
Code: 


        ```
        var componentName=ComponentName(this,com.kotlin.siddhant.jobschedulerexample.MJobService::class.java)
        
        var builder=JobInfo.Builder(JOB_ID,componentName) // we build Job here and assign job id to identify JOB Uniquely
        
        builder.setPeriodic(5000) // we set Period for job to be executed
       
        builder.setRequiredNetworkType(JobInfo.NETWORK_TYPE_ANY) // to define required network type
        
        builder.setPersisted(true) //if set true this job will exist after system reboot as well
        
       // builder.setRequiresBatteryNotLow(true) // schedule job when battery not low
       
        //builder.setRequiresStorageNotLow(true)  // schedule job when Storage is not low
        
        builder.setRequiresCharging(true)    // schedule job when charging
        
        jobInfo=builder.build()
        ```

# What is Volatile?
https://dzone.com/articles/java-volatile-keyword-0

# Serialization & Transient?
Before understanding the transient keyword, one has to understand the concept of serialization. If the reader knows about serialization, please skip the first point.

* **What is serialization?** <br>
Serialization is the process of making the object's state persistent. That means the state of the object is converted into a stream of bytes and stored in a file. In the same way, we can use the deserialization to bring back the object's state from bytes. This is one of the important concepts in Java programming because serialization is mostly used in networking programming. The objects that need to be transmitted through the network have to be converted into bytes. For that purpose, every class or interface must implement the Serializable interface. It is a marker interface without any methods.

* **Now what is the transient keyword and its purpose?** <br>
By default, all of object's variables get converted into a persistent state. In some cases, you may want to avoid persisting some variables because you don't have the need to persist those variables. So you can declare those variables as transient. If the variable is declared as transient, then it will not be persisted. That is the main purpose of the transient keyword.

I want to explain the above two points with the following example:
package javabeat.samples;

`import java.io.FileInputStream;` <br>
`import java.io.FileOutputStream;`<br>
`import java.io.IOException;`<br>
`import java.io.ObjectInputStream;`<br>
`import java.io.ObjectOutputStream;`<br>
`import java.io.Serializable;`<br>

`class NameStore implements Serializable{`<br>
    `private String firstName;`<br>
    `private transient String middleName;`<br>
    `private String lastName;`<br>

    `public NameStore (String fName, String mName, String lName){`
        `this.firstName = fName;`
        `this.middleName = mName;`
        `this.lastName = lName;`
    `}`

    `public String toString(){`
        `StringBuffer sb = new StringBuffer(40);`
        `sb.append("First Name : ");`
        `sb.append(this.firstName);`
        `sb.append("Middle Name : ");`
        `sb.append(this.middleName);`
        `sb.append("Last Name : ");`
        `sb.append(this.lastName);`
        `return sb.toString();`
    `}`
`}`

`public class TransientExample` <br>
`{`<br>
    `public static void main(String args[]) throws Exception` <br>
     `{`<br>
        `NameStore nameStore = new NameStore("Steve", "Middle","Jobs");`<br>

        `ObjectOutputStream o = new ObjectOutputStream(new FileOutputStream("nameStore"));`

        `o.writeObject(nameStore);`

        `o.close();`

        `ObjectInputStream in = new ObjectInputStream(new FileInputStream("nameStore"));`

        `NameStore nameStore1 = (NameStore)in.readObject();`

        `System.out.println(nameStore1);`

    `}`
`}`

# Service Example<br>
https://www.simplifiedcoding.net/android-service-example/


# What is DDMS and what can it do?

* DDMS is short for Dalvik Debug Monitor Server. It ships natively with Android and contains a number of useful debugging features including: location data spoofing, port-forwarding, network traffic tracking, incoming call/SMS spoofing, thread and heap information, screen capture, and the ability to simulate network state, speed, and latenc

# how to make different layout for potrait and landscape?<br>
https://stackoverflow.com/questions/4858026/android-alternate-layout-xml-for-landscape-mode

# how to not to loose data when we move from potrait to landscape mode layout?
https://stackoverflow.com/questions/10126845/handle-screen-rotation-without-losing-data-android

# What is handler and why Handler?<br>
https://medium.com/@ankit.sinhal/handler-in-android-d138c1f4980e<br>

# what is Understanding Android Core: Looper, Handler, and HandlerThread ? <br>
https://blog.mindorks.com/android-core-looper-handler-and-handlerthread-bd54d69fe91a <br>
https://stackoverflow.com/questions/7597742/what-is-the-purpose-of-looper-and-how-to-use-it <br>

# What is differene between throw and throws?<br>
https://www.javatpoint.com/difference-between-throw-and-throws-in-java<br>

# How to upgrade database without loosing data?<br>
https://thebhwgroup.com/blog/how-android-sqlite-onupgrade<br>

# what is android?
* Open source OS
* based on linux kernal-(not linux os)
* NOT programming language

**what can we use for SQLite Security?**<br>
* SQLCipher

**what is Intent?**
* It is messaging object.
* It uses when you want to pass **data** from one screen to another.

<br>**Types -**
1. Explicit
   Where you have to specify target component.
   ex. Intent i=new Intent(ActivityOne.this,ActivityTwo.class)
2. Implicit-
  where you dont have to specify target component,System specifies what action has to be done.
    ex - Intent chooser=Intent.createChooser(send,title)

**What is ANR?**<br>
When the application tries to run a long operation on the main thread, but gets no response after a long time.

**which are orientations?**
* vertical
* Horizontal

**What is ADB(Android Debug Bridge)?**<br>
It provides a connection between System and Emulator.

**What Android Architecture Consist of?**
* Android Architecture - Camera Calculator
* Framework - Content Provider ,View System,Managers-(Activity,Location,Package,Notification)
        
* Libraries - 
* Linux Kernal

# Difference between Activity and Service?
* Activity -Runs always on Foreground
* Service -Runs on Background,Useful to run the long process.

# what is Manifest File?
* Play important role in app
* where we declare all permissions-location, internet.
* declare all Services
* declare all Activities

# which api you worked on?
* Analytics
* Cloud Messaging
* Authentication
* Realtime Database
* Storage
* Performance Monitoring
* Remote Config
* Test Lab
* Sign in with Google
* Maps
* Places
* cleverTap

# Difference between ArrayAdapter and BaseAdapter.
* **BaseAdapter** as the name suggests, is a base class for all the adapters.
* When you are extending the Base adapter class you need to implement all the methods like getCount(), getId() etc.
* **ArrayAdapter** is a class which can work with array of data. You need to override only getview() method.
* **ListAdapter** is a an interface implemented by concrete adapter classes.
* BaseAdapter is an abstract class whereas ArrayAdapter and ListAdapter are the concrete classes.
* ArrayAdapter and ListAdapter classes are developed since  in general we deal with the array data sets and list data sets.

# Difference between RecyclerView and ListView?
RecyclerView was created as a ListView improvement, so yes, you can create an attached list with ListView control, but using RecyclerView is easier as it<br>

Reuses cells while scrolling up/down - this is possible with implementing View Holder in the listView adapter, but it was an optional thing, while in the RecycleView it's the default way of writing adapter.
Decouples list from its container - so you can put list items easily at run time in the different containers (linearLayout, gridLayout) with setting LayoutManager.<br>
Example:
<br>
`mRecyclerView = (RecyclerView) findViewById(R.id.my_recycler_view);`<br>
`mRecyclerView.setLayoutManager(new LinearLayoutManager(this));`<br>
`//or`<br>
`mRecyclerView.setLayoutManager(new GridLayoutManager(this, 2));`<br>
Animates common list actions - Animations are decoupled and delegated to ItemAnimator.<br>

# What is RecyclerView?

* The RecyclerView widget is a more advanced and flexible version of ListView.

**Why RecyclerView?**

* RecyclerView is a container for displaying large data sets that can be scrolled very efficiently by maintaining a limited number of views.

**When you should use RecyclerView?**

* You can use the RecyclerView widget when you have data collections whose elements changes at runtime based on user action or network events<br>

**new extended functionalities available in RecyclerView that are not directly supported by ListView as following**:<br>

* Direct horizontal scrolling is not supported in ListView. Whereas in RecyclerView you can easily implement horizontal and vertical scrolling.<br>
* ListView supports only simple linear list view with vertical scrolling. Whereas RecyclerView supports three types of lists using RecyclerView.LayoutManager. 1) StaggeredGridLayoutManager 2) GridLayoutManager, 3) LinearLayoutManager. I will give you brief introduction about all these 3 lists later.<br>
* ListView gives you an option to add divider using dividerHeight parameter whereas RecyclerView enable you to customize divider (spacing) between two element<br>

# Name 4 ways Android allows you to store data?
 Any of the following 5 possible options are acceptable:
* SharedPreferences
* Internal Storage
* External Storage
* SQLite Database
* Network connection

# What items or folders are important in every Android project?
Answer: The developer should name at least 4 of these 6 items below, as these are essential within each Android project:
* AndroidManifest.xml
* build.xml
* bin/
* src/
* res/
* assets/

# What are containers?
Containers hold objects and widgets together, depending on which items are needed and in what arrangement they need to be in. Containers may hold labels, fields, buttons, or even child containers, as examples.

**What is AIDL?**

# What information do you need before you begin coding an Android app for a client?
You want to find out that this person will seek to truly understand what you are trying to accomplish with your app, and the functionality. The following items are good to hear:

* Objective statement or purpose of the app for the app publisher
* Description of the target audience or user demographics
* Any existing apps that it might be similar to
* Wireframes
* Artwork; The best developers will say they require the artwork to be completed before development. This avoids delays, and helps the developer understand the look, feel and branding you are trying to achieve .

# What is Service?
* A service is a component which runs in the background without direct interaction with the user
* service has no user interface, it is not bound to the lifecycle of an activity.
* Services are used for repetitive and potentially long running operations, i.e., Internet downloads, checking for new data, data processing, updating content providers and the like.<br>
**States of Services?**<br>
1. Started-
A service is started when an application component, such as an activity, starts it by calling startService(). Once started, a service can run in the background indefinitely, even if the component that started it is destroyed.
2. Bound-
A service is bound when an application component binds to it by calling bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, get results, and even do so across processes with interprocess communication (IPC).<br>

# Difference between Service and IntentService?
* https://stackoverflow.com/questions/15524280/service-vs-intentservice<br>

# Different Type of Services?
These are the three different types of services:<br>
**1. Foreground** <br>
A foreground service performs some operation that is noticeable to the user. For example, an audio app would use a foreground service to play an audio track. Foreground services must display a status bar icon. Foreground services continue running even when the user isn't interacting with the app.<br>
**2. Background** <br>
A background service performs an operation that isn't directly noticed by the user. For example, if an app used a service to compact its storage, that would usually be a background service.
Note: If your app targets API level 26 or higher, the system imposes restrictions on running background services when the app itself is not in the foreground. In most cases like this, your app should use a scheduled job instead.<br>

**3. Bound** <br>
A service is bound when an application component binds to it by calling bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, receive results, and even do so across processes with interprocess communication (IPC). A bound service runs only as long as another application component is bound to it. Multiple components can bind to the service at once, but when all of them unbind, the service is destroyed<br>

# How to start service again after reboot device in Android?
You can use broadcast receiver to restart service,
`<action android:name="android.intent.action.BOOT_COMPLETED" />`
BOOT_COMPLETED event we can use to identify system is booted which calls broadcast receiver and broadcast receiver will startservice again.

# LifeCycle Of Service?
![](https://www.tutorialspoint.com/android/images/services.jpg)<br>

# What is BroadCast Reciever?
* Broadcast Receivers simply respond to broadcast messages from other applications or from the system itself. These messages are sometime called events or intents
**to make BroadcastReceiver works for the system broadcasted intents** −
* Creating the Broadcast Receiver.
* Registering Broadcast Receiver

# Creating the Broadcast Receiver
* A broadcast receiver is implemented as a subclass of BroadcastReceiver class and overriding the onReceive() method where each message is received as a Intent object parameter.

`public class MyReceiver extends BroadcastReceiver 
 {`<br>
   `@Override`<br>
   `public void onReceive(Context context, Intent intent) {`<br>
      `Toast.makeText(context, "Intent Detected.", Toast.LENGTH_LONG).show();`<br>
   `}`<br>
`}`<br>

# Registering Broadcast Receiver
An application listens for specific broadcast intents by registering a broadcast receiver in AndroidManifest.xml file.<br>
`<application`
   `android:icon="@drawable/ic_launcher"`
   `android:label="@string/app_name"`
   `android:theme="@style/AppTheme" >`
   `<receiver android:name="MyReceiver">`
   
      `<intent-filter>`
         `<action android:name="android.intent.action.BOOT_COMPLETED">`
         `</action>`
      `</intent-filter>`
   
   `</receiver>`
`</application>`<br>

# Few More Questions on BroadCast Reciever?
http://skillgun.com/android/receivers/interview-questions-and-answers/paper/30<br>

# What is Content Provider?
* A content provider component supplies data from one application to others on request. Such requests are handled by the methods of the ContentResolver class. 
* A content provider can use different ways to store its data and the data can be stored in a database, in files, or even over a network.<br>
![](https://www.tutorialspoint.com/android/images/content.jpg)

**onCreate()** This method is called when the provider is started.

**query()** This method receives a request from a client. The result is returned as a Cursor object.

**insert()** This method inserts a new record into the content provider.

**delete()** This method deletes an existing record from the content provider.

**update()** This method updates an existing record from the content provider.

**getType()** This method returns the MIME type of the data at the given URI.

# Android OS  Naughat Changes?
https://www.androidauthority.com/android-7-0-features-673002/
* Multi-window Support
* Notification Bar Toggles
* Notifications: redesigned, bundled and Quick Reply-able
* Notification prioritization
* Customizable Quick Settings
* Doze Mode on the Go
* Multi-language support, emoji and app links
* New Settings menu - The essential information contained in each Settings section is now displayed on the main page.
* Do Not Disturb
* Data Saver - Data Saver denies internet access to background apps when you're connected to cellular data.
* Seamless updates
* File-based encryption
* Emergency info <br>

# What are the new features of oreo?
https://www.android.com/versions/oreo-8-0/<br>
**AutoFill:**<br>
With your permission, AutoFill remembers your logins to get you into your favourite apps at supersonic speed.
**Picture-in-picture:**<br>
Allows you to see two apps at once. It's like having super strength and laser vision.<br>
**Dive into more apps with fewer taps**<br>
* Notification dots:<br>
Press the notification dots to quickly see what's new, and easily clear them by swiping away.<br>

# Android Instant Apps:
Teleport directly into new apps straight from your browser, no installation needed.<br>


# Activity LifeCycle?<br>
![](https://developer.android.com/guide/components/images/activity_lifecycle.png)<br>
   ** What is the difference between Activity and AppCompatActivity?**
* AppCompatActivity provides native ActionBar support that is consistent across the application. Also it provides backward compatibility for other material design components till SDK version 7(ActionBar was natively available since SDK 11). Extending an Activity doesn’t provide any of these.<br>

**Activity, AppCompatActivity, FragmentActivity and ActionBarActivity. How are they related?**<br>
* Activity is the base class. FragmentActivity extends Activity. AppCompatActivity extends FragmentActivity. ActionBarActivity extends AppCompatActivity.
* FragmentActivity is used for fragments.
* Since the build version 22.1.0 of the support library, ActionBarActivity is deprecated. It was the base class of appcompat-v7.
* At present, AppCompatActivity is the base class of the support library. It has come up with many new features like ToolBar, tinted widgets, material design color pallets etc.

**Describe the structure of Android Support Library?**
* Android Support Library is not just a single library. It’s a collection of libraries that have different naming conventions and usages. On a higher level, it’s divided into three types;

* **Compatibility Libraries**: These focus on back porting features so that older frameworks can take advantage of newer releases. The major libraries include v4 and v7-appcompat. v4 includes classes like DrawerLayout and ViewPager while appcompat-v7 provides classes for support ActionBar and ToolBar.
* **Component Libraries**: These include libraries of certain modules that don’t depend on other support library dependencies. They can be easily added or removed. Examples include v7-recyclerview and v7-cardview.
* **Miscellaneous libraries**: The Android support libraries consists of few other libraries such as v8 which provides support for RenderScript, annotations for supporting annotations like @NonNull.

**Mention two ways to clear the back stack of Activities when a new Activity is called using intent.**

* The first approach is to use a FLAG_ACTIVITY_CLEAR_TOP flag.
`Intent intent= new Intent(ActivityA.this, ActivityB.class);`<br>
`intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);`<br>
`startActivity(intent);`<br>
`finish();`<br>

* The second way is by using FLAG_ACTIVITY_CLEAR_TASK and FLAG_ACTIVITY_NEW_TASK in conjunction.
`Intent intent= new Intent(ActivityA.this, ActivityB.class);`<br>
`intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);`<br>
`startActivity(intent);`<br>

# difference between FLAG_ACTIVITY_CLEAR_TASK and FLAG_ACTIVITY_CLEAR_TOP?
FLAG_ACTIVITY_CLEAR_TASK is used to clear all the activities from the task including any existing instances of the class invoked. The Activity launched by intent becomes the new root of the otherwise empty task list. This flag has to be used in conjunction with FLAG_ ACTIVITY_NEW_TASK.<br>

FLAG_ACTIVITY_CLEAR_TOP on the other hand, if set and if an old instance of this Activity exists in the task list then barring that all the other activities are removed and that old activity becomes the root of the task list. Else if there’s no instance of that activity then a new instance of it is made the root of the task list. Using FLAG_ACTIVITY_NEW_TASK in conjunction is a good practice, though not necessary.<br>

**How do you disable onBackPressed()?**<br>
`The onBackPressed() method is defined as shown below:`
    `@Override`<br>
    `public void onBackPressed() {`<br>
        `super.onBackPressed();`<br>
    `}` <br>
* To disable the back button and preventing it from destroying the current activity and going back we have to remove the line super.onBackPressed();<br>

# Fragment LifeCycle?
![](https://developer.android.com/images/fragment_lifecycle.png)<br>

**getFragmentManager()**
* Return the FragmentManager for interacting with fragments associated with this activity.

* **FragmentManager** which is used to create transactions for adding, removing or replacing fragments.

**`fragmentManager.beginTransaction();`**<br>
* Start a series of edit operations on the Fragments associated with this FragmentManager.

* The FragmentTransaction object which will be used.
* fragmentTransaction.replace(R.id.fragment_container, mFeedFragment);
* Replaces the current fragment with the mFeedFragment on the layout with the id: R.id.fragment_container

 **`fragmentTransaction.addToBackStack(null);`** <br>
 Add this transaction to the back stack. This means that the transaction will be remembered after it is committed, and will reverse its operation when later popped off the stack.

Useful for the return button usage so the transaction can be rolled back. The parameter name:

Is an optional name for this back stack state, or null.

# Types of Fragments?
Basically fragments are divided as three stages as shown below.<br>

* **Single frame fragments** − Single frame fragments are using for hand hold devices like mobiles, here we can show only one fragment as a view.<br>

* **List fragments** − fragments having special list view is called as list fragment <br>

* **Fragments transaction** − Using with fragment transaction. we can move one fragment to another fragment.<br>


**Difference between add and replace?**
* replace removes the existing fragment and adds a new fragment.

* add retains the existing fragments and adds a new fragment that means existing fragment will be active and they wont be in 'paused' state hence when a back button is pressed onCreateView() is not called for the existing fragment(the fragment which was there before new fragment was added).

**why to use replace?**
* Replace is the first removal of the same id all the fragments, and then add the current fragment.
* multi-layer is certainly more than a layer of waste, so it is recommended to use replace

**Service vs AsyncTask?**<br>
* http://vardhan-justlikethat.blogspot.in/2013/11/android-comparision-chart-asynctask-vs.html

**Difference Between Service,Thread,IntentService,AsyncTask?** <br>
* http://techtej.blogspot.in/2011/03/android-thread-constructspart-4.html

**How to avoid memory leak?**<br>
https://mindorks.com/blog/detecting-and-fixing-memory-leaks-in-android<br>

**What is SQLite and Methods?**<br>
* SQLite is an Open Source database. SQLite supports standard relational database features like SQL syntax, transactions and prepared statements. The database requires limited memory at runtime (approx. 250 KByte).<br>

**Methods -**
**onCreate(SQLiteDatabase db)**-<br>
* called only once when database is created for the first time
**onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) -**
* called when database needs to be upgraded.

**How to Use Cursor?**<br>
`public Contact getContact(int id) {`<br>
    `SQLiteDatabase db = this.getReadableDatabase();`<br>
 
    `Cursor cursor = db.query(TABLE_CONTACTS, new String[] { KEY_ID,`<br>
            `KEY_NAME, KEY_PH_NO }, KEY_ID + "=?",`<br>
            `new String[] { String.valueOf(id) }, null, null, null, null);`<br>
    `if (cursor != null)`<br>
        `cursor.moveToFirst();`<br>
 
    `Contact contact = new Contact(Integer.parseInt(cursor.getString(0)),`<br>
            `cursor.getString(1), cursor.getString(2));`<br>
    `// return contact`<br>
    `return contact;`<br>
`}`<br>

* SQLite supports the following data types:<br>

* TEXT(similar to String in Java)

* INTEGER(similar to long in Java)

* REAL (similar to double in Java)

**When to use dependancies?** <br>
* In Android Studio, dependencies allows us to include external library or local jar files or other library modules in our Android project.<br>

add Dependancies -


**What is Proguard?**<br>
* Proguard is free Java class file shrinker, optimizer, obfuscator, and preverifier. It detects and removes unused classes, fields, methods, and attributes. It optimizes bytecode and removes unused instructions. It renames the remaining classes, fields, and methods using short meaningless names.

* Shrinking – detects and removes unused classes, fields, methods, and attributes.

* Optimization – analyzes and optimizes the bytecode of the methods.

* Obfuscation – renames the remaining classes, fields, and methods using short meaningless names.

* **Shrink Your Code and Resources**<br>
detects and removes unused classes, fields, methods, and attributes from your packaged app, including those from included code libraries (making it a valuable tool for working around the 64k reference limit). ProGuard also optimizes the bytecode, removes unused code instructions, and obfuscates the remaining classes, fields, and methods with short names.<br>
* **Shrink Your Code**<br>
* add minifyEnabled true to the appropriate build type in your build.gradle file.
* enable code shrinking on your final APK used for testing, because it might introduce bugs if you do not sufficiently customize which code to keep.<br>

**ProGuard outputs the following files:**<br>
**dump.txt**<br>
Describes the internal structure of all the class files in the APK.<br>
**mapping.txt**<br>
Provides a translation between the original and obfuscated class, method, and field names.<br>
**seeds.txt**<br>
Lists the classes and members that were not obfuscated.<br>
**usage.txt**><br>
Lists the code that was removed from the APK.<br>
These files are saved at <module-name>/build/outputs/mapping/release/.<br>

**Why to Use keep in proguard?**
* To fix errors and force ProGuard to keep certain code, add a -keep line in the ProGuard configuration file. For example:

`-keep public class MyClass`<br>

**Security Practices in android?**<br>
* IMPLEMENT A GOOD MOBILE ENCRYPTION POLICY
* HAVE A SOLID API SECURITY STRATEGY - Use SSL,Pinned Certificate
* PUT IDENTIFICATION, AUTHENTICATION, AND AUTHORIZATION MEASURES IN PLACE.
* App Data Access Permission
* Password security
* Use Proguard - Obfuscation

**What is AIDL?**
* AIDL (Android Interface Definition Language) is similar to other IDLs you might have worked with. It allows you to define the programming interface that both the client and service agree upon in order to communicate with each other using interprocess communication (IPC). On Android, one process cannot normally access the memory of another process. So to talk, they need to decompose their objects into primitives that the operating system can understand, and marshall the objects across that boundary for you. The code to do that marshalling is tedious to write, so Android handles it for you with AIDL.

**AsyncTask?**
When an asynchronous task is executed, the task goes through 4 steps:
`class AsyncTaskExample extends AsyncTask<Void, Integer, String> {`<br>
    
    `private  final String TAG = AsyncTaskExample.class.getName();`<br>
    `protected void onPreExecute(){`<br>
        `Log.d(TAG, "On preExceute...");`<br>
    `}`<br>
    
    `protected String doInBackground(Void...arg0) {`<br>
        <br>
        `Log.d(TAG, "On doInBackground...");`<br>
        
        `for(int i = 0; i<5; i++){`<br>
            `Integer in = new Integer(i);`<br>
            `publishProgress(i);`<br>
        `}`<br>
        
        `return "You are at PostExecute";}`<br>
    <br>
    `protected void onProgressUpdate(Integer...a){`<br>
        `Log.d(TAG,"You are in progress update ... " + a[0]);`<br>
    `}`<br>
    <br>
    `protected void onPostExecute(String result) {`<br>
        `Log.d(TAG,result); `<br>
    `}`
`}`
* **onPreExecute()**, invoked on the UI thread before the task is executed. This step is normally used to setup the task, for instance by showing a progress bar in the user interface.
* **doInBackground(Params...)**, invoked on the background thread immediately after onPreExecute() finishes executing. This step is used to perform background computation that can take a long time. The parameters of the asynchronous task are passed to this step. The result of the computation must be returned by this step and will be passed back to the last step. This step can also use publishProgress(Progress...) to publish one or more units of progress. These values are published on the UI thread, in the onProgressUpdate(Progress...) step.
* **onProgressUpdate(Progress...)**, invoked on the UI thread after a call to publishProgress(Progress...). The timing of the execution is undefined. This method is used to display any form of progress in the user interface while the background computation is still executing. For instance, it can be used to animate a progress bar or show logs in a text field.
* **onPostExecute(Result)**, invoked on the UI thread after the background computation finishes. The result of the background computation is passed to this step as a parameter.

**which material Design you used?**
* RecyclerView
* Navigation Drawer
* TextInputLayout
* Snackbar
* Floation Action Button
* SlidingTabLayout<br>


**Differennce between Thread and asyncTask?**<br>
When you use a Thread, you have to update the result on the main thread using the runOnUiThread() method, while an AsyncTask has the onPostExecute() method which automatically executes on the main thread after doInBackground() returns.<br>

