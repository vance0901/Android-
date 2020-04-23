# Android-Android对回调接口的理解
https://blog.csdn.net/zhuhai0613/article/details/53423326
一直以来对回调接口不是很理解,查了很多资料,现在终于理解了.下面写写我对回调的理解:

    Android中的回调类似于Java中的事件监听机制,分为事件源和事件,只不过是事件源本身来进行事件处理,事件源把某某事件给事件处理,事件处理的结果返回给事件源.

   如何自己写回调,增加程序的可扩展性,松耦合性?

   1.定义接口,里面的方法根据需要写相应的参数

   2.让事件源实现接口

   3.让事件源持有事件的对象

  4.让事件注册接口(写setXXX--一般适用于fragment,Activity,Service或构造函数--一般适用于普通类)
 5.事件源开启注册(调setXXX或构造函数)
   6.让事件源调事件的方法

   7.让事件的方法调接口

   8.事件源中完成处理

   下面写代码说明,老师代表事件源,学生代表事件

package com.callback.test;
 
/**
 * @author zhuhai
 * 1.定义接口,里面的方法根据需要写相应的参数
 *
 */
public interface CallBackInterface {
 
	//下面的方法可以灵活定义
	public void execute();
 
}


package com.callback.test;
/**
 * 
 * @author zhuhai
 * 2.让事件源实现接口---implements CallBackInterface
 *
 */
public class Teacher implements CallBackInterface {
 
	public Teacher(String str) {
		System.out.println(str);
 
	}
 
	public Teacher() {
	}
 
	public static void main(String[] args) {
 
		new Teacher("课堂作业几时写完?").aa();
	}
 
	private void aa() {
		// TODO Auto-generated method stub
 
		/**
		 * 3.让事件源持有事件的对象--这里就是new
         * 4.让事件注册接口(写setXXX--一般适用于fragment,Activity,Service或构造函数--一般适用于普通类)--这里相当于传入this
         * 为方便简写成一起了
		 * 
		 * doSome--6.让事件源调事件的方法
		 */
		new Student(this).doSome();
 
	}
 
	/**
	 * 8.事件源中完成处理
	 */
	@Override
	public void execute() {
		// TODO Auto-generated method stub
		System.out.println("收到了！！" + System.currentTimeMillis());
 
	}
 
}

package com.callback.test;
 
public class Student {
 
	private CallBackInterface callBack = null;
 
	/**
	 * 5.事件源开启注册(调setXXX或构造函数)
	 * @param callBack
	 */
	public Student(CallBackInterface callBack) {
		this.callBack = callBack;
	}
	
	public Student(){
		
	}
	
 
	
	public void doSome() {
		
		for (int i = 0; i < 10; i++) {
			System.out.println("第【" + i + "】作业写完了！");
		}
 
		/**
		 * 7.让事件的方法调接口
		 */
		callBack.execute();
	}
 
}
————————————————
版权声明：本文为CSDN博主「zhuhai0613」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zhuhai0613/article/details/53423326
