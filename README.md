
# AndroidInterview-Questions
[![ko-fi](https://www.ko-fi.com/img/donate_sm.png)](https://ko-fi.com/F1F1J3S3)

# How android architecture works?

It have reference of below class because we can change its lower structure without solving inter dependancies.

[![Fibonacci RMI Java EE](https://www.youtube.com/watch?v=BofWWZE1wts]

Youtube -
https://www.youtube.com/watch?v=BofWWZE1wts

# The difference between Kotlin’s functions: ‘let’, ‘apply’, ‘with’, ‘run’ and ‘also’
![](https://github.com/siddhpatil6/AndroidInterview-Questions/blob/master/scope%20function1.png)

![](https://github.com/siddhpatil6/AndroidInterview-Questions/blob/master/scope%20function2.png)

![](https://github.com/siddhpatil6/AndroidInterview-Questions/blob/master/scope%20function%203.png)

![](https://github.com/siddhpatil6/AndroidInterview-Questions/blob/master/scope%20function%204.png)


# Normal vs. extension function
If we look at with and T.run, both functions are actually pretty similar. The below does the same thing.

```
with(webview.settings) {
    javaScriptEnabled = true
    databaseEnabled = true
}
// similarly
webview.settings.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```
However, their different is one is a normal function i.e. with, while the other is an extension function i.e. T.run.

So the question is, what is the advantage of each?

Imagine if webview.settings could be null, they will look as below.

```
// Yack!
with(webview.settings) {
      this?.javaScriptEnabled = true
      this?.databaseEnabled = true
   }
}
```
---------------------------------------------
```
// Nice.
webview.settings?.run {
    javaScriptEnabled = true
    databaseEnabled = true
}
```
<b> In this case, clearly T.run extension function is better, as we could apply check for nullability before using it. </b>
# This vs. it argument
If we look at T.run and T.let, both functions are similar except for one thing, the way they accept the argument. The below shows the same logic for both functions.

stringVariable?.run {
      println("The length of this String is $length")
}
// Similarly.
stringVariable?.let {
      println("The length of this String is ${it.length}")
}
If you check the T.run function signature, you’ll notice the T.run is just made as extension function calling block: T.(). Hence all within the scope, the T could be referred as this.In programming, this could be omitted most of the time. Therefore in our example above, we could use $length in the println statement, instead of ${this.length}. I call this as sending in this as argument.

# advantages of T.let function as below: -

The T.let does provide a clearer distinguish use the given variable function/member vs. the external class function/member
In the event that this can’t be omitted e.g. when <b>it</b> is sent as a parameter of a function, it is shorter to write than this and clearer.
The T.let allow better naming of the converted used variable i.e. you could convert it to some other name.

```
stringVariable?.let {
      nonNullString ->
      println("The non null string is $nonNullString")
}
```

# what are rules for Data class?

### Rules for Creating Data Classes
The Kotlin documentation on data classes notes that there are some basic restrictions in order to maintain consistency/behaviour of generated code:

1. The primary constructor needs to have at least one parameter;
2. All primary constructor parameters need to be marked as val or var;
3. Data classes cannot be abstract, open, sealed or inner;
4. Data classes may not extend other classes (but may implement interfaces).

# Difference between Classes and Data classes or Explain Data classes?
### Tired of writing (or generating) lengthy, boilerplate code for objects which do nothing but store data?
<b> Well, Kotlin has the solution for you! </b>

Almost every software project we create has a number of classes which exist solely to store data/state but have almost no actual functionality in terms of operations. In more complex apps, this number can be rather high (applications which feature a clean architecture approach often have 2–3 times as many due to a separation of entities between layers).

These generally contain the same concepts every time:

A constructor
Fields to store data
Getter and setter functions
hashCode(), equals() and toString() functions

<b> Example </b> — Storing Video Game Data
If we wanted to store some data about a video game in Java, we would usually create a class similar to this:

```
public class VideoGame {

    private String name;
    private String publisher;
    private int reviewScore;

    public VideoGame(String name, String publisher, int reviewScore) {
        this.name = name;
        this.publisher = publisher;
        this.reviewScore = reviewScore;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPublisher() {
        return publisher;
    }

    public void setPublisher(String publisher) {
        this.publisher = publisher;
    }

    public int getReviewScore() {
        return reviewScore;
    }

    public void setReviewScore(int reviewScore) {
        this.reviewScore = reviewScore;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        VideoGame that = (VideoGame) o;

        if (reviewScore != that.reviewScore)
            return false;
        if (name != null ? !name.equals(that.name) :
                that.name != null) {
            return false;
        }
        return publisher != null ?
                publisher.equals(that.publisher) :
                that.publisher == null;

    }

    @Override
    public int hashCode() {
        int result = name != null ? name.hashCode() : 0;
        result = 31 * result + (publisher != null ?
                publisher.hashCode() : 0);
        result = 31 * result + reviewScore;
        return result;
    }

    @Override
    public String toString() {
        return "VideoGame{" +
                "name='" + name + '\'' +
                ", publisher='" + publisher + '\'' +
                ", reviewScore=" + reviewScore +
                '}';
    }
}
```
### Data Classes in Kotlin

Fortunately for us, the above code is no longer necessary in Kotlin due to the useful data class concept provided by the language. A data class is a class in Kotlin created to encapsulate all of the above functionality in a succinct manner.

To recreate the VideoGame class in Kotlin, we can simply write:

```
data class VideoGame(val name: String, val publisher: String, var reviewScore: Int)
```
<b> Much better! </b>

When we specify the data keyword in our class definition, Kotlin automatically generates field accessors, hashCode(), equals(), toString(), as well as the useful copy() and componentN() functions (more on these later).

Any of the functions above which are manually defined by us in the class will not be generated.

# How to specify Visibility Modifiers in Data class?

We can control the visibility modifiers of the getters/setters generated by providing them in the constructor:

data class VideoGame(private val name: String, val publisher: String, private var reviewScore: Int

# How to specify read only parameter in Data class?
### Read-Only Fields
If we only wish to expose getters and not setters, we just provide <b>val</b> instead of var for each field (val properties are Kotlin’s equivalent of final in Java). In the below example, name and reviewScore have read/write access, while publisher is read-only:

```
data class VideoGame(var name: String, val publisher: String, private var reviewScore: Int
```
# what is copy() Function in Data class?
Since our data classes are immutable, we must create a copy if we wish to change some data. We are also able to specify if we only wish to change specific attributes for the new copy. For example, if we wish to change the review score of a game, this can be done by writing:

```
val game: VideoGame = VideoGame("Gears of War", "Epic Games", 8)
val betterGame = game.copy(reviewScore = 10)
```

# How to enable extension in android which will remove findViewById?

```
apply plugin: 'kotlin-android-extensions'
```

# How to make class parceable in kotlin?
 
	
	 android
	 {
	    androidExtensions 
	    {
		experimental = true
	    }
	  }
	  
 <b>In data class </b> <br>
	```
	@Parcelize
	data class Error(var ErrorCause:String,var MessageToUser:String,var Code: Int?):Parcelable
	```
	
# MVVM

## Loading data with loaders
![](https://developer.android.com/images/topic/libraries/architecture/viewmodel-replace-loader.png)

## Loading data with ViewModel

![](https://developer.android.com/images/topic/libraries/architecture/viewmodel-replace-loader.png)


# Android Architecture Component -
## LiveData -
LiveData is an observable data holder. It is also a lifecycle aware. By lifecycle aware I mean, it can only be observed in the context of a lifecycle, more precisely in the context of an Activity or Fragment lifecycle. By passing the reference of an Activity or Fragment, it can understand whether your UI onScreen, offScreen or Destroyed. After passing the UI object to LiveData, whenever the data in the live data changes. It notifies the lifecycle owner with updates and then the UI redraw itself with updates.

### Advantages of LiveData:
### No memory leaks: 
Observers are bound to Lifecycle objects and clean up after themselves when their associated life cycle destroyed.

### No crashes due to stopped activities: 
It means if an activity is in the back stack, then it doesn’t receive any LiveData stream.

### Always up to date: 
It receives the latest data upon becoming active again.

### No more manual life:
cycling handle: Observers just observe relevant data and don’t stop or resume observation. LiveData manages all of this under the hood.

### Ensures your UI matches the data state: 
LiveData notifies the observer object whenever lifecycle state changes. Instead of updating the UI every-time when the data changes, your observer can update the UI every time there’s a change.
### Proper configuration changes: 
If an observer is recreated due to a configuration change, like device rotation, it immediately receives the latest available data.
### Sharing resources: 
You can extend LiveData object using the singleton pattern to wrap system services so that they can be shared in your app.


# Constraint Layout
Concepts -
### 1.0 Version
1. Constraints
2. Margins
3. Baselines
4. Chains

### 2.0 Version
1. Guidelines <br>
- https://www.youtube.com/watch?v=GDL7fWejHkI
2. Barriers
3. Groups
4. Helpers


# What is MVVM?
### Android MVVM
### MVVM stands for Model, View, ViewModel.

### Model:
This holds the data of the application. It cannot directly talk to the View. Generally, it’s recommended to expose the data to the ViewModel through Observables.
### View:
It represents the UI of the application devoid of any Application Logic. It observes the ViewModel.
### ViewModel:
It acts as a link between the Model and the ViewModel. It’s responsible for transforming the data from the Model. It provides data streams to the View. It also uses hooks or callbacks to update the View. It’ll ask for the data from the Model.
The following flow illustrates the core MVVM Pattern.

### android mvvm pattern

 ![](https://github.com/siddhpatil6/AndroidInterview-Questions/blob/master/android-mvvm-pattern.png)
 

### How does this differ from MVP?

ViewModel replaces the Presenter in the Middle Layer.
The Presenter holds references to the View. The ViewModel doesn’t.
The Presenter updates the View using the classical way (triggering methods).
The ViewModel sends data streams.
The Presenter and View are in a 1 to 1 relationship.
The View and the ViewModel are in a 1 to many relationship.
The ViewModel does not know that the View is listening to it.

## There are two ways to implement MVVM in Android:
 1. Data Binding
 2. RXJava


# Difference between run and apply in kotlin ?


# How to give button equal space?
https://stackoverflow.com/questions/37518745/evenly-spacing-views-using-constraintlayout

# Differennce betweeen Data Class and Class?

# What is view binding library or like butterknife in kotlin?

https://antonioleiva.com/kotlin-android-extensions/

# What is Optional Parameters in Kotlin?
In particular if you have the following code:
```
class Book{

   fun update(title: String, subtitle : String = "No Subtitle", abridged : Boolean = false){
      
   }
}
```
In kotlin you can simply make a call like the following:

```
book.update("Arbian Nights")
```

Or just specify a particular parameter that you want:

book.update("Arabian Nights", abridged = true)
Or even change the optional parameter order:

```
book.update("Arabian Nights", abridged = true, subtitle = "An Oxford Translation")
```

Going back to the question, since Java doesn’t have support for optional parameters how is this handled? Does Kotlin generate all the different variations making any combination safe? I can see how trying to support this on the Java side could lead to problems with allowing changeable order. For example if the function was defined as this:

```
class Book{

   fun update(title: String, subtitle : String = "No Subtitle", author : String = “Jim Baca”){
      
   }
}
```

And we wanted to call this from the Java side. How would the compiler know the difference between:

```
book.update("Arabian Nights", “No Subtitle”);
```

And this method call?

```
book.update("Arabian Nights", “Jim Baca”);
```

Given the above it’s not surprising that Kotlin doesn’t support optional parameters on the Java side. I was hoping for optional parameters would define the following functions:


```
update(String title)
update(String title, String subtitle)
update(String title, String subtitle, String author)
```


Update: Thanks Kirill Rakhman for pointing out that you can use @JvmOverloads annotation before your method definition you get this desired effect.

Of course this would create an odd situation since it doesn’t allow for parameter reordering which. Optional parameters in Kotlin become mandatory in Java and the parameter order is the exact same as it is defined in Kotlin, unless @JvmOverloads annotation is used. E.g. if we had this definition in Kotlin:


```
class Book{

   fun update(title: String, subtitle : String = "No Subtitle", author : String = “Jim Baca”){
      
   }
}
```

Then the correct way of calling the function would be:

```
book.update("Arabian Nights", “No Subtitle”, “Jim Baca”);
```

However if we had this:

```
class Book{
   
   @JvmOverloads fun update(title: String, subtitle : String = "No Subtitle", author : String = “Jim Baca”){
      
   }
}
```

Then we could call it with any of the following

```
book.update("Arabian Nights");
book.update("Arabian Nights", "No Subtitle");
book.update("Arabian Nights", "No Subtitle", "Jim Baca");
```


# How do we return multiple values from a function in Kotlin?

You can't create arbitrary tuples in Kotlin, instead, you can use data classes. One option is using the built in Pair and Triple classes that are generic and can hold two or three values, respectively. You can use these combined with destructuring declarations like this:

```
fun getPair() = Pair(1, "foo")

val (num, str) = getPair()
You can also destructure a List or Array, for up to the first 5 elements:

fun getList() = listOf(1, 2, 3, 4, 5)

val (a, b, c, d, e) = getList()
The most idiomatic way however would be to define your own data class, which allows you to return a meaningful type from your function:

data class Time(val hour: Int, val minute: Int, val second: Int)

fun getTime(): Time {
    ...
    return Time(hour, minute, second)
}

val (hour, minute, second) = getTime()
```


# What is functional interface?
Functional Interfaces In Java
A functional interface is an interface that contains only one abstract method. They can have only one functionality to exhibit. From Java 8 onwards, lambda expressions can be used to represent the instance of a functional interface. A functional interface can have any number of default methods. Runnable, ActionListener, Comparable are some of the examples of functional interfaces.
Before Java 8, we had to create anonymous inner class objects or implement these interfaces.

```
// Java program to demonstrate functional interface 
  
class Test 
{ 
    public static void main(String args[]) 
    { 
        // create anonymous inner class object 
        new Thread(new Runnable() 
        { 
            @Override
            public void run() 
            { 
                System.out.println("New thread created"); 
            } 
        }).start(); 
    } 
} 
```



# Difference between abstract class and interface?

https://www.javatpoint.com/difference-between-abstract-class-and-interface

# can we create instance of abstract class?
No, you cannot create an instance of an abstract class because it does not have a complete implementation. The purpose of an abstract class is to function as a base for subclasses. It acts like a template, or an empty or partially empty structure, you should extend it and build on it before you can use it.
 
# Lateinit versus lazy
At first, lateinit var and by lazy {...} sound quite similar. However, there are significant differences between the two of them: <br>

The lazy {...} delegate can only be used for val properties; lateinit can only be used for var properties.
A lateinit var property can't be compiled into a final field, hence you can't achieve immutability.
A lateinit var property has a backing field to store the value, whereas lazy {...} creates a delegate object that acts as a container for the value once created and provides a getter for the property. If you need the backing field to be present in the class, you will have to use lateinit. <br>
The lateinit property cannot be used for nullable properties or Java primitive types. This is a restriction imposed ...
<br>
 
 # Differenece between var and val?
 Var – Variable
– The object stored in the variable could change (vary) in time. Means you can change or assign new value in variable latter.

```
var reasignableString = "hello"
reasignableString = "Hello eyehunt" // OK

var reasignableString = "hello"
reasignableString = "Hello eyehunt" // OK
``` 

Val – Value
– The object stored in val, could not vary in time. Once assigned the val becomes read only, like a constant in Java Programming language (Final variables etc). The properties of the object (as Val) could be changed, but the object itself is read-only.

```
val constant = "hello"
constant = "Var vs Val" // Not allowed for `val`

val constant = "hello"
constant = "Var vs Val" // Not allowed for `val`
val in kotlin is like final keyword in java
```
 

### Other ways to says same :
Variables defined with var are mutable (Read and Write)

Variables defined with val are immutable (Read only)
 
 # Difference between String,StringBuffer ,StringBuilder?
  ![](https://www.startertutorials.com/corejava/wp-content/uploads/2015/09/Strings-Classes-in-Java.png)
 
 # Access modifiers
 ![](https://www.geeksforgeeks.org/wp-content/uploads/Access-Modifiers-in-Java.png)
 
# How to do ViewBinding or how we can achive it?
ButterKnife <br>
https://www.androidhive.info/2017/10/android-working-with-butterknife-viewbinding-library/

# why compile changed to implementation?
https://stackoverflow.com/questions/44402024/why-android-change-compile-to-implementation-configuration-in-gradle-depende
https://stackoverflow.com/questions/44493378/whats-the-difference-between-implementation-and-compile-in-gradle

# Explain MVVM?

https://medium.com/upday-devs/android-architecture-patterns-part-3-model-view-viewmodel-e7eeee76b73b
https://medium.com/@husayn.hakeem/android-by-example-mvvm-data-binding-introduction-part-1-6a7a5f388bf7

# Why to use Application class in android?
https://github.com/codepath/android_guides/wiki/Understanding-the-Android-Application-Class

# wht to use multiDex android?
https://android.jlelse.eu/how-to-configure-multidex-in-an-application-android-f221198707ed
https://stackoverflow.com/questions/33588459/what-is-android-multidex

# What is App Indexing?
Firebase App Indexing gets your app into Google Search. If users have your app installed, they can launch your app and go directly to the content they're searching for. App Indexing reengages your app users by helping them find both public and personal content right on their device, even offering query autocompletions to help them more quickly find what they need. If users don’t yet have your app, relevant queries trigger an install card for your app in Search results.
https://firebase.google.com/docs/app-indexing/ <br>
https://searchengineland.com/app-indexing-matters-future-search-216884

# What is APK Expansion?
Google Play currently requires that your APK file be no more than 100MB. For most applications, this is plenty of space for all the application's code and assets. However, some apps need more space for high-fidelity graphics, media files, or other large assets. Previously, if your app exceeded 100MB, you had to host and download the additional resources yourself when the user opens the app. Hosting and serving the extra files can be costly, and the user experience is often less than ideal. To make this process easier for you and more pleasant for users, Google Play allows you to attach two large expansion files that supplement your APK.

Google Play hosts the expansion files for your application and serves them to the device at no cost to you. The expansion files are saved to the device's shared storage location (the SD card or USB-mountable partition; also known as the "external" storage) where your app can access them. On most devices, Google Play downloads the expansion file(s) at the same time it downloads the APK, so your application has everything it needs when the user opens it for the first time. In some cases, however, your application must download the files from Google Play when your application starts.

https://developer.android.com/google/play/expansion-files <br>
https://support.google.com/googleplay/android-developer/answer/2481797?hl=en

# How deep linking works? what tags we use?
Deeplinks are a concept that help users navigate between the web and applications. They are basically URLs which navigate users directly to the specific content in applications.
<b> What is Android App Links? </b>
On the other hand, Android App Links allow an app to designate itself as the default handler of application domain or URL. Unfortunately It works only on on Android 6.0 (API level 23) and higher.<br>
Tags - <br>
action category data <br>
P.S - Please check link to clearly understand - https://medium.com/@muratcanbur/intro-to-deep-linking-on-android-1b9fe9e38abd

# What is AsyncLoader? <br>
AsyncTaskLoader is the loader equivalent of AsyncTask. AsyncTaskLoader provides a method, loadInBackground(), that runs on a separate thread. The results of loadInBackground() are automatically delivered to the UI thread, by way of the onLoadFinished() LoaderManager callback <br>

# What is Multithreadind & How we can priotorize them?

You can have a look at the compatibility library's source code to get more info. What a FragmentActivity does is:

keep a list of LoaderManager's
make sure they don't get destroyed when you flip your phone (or another configuration change occurs) by saving instances using onRetainNonConfigurationInstance()
kick the right loader when you call initLoader() in your Activity
You need to use the LoaderManager to interface with the loaders, and provide the needed callbacks to create your loader(s) and populate your views with the data they return.

Generally it should be easier than managing AsyncTask's yourself. However, AsyncTaskLoader is not exactly well documented, so you should study the example in the docs and/or model your code after CursorLoader.

# How to handle AsyncTask when Activity Destroyed?

AsyncTask is an abstract Android class which helps the Android applications to handle the Main UI thread in efficient way. AsyncTask class allows us to perform long lasting tasks/background operations and show the result on the UI thread without affecting the main thread.

 AsyncTask processes are not automatically killed by the OS. AsyncTask processes run in the background and is responsible for finishing it's own job in any case. You can cancel your AsycnTask by calling cancel(true) method. This will cause subsequent calls to isCancelled() to return true. After invoking this method, onCancelled(Object) method is called instead of onPostExecute() after doInBackground() returns.
 
# difference between setContetView and inflate method?
<br>
setContentView is an Activity method only. Each Activity is provided with a FrameLayout with id "@+id/content" (i.e. the content view). Whatever view you specify in setContentView will be the view for that Activity. Note that you can also pass an instance of a view to this method, e.g. setContentView(new WebView(this)); The version of the method that you are using will inflate the view for you behind the scenes.

Fragments, on the other hand, have a lifecycle method called onCreateView which returns a view (if it has one). The most common way to do this is to inflate a view in XML and return it in this method. In this case you need to inflate it yourself though. Fragments don't have a setContentView method
<br>
# if we didn't mentioned setContentView in activity?

# broadcast receiver runs on which thread?
UI Thread

# What is the hashCode() and equals() used for?
https://www.journaldev.com/21095/java-equals-hashcode

# What is the difference between using == and .equals on an object?
Difference between == and .equals() method in Java
In general both equals() and “==” operator in Java are used to compare objects to check equality but here are some of the differences between the two:

Main difference between .equals() method and == operator is that one is method and other is operator.
We can use == operators for reference comparison (address comparison) and .equals() method for content comparison. In simple words, == checks if both objects point to the same memory location whereas .equals() evaluates to the comparison of values in the objects.
If a class does not override the equals method, then by default it uses equals(Object o) method of the closest parent class that has overridden this method. See this for detail
Coding Example:
// Java program to understand 
// the concept of == operator
```
public class Test {
    public static void main(String[] args)
    {
        String s1 = new String("HELLO");
        String s2 = new String("HELLO");
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```
# Difference between wait and sleep

A wait can be "woken up" by another thread calling notify on the monitor which is being waited on whereas a sleep cannot. Also a wait (and notify) must happen in a block synchronized on the monitor object whereas sleep does not:
```
Object mon = ...;
synchronized (mon) {
    mon.wait();
} 
```
At this point the currently executing thread w
aits and releases the monitor. Another thread may do
```
synchronized (mon) { mon.notify(); }
```
(On the same mon object) and the first thread (assuming it is the only thread waiting on the monitor) will wake up.

You can also call notifyAll if more than one thread is waiting on the monitor - this will wake all of them up. However, only one of the threads will be able to grab the monitor (remember that the wait is in a synchronized block) and carry on - the others will then be blocked until they can acquire the monitor's lock.

Another point is that you call wait on Object itself (i.e. you wait on an object's monitor) whereas you call sleep on Thread.

Yet another point is that you can get spurious wakeups from wait (i.e. the thread which is waiting resumes for no apparent reason). You should always wait whilst spinning on some condition as follows:
```
synchronized {
    while (!condition) { mon.wait(); }
}
```
# How thread communicate with each other?
Inter-thread communication in Java
Inter-thread communication or Co-operation is all about allowing synchronized threads to communicate with each other.

Cooperation (Inter-thread communication) is a mechanism in which a thread is paused running in its critical section and another thread is allowed to enter (or lock) in the same critical section to be executed.It is implemented by following methods of Object class:

wait()
notify()
notifyAll()

# How to maintain connectivity in bluetooh, wifi?

Discovering devices is only the first step to any Bluetooth operations, such as listening for incoming connection, requesting a connection, accepting a connection, and exchanging data. The connection mechanism follows that of the client-server model. In general, to implement a connection between two devices, one of them has to act as the server and the other the client. The server makes itself discoverable and waits for connection request from client. The client on the other hand initiates the connection using the server device's MAC address discovered during a discovery process.

1.Call the "listenUsingRfcommWithServiceRecord()" method of the "BluetoothAdapter" handle to return a secure RFCOMM (Radio Frequency Communication) "BluetoothServerSocket" with Service Record. For example:

```
BluetoothServerSocket  bluetoothServerSocket = bluetoothAdapter.listenUsingRfcommWithServiceRecord(getString(R.string.app_name), uuid);
```

The first string parameter is the service name which could be the app name. The second parameter is a Universally Unique Identifier (UUID) which is a standardized 128-bit format for a string ID that is used to uniquely identify the Bluetooth service in the app. The system will automatically write the service name, UUID, and the RFCOMM channel to the Service Discovery Protocol (SDP) database on the device. Remote Bluetooth devices can then use this same UUID to query the SDP server and discover which channel and service to connect to. You can generate a UUID string using some online UUID generator, and then convert the string to a UUID like this:

```
private final static UUID uuid = UUID.fromString("fc5ffc49-00e3-4c8b-9cf1-6b72aad1001a");
```
2. Call the "accept()" method of the "BluetoothServerSocket" to start listening for connection requests. The call will block the current thread until a connection is accepted or an exception has occurred. The "BluetoothServerSocket" will only accept a connection request that has a matching UUID. Once a connection is accepted, "accept()" will return a "BluetoothSocket" to manage the connection. Once the "BluetoothSocket" is acquired, you should close "BluetoothServerSocket" by calling its "close()" method as it is no longer needed. For example:
```
BluetoothSocket bluetoothSocket = bluetoothServerSocket.accept();
bluetoothServerSocket.close();
```
## Setting Up a Connecting Client
Once you have set up a device as a discoverable server holding an open "BluetoothServerSocket" and listening for connection request, any other Bluetooth device within range can discover and initiate connection request to it as a client. For that to happen, this client device must know the MAC address of that server device and a matching UUID to that particular connection.
```
listview.setOnItemClickListener(new AdapterView.OnItemClickListener() {
    @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        String  itemValue = (String) listview.getItemAtPosition(position);
        String MAC = itemValue.substring(itemValue.length() - 17);
        BluetoothDevice bluetoothDevice = bluetoothAdapter.getRemoteDevice(MAC);
        // Initiate a connection request in a separate thread
        ConnectingThread t = new ConnectingThread(bluetoothDevice);
        t.start();
    }
});
```
https://www.codeproject.com/Articles/814814/Android-Connectivity

# How to maintain stack of request androd ?

# How to Handle seat booking in app?
The classic method to do this is to use a transactional database (so there's no clashes) and to do a tentative allocation of the seat to you that expires after some length of time (e.g., 10 minutes for kiosks) that gives you enough time to pay. If the (customer-visible) transaction falls through or times out, the seat allocation can be released back into the pool. (All state changes are processed via the transactional database, and one customer-visible transaction might require many database-level transactions.)

Airlines will use a similar system (though much more complex due to the need to handle multiple flight legs!) for booking seats online. I would imagine that the timeout would be considerably longer; airline tickets are usually booked further ahead than movie tickets, and are more expensive as well.

# Number of Sensors in android?
categories of sensors.

## Motion Sensors
## Environmental sensors
## Position sensors

1. TYPE_ACCELEROMETER

Measures the acceleration force in m/s2 that is applied to a device on all three physical axes (x, y, and z), including the force of gravity.

2. TYPE_AMBIENT_TEMPERATURE

Measures the ambient room temperature in degrees Celsius (°C). See note below.	

3. TYPE_GYROSCOPE

Measures a device's rate of rotation in rad/s around each of the three physical axes (x, y, and z).

4. TYPE_PROXIMITY

Measures the proximity of an object in cm relative to the view screen of a device. This sensor is typically used to determine whether a handset is being held up to a person's ear.

5. TYPE_TEMPERATURE

Measures the temperature of the device in degrees Celsius (°C). This sensor implementation varies across devices and this sensor was replaced with the TYPE_AMBIENT_TEMPERATURE sensor in API Level 14


# why we need UML diagram?

A complex enterprise application with many collaborators will require a solid foundation of planning and clear, concise communication among team members as the project progresses.

Visualizing user interactions, processes, and the structure of the system you're trying to build will help save time down the line and make sure everyone on the team is on the same page.

## Structural UML diagrams

* Class diagram
* Package diagram
* Object diagram
* Component diagram
* Composite structure diagram
* Deployment diagram

## Behavioral UML diagrams

* Activity diagram
* Sequence diagram
* Use case diagram
* State diagram
* Communication diagram
* Interaction overview diagram
* Timing diagram

# How to identify signal strength of WIFI and Mobile Data?
Wifi:

```
WifiManager wifiManager = (WifiManager)context.getSystemService(Context.WIFI_SERVICE);
int linkSpeed = wifiManager.getConnectionInfo().getRssi();
```

In case of mobile it should work:
```
TelephonyManager telephonyManager =        (TelephonyManager)this.getSystemService(Context.TELEPHONY_SERVICE);
CellInfoGsm cellinfogsm = (CellInfoGsm)telephonyManager.getAllCellInfo().get(0);
CellSignalStrengthGsm cellSignalStrengthGsm = cellinfogsm.getCellSignalStrength();
cellSignalStrengthGsm.getDbm();
```
Then You should compare this signal levels and if WIFI signal is better keep it turn on, but if mobile is better disconnect wifi

# what feature came in oreo for development?
## Background Execution Limits

Whenever an app runs in the background, it consumes some of the device's limited resources, like RAM. This can result in an impaired user experience, especially if the user is using a resource-intensive app, such as playing a game or watching video. To improve the user experience, Android 8.0 (API level 26) imposes limitations on what apps can do while running in the background. This document describes the changes to the operating system, and how you can update your app to work well under the new limitations.

## Overview
Many Android apps and services can be running simultaneously. For example, a user could be playing a game in one window while browsing the web in another window, and using a third app to play music. The more apps are running at once, the more load is placed on the system. If additional apps or services are running in the background, this places additional loads on the system, which could result in a poor user experience; for example, the music app might be suddenly shut down.

To lower the chance of these problems, Android 8.0 places limitations on what apps can do while users aren't directly interacting with them. Apps are restricted in two ways:

## Background Service Limitations: 
While an app is idle, there are limits to its use of background services. This does not apply to foreground services, which are more noticeable to the user.

## Broadcast Limitations: 
With limited exceptions, apps cannot use their manifest to register for implicit broadcasts. They can still register for these broadcasts at runtime, and they can use the manifest to register for explicit broadcasts targeted specifically at their app.

https://androidresearch.wordpress.com/2013/05/10/dealing-with-asynctask-and-screen-orientation/

# If the Fragment inside activity & activity screen roatated while making network call, apps may crash, so how to handle that crash?

Manage the Object Inside a Retained Fragment <br>
Ever since the introduction of Fragments in Android 3.0, the recommended means of retaining active objects across Activity instances is to wrap and manage them inside of a retained “worker” Fragment. By default, Fragments are destroyed and recreated along with their parent Activitys when a configuration change occurs. Calling <b> Fragment#setRetainInstance(true) </b> allows us to bypass this destroy-and-recreate cycle, signaling the system to retain the current instance of the fragment when the activity is recreated. As we will see, this will prove to be extremely useful with Fragments that hold objects like running Threads, AsyncTasks, Sockets, etc. <br>
<br>
The sample code below serves as a basic example of how to retain an AsyncTask across a configuration change using retained Fragments. The code guarantees that progress updates and results are delivered back to the currently displayed Activity instance and ensures that we never accidentally leak an AsyncTask during a configuration change.<br>
better options for list :
https://androidresearch.wordpress.com/2013/05/10/dealing-with-asynctask-and-screen-orientation/
<br>

# How to make different layouts for phone and tablet?
```
res/layout/main_activity.xml           # For handsets (smaller than 600dp available width)
res/layout-sw600dp/main_activity.xml   # For 7” tablets (600dp wide and bigger)

```
or you can use

```
res/layout/main_activity.xml           # For handsets (smaller than 640dp x 480dp)
res/layout-large/main_activity.xml     # For small tablets (640dp x 480dp and bigger)
res/layout-xlarge/main_activity.xml    # For large tablets (960dp x 720dp and bigger)
```

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

The Firebase JobDispatcher is a library for scheduling background jobs in your Android app. It provides a JobScheduler-compatible API that works on all recent versions of Android (API level 14+) that have Google Play services installed.


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


### What are launching mode of Activity? <br>
There are four launch modes for activity. They are: <br>
1. standard <br>
2. singleTop <br>
3. singleTask <br>
4. singleInstance <br>

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
A Handler allows communicating back with UI thread from other background thread. This is useful in android as android doesn’t allow other threads to communicate directly with UI thread. <br>

* Why <br>

If you need to update the UI from another main Thread, you need to synchronize with the main thread. Because of this restrictions and complexity, Android provides additional constructed classes to handle concurrently in comparison with standard Java i.e. Handler or AsyncTask. <br>

![](https://cdn-images-1.medium.com/max/1600/1*wlcTBJ4C-T8Av9zdTbZccA.png)

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

### Service vs AsyncTask?
* http://vardhan-justlikethat.blogspot.in/2013/11/android-comparision-chart-asynctask-vs.html

**Difference Between Service,Thread,IntentService,AsyncTask?** <br>
* http://techtej.blogspot.in/2011/03/android-thread-constructspart-4.html

### How to avoid memory leak?**<br>
*  Holding reference of ui specific object in the background. <br>
*  Using static views <br>
* Using static context  <br>
* Using Context <br>
 Be careful while using context, deciding which context is suitable at places is most important. Use application context possible and use activity context only if required.
*  Never forget to say goodbye to listeners after being served. <br>
* Using inner class  <br>
* Using anonymous class  <br>
*  Using views in collection <br>



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

### Why to Use keep in proguard?
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

