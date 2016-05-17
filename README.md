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
# android为什么非要通过Handler机制来更新UI呢？
 其实在Handler机制中更新UI是为了解决多线程的并发问题，假设在一个Activity中，有多个线程去跟新UI，并且都没有加锁，则会导致更新界面混乱的问题，如果对跟新UI的操作都进行加锁处理的话又会导致性能的下降，所以出于以上两点的考虑，我们只用遵循android给我们提供的Handler机制就可以了
# Handler和Looper的区别是什么？
handler封装了消息的发送（主要包括消息发送给谁）<br>
looper内部包含了一个消息队列也就是MessageQueue,所有的handler发送的消息都走向这个消息队列<br>
looper.looper方法，就是一个死循环，不断地从MessageQueue取出消息，如果有消息就处理，没有就阻塞<br>
Handler内部与Looper进行关联，也就是在Handler的内部可以找到looper,找到了Looper也就是找到了MessageQueue,在Handler中发送消息，其实是想MwssageQueue队列中发送消息
总的来说:handler负责发送消息，looper负责接收消息，并直接把消息传递给handler自己,MessageQueue就是一个存储消息的容器


