---
layout: post
title: 无脑学习Service
date: 2014-03-20
categories: blog
tags: [android,service]
description: android service的简单总结。


---
###无脑学习Service

关于Service的概念无需多扯,直接从code开始,首先是一个继承自Service的类:

startService 启动服务
1.新建一个Service,这个Service里提供了一些播放器开始，暂停，停止的一些方法。

    private static final String TAG = "MyService======";  
  
    private MediaPlayer mediaPlayer;  
  
    @Override  
    public IBinder onBind(Intent intent) {  
        return null;  
    }  
  
    @Override  
    public void onCreate() {  
        Log.i(TAG, "onCreate============");  
        Toast.makeText(this, "show media player", Toast.LENGTH_SHORT).show();  
        if (mediaPlayer == null) {  
            mediaPlayer = MediaPlayer.create(this, R.raw.tmp);  
            mediaPlayer.setLooping(false);  
        }  
    }  
  
    @Override  
    public void onDestroy() {  
        Log.i(TAG, "onDestroy==========");  
        Toast.makeText(this, "stop media player", Toast.LENGTH_SHORT).show();  
        if (mediaPlayer != null) {  
            mediaPlayer.stop();  
            mediaPlayer.release();  
        }  
    }  
  
    @Override  
    public void onStart(Intent intent, int startId) {  
        Log.i(TAG, "onStart===========");  
        if (intent != null) {  
            Bundle bundle = intent.getExtras();  
            if (bundle != null) {  
                int op = bundle.getInt("op");  
                switch (op) {  
                case 1:  
                    play();  
                    break;  
                case 2:  
                    stop();  
                    break;  
                case 3:  
                    pause();  
                    break;  
                }  
            }  
        }  
    }  
  
    public void play() {  
        if (!mediaPlayer.isPlaying()) {  
            mediaPlayer.start();  
        }  
    }  
  
    public void pause() {  
        if (mediaPlayer != null && mediaPlayer.isPlaying()) {  
            mediaPlayer.pause();  
        }  
    }  
  
    public void stop() {  
        if (mediaPlayer != null) {  
            mediaPlayer.stop();  
            try {  
                mediaPlayer.prepare(); // 在调用stop后如果需要再次通过start进行播放,需要之前调用prepare函数  
            } catch (IOException ex) {  
                ex.printStackTrace();  
            }  
        }  
    } 

2.如何在我们的Activity里来启动并使用这个Service?新建一个Activity:
     
    



      private Button playBtn;  
    private Button stopBtn;  
    private Button pauseBtn;  
    private Button exitBtn;  
    private Button closeBtn;  
  
    private Intent intent;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.music_service);  
  
        playBtn = (Button) findViewById(R.id.play);  
        stopBtn = (Button) findViewById(R.id.stop);  
        pauseBtn = (Button) findViewById(R.id.pause);  
        exitBtn = (Button) findViewById(R.id.exit);  
        closeBtn = (Button) findViewById(R.id.close);  
  
        playBtn.setOnClickListener(this);  
        stopBtn.setOnClickListener(this);  
        pauseBtn.setOnClickListener(this);  
        exitBtn.setOnClickListener(this);  
        closeBtn.setOnClickListener(this);  
  
    }  
  
    @Override  
    public void onClick(View v) {  
        int op = -1;  
        intent = new Intent("com.hehe.service.musicService");  
        switch (v.getId()) {  
        case R.id.play: // play music  
            op = 1;  
            break;  
        case R.id.stop: // stop music  
            op = 2;  
            break;  
        case R.id.pause: // pause music  
            op = 3;  
            break;  
        case R.id.close: // close activity  
            this.finish();  
            break;  
        case R.id.exit: // stopService  
            op = 4;  
            stopService(intent);  
            this.finish();  
            break;  
        }  
  
        Bundle bundle = new Bundle();  
        bundle.putInt("op", op);  
        intent.putExtras(bundle);  
  
        startService(intent); // startService  
    }  
  
    @Override  
    public void onDestroy() {  
        super.onDestroy();  
  
        if (intent != null) {  
            stopService(intent);   
        }  
    }  
    
**在这个类里可以看到点击按钮后产生一个操作数字,把这个数字放入Bundle后启动Service,这个时候Service如果没有启动,会直接创建(onCreate)并获取到这个操作符来匹配执行对象的方法,如果Service已经启动,则会直接走Service的onStart()方法来匹配执行,Service的onCreate只会执行一次,而onStart会执行多次,根据你的调用情况.**

布局文件:

    <?xml version="1.0" encoding="utf-8"?>  
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="fill_parent"  
    android:layout_height="fill_parent"  
    android:orientation="vertical" >  
  
    <Button  
        android:id="@+id/play"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="@string/play" />  
  
    <Button  
        android:id="@+id/stop"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="@string/stop" />  
  
    <Button  
        android:id="@+id/pause"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="@string/pause" />  
  
    <Button  
        android:id="@+id/close"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="@string/close" />  
  
    <Button  
        android:id="@+id/exit"  
        android:layout_width="fill_parent"  
        android:layout_height="wrap_content"  
        android:text="@string/exit" />  
  
</LinearLayout>  



上面是使用startService的方式来启动Service,下面会介绍如何使用bindService的方式来启动,两者的方式有何区别以及为何使用,请点击：http://blog.csdn.net/nulladdress/article/details/21615329

新建一个Service:

    public class LocalService extends Service {  
      
    private static final String TAG = "LocalService============";  
      
    private IBinder binder = new LocalService.LocalBinder();  
      
    @Override  
    public IBinder onBind(Intent intent) {  
        return binder;  
    }  
      
    MediaPlayer mediaPlayer = null;  
  
    @Override  
    public void onCreate() {  
        Log.i(TAG, "onCreate");   
         //这里可以启动媒体播放器  
         if(mediaPlayer==null)  
             mediaPlayer=MediaPlayer.create(this, R.raw.tmp);  
        super.onCreate();  
    }  
      
    @Override  
    public void onStart(Intent intent, int startId) {  
        Log.i(TAG, "onStart");   
        super.onStart(intent, startId);  
    }  
      
  
    @Override  
    public void onDestroy() {  
        Log.i(TAG, "onDestroy");   
        super.onDestroy();  
    }  
  
    @Override  
    public int onStartCommand(Intent intent, int flags, int startId) {  
        Log.i(TAG, "onStartCommand");   
        return super.onStartCommand(intent, flags, startId);  
    }  
  
  
    //定义内容类继承Binder  
    public class LocalBinder extends Binder{  
        //返回本地服务  
        LocalService getService(){  
            return LocalService.this;  
        }  
    } 
    }  
