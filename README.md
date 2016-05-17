# android_Handler
what‘s  Handler? Howto use it?Let's sum up.<br/>
1.handler是android给我们用来更新UI的一套机制，也是一套消息的处理机制，我们可以通过他来发送消息，也可以用来处理消息<br/>
framework中所有的active中的生命周期的毁掉方法都是通过handler发送消息，根据不同的message做分支处理.
事件传递用处也很广泛.<br/>
2.在ui设计中必须使用handler机制，因为Android在设计过程中，就封装了一套消息创建，传递，处理的机制，如果不尊晕这样的机制是没有办法更新ui信息的，就会抛出异常<br>
例子：<br>
在主布局中新建一个textview，在mainActive.java文件中穿件一个线程来更新textview,textView.settext(),此时会抛出异常<br>
解决方法就是在改变ui的代码里面创建一个handler.post(new Runnable(){run()})在run方法里面改变就不会报错了.<br>
