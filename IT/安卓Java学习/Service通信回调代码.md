package com.example.communication;

import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;

public class MsgService extends Service {
	/**
	 * 进度条的最大值
	 */
	public static final int MAX_PROGRESS = 100;
	/**
	 * 进度条的进度值
	 */
	private int progress = 0;
	

	/**
	 * 更新进度的回调接口
	 */
	private OnProgressListener onProgressListener;


​	
	/**
	 * 注册回调接口的方法，供外部调用
	 * @param onProgressListener
	 */
	public void setOnProgressListener(OnProgressListener onProgressListener) {
		this.onProgressListener = onProgressListener;
	}
	 
	/**
	 * 增加get()方法，供Activity调用
	 * @return 下载进度
	 */
	public int getProgress() {
		return progress;
	}
	 
	/**
	 * 模拟下载任务，每秒钟更新一次
	 */
	public void startDownLoad(){
		new Thread(new Runnable() {
			
			@Override
			public void run() {
				while(progress < MAX_PROGRESS){
					progress += 5;
					
					//进度发生变化通知调用方
					if(onProgressListener != null){
						onProgressListener.onProgress(progress);
					}
					
					try {
						Thread.sleep(1000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					
				}
			}
		}).start();
	}

 

	/**
	 * 返回一个Binder对象
	 */
	@Override
	public IBinder onBind(Intent intent) {
		return new MsgBinder();
	}
	
	public class MsgBinder extends Binder{
		/**
		 * 获取当前Service的实例
		 * @return
		 */
		public MsgService getService(){
			return MsgService.this;
		}
	}

}





活动中代码

public class MainActivity extends Activity {
	private MsgService msgService;
	private ProgressBar mProgressBar;
	

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);


​		
		//绑定Service
		Intent intent = new Intent("com.example.communication.MSG_ACTION");
		bindService(intent, conn, Context.BIND_AUTO_CREATE);


​		
		mProgressBar = (ProgressBar) findViewById(R.id.progressBar1);
		Button mButton = (Button) findViewById(R.id.button1);
		mButton.setOnClickListener(new OnClickListener() {
			
			@Override
			public void onClick(View v) {
				//开始下载
				msgService.startDownLoad();
			}
		});
		
	}

 

	ServiceConnection conn = new ServiceConnection() {
		@Override
		public void onServiceDisconnected(ComponentName name) {
			
		}
		
		@Override
		public void onServiceConnected(ComponentName name, IBinder service) {
			//返回一个MsgService对象
			msgService = ((MsgService.MsgBinder)service).getService();
			
			//注册回调接口来接收下载进度的变化
			msgService.setOnProgressListener(new OnProgressListener() {
				
				@Override
				public void onProgress(int progress) {
					mProgressBar.setProgress(progress);
					
				}
			});
			
		}
	};
	 
	@Override
	protected void onDestroy() {
		unbindService(conn);
		super.onDestroy();
	}

实现了回调接口

实时监测服务中动态变化