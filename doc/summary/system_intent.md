#  如何隐士启动activity
,,,
final Intent intent = new Intent();
		 intent.setComponent(new ComponentName("com.android.documentsui", "com.android.documentsui.StartupMenuActivity"));
		 intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_RUN_STARTUP_MENU
		                 | Intent.FLAG_ACTIVITY_CLEAR_TOP);
		mContext.startActivity(intent);
,,,
