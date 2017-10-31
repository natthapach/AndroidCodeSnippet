# Android Code Snippet
## Simple Activity
suitable for use with fragment
### activity xml file
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_info"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <FrameLayout
        android:id="@+id/container_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
    </FrameLayout>
 	<TextView
     	android:id="@+id/textView"
     	android:layout_width="match_parent"
    	android:layout_height="match_parent"
    	android:freezesText="true"/> 	// for restore view

</RelativeLayout>
```
### activity java file
```java
public class Activity extends AppCompatActivity {

    ... attribute
    ... view instance

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity);

        if(savedInstanceState == null){
            getSupportFragmentManager().beginTransaction()
            .add(R.id.container_frame, Fragment.newInstance())
            .commit();
        }

        initInstance();
    }

    private void initInstance() {
        ... initialize view instance
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        // save only activity variable
    }

    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
        // restore value to activity
    }
}
```
## Contextor
must implement with custom application
```java
public class Contextor {
    private static Contextor ourInstance = new Contextor();
    private Context mContext;

    public static Contextor getInstance() {
        return ourInstance;
    }

    private Contextor() {
    }
    public void init(Context context){
        mContext = context;
    }
    public Context getContext(){
        return mContext;
    }
}
```
## Custom Application
must implment with contextor
```java
public class MainApplication extends Application{

    @Override
    public void onCreate() {
        super.onCreate();
        Contextor.getInstance().init(getApplicationContext());
    }

    @Override
    public void onTerminate() {
        super.onTerminate();
    }
}
```
## Singleton
```java
public class Singleton {
    private static Singleton ourInstance;
    private Context mContext;
    ... attribute
    public static StudentInfoUtil getInstance() {
        if(ourInstance == null)
           ourInstance =  new StudentInfoUtil();
        return ourInstance;
    }
    
    private StudentInfoUtil() {
        mContext = Contextor.getInstance().getContext();
    }

    ... method
}
```
## Fragment
```java
public class SFragment extends Fragment {

    ... attribute
    ... view instance

    public static SFragment newInstance(parameter ...){
        SFragment instance = new SFragment();
        /*
            set parameter to argument
         */
        Bundle bundle = new Bundle();
        // put parameter to bundle
        // set bundle to argument
        instance.setArguments(bundle);
        return instance;
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        /*
            get value from argument
         */
        // sumVar = getArgument().getInt("sumVar");
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, 
 			     @Nullable ViewGroup container, 
 			     @Nullable Bundle savedInstanceState) {
        View rootView = inflater.inflate(R.layout.fragment_student_info,
 					 container, false);
        initInstance(rootView);
        return  rootView;
    }

    private void initInstance(View rootView) {

    }

}
```
### Custom View Group
```java
public class RecipeItemView extends FrameLayout {
    public RecipeItemView(@NonNull Context context) {
        super(context);
        initInflate();
        initInstance();
    }

    public RecipeItemView(@NonNull Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        initInflate();
        initInstance();
    }

    public RecipeItemView(@NonNull Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initInflate();
        initInstance();
    }

    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
    public RecipeItemView(@NonNull Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        initInflate();
        initInstance();
    }

    private void initInflate(){
        inflate(getContext(), R.layout.view_recipe_item, this);
    }

    private void initInstance(){

    }

}


```
