---
layout:     post
title:      "避免重复造轮子ExpandTabView"
subtitle:   " \"教程 \""
date:       2017-11-10 16:01:10
author:     "Smm"
header-img: "img/004.jpg"
tags:
    - 教程
---



## ExpandTabView 是什么？

最新接触这样的view 是在美团下拉菜单ExpandTabView，一搜索到处都是这样的。
其他他就是 这样下来出现的情况
![](http://img.blog.csdn.net/20140922235621019?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTWluaU1pY2FsbA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

从中知道这样做起来真的很费劲，每次点击效果，已经列表显示，点击是否消失等诸多问题，已知困扰着我继续看了很多这个方法的blog 和实现的方式 最终可以理解为设计图是这样的
![](http://img.blog.csdn.net/20140923000721976?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTWluaU1pY2FsbA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


大多数都是上门都是toggerbuttom ，因为他有会 图片的点击效果的确定和取消 等现象，但是也避免了一部分，那就是上面的样式 都是确定的。才能统一进行。

所有这个部分就可以单独拿出来，作为一个自定义控件来处理了。

	<LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="44dp"
	        android:background="@drawable/layout_line_bottom"
	        android:gravity="center|center_vertical"
	        android:orientation="horizontal">
	
	        <com.xxx.view.CustomerSearchBar
	            android:id="@+id/customers_bar"
	            android:layout_width="match_parent"
	            android:layout_height="match_parent">
	
	        </com.xxx.view.CustomerSearchBar>
	
	    </LinearLayout>

这样界面就没有太多的出入了

接着就是往自定义控件里面添加数据就可以了。这样就是ExpandTabView 所带来简单好用的方式了。
其实就是自定义了view ，view中添加了toggerbuttom + popwindow 等 基本控件
和drawable 对背景颜色和图片的处理。
开始做吧

## 我接触 view

项目中ui或者产品经理对这个特别有要求，所有这个是比不可以少的。
我做的总归一下几种吧
统一样式 的
![](https://i.imgur.com/XQKYmQZ.jpg)

![](https://i.imgur.com/tnERvQw.png)
![](https://i.imgur.com/1R7nCPv.png)
 ![](https://i.imgur.com/HLIhlyT.jpg)![](https://i.imgur.com/o4r4ixF.jpg)
![](https://i.imgur.com/TihQ2m6.jpg)

非统一样式的
![](https://i.imgur.com/4kSw9DS.png)
![](https://i.imgur.com/PxHocCG.png)

基本上就分为这两类。

统一样式的，。上面可以从网络上获取，更加json出去处理数据。

非统一的 一般都是固定的死的。

对于这个两种 我也恨死产品了，不过还是对待技术和样式还是可以理解的。

都是对这个linearyout 进行封装。


## 实现步骤

1，更加需求选择 不同的样式和 思路去写布局。
2，新建集成 LinearLayout或RelativeLayout 为了是添加 不同的试图
3，添加方法

在点击某个 头部view 的时候进行不同 view 添加
 
① 种是 view 显示和隐藏  ，主要对 layout 的底部view 进行 半透明黑色处理，进行不同效果显示。
		
        /*
         * 顶部筛选条
         */
        fixedTabIndicator = new FixedTabIndicator(getContext());
        fixedTabIndicator.setId(R.id.fixedTabIndicator);
        addView(fixedTabIndicator, -1, UIUtil.dp(getContext(), 45));

        LayoutParams params = new LayoutParams(LayoutParams.MATCH_PARENT,LayoutParams.MATCH_PARENT);
        params.addRule(BELOW, R.id.fixedTabIndicator);

        /*
         * 2.添加contentView,内容界面
         */
        addView(contentView, params);

        /*
         * 3.添加展开页面,装载筛选器list
         */
        frameLayoutContainer = new FrameLayout(getContext());
        frameLayoutContainer.setBackgroundColor(getResources().getColor(R.color.black_p50));
        addView(frameLayoutContainer, params);

        frameLayoutContainer.setVisibility(GONE);

② popwindow 来添加view

	
		
 		RelativeLayout popContainerView = new RelativeLayout(mContext);

        popContainerView.setBackgroundColor(Color.parseColor("#b0000000"));

        RelativeLayout.LayoutParams rl = new RelativeLayout.LayoutParams(RelativeLayout.LayoutParams.MATCH_PARENT, (int) (mDisplayHeight * 0.4));

        popOneListView = new PopOneListView(mContext);

        if (popOneListView != null) {
            popContainerView.addView(popOneListView, rl);
            popContainerView.setOnClickListener(new OnClickListener() {
                @Override
                public void onClick(View v) {
                    customersBarPaixunImg.setBackgroundResource(R.mipmap.customer_arrow_down);
	//                    onExpandPopView();
                }
            });

        }
        mViewLists.add(popContainerView);

		if (mPopupWindow == null) {
            mPopupWindow = new PopupWindow(mViewLists.get(mSelectPosition), mDisplayWidth, mDisplayHeight);
	//            mPopupWindow.setAnimationStyle(R.style.PopupWindowAnimation);
            mPopupWindow.setFocusable(false);
            mPopupWindow.setOutsideTouchable(true);
        }
        mPopupWindow.setOnDismissListener(this);
        mPopupWindow.dismiss();
        showPopView(mSelectPosition);
		mPopupWindow.setContentView(mViewLists.get(mSelectPosition));
        mPopupWindow.showAsDropDown(this, 0, 0);

目的都是为了一种，显示的时候看到是白色的，底部列表里面 是半透明 黑色。

4，进行view 试图的创建，然后显示就行了。 从上面看了都是添加的不同的view ，
单种列好做，和普通列表一样，从数据源里面获取数据，然后显示处理就行。
两列的点击一列，显示另一类数据，对多个列表处理，点击一个列表获取不同的列表数据，
然后进行不同另一类处理。
5，切换效果和点击事件处理以及对外接口使用。

## 优化方式

1. 统一方式

①顶部的view 是已知的，所有直接对外有修改每个试图字就行了。进行公共adapter 的处理，
每个顶部对应不同的view ，但是这个相对是统一的处理直接给出 data数据进行。



2. 自定义方式

![](https://i.imgur.com/4kSw9DS.png)
这样的试图 仔细看吧，view 不一样，所以我很难统一，只能先吧头部view 和设计图一致。

然后用pupview出处理view

我还是比较愿意写这样的。因为我不用过多的处理点击其他地方是否隐藏的问题。

统一地方显示
统一地方隐藏
对外接口 调用 。
 
大体思路就是这样的。图片多了，代码少了。不过还是很辛苦的写完了。

## 致谢

[二级菜单ExpandPopView的使用和实现](http://blog.csdn.net/tom_xiaoxie/article/details/50390506)

[【ExpandTabView】Android 仿大众，美团下拉菜单ExpandTabView](http://blog.csdn.net/dodod2012/article/details/52117854)

github 

[DropDownMenu](https://github.com/baiiu/DropDownMenu)



 


