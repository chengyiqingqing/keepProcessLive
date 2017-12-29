# KeepProcLive
Android进程保活主流方案集锦

这里面集成的方案包括：

1.  Service指定为START_STICKY 被系统回收的进程会被系统重新拉起

2.  Service设置为前台进程 将后台进程设置为前台进程，提高进程优先级

3.  1像素Activity方案 关屏后加载1个像素的Activity到Window，提高锁屏 后的进程优先级

4.  静态广播自启 利用监听开机启动广播、网络变化广播、应用安装删 除等广播，接收到广播后实现自启

5.  JobSchedule (5.0以上)和AlarmManager 利用Android的API某些机制去实现自启

6.   账号同步拉活 利用Android自身的账号同步机制周期拉活

7.   守护进程 :  这块为了解决5.0以上机器拉活的问题，采用了文件锁监听进程死亡的机制，
    具体参考：http://blog.csdn.net/marswin89/article/details/50916631
    
### Activity的销毁时处理和模拟销毁
    Activity的销毁一般分为两种情况：

    当用户按返回按钮或你的Activity通过调用finish()销毁时，这属于正常销毁，此时是不需要恢复状态的，因为下次回来又是重新创建新的实例。
    如果Activity当前被停止或长期未使用，或者前台Activity需要更多资源以致系统必须关闭后台进程恢复内存，系统也可能会销毁Activity，这属于非正常销毁，尽管Activity实例被销毁，但系统会保存其状态，这样，如果用户导航回该Activity，系统会使用保存了该Activity被销毁时的状态数据来创建Activity的新实例。
    屏幕旋转、键盘可用性改变、 语言改变都可以归结为第二种情况；值得一提的是，如果需要模拟这种情况的Activity销毁，可以打开开发者选项，选择不保留活动（英文为Do not keep activities），即可模拟内存不足时的系统行为

