<script type="text/javascript">
        var head = document.getElementsByTagName('head')[0],
        meta = document.createElement('meta');
        meta.setAttribute('content','text/html; charset=utf-8');
        head.appendChild(meta);


        let title = document.querySelector("h1.project-name");
        title.innerText = "王侯不拜雨烟屐, 方知无欲是逍遥";
        title.style.fontStyle = 'italic';
</script>

# Android Navigation 实战

最新的Android支持库都是基于最新的androidx进行整合，使用最新的Android Studio会强制使用
AndroidX作为支持库。

## 集成

### 添加依赖

```
implementation deps.navigation.fragment_ktx
implementation deps.navigation.ui_ktx
implementation deps.navigation.runtime_ktx
```
这里使用配置好的，如果向了解详细依赖包可以参考version.gradle

### 创建Navigation布局文件

使用Navigation首先需要创建一个Navigation布局文件，在`res/`目录下新建`navigation`目录
，然后在`res`目录右键，选择`New->Navigation resource file`，填入名称`nav_simple`。

这样就创建好一个Navigation布局文件，打开可以看到
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:id="@+id/nav_simple"
            tools:ignore="UnusedNavigation">

</navigation>
```
在布局文件的底部可以看到Design和Text两个选项，
Design是图形化的编辑器，后续大部分设计都是在这里就可以搞定了。

### 编辑器

![Navigation](https://baiyuas.github.io/img/nav01.jpg)

左侧区域是列出所有页面,每个页面定义为Destination

中间为所有页面之间关联的图形化结果

右侧可以修改每个页面的属性和其他操作

快捷区域可以添加页面,修改页面属性和其他操作

底部可以切换图形化编辑和代码编辑

## 基本使用

我们通过图形化编辑器来实现一个简单页面,我们新建一个Activity,命名为MainActivity

```
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}

```

在清单文件中注册该活动

```
    <application
            android:label="@string/app_name"
            android:theme="@style/AppTheme"
            ....>

        <activity
                android:name=".MainActivity"
                android:theme="@style/BaseAppTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        .....
```

为该活动创建布局文件

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

    <fragment
            android:layout_width="0dp"
            android:layout_height="0dp"
            android:name="androidx.navigation.fragment.NavHostFragment"
            app:defaultNavHost="true"
            app:navGraph="@navigation/nav_simple"
            android:id="@+id/nav_container"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

在布局文件中我们需要给NavHostFragment指定导航布局,在res/navigation创建一个简单的布局nav_simple.xml

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/nav_simple">

</navigation>
```

切换到图形编辑页面,在快捷工具区域点击最左侧的添加按钮,新建一个Fragment

![](https://baiyuas.github.io/img/nav02.png)

![](https://baiyuas.github.io/img/nav03.png)

这时候默认新建的Fragment为app:startDestination,也就是默认启动页面.这样启动后就可以看到我们创建的页面了

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/nav_sample"
        app:startDestination="@id/firstFragment">

    <fragment
            android:id="@+id/firstFragment"
            android:name="com.example.navigation.FirstFragment"
            android:label="FirstFragment" />
</navigation>
```


## 页面跳转

以前我们切换Fragment页面都是类似如下代码

```
    supportFragmentManager.beginTransaction()
        .add(R.id.bottomNavigationView, fragemnt)
        .commit()
```

但是使用Navigation简单很多.
我们按着上面的方式在添加一个目标页面SecondFrament

![](https://baiyuas.github.io/img/nav04.png)

![](https://baiyuas.github.io/img/nav05.png)

如上图只要连线到指定目标页面就可以了,xml布局中会自动添加如下代码

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/nav_"
        app:startDestination="@id/firstFragment">

    <fragment
            android:id="@+id/firstFragment"
            android:name="com.example.navigation.FirstFragment"
            android:label="FirstFragment" >
        // 自动生成
        <action
                android:id="@+id/action_firstFragment_to_secondFragment"
                app:destination="@id/secondFragment" />
    </fragment>
    <fragment
            android:id="@+id/secondFragment"
            android:name="com.example.navigation.SecondFragment"
            android:label="SecondFragment" />
</navigation>
```

这样我们在FristFragment页面调用如下代码就可以实现页面的切换了

```
  btn_jump.setOnClickListener {
      findNavController().navigate(R.id.action_firstFragment_to_secondFragment)
  }
``` 

但是Navigation切换页面是通过replace方法,所以返回上一个页面Fragment会重新初始化重建,解决方法可以参考[https://blog.csdn.net/WitheredSkull/article/details/88532687](https://blog.csdn.net/WitheredSkull/article/details/88532687)

## 参数传递

需要传递参数只需要调用NavController重载的navigation()方法

    navigate(@IdRes int resId, @Nullable Bundle args) 

将参胡通过Bundle传递即可,在目标页面通过`arguments`变量就可以获取到

```
  arguments?.let {
      tv_argument.text = it.getString("Arg_Name")
  }
```

对于页面跳转,传递数据,还可以通过navigation-arg插件来实现,会更加简单

应用插件需要在工程根目录下添加build.gradle插件

```
dependencies {
        classpath deps.android_gradle_plugin
        classpath deps.kotlin.plugin
        classpath deps.navigation.safe_args_plugin // navigation-safe-args-gradle-plugin
    }
```
app的mobule下build.gradle中应用插件

```
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
// 应用插件
apply plugin: "androidx.navigation.safeargs.kotlin"
```

这样点击编译后就会在build目录下自动生成相关类,如下图:

![](https://baiyuas.github.io/img/nav06.png)

在需要跳转的地方按着如下调用

```
FirstFragmentDirections.actionFirstFragmentToSecondFragment()
```

在目标页面获取参数可以通过`navArgs`来获取

```
    private val receiveArg by navArgs<ReceiveArgsFragmentArgs>()

    ....

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        btn_show_nav_args.setOnClickListener {
            tv_argument.text = receiveArg.ArgName
        }
    }
```

`navArgs`是Fragment一个扩展内联函数,从下面源码可以看出,通过懒加载方法读取`arguments`Bundle中的数据

```
@MainThread
inline fun <reified Args : NavArgs> Fragment.navArgs() = NavArgsLazy(Args::class) {
    arguments ?: throw IllegalStateException("Fragment $this has null arguments")
}
..

class NavArgsLazy<Args : NavArgs>(
    private val navArgsClass: KClass<Args>,
    private val argumentProducer: () -> Bundle
) : Lazy<Args> {
    private var cached: Args? = null

    override val value: Args
        get() {
            var args = cached
            if (args == null) {
                val arguments = argumentProducer()
                val method: Method = methodMap[navArgsClass]
                    ?: navArgsClass.java.getMethod("fromBundle", *methodSignature).also { method ->
                        // Save a reference to the method
                        methodMap[navArgsClass] = method
                    }

                @Suppress("UNCHECKED_CAST")
                args = method.invoke(null, arguments) as Args
                cached = args
            }
            return args
        }

    override fun isInitialized() = cached != null
```

## Deeplink跳转

Deeplink可以让我们在app外或者启动活动后,直接跳转到指定的Deeplink页面. 该功能类似ARouter中我们指定链接后,在浏览器拉起App跳转到指定页面.

我们新建一个DeeplinkFragment来展示Deeplink跳转效果.然后在nav_simple.xml中,通过图形设计面板右侧,点击`Deep Links +`,输入链接

![](https://baiyuas.github.io/img/nav07.png)

![](https://baiyuas.github.io/img/nav08.png)


在xml文件中会生成如下代码:

```
    <fragment
            android:id="@+id/deeplinkFragment"
            android:name="com.example.navigation.DeeplinkFragment"
            android:label="DeeplinkFragment">


        <deepLink
                android:id="@+id/deepLink"
                app:uri="www.example.com" />
    </fragment>
```

我们在清单文件中使用该Fragment的Activity添加intent-filter

```
.....

<activity android:name=".MainActivity"
            android:theme="@style/BaseAppTheme">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>

        <nav-graph android:value="@navigation/nav_simple" />
</activity>
......
```

这样在外部通过链接进入App,就可以直接打开页面. 这里是通过`<nav-graph>`标签,在编译打包后,会自动转化为`<intent-filter>`,但是默认的`scheme`为***http,https***. 如果自定义的可以自己注册intent-filter

```
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <category android:name="android.intent.category.BROWSABLE"/>
    <data android:scheme="juejin" android:host="www.deeplink.com" android:pathPattern="/.*"/>
</intent-filter>
```


* 场景1

安装好App后,我们用adb命令,通过链接打开指定的页面

    adb shell am start -a android.intent.action.VIEW -d "http://www.deeplink.com"

* 场景2

通过PendingIntent拉起指定页面,这里通过NavDeepLinkBuilder可以构建PendingIntent

```
NavDeepLinkBuilder(context)
        .setGraph(R.navigation.nav_simple)
        .setDestination(R.id.deeplinkFragment)
        .setArguments(Bundle().apply {
            putString("Arg_From", "AppWidget")
        })
        .createPendingIntent()
```

* 场景3

通过h5唤起App,这里一般就需要自己定义scheme(非http,https),否则浏览器会识别为链接打开,不会拉起App(Google Chrome除外)

## 跳转动画

Fragment切换可以通过给`<action>`配置如下属性实现

* app:enterAnim

进入页面动画

* app:exitAnim

退出页面动画

* app:popEnterAnim

退出页面再次显示动画

* app:popExitAnim

进入页面被弹出动画

如下示例:

        <action
                android:id="@+id/action_secondFragment_to_animFragment"
                app:destination="@id/animFragment"
                app:enterAnim="@anim/right_in"
                app:exitAnim="@anim/fade_out"
                app:popEnterAnim="@anim/left_in"
                app:popExitAnim="@anim/right_out"
                app:popUpTo="@id/firstFragment" />


代码实现 

NavController中有个重载的方法

    navigate(@NonNull NavDirections directions, @Nullable NavOptions navOptions)

NavOptions与xml中<action>属性是对应的,所以可以通过NavOptions.Builder设置动画

```
findNavController().navigate(
    SecondFragmentDirections.actionSecondFragmentToAnimFragment(), NavOptions.Builder()
        .setEnterAnim(R.anim.nav_default_enter_anim)
        .setExitAnim(R.anim.nav_default_exit_anim)
        .setPopEnterAnim(R.anim.nav_default_pop_enter_anim)
        .setPopExitAnim(R.anim.nav_default_pop_exit_anim)
        .build()
)
```

***说明:弹出返回popUp属性***

popUp指定的页面必须在返回堆栈内,否则无效固
popUpToInclusive true 则指定要返回的页面也会弹出堆栈

问题:在设置了popUp后属性app:exitAnim的动画无效果
Fixed:
A->B->C
B->C 设置的action动画app:exitAnim不会作用与B
A->B 设置的action的动画app:popExitAnim作用于B退出
因为B在进入C的时候是弹出堆栈所以设置弹出动画
而B->C设置的弹出动画则作用于C退出

## 共享元素切换

Navigation也支持共享元素的切换.通过调用NavController重载的navigate方法

    public void navigate(@NonNull NavDirections directions,
            @NonNull Navigator.Extras navigatorExtras) 

参数1是掉转目标,参数2指定共享的View,如下

```
            val extra = FragmentNavigatorExtras(
                iv_thumb to SHARE_ELEMENT_NAME
            )
            findNavController().navigate(
                FirstFragmentDirections.actionFirstFragmentToShareElementFragment(),
                extra
            )
```

注意这里的SHARE_ELEMENT_NAME与下面给View设置的`transitionName`属性值是相同的

同时需要在两个Fragment的xml布局中对共享的View设置如下属性,并且值相同

     android:transitionName=""


## 对话框

对话框是使用DialogFragment,而DialogFragment本身就是集成Fragment,所以对话框的使用和导航Fragment使用方法是一样的,不同之处就是navigation的xml文件中使用的是`<dialog>`标签

```
    <dialog
            android:id="@+id/navDialogFragment"
            android:name="com.example.navigation.NavDialogFragment"
            android:label="NavDialogFragment"
            tools:layout="@layout/dialog_update_layout" />
```


## 整合DrawerLayout和BottomNavigationView

对于与DrawableLayout,BottomNavigationView一起使用主要依靠NavigationUI这个工具类,
通过重载setupWithNavController来实现对Toolbar, DrawableLayout等组件的组合控制

- 与DrawableLayout组合

协调DrwableLayout是通过调用下面方法实现

    public static void setupWithNavController(@NonNull final NavigationView navigationView,
            @NonNull final NavController navController) 


协调BottomNavigationView是通过调用下面方法实现

    public static void setupWithNavController(
            @NonNull final BottomNavigationView bottomNavigationView,
            @NonNull final NavController navController) {

核心代码示例:

```
        ....
        navController = findNavController(R.id.drawer_nav_fragment)

        appBarConfiguration = AppBarConfiguration(navController.graph, drawer_layout)

        // Set up ActionBar
        setSupportActionBar(toolbar)
        setupActionBarWithNavController(navController, appBarConfiguration)

        // Set up navigation menu
        navigation_view.setupWithNavController(navController)

        // if bottomNavigationView
        bottom_nav_view.setupWithNavController(navController)
```

在布局文件中BottomNavigationView和NavigationView都要指定`app:menu`属性. 这里要注意一点,`res/menu`下创建的文件中`<item>`标签的Id必须与`res/navigation`下fragment的id相同才可以实现点击效果

res/menu/bottom_nav_menu.xml

```
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
            android:id="@+id/home"
            android:contentDescription="@string/cd_home"
            android:icon="@drawable/ic_home"
            android:title="@string/title_home" />
    <item
            android:id="@+id/list"
            android:contentDescription="@string/cd_list"
            android:icon="@drawable/ic_list"
            android:title="@string/title_list" />
    <item
            android:id="@+id/register"
            android:contentDescription="@string/cd_form"
            android:icon="@drawable/ic_feedback"
            android:title="@string/title_register" />
</menu>

```

res/navigation/nav_bottom.xml

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/nav_bottom_simple"
        app:startDestination="@id/home">

    <fragment
            android:id="@+id/home"
            android:name="com.example.navigation.nav.Home"
            android:label="Home" />
    <fragment
            android:id="@+id/list"
            android:name="com.example.navigation.nav.List"
            android:label="List" />
    <fragment
            android:id="@+id/register"
            android:name="com.example.navigation.nav.Register"
            android:label="Register" />
</navigation>
```
我们可以通过NavigationUI中的代码找到答案

```

    public static boolean onNavDestinationSelected(@NonNull MenuItem item,
            @NonNull NavController navController) {
                .....
        NavOptions options = builder.build();
        try {
            //TODO provide proper API instead of using Exceptions as Control-Flow.
            navController.navigate(item.getItemId(), null, options); // MenuItem的Id也是导航fragment的Id
            return true;
        } catch (IllegalArgumentException e) {
            return false;
        }

        .....

```


至此Navigation组件的使用介绍完成,欢迎start[![star](https://gitee.com/baiyuas/ExampleNavigation/badge/star.svg?theme=dark)](https://gitee.com/baiyuas/ExampleNavigation)


#### **题外:踩坑小记**

更新了AndroidStudio 3.6Canary 3 到 3.6Canary 4编译后报错,记录下:

AndroidStudio升级踩坑日志:

AndroidStudio 3.6 Canary 4
kotlin_version = 1.3.40
gradle_plugin = 3.6.0-alpha04 否则报错

constrain-layout 2.0.0-alpha3以下可以,以上的版本编译报错