# Navigation

## 1. 配置环境
  
### module 中的 build 脚本：

        def nav_version = "2.0.0"
        implementation "androidx.navigation:navigation-fragment:$nav_version" // For Kotlin use navigation-fragment-ktx
        implementation "androidx.navigation:navigation-ui:$nav_version" // For Kotlin use navigation-ui-ktx

### root 的 build 脚本：

        buildscript {
            ...
            dependencies {
                ...
                classpath "androidx.navigation:navigation-safe-args-gradle-plugin:2.0.0"
            }
        }

## 2. 概念解释

### 2.1 导航容器（navigation container）

    在项目中，一般为 Activity
  
### 2.2 导航控制器（navigation controller）

* 系统已经为我们实现了一个导航控制器，**androidx.navigation.fragment.NavHostFragment**。
* 使用方式:将以下代码加入到导航容器（ Activity 中的 layout 文件内）
```
    <fragment
            android:id="@+id/nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:defaultNavHost="true"
            app:navGraph="@navigation/nav_graph"/>
```

* 关于导航视图 app:navGraph="@navigation/nav_graph"，见 2.5。

### 2.3 目标页面（destination）

    App 中所有的 Fragment 都可作为目标页面。

### 2.4 跳转动作（action）

    App 中任意两个目标页面之间的逻辑跳转，都称作一个跳转动作。

### 2.5 导航视图（navigation graph）

    导航控制器需要指定一个导航视图，导航视图包括了所有的目标页面以及跳转动作。

## 3. FAQ

### 3.1 如何创建导航视图（navigation graph）？

    res -> 右键 -> new -> Android Resources File，在 Resource Type 中选择 Navigation。

### 3.2 如何创建一个目标页面（destination）？

    res -> navigation -> 打开一个导航视图 -> 切换到 Design 模式
    -> 点击导航视图面板中的加号（New Destination)
    ->（Create new destination) -> 输入目标页面名称

    注：取消勾选 「Include fragment factory methods」 和 「Include interface callbacks」这两个选项,一般用不到。

### 3.3 如何创建一个跳转动作（action）？

    res -> navigation -> 打开一个导航视图 -> 切换到 Design 模式
    -> 选中一个目标页面 -> 目标页面右侧会有一个圆点
    -> 拖动这个圆点链接到另一个目标页面

    这样一个跳转动作就完成了。

### 3.4 如何获取导航控制器（navigation controller）？

    有三种方式可以获取到导航控制器

* 通过 View 获取
  * Kotlin
    `View.findNavController()`
  * Java
    `Navigation.findNavController(View)`
* 通过 Fragment 获取
  * Kotlin
    `Fragment.findNavController()`
  * Java
    `NavHostFragment.findNavController(Fragment)`
* 通过 Activity 获取
  * Kotlin
    `Activity.findNavController(viewId: Int)`
  * Java
    `Navigation.findNavController(Activity, @IdRes int viewId)`

### 3.5 如何执行跳转？

* 首先，所有的跳转都是通过导航控制器（navigation controller）来完成的，所以，我们先要拿到 navigation controller 对象，见 3.4。
* 拿到 navigation controller 对象之后，我们就可以执行它的 navigate 方法，传入跳转动作的 int 类型 ID，进行相应的跳转。
* 示例：`findNavController().navigate(R.id.action_page1Fragment_to_page2Fragment )`
* 关于传递参数：只需要继续添加参数即可：
  * `val args = Bundle()`
  * `args.putString(ARG,"TEST DATA....")`
  * `findNavController().navigate(R.id.action_page1Fragment_to_page2Fragment ,args)`

### 3.6 如何控制页面的栈？

* 正常情况下，每次导航一个新的目标页面，都把目标页面置为栈顶。
* 如果某一个跳转（action）执行之后，希望把它的目标页面（destination）置顶并清除原来的栈，就需要在它对应的 action 上加入下面两个属性：
  * `app:popUpToInclusive="true"`
  * `app:popUpTo="@+id/page1Fragment"`