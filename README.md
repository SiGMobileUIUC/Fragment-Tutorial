# Fragment Tutorial

## What is a Fragment?
The main "windows" for our Android apps are called Activities. In Android, a Fragment is like a sub-Activity. It is a modular piece of the window that has its own lifecycle and can be added or removed from an Activity while the Activity is still running. A common practice that is becoming popular is to have single-Activity applications that swap Fragments in and out instead of transitioning between multiple Activites. Here is a link with more info on Fragments: https://developer.android.com/guide/components/fragments

## Tutorial
Start a new Android project and create an Empty Activity. If you don't know how to do this, go the [First-Tutorial](https://github.com/SiGMobileUIUC/FirstTutorial) and follow along there.

### How do we create Fragments?
In your project, create two new files alongside the `MainActivity.java` file. Call this files `Fragment1.java` and `Fragment2.java`, respectively. Here's what their content should be:

Fragment1
```Java
public class Fragment1 extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_1, null);
    }

    public static Fragment1 newInstance() {
        return new Fragment1();
    }
}
```

Fragment2
```Java
public class Fragment2 extends Fragment {

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_2, null);
    }

    public static Fragment2 newInstance() {
        return new Fragment2();
    }
}
```

These files are pretty much identical. Let's walk through what they do.

1. We define the classes as extending `Fragment`, which is the Android SDK class that represents fragments in code.
2. We override the `onCreateView()` function that is defined in the Android `Fragment` class. Basically, we are now telling Android how it should create the View for this Fragment. We are inflating the Views for each Fragment from layout files (which we will create in a minute).
3. We create a function `newInstance()` that creates an instance of our Fragment. This is not especially useful since our Fragment doesn't require any additional setup or arguments, but it is standard practice to do this so that code using our Fragments doesn't need to worry about the setup and creation process.

### Layout Files
Now that we've created the Java classes for our Fragments, we need to create the two layout files `fragment_1.xml` and `fragment_2.xml` so that we can actually call `R.layout.fragment_1` and `R.layout.fragment_2` in the layout code we've defined above.

Navigate to the `res` folder in your Android project and create these two files. Here is what their content should look like:

fragment_1
```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is fragment 1!" />
</LinearLayout>
```

fragment_2
```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textView2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="This is fragment 2!" />
</LinearLayout>
```
Again, these are very similar. All they do is define TextViews that say the name of the fragment. In a real application, we can have much more complex interaction and logic happening in a Fragment, just as if it was another Activity.

Now that we've define the layout for our Fragments, we also need to define a layout for our MainActivity. Put this code in the `activity_main.xml` file:

```XML
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:id="@+id/fragment1Button"
            android:text="Fragment 1"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1">
        </Button>

        <Button
            android:id="@+id/fragment2Button"
            android:text="Fragment 2"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1">
        </Button>

    </LinearLayout>

    <FrameLayout
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

What we've done here is just created two buttons that are side-by-side on the top of the screen and then a FrameLayout that acts as a "Fragment Container" in the rest of the screen. Now, what we want to do is make it so that when we press each button, it will replace the content of the FrameLayout with the correct Fragment.

### Logic
Now that we have all of our layouts define, we can start writing the logic of our app. Go to the `MainActivity.java` file and start writing in the `onCreate()` method.

The first thing we need to do is get Java versions of our Buttons, so that we can define what happens when they are clicked. To do this, use the following code:

```Java
Button fragment1Button = findViewById(R.id.fragment1Button);
Button fragment2Button = findViewById(R.id.fragment2Button);
```

Now, we need to define what happens when they are clicked. To do this, create two `OnClickListeners`, like this:

```Java
fragment1Button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
       
    }
});

fragment2Button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        
    }
});
```

Now, we must actually create the Fragments and then replace the FrameLayout content with them. To do this, use the following code:

```Java
Fragment fragment = Fragment1.newInstance();
getSupportFragmentManager().beginTransaction().replace(R.id.fragment_container, fragment).commit();
```

First, we create an instance of our Fragment. Then, we get the `SupportFragmentManager`, which is an Android SDK class. We begin a transaction (which is basically any action that involves adding, removing, replacing, etc a Fragment) to replace the content of the FrameLayout with id fragment_container with the Fragment we just created. Then we commit this transaction, which is what actually makes it happen.

So, our `MainActivity.java` should end up looking like this:

```Java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button fragment1Button = findViewById(R.id.fragment1Button);
        Button fragment2Button = findViewById(R.id.fragment2Button);

        fragment1Button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Fragment fragment = Fragment1.newInstance();
                getSupportFragmentManager().beginTransaction().replace(R.id.fragment_container, fragment).commit();
            }
        });

        fragment2Button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Fragment fragment = Fragment2.newInstance();
                getSupportFragmentManager().beginTransaction().replace(R.id.fragment_container, fragment).commit();
            }
        });
    }
}
```

If you run this and press the buttons, the fragments should get replaced by each other.