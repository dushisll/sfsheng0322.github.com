---
layout: post
author: 孙福生
title: IntentService 示例与详解
cover: "zzz"
categories: Android
tags: Technology
---

IntentService 是比较少使用的，如果你没听过也不意外，就像 HandlerThread 很多开发者没用过或没听过，不过我也仅仅是在demo中使用。Google 为方便开发者使用，提高开发效率，封装了 IntentService 和 HandlerThread。HandlerThread 继承自 Thread，内部封装了 Looper。那 IntentService 呢？

IntentService 是继承自 Service 并处理异步请求的一个类，在 IntentService 内有一个工作线程来处理耗时操作，当任务执行完后，IntentService 会自动停止，不需要我们去手动结束。如果启动 IntentService 多次，那么每一个耗时操作会以工作队列的方式在 IntentService 的 onHandleIntent 回调方法中执行，依次去执行，执行完自动结束。

## 示例代码

#### 实现的功能

示例代码的功能是模仿下载在服务线程中每隔50ms计数一次直到计数为100停止，发送服务和线程运行状态的广播，Fragment中接收广播进行UI更新，如此。

#### MyIntentService 全部代码

    public class MyIntentService extends IntentService {

        private boolean isRunning = true;
        private int count = 0;
        private LocalBroadcastManager mLocalBroadcastManager;

        public MyIntentService() {
            super("MyIntentService");
        }

        @Override
        public void onCreate() {
            super.onCreate();
            mLocalBroadcastManager = LocalBroadcastManager.getInstance(this);
            sendServiceStatus("服务启动");
        }

        @Override
        protected void onHandleIntent(Intent intent) {
            try {
                sendThreadStatus("线程启动", count);
                Thread.sleep(1_000);
                sendServiceStatus("服务运行中...");

                isRunning = true;
                count = 0;
                while (isRunning) {
                    count++;
                    if (count >= 100) {
                        isRunning = false;
                    }
                    Thread.sleep(50);
                    sendThreadStatus("线程运行中...", count);
                }
                sendThreadStatus("线程结束", count);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onDestroy() {
            super.onDestroy();
            sendServiceStatus("服务结束");
        }

        // 发送服务状态信息
        private void sendServiceStatus(String status) {
            Intent intent = new Intent(IntentServiceFragment.ACTION_TYPE_SERVICE);
            intent.putExtra("status", status);
            mLocalBroadcastManager.sendBroadcast(intent);
        }

        // 发送线程状态信息
        private void sendThreadStatus(String status, int progress) {
            Intent intent = new Intent(IntentServiceFragment.ACTION_TYPE_THREAD);
            intent.putExtra("status", status);
            intent.putExtra("progress", progress);
            mLocalBroadcastManager.sendBroadcast(intent);
        }
    }

#### Fragment 中的广播相关代码

    public class MyBroadcastReceiver extends BroadcastReceiver {
        @Override
        public void onReceive(Context context, Intent intent) {
            switch (intent.getAction()) {
                case ACTION_TYPE_SERVICE:
                    tvServiceStatus.setText("服务状态：" + intent.getStringExtra("status"));
                    break;
                case ACTION_TYPE_THREAD:
                    int progress = intent.getIntExtra("progress", 0);
                    tvThreadStatus.setText("线程状态：" + intent.getStringExtra("status"));
                    progressBar.setProgress(progress);
                    tvProgress.setText(progress + "%");
                    break;
            }
        }
    }

#### 注册服务启动服务

    // 在 Manifest 中注册服务
    <service android:name=".service.MyIntentService"/>

    // 像启动 Service 那样启动 IntentService
    Intent startIntent = new Intent(getActivity(), MyIntentService.class);
    getActivity().startService(startIntent);

#### [具体参考GitHub上项目](https://github.com/sfsheng0322/In-depthStudy)

## Gif图演示


1、点击［启动 IntentService］，服务运行，线程计数完成后，服务和线程都结束。

<img src="/assets/gifs/gif_is1.gif" style="width: 50%;"/>

2、点击［启动 IntentService］，服务运行，线程计数完成前，点击［停止 IntentService］，服务结束，线程计数完成后线程结束。

<img src="/assets/gifs/gif_is2.gif" style="width: 50%;"/>

3、点击两次［启动 IntentService］，服务运行，第一次线程计数完成后，进行第二次线程计数，两次完成后，服务和线程都结束。

<img src="/assets/gifs/gif_is3.gif" style="width: 50%;"/>

4、点击两次［启动 IntentService］，服务运行，在第一次线程计数完成前，点击［停止 IntentService］，服务结束，第一次线程计数结束后不进行第二次计数。

<img src="/assets/gifs/gif_is4.gif" style="width: 50%;"/>

## 查看源码

下面，从这几个功能点查看下源码：

#### 1、启动 IntentService 为什么不需要新建线程？

    // IntentService 源码中的 onCreate() 方法
    @Override
    public void onCreate() {
        super.onCreate();
        // HandlerThread 继承自 Thread，内部封装了 Looper，在这里新建线程并启动，所以启动 IntentService 不需要新建线程。
        HandlerThread thread = new HandlerThread("IntentService[" + mName + "]");
        thread.start();

        // 获得工作线程的 Looper，并维护自己的工作队列。
        mServiceLooper = thread.getLooper();
        // mServiceHandler 是属于工作线程的。
        mServiceHandler = new ServiceHandler(mServiceLooper);
    }

    private volatile ServiceHandler mServiceHandler;

    private final class ServiceHandler extends Handler {
        public ServiceHandler(Looper looper) {
            super(looper);
        }

        @Override
        public void handleMessage(Message msg) {
            // onHandleIntent 方法在工作线程中执行，执行完调用 stopSelf() 结束服务。
            onHandleIntent((Intent)msg.obj);
            stopSelf(msg.arg1);
        }
    }

    @WorkerThread
    protected abstract void onHandleIntent(Intent intent);

#### 2、为什么不建议通过 bindService() 启动 IntentService？

    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

IntentService 源码中的 onBind() 默认返回 null；不适合 bindService() 启动服务，如果你执意要 bindService() 来启动 IntentService，可能因为你想通过 Binder 或 Messenger 使得 IntentService 和 Activity 可以通信，这样那么 onHandleIntent() 不会被回调，相当于在你使用 Service 而不是 IntentService。

#### 3、为什么多次启动 IntentService 会顺序执行事件，停止服务后，后续的事件得不到执行？

IntentService 中使用的 Handler、Looper、MessageQueue 机制把消息发送到线程中去执行的，所以多次启动 IntentService 不会重新创建新的服务和新的线程，只是把消息加入消息队列中等待执行，而如果服务停止，会清除消息队列中的消息，后续的事件得不到执行。

    @Override
    public void onStart(Intent intent, int startId) {
        Message msg = mServiceHandler.obtainMessage();
        msg.arg1 = startId;
        msg.obj = intent;
        mServiceHandler.sendMessage(msg);
    }

    @Override
    public void onDestroy() {
        mServiceLooper.quit();
    }

#### [具体参考GitHub上项目](https://github.com/sfsheng0322/In-depthStudy)

