### 1. Android Textview显示下划线的两种方法

1.在资源文件里的写法
  
   	<string name="key"><u>content</u></string>`

2.在代码中的写法
	
	TextView textView = (TextView)findViewById(R.id.testView);   
	textView.setText(Html.fromHtml("<u>"+"content"+"</u>"));  