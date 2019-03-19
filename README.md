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


