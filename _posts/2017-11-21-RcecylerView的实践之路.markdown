---
layout:     post
title:      "RececylerView 的实践之路"
subtitle:   " \"教程 \""
date:       2017-11-21 10:16:10
author:     "Smm"
header-img: "img/003.jpg"
tags:
    - 教程
---


## 前言

写RececylerView  本来是为了学习他特有特性去学习的，本来是看到他的高度和深度，从而去学习的，但是在实际中
我要找好多案例，以及一些测试代码，但是都没有很好的，很全面的的内容，反而拖地时间又长了，所以感觉写到哪里就是哪里吧，还有很多觉得应该涉及到的都会提到吧。 之前开始用也是在16年的时候，总监让看新的知识的时候，才看到有一个控件，突然看到Android Material Design 中新的控件，从而有对design 控件都有看，还看了一会源码，真的看得快，记得也很慢。又对io大会里面的有看了看。哎，自己每天在刷什么流量啊？ 对于listview 还是很想还是想用，所以强迫自己放弃。

## rececylerview 基础

RecyclerView与ListView原理是类似的：都是仅仅维护少量的View并且可以展示大量的数据集。RecyclerView用以下两种方式简化了数据的展示和处理:

使用LayoutManager来确定每一个item的排列方式。
为增加和删除项目提供默认的动画效果。
你也可以定义你自己的LayoutManager和添加删除动画，RecyclerView项目结构如下：

 ![](http://www.jcodecraeer.com/uploads/20141021/16791413904189.png)

Adapter：使用RecyclerView之前，你需要一个继承自RecyclerView.Adapter的适配器，作用是将数据与每一个item的界面进行绑定。
LayoutManager：用来确定每一个item如何进行排列摆放，何时展示和隐藏。回收或重用一个View的时候，LayoutManager会向适配器请求新的数据来替换旧的数据，这种机制避免了创建过多的View和频繁的调用findViewById方法（与ListView原理类似）。
目前SDK中提供了三种自带的LayoutManager:

LinearLayoutManager
GridLayoutManager
StaggeredGridLayoutManager


LinearLayoutManager 的使用最多。 横向的用的也很少，

GridLayoutManager 使用一般

StaggeredGridLayoutManager 几乎没用到

## 注意

1，adapter的使用

	 public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
			、、、、
	}

2，高度是固定的

	//如果可以确定每个item的高度是固定的，设置这个选项可以提高性能
	mRecyclerView.setHasFixedSize(true);

3，viewholder 的使用

	public static class ViewHolder extends RecyclerView.ViewHolder {
        public TextView mTextView;
        public ViewHolder(View view){
        super(view);
            mTextView = (TextView) view.findViewById(R.id.text);
        }
    }


4，横向布局

	mLayoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);

5，Grid布局

	mLayoutManager = new GridLayoutManager(context,columNum);
	mRecyclerView.setLayoutManager(mLayoutManager);

6.添加点击事件

	public static interface OnRecyclerViewItemClickListener {
	    void onItemClick(View view , DataModel data);
	}
	
	private OnRecyclerViewItemClickListener mOnItemClickListener = null;
	    public void setOnItemClickListener(OnRecyclerViewItemClickListener listener) {
	    this.mOnItemClickListener = listener;
	}
	
	public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> implements View.OnClickListener{
	    @Override
	    public ViewHolder onCreateViewHolder(ViewGroup viewGroup, final int i) {
	        View view = LayoutInflater.from(viewGroup.getContext()).inflate(R.layout.item, viewGroup, false);
	        ViewHolder vh = new ViewHolder(view);
	        //将创建的View注册点击事件
	        view.setOnClickListener(this);
	        return vh;
	    }
	    @Override
	    public void onBindViewHolder(ViewHolder viewHolder, final int i) {
	        viewHolder.mTextView.setText(datas.get(i).title);
	        //将数据保存在itemView的Tag中，以便点击时进行获取
	        viewHolder.itemView.setTag(datas.get(i));
	    }
	    ...
	    @Override
	    public void onClick(View v) {
	        if (mOnItemClickListener != null) {
	            //注意这里使用getTag方法获取数据
	            mOnItemClickListener.onItemClick(v,(DataModel)v.getTag());
	        }
	    }
	    ...
	}
	mRecyclerView.setAdapter(mAdapter);
	mAdapter.setOnItemClickListener(new MyAdapter.OnRecyclerViewItemClickListener() {
	    @Override
	    public void onItemClick(View view, DataModel data) {
	        //DO your fucking bussiness here!
	    }
	});

7, 删除 数据 主要对adapter中数据处理
 
	public void removeItem(DataModel model) {
	    int position = datas.indexOf(model);
	    datas.remove(position);
	    notifyItemRemoved(position);//Attention!
	}

8,清空数据  主要对adapter中数据处理

	public void clear() {
	    if（datas ！= null）{
		    datas.clear
		    notifyDataSetChanged();
		}
	}

9,添加数据 主要对adapter中数据处理

	public void addList(List<String> list) {
	    if（mList ！= null）{
		    mList.addAll(list);
		}else{
			mList = list;
		} 
		notifyDataSetChanged();
	}

10,动画

	mRecyclerView.setItemAnimator(newDefaultItemAnimator());

11, scrollToPosition(int position)滑动到指定item

12.获取当前RecyclerView首尾可见item的adapter positon方法
 
13.setStackFromEnd(boolean stackFromEnd)
当从堆底部开始展示设置为true时，列表便会从底部开始展示内容

设置为true时，RecycelrView会自动滑倒尾部，直到最后一条数据完整展示 


## LayoutManager 的使用

这里不是简单的写 
LinearLayoutManager 的使用最多。 横向的用的也很少，
GridLayoutManager 使用一般
StaggeredGridLayoutManager 几乎没用到
这个几种方法，而是写自定义一些方法。
当然做法就是写一个类来继承RecyclerView.LayoutManager
首先看看几个重要的方法


generateDefaultLayoutParams()
这是一个必须重写的方法，当然仅仅实现这个方法不行，虽然能编译通过。这个方法是给RecyclerView的子View创建一个默认的LayoutParams，实现起来也十分简单。

onLayoutChildren
这个方法显然是用于放置子view的位置，十分重要的一个方法。


canScrollVertically()和canScrollHorizontally()
若想要RecyclerView能水平或者竖直滚动这两个方法需要重写返回true


scrollVerticallyBy()和scrollHorizontallyBy()
在水平或者竖直滚动时会分别调用这两个方法，dx，dy代表每次的增长值，返回值是真实移动的距离

效果也现的很重要了，学习可以看下面一篇文章。

[学习连接](http://www.jianshu.com/p/715b59c46b74)

其实我用到的也是在用RecyclerView打造一个轮播图中用到的，在下面再谈，因为一个综合使用的情况下，
必然会用到很多东西，有了这些东西才能一撮而就的。


## RecyclerView.ItemDecoration 

我用到RecyclerView 的使用要做成两行并且都是正方形出现在列表里面。
我是这样做的，单独拿出来额了。
这个端代码是基本的使用，是底部划线部分，还有是什么方向的线

	addItemDecoration(new DividerItemDecoration(this,
	DividerItemDecoration.VERTICAL_LIST));

我的代码

	int d = getContext().getResources().getColor(R.color.line);
        mainRecyclerview.addItemDecoration(new GridDivider(getContext(), 1, d));

在GridDivider 中
 
	import android.content.Context;
	import android.content.res.TypedArray;
	import android.graphics.Canvas;
	import android.graphics.Paint;
	import android.graphics.Rect;
	import android.graphics.drawable.Drawable;
	import android.os.Build;
	import android.support.annotation.RequiresApi;
	import android.support.v7.widget.RecyclerView;
	import android.view.View;
	
	import com.kesun.mifeng.main.R;
	
	
	public class GridDivider extends RecyclerView.ItemDecoration {
	
	    private Drawable mDividerDarwable;
	
	    private int mDividerHight = 1;
	
	    private Paint mColorPaint;
	
	    public final int[] ATRRS = new int[]{android.R.attr.listDivider};
	
	    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
	    public GridDivider(Context context) {
	        final TypedArray ta = context.obtainStyledAttributes(ATRRS);
	        this.mDividerDarwable = ta.getDrawable(0);
	        if (mDividerDarwable == null) {
	            mDividerDarwable = context.getDrawable(R.mipmap.ic_launcher);
	        }
	        ta.recycle();
	    }
	
	        /*
	       int dividerHight 分割线的线宽
	       int dividerColor 分割线的颜色
	   */
	
	
	    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
	    public GridDivider(Context context, int dividerHight, int dividerColor) {
	        this(context);
	        mDividerHight = dividerHight;
	        mColorPaint = new Paint();
	        mColorPaint.setColor(dividerColor);
	    }
	
	        /*
	       int dividerHight 分割线的线宽
	       Drawable dividerDrawable 图片分割线
	   */
	
	
	    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
	    public GridDivider(Context context, int dividerHight, Drawable dividerDrawable) {
	        this(context);
	        mDividerHight = dividerHight;
	        mDividerDarwable = dividerDrawable;
	    }
	
	
	    @Override
	
	
	    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
	        super.onDraw(c, parent, state);
	//画水平和垂直分割线
	        drawHorizontalDivider(c, parent);
	        drawVerticalDivider(c, parent);
	    }
	
	
	    public void drawVerticalDivider(Canvas c, RecyclerView parent) {
	        final int childCount = parent.getChildCount();
	        for (int i = 0; i < childCount; i++) {
	            final View child = parent.getChildAt(i);
	            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child.getLayoutParams();
	
	            final int top = child.getTop() - params.topMargin;
	            final int bottom = child.getBottom() + params.bottomMargin;
	
	            int left = 0;
	            int right = 0;
	
	//左边第一列
	            if ((i % 3) == 0) {
	//item左边分割线
	                left = child.getLeft();
	                right = left + mDividerHight;
	                mDividerDarwable.setBounds(left, top, right, bottom);
	                mDividerDarwable.draw(c);
	                if (mColorPaint != null) {
	                    c.drawRect(left, top, right, bottom, mColorPaint);
	                }
	//item右边分割线
	                left = child.getRight() + params.rightMargin - mDividerHight;
	                right = left + mDividerHight;
	            } else {
	//非左边第一列
	                left = child.getRight() + params.rightMargin - mDividerHight;
	                right = left + mDividerHight;
	            }
	//画分割线
	            mDividerDarwable.setBounds(left, top, right, bottom);
	            mDividerDarwable.draw(c);
	            if (mColorPaint != null) {
	                c.drawRect(left, top, right, bottom, mColorPaint);
	            }
	
	        }
	    }
	
	
	    public void drawHorizontalDivider(Canvas c, RecyclerView parent) {
	
	        final int childCount = parent.getChildCount();
	        for (int i = 0; i < childCount; i++) {
	            final View child = parent.getChildAt(i);
	            RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child.getLayoutParams();
	
	            final int left = child.getLeft() - params.leftMargin - mDividerHight;
	            final int right = child.getRight() + params.rightMargin;
	            int top = 0;
	            int bottom = 0;
	
	// 最上面一行
	            if ((i / 3) == 0) {
	//当前item最上面的分割线
	                top = child.getTop();
	//当前item下面的分割线
	                bottom = top + mDividerHight;
	                mDividerDarwable.setBounds(left, top, right, bottom);
	                mDividerDarwable.draw(c);
	                if (mColorPaint != null) {
	                    c.drawRect(left, top, right, bottom, mColorPaint);
	                }
	                top = child.getBottom() + params.bottomMargin;
	                bottom = top + mDividerHight;
	            } else {
	                top = child.getBottom() + params.bottomMargin;
	                bottom = top + mDividerHight;
	            }
	//画分割线
	            mDividerDarwable.setBounds(left, top, right, bottom);
	            mDividerDarwable.draw(c);
	            if (mColorPaint != null) {
	                c.drawRect(left, top, right, bottom, mColorPaint);
	            }
	        }
	    }
	 
	}

在GridDivider 中已经写死了必然会是九宫格部分。
主要是drawVerticalDivider和drawHorizontalDivider 部分，对横向和竖向部分进行的划线。
还好的时候都有Canvas 这个参数，才能进行不同效果的处理。
也可以进行其他的效果，可以搜索。
[学习连接](http://www.jianshu.com/p/b46a4ff7c10a)

## viewholder的使用

主要写点自定义viewholder 方法

[学习地址1](https://segmentfault.com/a/1190000005184768 )

[学习地址2](http://www.jianshu.com/p/95bd3dd02d42?from=timeline)

## gridview和listview 的切换
	
RececylerView 在使用中，可以切换无非就是grid 一套，和 list 的一套。两种都写下来。

就是转换的一下形式而已。减少了gridview 和listview 的两种控件的同时使用，在 淘宝列表中 ，常常会看到这样的问题。

[学习连接](https://github.com/gjiazhe/LayoutSwitch)


## 多item

认识RecyclerView.Adapter 里面的各个方法进行分析

对onCreateViewHolder 这个方法的理解

	@Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        if (viewType == ITEM_TYPE.ITEM_TYPE_IMAGE.ordinal()) {
            return new ImageViewHolder(mLayoutInflater.inflate(R.layout.item_image, parent, false));
        } else {
            return new TextViewHolder(mLayoutInflater.inflate(R.layout.item_text, parent, false));
        }
    }
	
	同年对 getItemViewType 
	
	 @Override
    public int getItemViewType(int position) {
        return position % 2 == 0 ? ITEM_TYPE.ITEM_TYPE_IMAGE.ordinal() : ITEM_TYPE.ITEM_TYPE_TEXT.ordinal();
    }
	来分辨出来 不同的type 显示不同
	显示和操作等功能在onBindViewHolder这个方法里面
	
	@Override  
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {  
        if (holder instanceof TextViewHolder) {  
            ((TextViewHolder) holder).mTextView.setText(mTitles[position]);  
        } else if (holder instanceof ImageViewHolder) {  
            ((ImageViewHolder) holder).mTextView.setText(mTitles[position]);  
        }  
    }  
	
	所有看到viewholder 较为很重要的部分。
	

 多Item布局实现(MultipleItem)

## 嵌套RecycleView
RecyclerView 与 ScrollView嵌套
FullyLinearLayoutManager 的使用，可以归纳到layoutMannager的使用中
FullyGridLayoutManager 的使用 

问题会出现
1.RecyclerView 显示不全
2.RecyclerView 与ScrollView 滑动冲突
3.嵌套布局不显示在顶部，直接显示RecyclerView 第一个item

[解决方法1](http://blog.csdn.net/u014434080/article/details/70256098)
[解决方法2](http://blog.csdn.net/a2241076850/article/details/63262432)

## 用RecyclerView打造一个轮播图
RecyclerView + SnapHelper 
https://juejin.im/post/5a13a28c51882512a860ee6a
http://www.jcodecraeer.com/a/opensource/2017/0802/8326.html
http://www.jcodecraeer.com/a/opensource/2016/0905/6596.html



## 下拉刷新上拉加载更多

1，下拉刷新 SwipeRefreshLayout + RecyclerView 来实现

2，上拉更多 
	
过程 

adapter 中添加 底部 菜单，添加等待界面
recyclerview 判断是否到达底部

recyclerview 中 多个item 中 的添加 上面讲的了

滑到底部判断
	
		public abstract class MyOnScrollListener extends RecyclerView.OnScrollListener {
		// 用来标记是否正在向上滑动
		private boolean isSlidingUpward = false;

		@Override
		public void onScrollStateChanged(RecyclerView recyclerView, int newState) {
			super.onScrollStateChanged(recyclerView, newState);
			LinearLayoutManager manager = (LinearLayoutManager) recyclerView.getLayoutManager();
			// 当不滑动时
			if (newState == RecyclerView.SCROLL_STATE_IDLE) {
				// 获取最后一个完全显示的itemPosition
				int lastItemPosition = manager.findLastCompletelyVisibleItemPosition();
				int itemCount = manager.getItemCount();

				// 判断是否滑动到了最后一个item，并且是向上滑动
				if (lastItemPosition == (itemCount - 1) && isSlidingUpward) {
					// 加载更多
					onLoadMore();
				}
			}
		}

		@Override
		public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
			super.onScrolled(recyclerView, dx, dy);
			// 大于0表示正在向上滑动，小于等于0表示停止或向下滑动
			isSlidingUpward = dy > 0;
		}

		/**
		 * 加载更多回调
		 */
		public abstract void onLoadMore();
	}

自定义一adapter 对原来的adapter 没有任何影响 对于不同的参数显示不同	
	
	public class LoadMoreWrapper extends RecyclerView.Adapter<RecyclerView.ViewHolder> {
	
	    private RecyclerView.Adapter adapter;
	
	    // 普通布局
	    private final int TYPE_ITEM = 1;
	    // 脚布局
	    private final int TYPE_FOOTER = 2;
	    // 当前加载状态，默认为加载完成
	    private int loadState = 2;
	    // 正在加载
	    public final int LOADING = 1;
	    // 加载完成
	    public final int LOADING_COMPLETE = 2;
	    // 加载到底
	    public final int LOADING_END = 3;
	
	    public LoadMoreWrapper(RecyclerView.Adapter adapter) {
	        this.adapter = adapter;
	    }
	
	    @Override
	    public int getItemViewType(int position) {
	        // 最后一个item设置为FooterView
	        if (position + 1 == getItemCount()) {
	            return TYPE_FOOTER;
	        } else {
	            return TYPE_ITEM;
	        }
	    }
	
	    @Override
	    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
	        if(viewType == TYPE_FOOTER){
	            View view = LayoutInflater.from(parent.getContext())
	                    .inflate(R.layout.layout_refresh_footer, parent, false);
	            return new FootViewHolder(view);
	        } else {
	            return adapter.onCreateViewHolder(parent, viewType);
	        }
	    }
	
	    @Override
	    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
	        if(holder instanceof FootViewHolder){
	            FootViewHolder footViewHolder = (FootViewHolder) holder;
	
	            switch (loadState){
	                case LOADING: // 正在加载
	                    footViewHolder.pbLoading.setVisibility(View.VISIBLE);
	                    footViewHolder.tvLoading.setVisibility(View.VISIBLE);
	                    footViewHolder.llEnd.setVisibility(View.GONE);
	                    break;
	
	                case LOADING_COMPLETE: // 加载完成
	                    footViewHolder.pbLoading.setVisibility(View.INVISIBLE);
	                    footViewHolder.tvLoading.setVisibility(View.INVISIBLE);
	                    footViewHolder.llEnd.setVisibility(View.GONE);
	                    break;
	
	                case LOADING_END: // 加载到底
	                    footViewHolder.pbLoading.setVisibility(View.GONE);
	                    footViewHolder.tvLoading.setVisibility(View.GONE);
	                    footViewHolder.llEnd.setVisibility(View.VISIBLE);
	                    break;
	            }
	
	        }else {
	            adapter.onBindViewHolder(holder,position);
	        }
	    }
	
	    public class FootViewHolder extends RecyclerView.ViewHolder {
	        ProgressBar pbLoading;
	        TextView tvLoading;
	        LinearLayout llEnd;
	
	        FootViewHolder(View itemView) {
	            super(itemView);
	            pbLoading = (ProgressBar) itemView.findViewById(R.id.pb_loading);
	            tvLoading = (TextView) itemView.findViewById(R.id.tv_loading);
	            llEnd = (LinearLayout) itemView.findViewById(R.id.ll_end);
	        }
	    }
	
	    @Override
	    public int getItemCount() {
	        return adapter.getItemCount() + 1;
	    }
	
	    public void setLoadState(int loadState) {
	        this.loadState = loadState;
	        notifyDataSetChanged();
	    }
	}

这样就是开始新的rececylerview的正常使用就可以了。

## 好下来刷新和加载更多
SmartRefreshLayout + loadinglib 为空 没有网络 等layout的结合。
[https://github.com/scwang90/SmartRefreshLayout](https://github.com/scwang90/SmartRefreshLayout)


## 扩张

https://juejin.im/post/5a7c40325188254e76179bbc?utm_source=gold_browser_extension
vlayout 的使用最多。
结合上拉更多

http://blog.csdn.net/lyb2518/article/details/78591805

## 后谢
[http://blog.csdn.net/qibin0506/article/details/52676670](http://blog.csdn.net/qibin0506/article/details/52676670)
[http://www.jcodecraeer.com/a/opensource/2017/0922/8540.html](http://www.jcodecraeer.com/a/opensource/2017/0922/8540.html)
[https://juejin.im/post/5a13a28c51882512a860ee6a](https://juejin.im/post/5a13a28c51882512a860ee6a)
[http://www.jcodecraeer.com/a/opensource/2017/0802/8326.html](http://www.jcodecraeer.com/a/opensource/2017/0802/8326.html)
[http://www.jcodecraeer.com/a/opensource/2016/0905/6596.html](http://www.jcodecraeer.com/a/opensource/2016/0905/6596.html)
[http://www.jianshu.com/p/95bd3dd02d42?from=timeline](http://www.jianshu.com/p/95bd3dd02d42?from=timeline)

[http://blog.csdn.net/lmj623565791/article/details/38026503](http://blog.csdn.net/lmj623565791/article/details/38026503)

[http://blog.csdn.net/qxf5777404/article/details/70162132](http://blog.csdn.net/qxf5777404/article/details/70162132)