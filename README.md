package com.wanata.lock.util;

import android.os.Handler;
import android.os.Looper;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ManagerThread extends Thread {
    public  static ManagerThread managerThread;
    private Handler mHandler;
    private ExecutorService mExecutorService;
    private ManagerThread () {
        init();
    }
    private void init() {
        if (mExecutorService == null || mExecutorService.isShutdown() || mExecutorService.isTerminated()) {
            mExecutorService = Executors.newSingleThreadExecutor();
        }
        mHandler = new Handler(Looper.myLooper());
    }
    public static ManagerThread getInstance() {
        if (managerThread == null) {
            managerThread = new ManagerThread();
        }
        return managerThread;
    }
    public void runOnUIThread(Runnable runnable) {
        mHandler.post(runnable);
    }
    public void runOnOtherThread (Runnable runnable) {
        mExecutorService.submit(runnable);
    }

}
