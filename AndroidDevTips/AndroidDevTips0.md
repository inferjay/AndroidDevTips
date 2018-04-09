
### 1. Minimum Required SDK

用API level表示你的应用支持的最低Android版本，为了支持尽可能多的设备，你应该设置为能支持你应用核心功能的最低API版本。如果某些非核心功能	尽	在较高版本的API支持，你可以只在支持这些功能的版本上开启它们(参考Supporting Different Platform Versions),此处采用默认值即可。
	
### 2. Target SDK

表示你测试过你的应用支持的最高Android版本(同样用API level表示).当Android发布最新版本后，你应该在最新版本的Android测试你的应用同时更新target sdk到Android最新版本，以便充分利用Android新版本的特性。

### 3. Compile With

是你的应用将要编译的目标Android版本，此处默认为你的SDK已安装的最新Android版本(目前应该是4.1或更高版本，如果你没有安装一个可用Android版	本，就要先用SDK Manager来完成安装)，你仍然可以使用较老的版本编译项目，但把该值设为最新版本，你可以使用Android的最新特性同时可以优化应用	来提高用户体验，运行在最新的设备上.

### 4. 列举Android SDK下载好的可用platforms

	android list targets

### 5. 命令行创建项目

	android create project --target <target-id> --name MyFirstApp \ --path <path-to-workspace>/MyFirstApp --activity MainActivity \ --package com.example.myfirstapp
		
### 6. 命令行安装应用

	adb install Apk安装包完整路径
		
### 7. 命令行打开Android Virtual Device Manager

	android avd
		
### 8. android:onClick属性

`android:onClick`属性中提供的方法名字匹配，它们的名字必须一致，特别是，这个方法必须满足以下条件： 公共的 没有返回值 有一个唯一的视图（View）参数（这个视图就是将被点击的视图） 接下来，你可以在这个方法中编写读取文本内容的代码，并将该内容传到另一个Activity。
		
###9. android:parentActivityName属性		
`android:parentActivityName` 属性在应用程序中该Activity的逻辑父类Activity的名称。 系统使用此值来实现默认导航操作，比如在安卓4.1（API级别16）或者更高版本。 使用支持库并且如下所示的 `meta-data` 元素可以为安卓旧版本提供相同功能。	

例如：
				
	<activity android:name="com.example.myfirstapp.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
    	
### 10. Menu Item的android:showAsAction属性
 
这个属性可接受的值有：
		
 	always：这个值会使菜单项一直显示在Action Bar上。　　
 	ifRoom：如果有足够的空间，这个值会使菜单项显示在Action Bar上。
 	never：这个值使菜单项永远都不出现在Action Bar上。
 	withText：这个值使菜单项和它的图标，菜单文本一起显示
 	collapseActionView：你能够让操作视窗可以折叠起来。
 	
如果你为了兼容 `Android 2.1` 以下版本使用了 Support 库，在 android 命名空间下 `showAsAction` 属性是不可用的。Support 库会提供替代它的属性，你必须声明自己的 XML 命名空间，并且使用该命名空间作为属性前缀。（一个自定义 XML 命名空间需要以你的 app 名称为基础，但是可以取任何你想要的名称，它的作用域仅仅在你声明的文件之内。）例如：
 
 	<menu xmlns:android="http://schemas.android.com/apk/res/android"
      	xmlns:yourapp="http://schemas.android.com/apk/res-auto" >
    	<!-- Search, should appear as action button -->
    	<item android:id="@+id/action_search"
          	android:icon="@drawable/ic_action_search"
          	android:title="@string/action_search"
          	yourapp:showAsAction="ifRoom"  />
    			...
	</menu>

### 11. 为下级 Activity 添加向上按钮

当运行在 `Android 4.1(API level 16)` 或更高版本，或者使用 `Support` 库中的 `ActionBarActivity` 时，实现向上导航需要你在 `manifest` 文件中声明父 `activity` ，同时在 `action bar` 中设置向上按钮可用。
	
	<activity
        android:name="com.example.myfirstapp.DisplayMessageActivity"
        android:label="@string/title_activity_display_message"
        android:parentActivityName="com.example.myfirstapp.MainActivity" >
        <!-- Parent activity meta-data to support 4.0 and lower -->
        <meta-data
            android:name="android.support.PARENT_ACTIVITY"
            android:value="com.example.myfirstapp.MainActivity" />
    </activity>
    	
然后，通过调用 `setDisplayHomeAsUpEnabled()` 来把 `app icon` 设置成可用的向上按钮：
		
	@Override
	public void onCreate(Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
    	setContentView(R.layout.activity_displaymessage);

    	getSupportActionBar().setDisplayHomeAsUpEnabled(true);
    	// If your minSdkVersion is 11 or higher, instead use:
    	// getActionBar().setDisplayHomeAsUpEnabled(true);
	}

### 12. Action Bar 风格化

> 注释：如果你为 `action bar` 使用了 Support 库的 API，那你必须使用（或重写） `Theme.AppCompat` 家族式样（甚至 `Theme.Holo` 家族，在 `API level 11 或更高版本中可用）`。如此一来，你声明的每一个式样属性都必须被声明两次：一次使用平台的式样属性（`android:` 属性），另一次使用 Support 库中的式样属性（`appcompat.R.attr`属性 —— 这些属性的上下文其实就是你的app）。更多细节请查看下面的示例。

	
####使用一个 Android 主题	
Android 包含两个基本的 activity 主题，这两个主题决定了 action bar 的颜色：

	Theme.Holo，一个 “暗” 的主题
	Theme.Holo.Light，一个 “淡” 的主题

这些主题即可以被应用到 app 全局，又可以为单一的 `activity` 通过在 `manifest` 文件中设置 `application` 元素或 activity 元素的 `android:theme` 属性。

例如：

	<application android:theme="@android:style/Theme.Holo.Light" ... />

你可以通过声明 `activity` 的主题为 `Theme.Holo.Light.DarkActionBar` 来达到如下效果：`action bar` 为暗色，其他部分为淡色。

当使用 Support 库时，必须使用 `Theme.AppCompat` 主题替代：

	Theme.AppCompat，一个“暗”的主题
	Theme.AppCompat.Light，一个“淡”的主题
	Theme.AppCompat.Light.DarkActionBar，一个带有“暗” action bar 的“淡”主题

一定要确保你使用的 `action bar icon` 的颜色与 `action bar` 本身的颜色有差异。为了能帮助到你，`Action Bar Icon Pack` 为 Holo “暗”和“淡”的` action bar` 提供了标准的 `action icon`。

#### 13. 自定义背景
	
为 `activity` 创建一个自定义主题，通过重写 `actionBarStyle` 属性来改变 `action bar` 的背景。`actionBarStyle` 属性指向另一个式样；在该式样里，通过指定一个 `drawable` 资源来重写 `background` 属性。

如果你的 app 使用了 `navigation tabs` 或 `split action bar` ，你也可以通过分别设置backgroundStacked 和 backgroundSplit 属性来为这些条指定背景。

> 注意：声明一个合适的父主题，进而你的自定义主题和式样可以继承父主题的式样，这点很重要。如果没有父式样，你的 action bar 将会失去很多式样属性，除非你自己显式的对他们进行声明。	

**仅支持 Android 3.0 和更高**
		
当仅支持 `Android 3.0 `和更高版本时，你可以像这样定义 `action bar` 的背景：

res/values/themes.xml

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    	<!-- the theme applied to the application or activity -->
    	<style name="CustomActionBarTheme"
           		parent="@android:style/Theme.Holo.Light.DarkActionBar">
        	<item name="android:actionBarStyle">@style/MyActionBar</item>
    	</style>

    	<!-- ActionBar styles -->
    	<style name="MyActionBar"
          		 parent="@android:style/Widget.Holo.Light.ActionBar.Solid.Inverse">
        		<item name="android:background">@drawable/actionbar_background</item>
    	</style>
	</resources>

然后，将你的主题应该到你的 app 全局或单个的 activity 之中：

	<application android:theme="@style/CustomActionBarTheme" ... />	
**支持 Android 2.1 和更高**
	
当使用 Support 库时，上面同样的主题必须被替代成如下：

res/values/themes.xml

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    	<!-- the theme applied to the application or activity -->
   	 	<style name="CustomActionBarTheme"
           		parent="@style/Theme.AppCompat.Light.DarkActionBar">
        	<item name="android:actionBarStyle">@style/MyActionBar</item>

        	<!-- Support library compatibility -->
        	<item name="actionBarStyle">@style/MyActionBar</item>
    	</style>

    	<!-- ActionBar styles -->
   		<style name="MyActionBar"
           parent="@style/Widget.AppCompat.Light.ActionBar.Solid.Inverse">
        	<item name="android:background">@drawable/actionbar_background</item>

        	<!-- Support library compatibility -->
        	<item name="background">@drawable/actionbar_background</item>
    	</style>
	</resources>
	
然后，将你的主题应该到你的 app 全局或单个的 activity 之中：

	<application android:theme="@style/CustomActionBarTheme" ... />

#### 14. 自定义文本颜色

修改 `action bar` 中的文本颜色，你需要分别重写每个元素的属性：

* Action bar 的标题：创建一种自定义式样，并指定 `textColor` 属性；同时，在你的自定义 `actionBarStyle` 中为 `titleTextStyle` 属性指定为刚才的自定义式样。

	> 注释：被应用到 titleTextStyle 的自定义式样应该使用 TextAppearance.Holo.Widget.ActionBar.Title 作为父式样。

* Action bar 的页签：在你的 activity 主题中重写 `actionBarTabTextStyle`
* Action 按钮：在你的 activity 主题中重写 `actionMenuTextColor`

**仅支持 Android 3.0 和更高**

当仅支持 Android 3.0 和更高时，你的式样 XML 文件应该是这样的：

res/values/themes.xml

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    	<!-- the theme applied to the application or activity -->
    	<style name="CustomActionBarTheme"
           parent="@style/Theme.Holo">
        	<item name="android:actionBarStyle">@style/MyActionBar</item>
        	<item name="android:actionBarTabTextStyle">@style/MyActionBarTabText</item>
        	<item name="android:actionMenuTextColor">@color/actionbar_text</item>
    	</style>

    	<!-- ActionBar styles -->
    	<style name="MyActionBar"
           parent="@style/Widget.Holo.ActionBar">
        	<item name="android:titleTextStyle">@style/MyActionBarTitleText</item>
    	</style>

    	<!-- ActionBar title text -->
    	<style name="MyActionBarTitleText"
           parent="@style/TextAppearance.Holo.Widget.ActionBar.Title">
        	<item name="android:textColor">@color/actionbar_text</item>
    	</style>

    	<!-- ActionBar tabs text styles -->
    	<style name="MyActionBarTabText"
           parent="@style/Widget.Holo.ActionBar.TabText">
        	<item name="android:textColor">@color/actionbar_text</item>
    	</style>
	</resources>

**支持 Android 2.1 和更高**

当使用 Support 库时，你的式样 XML 文件应该是这样的：

res/values/themes.xml

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    	<!-- the theme applied to the application or activity -->
    	<style name="CustomActionBarTheme"
           parent="@style/Theme.AppCompat">
        	<item name="android:actionBarStyle">@style/MyActionBar</item>
       	 	<item name="android:actionBarTabTextStyle">@style/MyActionBarTabText</item>
        	<item name="android:actionMenuTextColor">@color/actionbar_text</item>

        	<!-- Support library compatibility -->
        	<item name="actionBarStyle">@style/MyActionBar</item>
        	<item name="actionBarTabTextStyle">@style/MyActionBarTabText</item>
        	<item name="actionMenuTextColor">@color/actionbar_text</item>
   	 	</style>

    	<!-- ActionBar styles -->
    	<style name="MyActionBar"
           parent="@style/Widget.AppCompat.ActionBar">
        	<item name="android:titleTextStyle">@style/MyActionBarTitleText</item>

        	<!-- Support library compatibility -->
        	<item name="titleTextStyle">@style/MyActionBarTitleText</item>
    	</style>

    	<!-- ActionBar title text -->
    	<style name="MyActionBarTitleText"
           parent="@style/TextAppearance.AppCompat.Widget.ActionBar.Title">
        	<item name="android:textColor">@color/actionbar_text</item>
        	<!-- The textColor property is backward compatible with the Support Library -->
    	</style>

    	<!-- ActionBar tabs text -->
    	<style name="MyActionBarTabText"
           parent="@style/Widget.AppCompat.ActionBar.TabText">
        	<item name="android:textColor">@color/actionbar_text</item>
        	<!-- The textColor property is backward compatible with the Support Library -->
    	</style>
	</resources>
	
#### 15. 自定义 Tab Indicator

为 activity 创建一个自定义主题，通过重写 `actionBarTabStyle` 属性来改变 `navigation tabs` 使用的指示器。`actionBarTabStyle` 属性指向另一个式样资源；在该式样资源里，通过指定一个状态列表 `drawable` 来重写 `background` 属性。

> 注释：一个状态列表 `drawable` 是重要的，以便通过不同的背景来指出当前选择的 `tab` 与其他 `tab` 的区别。更多关于如何创建一个 `drawable` 资源来处理多个按钮状态，请阅读 `State List `文档。
例如，这是一个状态列表 `drawable`，为一个 `action bar tab` 的多种不同状态分别指定背景图片：

res/drawable/actionbar_tab_indicator.xml

	<?xml version="1.0" encoding="utf-8"?>
	<selector xmlns:android="http://schemas.android.com/apk/res/android">

		<!-- STATES WHEN BUTTON IS NOT PRESSED -->

    	<!-- Non focused states -->
    	<item android:state_focused="false" android:state_selected="false"
          	android:state_pressed="false"
          	android:drawable="@drawable/tab_unselected" />
    	<item android:state_focused="false" android:state_selected="true"
          	android:state_pressed="false"
          	android:drawable="@drawable/tab_selected" />

    	<!-- Focused states (such as when focused with a d-pad or mouse hover) -->
    	<item android:state_focused="true" android:state_selected="false"
          	android:state_pressed="false"
          	android:drawable="@drawable/tab_unselected_focused" />
    	<item android:state_focused="true" android:state_selected="true"
          	android:state_pressed="false"
          	android:drawable="@drawable/tab_selected_focused" />


		<!-- STATES WHEN BUTTON IS PRESSED -->

    	<!-- Non focused states -->
    	<item android:state_focused="false" android:state_selected="false"
          	android:state_pressed="true"
          	android:drawable="@drawable/tab_unselected_pressed" />
    	<item android:state_focused="false" android:state_selected="true"
        	android:state_pressed="true"
        	android:drawable="@drawable/tab_selected_pressed" />

    	<!-- Focused states (such as when focused with a d-pad or mouse hover) -->
    	<item android:state_focused="true" android:state_selected="false"
          	android:state_pressed="true"
          	android:drawable="@drawable/tab_unselected_pressed" />
    	<item android:state_focused="true" android:state_selected="true"
          	android:state_pressed="true"
          	android:drawable="@drawable/tab_selected_pressed" />
	</selector>
	
**仅支持 Android 3.0 和更高**

当仅支持 Android 3.0 和更高时，你的式样 XML 文件应该是这样的：

res/values/themes.xml

<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!-- the theme applied to the application or activity -->
    <style name="CustomActionBarTheme"
           parent="@style/Theme.Holo">
        <item name="android:actionBarTabStyle">@style/MyActionBarTabs</item>
    </style>

    <!-- ActionBar tabs styles -->
    <style name="MyActionBarTabs"
           parent="@style/Widget.Holo.ActionBar.TabView">
        <!-- tab indicator -->
        <item name="android:background">@drawable/actionbar_tab_indicator</item>
    </style>
</resources>

**支持 Android 2.1 和更高**

当使用 Support 库时，你的式样 XML 文件应该是这样的：

res/values/themes.xml

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    	<!-- the theme applied to the application or activity -->
    	<style name="CustomActionBarTheme"
           parent="@style/Theme.AppCompat">
        	<item name="android:actionBarTabStyle">@style/MyActionBarTabs</item>

        	<!-- Support library compatibility -->
        	<item name="actionBarTabStyle">@style/MyActionBarTabs</item>
   	 	</style>

    	<!-- ActionBar tabs styles -->
    	<style name="MyActionBarTabs"
           parent="@style/Widget.AppCompat.ActionBar.TabView">
        	<!-- tab indicator -->
        	<item name="android:background">@drawable/actionbar_tab_indicator</item>

        	<!-- Support library compatibility -->
        	<item name="background">@drawable/actionbar_tab_indicator</item>
    	</style>
	</resources>
	
> 更多资源 

> * 关于 action bar 的更多式样属性，请查看 [Action Bar](https://developer.android.com/guide/topics/ui/actionbar.html#Style) 指南
> * 学习更多式样的工作机制，请查看 [式样和主题](https://developer.android.com/guide/topics/ui/themes.html) 指南
> * 全面的 action bar 式样，请尝试 [Android Action Bar](http://www.actionbarstylegenerator.com/) 式样生成器	

### 16.适配不同的语言

为了支持多国语言，在 `res/` 中创建一个额外的 values 目录以连字符和ISO国家代码结尾命名，比如 `values-es/` 是包含简单的区域资源，语言代码为"es"的区域设置目录。Android会在运行时根据设备的区域设置，加载相应的资源。

若你决定支持某种语言，则需要创建资源子目录和字符串资源文件，例如:

	MyProject/
    	res/
       		values/
           		strings.xml
       		values-es/
           		strings.xml
       		values-fr/
           		strings.xml
           		
添加不同区域语言的字符串值到相应的文件。

在运行时，Android系统会根据用户设备当前的区域设置，使用相应的字符串资源。

### 17.使用字符资源

你可以在你的源代码和其他XML文件中，通过 `<string>` 元素的 `name` 属性来引用你的字符串资源。

在你的源代码中你可以通过` R.string.<string_name>` 语法来引用一个字符串资源，很多方法都可以通过这种方式来接受字符串。

例如:

	// 从你的 app's 资源中获取一个字符串资源
	String hello = getResources().getString(R.string.hello_world);

	// 或者提供给一个需要字符串作为参数的方法
	TextView textView = new TextView(this);
	textView.setText(R.string.hello_world);
	在其他XML文件中，每当XML属性要接受一个字符串值时，你都可以通过@string/<string_name>语法来引用字符串资源。

例如:

	<TextView
    	android:layout_width="wrap_content"
    	android:layout_height="wrap_content"
    	android:text="@string/hello_world" />
    	
### 18.获取SharedPreference

* **getSharedPreferences()** — 如果你需要多个通过名称参数来区分的shared preference文件, 名称可以通过第一个参数来指定。你可以在你的app里面通过任何一个Context 来执行这个方法。
* **getPreferences()** — 当你的activity仅仅需要一个shared preference文件时。因为这个方法会检索activitiy下的默认的shared preference文件，并不需要提供文件名称

### 19.android:installLocation

`android:installLocation` 属性来指定程序也可以被安装到external storage

###17.文件存储区域："internal" 与 "external" 存储

* **Internal storage**:
	* 总是可用的。
 	* 这里的文件默认是只能被你的app所访问的。
	* 当用户卸载你的app的时候，系统会把internal里面的相关文件都清除干净。
	* Internal是在你想确保不被用户与其他app所访问的最佳存储区域。
* **External storage**:
	* 并不总是可用的，因为用户可以选择把这部分作为USB存储模式，这样就不可以访问了。
	* 是大家都可以访问的，因此保存到这里的文件是失去访问控制权限的。
	* 当用户卸载你的app时，系统仅仅会删除external根目录（ `getExternalFilesDir()` ）下的相关文件。
	* External是在你不需要严格的访问权限并且你希望这些文件能够被其他app所共享或者是允许用户通过电脑访问时的最佳存储区域。

### 20.保存到Internal Storage

* **getFilesDir()** : 返回一个 File ，代表了你的app的internal目录。
* **getCacheDir()** : 返回一个 File ，代表了你的app的internal缓存目录。请确保这个目录下的文件在一旦不再需要的时候能够马上被删除，还有请给予一个合理的大小，例如1MB 。如果系统的内存不够，会自行选择删除缓存文件。为了在那些目录下创建一个新的文件，你可以使用 File() 构造器，如下：

		File file = new File(context.getFilesDir(), filename);
		
### 21.保存文件到External Storage

因为external storage可能是不可用的，那么你应该在访问之前去检查是否可用。你可以通过执行 getExternalStorageState() 来查询external storage的状态。如果返回的状态是MEDIA_MOUNTED, 那么你可以读写。示例如下：

```
 /* Checks if external storage is available for read and write */
public boolean isExternalStorageWritable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state)) {
        return true;
    }
    return false;
}

/* Checks if external storage is available to at least read */
public boolean isExternalStorageReadable() {
    String state = Environment.getExternalStorageState();
    if (Environment.MEDIA_MOUNTED.equals(state) ||
        Environment.MEDIA_MOUNTED_READ_ONLY.equals(state)) {
        return true;
    }
    return false;
}
```

尽管external storage对与用户与其他app是可修改的，那么你可能会保存下面两种类型的文件。

* **Public files** :这些文件对与用户与其他app来说是public的，当用户卸载你的app时，这些文件应该保留。例如，那些被你的app拍摄的图片或者下载的文件。
* **Private files**: 这些文件应该是被你的app所拥有的，它们应该在你的app被卸载时删除掉。尽管那些文件从技术上可以被用户与其他app所访问，实际上那些文件对于其他app是没有意义的。所以，当用户卸载你的app时，系统会删除你的app的private目录。例如，那些被你的app下载的缓存文件。
如果你想要保存文件为public形式的，请使用 [getExternalStoragePublicDirectory()](http://developer.android.com/reference/android/os/Environment.html#getExternalStoragePublicDirectory(java.lang.String)) 方法来获取一个 File 对象来表示存储在external storage的目录。这个方法会需要你带有一个特定的参数来指定这些public的文件类型，以便于与其他public文件进行分类。参数类型包括 [DIRECTORY_MUSIC](http://developer.android.com/reference/android/os/Environment.html#DIRECTORY_MUSIC) 或者 [DIRECTORY_PICTURES](http://developer.android.com/reference/android/os/Environment.html#DIRECTORY_PICTURES). 如下:

```
public File getAlbumStorageDir(String albumName) {
    // Get the directory for the user's public pictures directory.
    File file = new File(Environment.getExternalStoragePublicDirectory(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}

```

如果你想要保存文件为私有的方式，你可以通过执行getExternalFilesDir()) 来获取相应的目录，并且传递一个指示文件类型的参数。每一个以这种方式创建的目录都会被添加到external storage封装你的app目录下的参数文件夹下（如下则是albumName）。这下面的文件会在用户卸载你的app时被系统删除。如下示例：

```
public File getAlbumStorageDir(Context context, String albumName) {
    // Get the directory for the app's private pictures directory.
    File file = new File(context.getExternalFilesDir(
            Environment.DIRECTORY_PICTURES), albumName);
    if (!file.mkdirs()) {
        Log.e(LOG_TAG, "Directory not created");
    }
    return file;
}
```

如果刚开始的时候，没有预定义的子目录存放你的文件，你可以在 [getExternalFilesDir()](http://developer.android.com/reference/android/content/Context.html#getExternalFilesDir(java.lang.String\))方法中传递 null. 它会返回你的app在external storage下的private的根目录。

请记住， `getExternalFilesDir()` 方法会创建的目录会在app被卸载时被系统删除。如果你的文件想在app被删除时仍然保留，请使用 getExternalStoragePublicDirectory().

不管你是使用 `getExternalStoragePublicDirectory()` 来存储可以共享的文件，还是使用 getExternalFilesDir() 来储存那些对与你的app来说是私有的文件，有一点很重要，那就是你要使用那些类似`DIRECTORY_PICTURES` 的API的常量。那些目录类型参数可以确保那些文件被系统正确的对待。例如，那些以`DIRECTORY_RINGTONES` 类型保存的文件就会被系统的media scanner认为是ringtone而不是音乐。

### 22.查询剩余空间

如果你事先知道你想要保存的文件大小，你可以通过执行 [getFreeSpace()](http://developer.android.com/reference/java/io/File.html#getFreeSpace(\)) or [getTotalSpace()](http://developer.android.com/reference/java/io/File.html#getTotalSpace(\)) 来判断是否有足够的空间来保存文件，从而避免发生IOException。那些方法提供了当前可用的空间还有存储系统的总容量。并没有强制要求在写文件之前一定有要去检查剩余容量。你可以尝试先做写的动作，然后通过捕获 IOException 。这种做法仅适合于你并不知道你想要写的文件的确切大小。

###20.删除文件(Delete a File)

你应该在不需要使用某些文件的时候，删除它。删除文件最直接的方法是直接执行文件的 delete() 方法。

	myFile.delete();
	
如果文件是保存在internal storage，你可以通过 Context 来访问并通过执行deleteFile()进行删除

	myContext.deleteFile(fileName);

>**Note**: 当用户卸载你的app时，android系统会删除下面的文件：

>* 所有保存到internal storage的文件。
>* 所有使用getExternalFilesDir()方式保存在external storage的文件 然而，你应该手动删除所有通过 getCacheDir() 方式创建的缓存文件，还有那些通常来说不会再用的文件。

### 23.定义Schema与Contract

SQL中一个中重要的概念是schema：一种DB结构的正式声明。schema是从你创建DB的SQL语句中生成的。你可能会发现创建一个创建一个伴随类（companion class）是很有益的，这个类成为合约类（contract class）,它用一种系统化并且自动生成文档的方式，显示指定了你的schema样式。

Contract Clsss是一些常量的容器。它定义了例如URIs, 表名，列名等。这个contract类允许你在同一个包下与其他类使用同样的常量。 它让你只需要在一个地方修改列名，然后这个列名就可以自动传递给你整个code。

一个组织你的contract类的好方法是在你的类的根层级定义一些全局变量，然后为每一个table来创建内部类。

>**Note**:通过实现 [BaseColumns](http://developer.android.com/reference/android/provider/BaseColumns.html) 的接口，你的内部类可以继承到一个名为_ID的主键，这个对于Android里面的一些类似cursor adaptor类是很有必要的。这样能够使得你的DB与Android的framework能够很好的相容。

例如，下面的例子定义了表名与这个表的列名：

```
public static abstract class FeedEntry implements BaseColumns {
    public static final String TABLE_NAME = "entry";
    public static final String COLUMN_NAME_ENTRY_ID = "entryid";
    public static final String COLUMN_NAME_TITLE = "title";
    public static final String COLUMN_NAME_SUBTITLE = "subtitle";
    ...
}
```
为了防止一些人不小心实例化contract类，像下面一样给一个空的构造器。

```
// Prevents the FeedReaderContract class from being instantiated.
private FeedReaderContract() {}
```

### 24.验证是否有App可以handle某个intent
下面是一个完整的例子，演示了如何创建一个intent来查看地图

```
// Build the intent
Uri location = Uri.parse("geo:0,0?q=1600+Amphitheatre+Parkway,+Mountain+View,+California");
Intent mapIntent = new Intent(Intent.ACTION_VIEW, location);

// Verify it resolves
PackageManager packageManager = getPackageManager();
List<ResolveInfo> activities = packageManager.queryIntentActivities(mapIntent, 0);
boolean isIntentSafe = activities.size() > 0;

// Start an activity if it's safe
if (isIntentSafe) {
    startActivity(mapIntent);
}
```
### 25.显示一个 App 选择界面

为了显示chooser, 需要使用createChooser()来创建Intent

```
Intent intent = new Intent(Intent.ACTION_SEND);
...

// Always use string resources for UI text. This says something like "Share this photo with"
String title = getResources().getText(R.string.chooser_title);
// Create and start the chooser
Intent chooser = Intent.createChooser(intent, title);
startActivity(chooser);
```

![image](http://hukai.me/android-training-course-in-chinese/basics/intents/intent-chooser.png)

### 26.接收 Activity 返回的结果

启动另外一个activity并不一定是单向的。你也可以启动另外一个activity然后接受一个result回来。为了接受这个result,你需要使用 `startActivityForResult()` (而不是startActivity())。

例如，你的app可以启动一个camera程序并接受拍的照片作为result。或者你可以启动People程序并获取其中联系的人的详情作为result。

当然，被启动的activity需要指定返回的result。它需要把这个result作为另外一个intent对象返回，你的activity需要在 `onActivityResult()` 的回调方法里面去接收result。

>**Note**:在执行 startActivityForResult()时，你可以使用explicit 或者 implicit 的intent。当你启动另外一个位于你的程序中的activity时，你应该使用explicit intent来确保你可以接收到期待的结果。

对于 `startActivityForResult()` 方法中的intent与之前介绍的并没有什么差异，只不过是需要在这个方法里面多添加一个int类型的参数。这个integer的参数叫做"request code"，它标识了你的请求。当你接收到result Intent时，可以从回调方法里面的参数去判断这个result是否是你想要的。

### 27.Intent Filter

为了尽可能确切的定义你的activity能够handle哪些intent，每一个intent filter都应该尽可能详尽的定义好action与data。

主要有下面三个方面需要定义：

* Action:一个想要执行的动作的名称。通常是系统已经定义好的值，例如 ACTION_SEND 或者 ACTION_VIEW。
* Data:Intent附带数据的描述。可以使用一个或者多个属性，你可以只定义MIME type或者是只指定URI prefix，也可以只定义一个URI scheme，或者是他们综合使用。
	>**Note**: 如果你不想handle Uri 类型的数据，那么你应该指定 android:mimeType 属性。例如 text/plain or image/jpeg.
* Category:提供一个附加的方法来标识这个activity能够handle的intent。通常与用户的手势或者是启动位置有关。系统有支持几种不同的categories,但是大多数都不怎么用的到。而且，所有的implicit intents都默认是 CATEGORY_DEFAULT 类型的。

```
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
        <data android:mimeType="image/*"/>
    </intent-filter>
</activity>
```

每一个发送出来的intent只会包含一个action与type，但是handle这个intent的activity的 `<intent-filter>` 是可以声明多个`<action>`, `<category>`与`<data>` 的。

如果任何的两对action与data是互相矛盾的，你应该创建不同的intent fliter来指定特定的action与type。

例如，假设你的activity可以handle 文本与图片，无论是ACTION_SEND 还是 ACTION_SENDTO 的intent。在这种情况下，你必须为两个action定义两个不同的intent filter。因为ACTION_SENDTO intent 必须使用 Uri 类型来指定接收者使用 send 或 sendto 的地址。例如：

```
<activity android:name="ShareActivity">
    <!-- filter for sending text; accepts SENDTO action with sms URI schemes -->
    <intent-filter>
        <action android:name="android.intent.action.SENDTO"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:scheme="sms" />
        <data android:scheme="smsto" />
    </intent-filter>
    <!-- filter for sending text or images; accepts SEND action and text or image data -->
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="image/*"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>

```

>**Note**:为了接受implicit intents, 你必须在你的intent filter中包含 CATEGORY_DEFAULT 的category
