# Handler

使用Handler原因：

Android为了线程安全，并不允许我们在UI线程外操作UI；很多时候做界面刷新都需要通过Handler来通知UI组件更新

##### 作用

就是发送与处理信息,如果希望Handler正常工作,在当前线程中要有一个Looper对象

**Message**

Handler接收与处理的消息对象

**MessageQueue**

消息队列,先进先出管理Message,在初始化Looper对象时会创建一个与之关联的MessageQueue;

**Looper**

每个线程只能够有一个Looper,管理MessageQueue,不断地从中取出Message分发给对应的Handler处理！

##### 执行流程：

当我子线程修改Activity中的UI组件时

新建一个Handler对象

通过这个对象向主线程发送信息;

发送的信息会先到主线程的MessageQueue进行等待

由Looper按先入先出顺序取出

根据message对象的what属性分发给对应的Handler进行处理！

##### 子线程->handler->message->messagequeue->looper->handler处理



### 相关的方法

> - void **handleMessage**(Message msg):处理消息的方法,通常是用于被重写!
> - **sendEmptyMessage**(int what):发送空消息
> - **sendEmptyMessageDelayed**(int what,long delayMillis):指定延时多少毫秒后发送空信息
> - **sendMessage**(Message msg):立即发送信息
> - **sendMessageDelayed**(Message msg):指定延时多少毫秒后发送信息
> - final boolean **hasMessage**(int what):检查消息队列中是否包含what属性为指定值的消息 如果是参数为(int what,Object object):除了判断what属性,还需要判断Object属性是否为指定对象的消息



### Tips:

1.handler需要一个looper对象，主线程中自动创建、子线程中需要自己创建

（Looper.prepare();  

​	Looper.loop();  

）

2.初始化looper对象会自动创建消息队列；

3.



### 主线程使用

指向对象引用需要重写它的方法，里面表示对消息的Handler处理代码，定义处理消息的方法

创建：

Handler myHandler = new Handler(){

	@Override  
	  //重写handleMessage方法 
	  public void handleMessage(Message msg) {  
	  //处理消息
	  }  
}



发送消息

消息的实例.方法（int what）

```
myHandler.sendEmptyMessage(0x123);  
```

what属性标记识别message







### 子线程中使用Handler

**1 )**直接调用Looper.prepare()方法即可为当前线程创建Looper对象,而它的构造器会创建配套的MessageQueue;
**2 )**创建Handler对象,重写handleMessage( )方法就可以处理来自于其他线程的信息了!
**3 )**调用Looper.loop()方法启动Looper



创建：

```
 public Handler mHandler;  
```





```
// 创建消息  
        Message msg = new Message();  
        msg.what = 0x123;  
        Bundle bundle = new Bundle();  
        bundle.putInt(string，int);  
        msg.setData(bundle);  
        // 向新线程中的Handler发送消息  
        Thread.mHandler.sendMessage(msg);  
```

```java
Message message = Message.obtain();
message.what = HANDLER_UI;
message.obj = “string对象”;
```





# Handler消息机制

主要包含两个消息队列，一个是消息的回收队列，另一个是Message Q队列。

#### 消息的回收队列：

消息回收队列是为Handler提供消息对象的，当Handler需要发送消息时，首先从消息回收队列中获取已被清空数据的消息对象，若消息对队列中此时没有消息对象，则创建新的消息对象。当消息对象被使用后，不会直接被当做垃圾回收，而是会进入消息的回收队列，在该队列中会将消息对象上的所有数据清空，之后在队列中等待被使用。

获取消息方法： Message message = Message.obtain();

创建消息方法：Message message1 = new Message()

#### MessageQ队列：

消息队列，用来管理消息，会根据消息的执行时间对消息进行排序。并通过Looper.loop对消息进行轮询取出，当队列中有消息时就将消息取出交给Handler的handlerMessage()回调进行消息的处理，当队列中没有消息时就进行阻塞等待，这种机制又被称作等待唤醒机制。Android中的等待唤醒机制是使用的Linux内核的管道流机制。



### 使用Handler向主线程发送消息

最终都是要操作主线程的UI更新

通过handler向主线程发送消息是Handler使用过程中最常规的一种方式，也是最普遍的一种使用方式。当创建Handler对象时，若使用空参构造创建，则创建出的Handler默认使用UI线程中的Looper，这个时候通过Looper.loop()取出的消息在handlerMessage()中进行处理时，是在UI线程中进行处理的。这也就是为什么大家常说：Handler是运行在主线程中的。



- 1 主线程->主线程 发送消息：在主线程中通过创建好的handler调用sendMessage()方法发送消息即可。
- 2 子线程->主线程 发送消息：在子线程中通过创建好的handler调用sendMessage()方法发送消息即可。



Looper机制

Looper，含有Looper的线程被称为UI线程，UI线程和子线程之间唯一的区别就是一个有Looper另外的一个没有。

直接创建一个含有Looper的子线程，即可实现向该子线程发送消息。

需要两步：

Handler向子线程发送消息：

- 1 在创建的子线程的run()方法中进行Looper相关的配置。
- 2 在创建Hander时的构造方法中传入1线程中的Looper。







主线程向子线程发送消息

handler = newHandler（thread.threadLooper){

//重写处理方法

}

线程异步执行

子线程需要创建looper

不能不正在在接受消息的时候looper会存在，会出现空指针风险

解决方案HandlerThread





### HandlerThread

```java
HandlerThread handlerThread = new HandlerThread();
handlerThread.start();
handlerThreadHandler = new Handler(handlerThread.getLooper()) {
 @Override
 public void handleMessage(Message msg) {
    super.handleMessage(msg);
    switch (msg.what) {
       case HANDLER_THREAD_HANDLER:
       //添加上当前线程名称
       String thrMsg = (String) msg.obj + "\n 收到的线程：" + Thread
       .currentThread().getName();
       runOnUiThread(() -> textView.setText(thrMsg));
       break;
		default:
         break;
    }
}
};
```





内存泄漏风险

对象使用完成将要回收，但是对象的引用还被其他类所持有，导致对象无法被GC回收

Handler使用静态类，内部通过弱引用的方式来持有对象的引用

```java
public static class MyHandler extends Handler {
    	WeakReference<Activity> mWeakReference;
        public MyHandler(Activity activity) {
            mWeakReference = new WeakReference<Activity>(activity);
        }
    	@Override
        public void handleMessage(Message msg) {
            
        }
    
}
```











































