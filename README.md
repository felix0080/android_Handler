# android_Handler
what‘s  Handler? Howto use it?Let's sum up.<br/>
1.handler是android给我们用来更新UI的一套机制，也是一套消息的处理机制，我们可以通过他来发送消息，也可以用来处理消息<br/>
framework中所有的active中的生命周期的毁掉方法都是通过handler发送消息，根据不同的message做分支处理.
事件传递用处也很广泛.<br/>
2.在ui设计中必须使用handler机制，因为Android在设计过程中，就封装了一套消息创建，传递，处理的机制，如果不尊晕这样的机制是没有办法更新ui信息的，就会抛出异常<br>
例子：<br>
在主布局中新建一个textview，在mainActive.java文件中穿件一个线程来更新textview,textView.settext(),此时会抛出异常<br>
解决方法就是在改变ui的代码里面创建一个handler.post(new Runnable(){run()})在run方法里面改变就不会报错了.<br>
# 如何实现图片的每隔一段时间换一张呢？
我们首先要定义一个imageview 然后定义一个数组，将数组中添加三张图片，然后我们声明一个runnable方法集成Runnable,在run方法中通过循环来调用setimageResource方法再调用handler.postDelayed方法每隔一秒钟执行myRunnable一次然后再在onCreate方法中调用一下postDelayed方法就可以实现了<br/>
# 如何实现消息的发送和接受处理呢？
当定义handler的时候 实现其中的handleMessage方法，在oncreate方法中还是通过穿件一个线程再创建一个message对象（handler.obtainMessage();） 通过handler.sendMessage（message.sendToTarget();）方法传出然后再handleMessage方法接受并使用
# 如何实现终止handler?
定义一个按钮，在onclick方法里面写handler.removeCallbacks(myRunnable);将他的毁掉终止即可.
# 如何实现截获消息?
 在定义handler的时候，new Callback()函数，然后实现handleMessage方法，如果返回值为true，则截获。
