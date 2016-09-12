# 四大组件-Activity

[TOC]

## 四大组件

- Activity-活动
- Service-服务
- Broadcast Recever-广播接收者
- Content Provider-内容提供者

## Activity

Activity是一个程序运行时的界面，所有的交互操作，在Activity进行

### 创建Activity步骤

#### 第一种方式

1. 新建一个Class，继承Activity
2. 重写onCreate()方法
3. 绑定布局：setContentView(int layoutId)
4. 在清单文件注册

#### 第二种方式

1. 新建一个Activity
2. 填写Activity名字和布局文件的名字
3. 点finish

### 启动Activity

需要使用Intent，在构造方法中，

- 第一个参数给当前Activity的示例对象，
- 第二个参数给要跳转的Activity的类名.class

```java
//通过Intent确定要跳转的Activity
Intent intent = new Intent(this, Main2Activiy.class);
//开始跳转
startActivity(intent);
```

### Activity生命周期

1. onCreate - 创建
2. onStart - 启动
3. onResume - 可见可交互
4. onPause - 暂停
5. onStop - 停止
6. onDestory - 重启
7. onRestart - 销毁

### Activity栈

#### 栈的特点

- 栈是一种特殊的线性表
- 栈遵循着先进后出的规则
- 栈有两个动作：入栈和出栈

#### 获取Activity栈

1. 获取ActivityManager：

```java
ActivityManager manager = (ActivityManager) getSystemService(ACTIVITY_SERVICE);
```

2. 通过ActivityManager里的getRunningTask(int num)获取指定长度的栈的集合

```java
List<ActivityManager.RunningTaskInfo> runningTasks = manager.getRunningTasks(1);
```

3. 获取栈中的Activity

```java
//栈顶
String firstActivity = runningTasks.get(0).topActivity.getClassName();
//栈底
String lastActiivity = runningTasks.get(0).baseActivity.getClassName()
```
### Activity启动模式

#### launchMode

在清单文件中设置Activity的launchMode属性，可以改变Activity的启动模式，该属性可以设置一下4个值：

1. standard：默认，每次启动Activity都会创建一个新的实例
2. singleTop：启动Activity时，如果Activity位于栈顶，则不再创建实例，直接使用
3. singleTask：启动Activity时，不管Activity实例是否位于栈顶，都会销毁上方Activity实例，并将其置于栈顶，直接使用
4. singleInstance：无论当前Activity是否在同一个APP，都只有一个实例

#### onNewIntent()回调

- 当Activity实例已经存在，再次启动没有重新创建时，会执行此回调
- 形参Intent中包含传递的参数

### Activity状态恢复

#### 重构

当一个Activity遇到状态切换时，就会发生重构，如：横竖屏切换。Activity在重构时，系统会先销毁，再重新创建，数据会丢失。我们需要通过代码保存需要的数据

#### 重写方法

1. `onSavaInstanceState()`：重构之前保存数据
   * 系统即将触发Activity重构前就会被调用
   * 资源不足，系统认为这个Activity有可能被回收时会被调用
   * 参数outState，由系统负责维护和传入，用来存放需要保存的数据
2. `onRestoreInstanceState()`：重构之后恢复数据
   * 事实上onCreate的参数就足以帮助我们恢复状态
   * 如果Activity经历了save状态的方法，传入的参数中就带有我们保存下来的数据
3. `onCreate`：重构之后恢复数据
   * 在所有初始化结束后再恢复数据

#### 例子

```java
/**
 * 重构之前保存数据
 * @param outState
 */
@Override
protected void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    //保存当前进度
    outState.putString("progress",textView.getText().toString());
}

/**
 * 重构之后恢复数据
 * @param savedInstanceState
 */
@Override
protected void onRestoreInstanceState(Bundle savedInstanceState) {
    super.onRestoreInstanceState(savedInstanceState);
    String progress = savedInstanceState.getString("progress");
    textView.setText(progress);
}
```

### Intent意图

#### 传参

在startActivity之前，需要在intent对象中放入要传递的参数，使用的方法：

```java
public Intent putExtra(String name, String value)
```

获取参数：

1. 在跳转到的Activity中获取intent对象

```java
public Intent getIntent()
```

2. 从intent对象中根据类型获取参数

```java
public String getStringExtra(String name)
```

#### 返回

有时某些Activity会有返回值，比如登录，不管是登录成功还是失败，在登录结束的时候，都应该结束掉当前Activity，并提供返回值给上一个Activity

1. 在MainActivity中跳转：

```java
Intent intent = new Intent(this, LoginActivity.class);
//注意第二个参数是请求码，代表某一个请求
startActivityForResult(intent,REQ_LOGIN);
```

2. 在LoginActivity中：

```java
//使用无参构造方法创建Intent
Intent intent = new Intent();
//将需要返回的数据放入Intent
intent.putExtra("userName","张三");
//设置状态为成功
setResult(RESULT_OK,intent);
//结束当前Activity
finish();
```

3. 重写MainActivity中`onActivityResult`方法

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
  super.onActivityResult(requestCode, resultCode, data);
  switch (requestCode){
    case REQ_LOGIN://去登陆的返回值
      if(resultCode == RESULT_OK){//正常返回数据
        //从Intent对象中获取数据
        String userName = data.getStringExtra("userName");
        textView.setText(userName);
      }
      break;
  }
}
```

## 练习

### 练习一

在MainActivity中有一个TextView、两个Button。其中一个Button跳转到登录页面，另一个跳转到注册页面。

要求：

	1. 登录和注册两个页面都会有返回值
	2. 返回值可以说明登录操作或注册操作是否成功
	3. 将结果显示到TextView
	4. 重构数据不会丢失

### 练习二

短信助手，

界面：

1. 主界面：2个EditText，一个显示号码，一个显示内容；两个按钮，一个选择号码，一个选择内容
2. 选择号码界面，选择内容界面

需求：

1. 当点击“选择号码”按钮后，跳转到号码选择列表，选择指定号码后自动返回至主界面，并将选择的号码显示在号码输入框中
2. 当点击“选择短信”按钮后，跳转到短信内容列表，选择指定短信内容后自动返回至主界面，并将选择的短信内容显示在内容输入框中
