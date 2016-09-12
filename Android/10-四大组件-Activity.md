# 四大组件-Activity

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
Intent intent = new Intent(this, Main2Activiy.class);
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