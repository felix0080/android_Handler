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
#如何实现自定义线程相关的handler如何实现？
首先创建一个线程，继承Thread,在县曾中实现run方法，创建一个handler对象，在run方法里调用Looper.preper()方法创建一个looper,初始化handler,默认情况下拿到当前线程的looper,拿到之后，处理Handler对象中的Handler的HandleMessage方法.<br>
创建完成后调用Looper.loop()方法循环处理消息.<br>
用法是创建MyThread对象，在onCreate中new出对象，调用start()方法休眠半秒钟之后，调用thread.handler.sendEmptyMessage(1);来处理此消息
#HandlerThread创建时容易遇到的问题
当我们创建Handler对象的时候会传入一个thread.looper对象参数，这个参数是创建在MyThread中的
，有可能发生的是当我们在创建Handler对象的时候looper对象并没有创建好，从而抛出了一个空指针异常，这种多线程并发的解决方法是new出一个HandlerThread对象，通过HandlerThread.getLooper就可以解决这种问题。
# 如何在子线程中向主线程发送消息呢？
在主线程中定义一个handler 在子线程也定义一个handler绑定一个LOOPER对象在里面调用主线程的sendMessageDelayed方法就可以实现向主线程传递参数了
# 如何根更新UI？
创建一个handler,创建一个子线程，在现成中调用handler.post方法，在里面就可以更新线程通过post调用，在内部会return 一个sendMessageDelayed方法，本质上是发送message<br>
runOnUIThread(activity)实质上是通过handler机制更新UI的.<br>
view自己的更新ui的方法，view.post(runnable)实质上也是通过Handler更新UI的
