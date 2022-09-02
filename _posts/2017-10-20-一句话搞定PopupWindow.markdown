---
layout:     post
title:      "一句话搞定PopupWindow"
subtitle:   " \"app \""
date:       2017-10-20 16:54:10
author:     "Smm"
header-img: "img/pop/bg.jpg"
tags:
    - app
---


## 前言

设计图给出的效果
![](https://i.imgur.com/WbxvfsN.png)

最终的效果
 ![](https://i.imgur.com/SklKd0d.jpg)

最终代码
common_tv_right_more 的点击事件

		if (view.getId() == R.id.common_tv_right_more) { 
            new PopTop.Builder(this)
                    .setView(findViewById(R.id.common_tv_right_more)) // 在某个空间的下面 
                    .setPopTopOnClick(new PopTop.PopTopOnClick() {
                        @Override
                        public void EditOnclick() {
                            /* 编辑 监听*/ 
                        } 
                        @Override
                        public void DelOnclick() {
                            /*删除 监听*/
                        } 
                    })
                    .show();
        }

接着开始想和学习怎么做了。

##使用场景
PopupWindow，顾名思义，就是弹窗，在很多场景下都可以见到它。例如ActionBar/Toolbar的选项弹窗，一组选项的容器，或者列表等集合的窗口等等。

## 基本使用
使用PopupWindow很简单，可以总结为三个步骤：

1、创建PopupWindow对象实例；

2、设置背景、注册事件监听器和添加动画；

3、显示PopupWindow。

	// 用于PopupWindow的View
    View contentView=LayoutInflater.from(context).inflate(layoutRes, null, false);
    // 创建PopupWindow对象，其中：
    // 第一个参数是用于PopupWindow中的View，第二个参数是PopupWindow的宽度，
    // 第三个参数是PopupWindow的高度，第四个参数指定PopupWindow能否获得焦点
    PopupWindow window=new PopupWindow(contentView, 100, 100, true);
    // 设置PopupWindow的背景
    window.setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
    // 设置PopupWindow是否能响应外部点击事件
    window.setOutsideTouchable(true);
    // 设置PopupWindow是否能响应点击事件
    window.setTouchable(true);
    // 显示PopupWindow，其中：
    // 第一个参数是PopupWindow的锚点，第二和第三个参数分别是PopupWindow相对锚点的x、y偏移
    window.showAsDropDown(anchor, xoff, yoff);
    // 或者也可以调用此方法显示PopupWindow，其中：
    // 第一个参数是PopupWindow的父View，第二个参数是PopupWindow相对父View的位置，
    // 第三和第四个参数分别是PopupWindow相对父View的x、y偏移
    // window.showAtLocation(parent, gravity, x, y);


## 使用showAsDropDown方法显示PopupWindow

通常情况下，调用showAsDropDown方法后PopupWindow将会在锚点的左下方显示（drop down）。但是，有时想让PopupWindow在锚点的上方显示，或者在锚点的中间位置显示，此时就需要用到showAsDropDown方法的xoff和yoff参数了。

这里我们的目的不仅包括上面提到的两种情况（锚点上方或锚点中部），而是囊括了水平和垂直方向各5种显示方式：




1. 水平方向：
	- ALIGN_LEFT：在锚点内部的左边；
	- ALIGN_RIGHT：在锚点内部的右边；
	- CENTER_HORI：在锚点水平中部；
	- TO_RIGHT：在锚点外部的右边；
	- TO_LEFT：在锚点外部的左边。


2. 垂直方向：
	- ALIGN_ABOVE：在锚点内部的上方；
	- ALIGN_BOTTOM：在锚点内部的下方；
	- CENTER_VERT：在锚点垂直中部；
	- TO_BOTTOM：在锚点外部的下方；
	- TO_ABOVE：在锚点外部的上方。



下面来看张图：  
![](http://images2015.cnblogs.com/blog/830999/201706/830999-20170617100745681-1495426999.png)
![](http://images2015.cnblogs.com/blog/830999/201706/830999-20170617100758259-937613284.png)

## showAsDropDown 可以做哪些效果
![](http://img.blog.csdn.net/20160517135203549)

![](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1625771311,2029667217&fm=27&gp=0.jpg)

![](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=1973224084,1527358680&fm=27&gp=0.jpg)

![](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3762434249,459927602&fm=27&gp=0.jpg)

![](http://images.cnitblog.com/blog/359646/201502/131212186043095.gif)

![](http://images2015.cnblogs.com/blog/1068713/201612/1068713-20161208162228710-179900096.png)

![](http://images2015.cnblogs.com/blog/1068713/201612/1068713-20161208162129882-857957949.png)

想要的效果 是在某个控件的下面 并且还要有偏移 角度，所以在偏移角度上
大体是多少再去调试。
所以在主要的方法落在了showAsDropDown 的下面。 想要写成通用的也不是不可以的，先不要想这么多，先去做，实现目前的效果再说。


由上面的基本方法中我可以看到
View contentView=LayoutInflater.from(context).inflate(layoutRes, null, false);

这个view 的产出，所以就可以把它单独拿起来，在单独页面中和xml 处理，底色背景和角度了，
以及整个view 中的点击事件处理，并且显得不那么啰嗦了，看简单的代码，就用到build设计模式，
突出了这个模式的优点，不断添加 各个设置条件。


[build设计模式](http://shimengmeng.win/2017/03/16/Android-Builder-%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F/ "build设计模式")

自己之前也写过关于设计模式的问题。

首先看是build 的时候创建view 或者说产出view 为了提供显示用

	public PopTop(Builder builder) {
        //窗口布局
        super(builder.context);
        this.builder = builder;
        Create();
    }

    public void Create() {
        //窗口布局
        setContentView(mainView = LayoutInflater.from(builder.context).inflate(R.layout.pop_top, null));
        //设置宽度
        setWidth(dip2px(builder.context, 100));
        //设置高度
        setHeight(LinearLayout.LayoutParams.WRAP_CONTENT);
        setTouchable(true);
        setFocusable(true);
        //设置显示隐藏动画
		//        setAnimationStyle(R.style.AnimTools);
        //设置背景透明
        setBackgroundDrawable(new ColorDrawable());
        /*          //监听窗口的焦点事件，点击窗口外面则取消显示*/
        getContentView().setOnFocusChangeListener(new View.OnFocusChangeListener() {

            @Override
            public void onFocusChange(View v, boolean hasFocus) {
                if (!hasFocus) {
                    dismiss();
                }
            }
        });
		//初始化 控件点击事件处理
        initView(mainView);
    }


再看build 做了哪些处理

	public static class Builder {
        protected final Context context;
        protected View view; 

        PopTopOnClick popTopOnClick;

        public Builder setPopTopOnClick(PopTopOnClick popTopOnClick) {
            this.popTopOnClick = popTopOnClick;
            return this;
        } 

        public Builder setView(View view) {
            this.view = view;
            return this;
        }

        public Builder(@NonNull Context context) {
            this.context = context;
        }

        @UiThread
        public PopupWindow build() {
            return new PopTop(this);
        }

        @UiThread
        public PopupWindow show() {
            PopupWindow popTop = build();
            int windowPos[] = calculatePopWindowPos(view);
            windowPos[0] -= 15; //x 轴向左偏移15像素
            windowPos[1] -= 10; //y 轴向上偏移10像素
            popTop.showAtLocation(view, Gravity.TOP | Gravity.START, windowPos[0], windowPos[1]);
            return popTop;
        }
    }

主要在show 中

对偏移量的处理，并且在合适的位置处理，在上面基本中说道。


	/**
     * 计算出来的位置，y方向就在anchorView的上面和下面对齐显示，x方向就是与屏幕右边对齐显示
     * 如果anchorView的位置有变化，就可以适当自己额外加入偏移来修正
     *
     * @param anchorView 呼出window的view
     * @return window显示的左上角的xOff, yOff坐标
     */
    public static int[] calculatePopWindowPos(final View anchorView) {

        final int windowPos[] = new int[2];
        final int anchorLoc[] = new int[2];
        // 获取锚点View在屏幕上的左上角坐标位置
        anchorView.getLocationOnScreen(anchorLoc);
        final int anchorHeight = anchorView.getHeight();
        // 获取屏幕的高宽
        final int screenHeight = getScreenHeight(anchorView.getContext());
        final int screenWidth = getScreenWidth(anchorView.getContext());
        mainView.measure(View.MeasureSpec.UNSPECIFIED, View.MeasureSpec.UNSPECIFIED);
        // 计算contentView的高宽
        final int windowHeight = mainView.getMeasuredHeight();
        final int windowWidth = mainView.getMeasuredWidth();
        // 判断需要向上弹出还是向下弹出显示
        final boolean isNeedShowUp = (screenHeight - anchorLoc[1] - anchorHeight < windowHeight);
        if (isNeedShowUp) {
            windowPos[0] = screenWidth - windowWidth;
            windowPos[1] = anchorLoc[1] - windowHeight;
        } else {
            windowPos[0] = screenWidth - windowWidth;
            windowPos[1] = anchorLoc[1] + anchorHeight;
        }
        return windowPos;
    }

大体流程就是这些了，至于点击事件，进行事件监听，也是从产生的pop的时候传递过来的。

所有代码 

	

	import android.content.Context;
	import android.graphics.drawable.ColorDrawable;
	import android.support.annotation.NonNull;
	import android.support.annotation.UiThread;
	import android.text.TextUtils;
	import android.view.Gravity;
	import android.view.LayoutInflater;
	import android.view.View;
	import android.widget.LinearLayout;
	import android.widget.PopupWindow;
	import android.widget.TextView;

 

	public class PopTop extends PopupWindow implements View.OnClickListener {

	    public static View mainView;
	    protected final Builder builder;
	    protected TextView popTvCustomerEdit;
	    protected TextView popTvCustomerDel;
	    protected LinearLayout popLlEdit;
	    protected TextView popTvCustomerSys;
	    protected TextView popTvCustomerCreate;
	    protected LinearLayout popLlCustomer;
	
	    public PopTop(Builder builder) {
	        //窗口布局
	        super(builder.context);
	        this.builder = builder;
	        Create();
	    }
	
	    public void Create() {
	        //窗口布局
	        setContentView(mainView = LayoutInflater.from(builder.context).inflate(R.layout.pop_top, null));
	        //设置宽度
	        setWidth(dip2px(builder.context, 100));
	        //设置高度
	        setHeight(LinearLayout.LayoutParams.WRAP_CONTENT);
	        setTouchable(true);
	        setFocusable(true);
	        //设置显示隐藏动画
	//        setAnimationStyle(R.style.AnimTools);
	        //设置背景透明
	        setBackgroundDrawable(new ColorDrawable());
	        /*          //监听窗口的焦点事件，点击窗口外面则取消显示*/
	        getContentView().setOnFocusChangeListener(new View.OnFocusChangeListener() {
	
	            @Override
	            public void onFocusChange(View v, boolean hasFocus) {
	                if (!hasFocus) {
	                    dismiss();
	                }
	            }
	        });
	        initView(mainView);
	    }
	
	    @Override
	    public void onClick(View view) {
	        dismiss();
	        if (view.getId() == R.id.pop_tv_customer_edit) {
	            if(builder.popTopOnClick != null){
	                builder.popTopOnClick.EditOnclick();
	            }
	        } else if (view.getId() == R.id.pop_tv_customer_del) {
	            if(builder.popTopOnClick != null){
	                builder.popTopOnClick.hashCode();
	            }
	        } else if (view.getId() == R.id.pop_tv_customer_sys) {
	            if(builder.popTopOnClick != null){
	                builder.popTopOnClick.SysOnclick();
	            }
	        } else if (view.getId() == R.id.pop_tv_customer_create) {
	            if(builder.popTopOnClick != null){
	                builder.popTopOnClick.CreateOnclick();
	            }
	        }
	    }
	
	    private void initView(View rootView) {
	        popTvCustomerEdit = (TextView) rootView.findViewById(R.id.pop_tv_customer_edit);
	        popTvCustomerEdit.setOnClickListener(PopTop.this);
	        popTvCustomerDel = (TextView) rootView.findViewById(R.id.pop_tv_customer_del);
	        popTvCustomerDel.setOnClickListener(PopTop.this);
	        popLlEdit = (LinearLayout) rootView.findViewById(R.id.pop_ll_edit);
	        popTvCustomerSys = (TextView) rootView.findViewById(R.id.pop_tv_customer_sys);
	        popTvCustomerSys.setOnClickListener(PopTop.this);
	        popTvCustomerCreate = (TextView) rootView.findViewById(R.id.pop_tv_customer_create);
	        popTvCustomerCreate.setOnClickListener(PopTop.this);
	        popLlCustomer = (LinearLayout) rootView.findViewById(R.id.pop_ll_customer);
	
	        popLlEdit.setVisibility(View.GONE);
	        popLlCustomer.setVisibility(View.GONE);
	
	        
	
	    }
	
	    public static class Builder {
	        protected final Context context;
	        protected View view; 
	        PopTopOnClick popTopOnClick;
	
	        public Builder setPopTopOnClick(PopTopOnClick popTopOnClick) {
	            this.popTopOnClick = popTopOnClick;
	            return this;
	        }
	
	        
	        public Builder setView(View view) {
	            this.view = view;
	            return this;
	        }
	
	        public Builder(@NonNull Context context) {
	            this.context = context;
	        }
	
	        @UiThread
	        public PopupWindow build() {
	            return new PopTop(this);
	        }
	
	        @UiThread
	        public PopupWindow show() {
	            PopupWindow popTop = build();
	            int windowPos[] = calculatePopWindowPos(view);
	            windowPos[0] -= 15; //x 轴向左偏移15像素
	            windowPos[1] -= 10; //y 轴向上偏移10像素
	            popTop.showAtLocation(view, Gravity.TOP | Gravity.START, windowPos[0], windowPos[1]);
	            return popTop;
	        }
	    }
	
	    public int dip2px(Context context, float dipValue) {
	        final float scale = context.getResources().getDisplayMetrics().density;
	        return (int) (dipValue * scale + 0.5f);
	    }
	
	    /**
	     * 计算出来的位置，y方向就在anchorView的上面和下面对齐显示，x方向就是与屏幕右边对齐显示
	     * 如果anchorView的位置有变化，就可以适当自己额外加入偏移来修正
	     *
	     * @param anchorView 呼出window的view
	     * @return window显示的左上角的xOff, yOff坐标
	     */
	    public static int[] calculatePopWindowPos(final View anchorView) {
	
	        final int windowPos[] = new int[2];
	        final int anchorLoc[] = new int[2];
	        // 获取锚点View在屏幕上的左上角坐标位置
	        anchorView.getLocationOnScreen(anchorLoc);
	        final int anchorHeight = anchorView.getHeight();
	        // 获取屏幕的高宽
	        final int screenHeight = getScreenHeight(anchorView.getContext());
	        final int screenWidth = getScreenWidth(anchorView.getContext());
	        mainView.measure(View.MeasureSpec.UNSPECIFIED, View.MeasureSpec.UNSPECIFIED);
	        // 计算contentView的高宽
	        final int windowHeight = mainView.getMeasuredHeight();
	        final int windowWidth = mainView.getMeasuredWidth();
	        // 判断需要向上弹出还是向下弹出显示
	        final boolean isNeedShowUp = (screenHeight - anchorLoc[1] - anchorHeight < windowHeight);
	        if (isNeedShowUp) {
	            windowPos[0] = screenWidth - windowWidth;
	            windowPos[1] = anchorLoc[1] - windowHeight;
	        } else {
	            windowPos[0] = screenWidth - windowWidth;
	            windowPos[1] = anchorLoc[1] + anchorHeight;
	        }
	        return windowPos;
	    }
	
	    /**
	     * 获取屏幕高度(px)
	     */
	    public static int getScreenHeight(Context context) {
	        return context.getResources().getDisplayMetrics().heightPixels;
	    }
	
	    /**
	     * 获取屏幕宽度(px)
	     */
	    public static int getScreenWidth(Context context) {
	        return context.getResources().getDisplayMetrics().widthPixels;
	    }
	
	    public interface PopTopOnClick{
	        void EditOnclick();
	        void DelOnclick();
	        void SysOnclick();
	        void CreateOnclick();
	    }

	}


最终你可以

	if (view.getId() == R.id.common_tv_right_more) { 
            new PopTop.Builder(this)
                    .setView(findViewById(R.id.common_tv_right_more)) // 在某个空间的下面 
                    .setPopTopOnClick(new PopTop.PopTopOnClick() {
                        @Override
                        public void EditOnclick() {
                            /* 编辑 监听*/ 
                        } 
                        @Override
                        public void DelOnclick() {
                            /*删除 监听*/
                        } 
                    })
                    .show();
        }


## 最后 同类想法
你可以写一个评论的pop
例如  
![](https://i.imgur.com/FtYcCL6.png)
点击弹出键盘 pop 显示处理，键盘消失pop 消失。
这个之前也有个项目里面有，但是没有这么写，就堆在一起，也没有反思，那个时候还没有blog，现在想一想真的太像了。

## 补充

本来想做一系类的pop 的底部和头部东西，有必要以后用，但是这种模式是好的。
今天补充的是不写了，因为我看到了一个很好的demo [地址CustomPopwindow](https://github.com/pinguo-zhouwei/CustomPopwindow)
效果
![](https://github.com/pinguo-zhouwei/CustomPopwindow/raw/master/image/pop_window.gif)

简直了。真的有心人。

还有一个dialog 和pop 在一起 可以重复使用的
[dialog和pop](http://blog.csdn.net/yiranaini_/article/details/52245756)

![](https://github.com/H07000223/FlycoDialog_Master/blob/master/gif/preview_popup_1.gif?raw=true)

还有一个挺好的
https://juejin.im/post/5865f43bac502e006129ba8a
在掘金上看到的和我认为一样的。

认准了思想了，就不会跑偏。