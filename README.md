## <font color=#C4573C size=5 face="黑体">前言</font>
在开发中，我们常常需要ViewPager结合Fragment一起使用.我们可以使用三方开源的PagerSlidingTabStrip去实现，或者viewpagerindicator。现在我们可以使用Design support library库的TabLayout去实现了。

TabLayout+ViewPager+Fragment成为了实现如下效果的标配
（效果图来自 暴风体育Android APP）
![这里写图片描述](https://github.com/soulrelay/VerticalViewPagerWithTabLayout/blob/master/sample/src/main/res/raw/screenshot1.gif)


不过这不是重点，重点是我们要实现的下图所示效果：垂直的TabLayout以及垂直的ViewPager，并完成二者的联动：

![这里写图片描述](https://github.com/soulrelay/VerticalViewPagerWithTabLayout/blob/master/sample/src/main/res/raw/screenshot2.gif)


下面这张是我分享的Demo示意图：

![这里写图片描述](https://github.com/soulrelay/VerticalViewPagerWithTabLayout/blob/master/sample/src/main/res/raw/screenshot3.gif)


## <font color=#C4573C size=5 face="黑体">问题</font>
这里不对相关代码做过多说明，使用的Github上造好的轮子，然后根据自己的业务需求做的相关改动，因为时间比较紧，这里聊聊期间碰到的困难
>* 方案调研过程中，第一套方案采用的VerticalViewPager继承自ViewPager，通过将Event的横向和纵向坐标进行交换完成ViewPager的垂直效果，但是会出现滑动冲突，即配合嵌套有RecyclerView或者ListView的Fragment会出现向下滑动不灵敏的问题，指定时间内解决效果不理想，用户体验不好 ，暂时放弃、
>*  第二套解决方案使用的VerticalViewPager继承自ViewGroup，按照作者的说明（Small library allowing you to have a VerticalViewPager. It's just a copy paste from the v19 ViewPager available in the support lib, where I changed all the left/right into top/bottom and X into Y.），同时代码解决了（VerticalViewPager scroll doesnt work when listview is used inside one of the fragment）的问题，结合VerticalTabLayout完美使用
>*  通过VerticalTabLayout的OnTabSelectedListener与VerticalViewPager的OnPageChangeListener完成二者之间的联动，通过VerticalViewPager的PageTransformer完成垂直ViewPager的自定义切换效果
>* VerticalTabLayout和VerticalViewPager通过线性布局水平放置，使用layout_weight进行比例分割，注意layout_width设置为0，否则可能导致右边VerticalViewPager中嵌套的数据显示不居中的问题
>* 期间设置Fragment时碰到一个没有解决的问题（Fragment with ViewPager setCustomAnimations not working），有知识的大大望不吝赐教
>* 还有就是业务逻辑 接口设计方面的问题，我觉得如果有相关竞品，相对成熟的可以作为参考（反编译APK查看布局代码，抓包查看相关接口和请求机制），为我们提供一种思路，然后基于我们自己的需求取其精华弃其糟粕

## <font color=#C4573C size=5 face="黑体">参考链接</font>
[VerticalTabLayout](https://github.com/qstumn/VerticalTabLayout)

[VerticalViewPager](https://github.com/castorflex/VerticalViewPager)

[android design library提供的TabLayout的用法](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0731/3247.html)