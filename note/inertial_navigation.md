## 惰性导航

作者：张斯托洛夫斯基
链接：https://www.zhihu.com/question/27847666/answer/47514863
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



导航解决的其实就是从哪儿来到哪儿去的问题。对此我们总是能想到指南针。
但是有一个经典的笑话，说一个人带着指南针迷路了：“我知道北在哪儿，可是我在哪儿啊？”
所以要完成导航，需要知道我在哪儿，还有北在哪儿，如果有目的地的话，还得知道目的地在哪儿，从而告诉用户，通往目的地的道路。其中，【我在哪儿】是非常重要的。

地上铺了方砖，你知道自己一开始在哪块砖上，然后向左三步，往前五步，向左转，再往后退四步，向后转，再往左走两步，等等，每一步都是一块砖的长度。
把这些告诉一个没在房间里的人，他在纸上画画，不看你也知道你现在应该在哪块砖上，朝向哪里。



惯性导航和一些其它导航方法的基本原理差不多就是这样。

你知道自己的初始位置，知道自己的初始朝向（姿态），知道自己每一时刻如何改变了朝向，知道自己每一时刻相对朝向是怎样走的，把这些加一起不停地推，走一步推一步，在不考虑各种误差时，得出的结果就应该正好是你现在的朝向和位置。

但是要怎么知道自己的方向和位置是怎么改变的呢？不同的导航系统用不同的传感器，有不同的方法，比如里程计用车辆上轮子转的周数，多普勒计程仪像蝙蝠一样往水底发射声波……而惯性导航之所以叫【惯性】导航，就是因为使用的是【惯性器件】，也就是加速度计和陀螺仪。

加速度计测量加速度，利用的原理是 a=F/M，测量物体的“惯性力”。
陀螺仪测量角速度，这是一个我个人觉得非常有意思的器件，我第一次意识到其原理的时候觉得好神奇。
如果把一个陀螺立在桌上，轻轻一推它的轴的上部，它会倒下；但如果把陀螺转起来以后再立在桌上，再这样推一下，它就会摇摇晃晃地竖着向前走去，好像有什么力量阻止陀螺倒下去一样。
同样的原理也能解释为什么自行车一旦骑起来就不像慢速前进或者原地站着那样容易倒下。

关于陀螺仪的原理，可以看神十太空授课的视频：

[神十 太空授课：陀螺晃动向前走 视频](https://link.zhihu.com/?target=http%3A//my.tv.sohu.com/us/63260461/58156050.shtml)

这样我们就有了基础的陀螺仪和加速度计，也知道了初始位置，我们可以放心的拿过来它们的数据然后积分再积分推位获取位置了吧？
但是等下，惯性器件为什么叫惯性器件呢，就是因为它输出的是相对惯性空间的数据，在地球上，可以大概认为它输出的是相对宇宙的数据。
这是个什么概念呢？——别忘了，地球是圆的，而且还是在自转的！

我们导航的时候，需要的是相对东向、北向、天向的数据。
这很好理解，如果不这样做而是直接使用相对宇宙的数据，看导航输出，你站在这里不动，十二小时以后导航仪告诉你，你现在大头朝“下”（其实依照你站的纬度不同，还不一定是大头朝下），会让使用者感觉混乱。
而位移上，相对宇宙的位移数据会忠实体现出地球的自转，那真是坐地日行八万里。而你想知道的只是你往东走了多少又往北走了多少目前北在哪里下在哪里接下来该怎么走而已。
所以我们需要把惯性系的数据转化成导航系（一般是地理系也就是东北天）数据，也就是要减去地球自转，和你在地球上经纬度变化所带来的角度变化。这个过程，在平台式惯导中是由一个始终跟踪所在位置东北天的物理平台实现的，在捷联式惯导中是由一系列公式和推算实现的。

不管是物理平台还是数学平台，当你拥有了这个平台之后，就可以先确定初始位置速度和姿态，然后将惯性器件输出积分再积分一步步加上去，获取载体的位置速度和姿态信息了。当然如果实际这样做，会面对很多新问题，需要一一加以解决。

以上是我对惯性导航原理的大概总结，我自己也在学习中，答案中很可能有错误或不足之处，欢迎各位同学或者老师指出来。