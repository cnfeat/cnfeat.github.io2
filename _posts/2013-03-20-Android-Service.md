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

<pre><code>
package com.hebo.studytest.service;

import java.io.IOException;

import android.app.Service;
import android.content.Intent;
import android.media.MediaPlayer;
import android.os.Bundle;
import android.os.IBinder;
import android.util.Log;
import android.widget.Toast;

import com.hebo.studytest.R;

public class MusicService extends Service {

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

}

</code></pre>


