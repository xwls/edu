# Fragment

## 概述

> Fragment是在Android3.0开始引入的，主要是为了模块化管理交互界面，支持更加动态和灵活的UI设计

### 特点

- 使用时和控件类似，必须依赖Activity，不能单独存活
- 所依赖的Activity称为宿主Activity
- 一个Activity中可以有多个Fragment，一个Fragment可以被多个Activity使用
- Fragment有自己的布局和布局中控件的事件处理逻辑
- Fragment有自己的生命周期，和宿主Activity息息相关
- 在一个Activity运行期间，可以动态创建、添加和删除Fragment

## 使用Fragment

### 创建

和Activity的创建方式类似

1. 自定义一个Class继承自Fragment或Fragment子类
2. 重写`onCreateView()`方法，返回View

#### 例子

```java
public class Fragment1 extends Fragment {

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_1, null);
        return view;
    }
}
```

### 加载

#### 静态加载

在Activity的布局文件中添加fragment标签

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    tools:context="com.oaec.fragmentdemo.MainActivity">

    <fragment
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:name="com.oaec.fragment.Fragment1"
        android:id="@+id/fragment"
        android:layout_gravity="center_horizontal"
        tools:layout="@layout/fragment_1" />
</LinearLayout>
```

**注意：** fragment标签必须注明name属性和id属性：

- name属性是此标签所加载的Fragment，
- id属性是一个标记

宿主Activity可以直接获取到Fragment中的控件

#### 动态加载

**第一步：** 在Activity布局中准备显示Fragment的Layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:orientation="vertical"
    tools:context="com.oaec.fragmentdemo.MainActivity">

    <!--准备放Fragment的布局-->
    <LinearLayout
        android:id="@+id/frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

    </LinearLayout>
</LinearLayout>
```

**第二步：** 在MainActivity代码中：

```java
//1.获取Fragment管理器
FragmentManager manager = getFragmentManager();
//2.开启事物
FragmentTransaction transaction = manager.beginTransaction();
//3.添加Fragment
transaction.add(R.id.frame,new Fragment1());
//4.提交事物
transaction.commit();
```

## Fragment生命周期

和Activity一样，Fragment也有自己的生命周期。并且受到其宿主Activity的影响

1. `onAttach` - Fragment和Activity发送关联的时候执行
2. `onCreate` - 创建Fragment时执行
3. `onCreateView` - 创建Fragment布局时执行
4. `onActivityCreated` - 当宿主Activity创建完成时执行
5. `onStart` - 启动Fragment时执行
6. `onResume` - Fragment可见可交互的时候执行
7. `onPause` - 暂停Fragment时执行
8. `onStop` - 停止Fragment时执行
9. `onDestroyView` - 销毁Fragment视图时执行
10. `onDestroy` - 销毁Fragment时执行
11. `onDetach` - 和Activity取消关联时执行

## Fragment之间的通讯

### Activity向Fragment发送数据

#### 添加Fragment时发送

- 发送

```java
//1.获取Fragment管理器
FragmentManager manager = getFragmentManager();
//2.开启事物
FragmentTransaction transaction = manager.beginTransaction();
//准备要发送的数据
Bundle bundle = new Bundle();
bundle.putString("data","Activity发送到Fragment的数据");
Fragment1 f1 = new Fragment1();
f1.setArguments(bundle);
//3.添加Fragment
transaction.add(R.id.frame, f1);
//4.提交事物
transaction.commit();
```

- 接收

```java
//获取Activity传过来的Bundle
Bundle bundle = getArguments();
//从Bundle里获取数据
String data = bundle.getString("data");
textView.setText(data);
```

#### Fragment运行中发送

- 在Fragment中提供公共方法用来接收数据

```java
public void changeText(String text){
    textView.setText(text);
}
```

- 在宿主Activity找到对应的Fragment，并调用接收数据的方法

```java
FragmentManager fragmentManager = getFragmentManager();
//通过FragmentManager根据TAG寻找指定Fragment
Fragment1 fragment1 = (Fragment1) fragmentManager.findFragmentByTag("f1");
fragment1.changeText("改变文字");
```

