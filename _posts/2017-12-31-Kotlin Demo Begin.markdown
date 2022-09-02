---
layout:     post
title:      "Kotlin Demo Begin"
subtitle:   " \" 翻译 \""
date:       2017-12-31 14:16:10
author:     "Smm"
header-img: "img/004.jpg"
tags:
    - 翻译
---



## 前言
这篇是翻译人家的，demo 是自己写的。为了学习kotlin 语言而看的这篇帖子的。

原文帖子名字为

[How to develop android image gallery app in Kotlin tutorial with complete source code](http://developine.com/develop-android-image-gallery-app-kotlin-with-source-code/#comments)

点击可以查看原文。
## 内容

如何使用Kotliny语言，进行android应用程序开发的图像画廊
![内容](http://developine.com/wp-content/uploads/2017/12/android-gallery-app-kotlin-canal-pic.png)

在本教程中，我们将开发一个完整的Android照片画廊，用kotlin语言，我将在文字结尾把画廊中应用程序的源码展现出来。

下面涵盖一下关于Kotlin的相关知识

- 如何在Kotlin实施单件模式。
- 如何通过Parcelable接口的对象从一个活动到另一个活动界面
- 共享prefrences，读取设备上的图片资源用 Kotlin。
- Kotlin中Recyclerview 的使用
- Kotlin中Recyclerview 的adapter 的使用
- NavigationView和drawer 的使用
- 用kotlin中把内部存储的图片进行获取和展示出来。
- 将使用Glide 显示图片
- android 权限方法的使用


如果你想展现你的广告和图像用RecyclerView ，我建议你可以通过这篇帖子

创建一个新的项目，用android studio 并且对kotlin 的支持

在权限文件中添加 下面权限


		<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

 ### 创建data 类名为“Albums” 集成Parcelable 
这个是我们的model 类，包含所有文件夹的信息如文件夹名（WhatsApp的图像、照相机）、文件夹中最近图像的图像路径、文件夹中的图像总数。


我们必须把这些信息获取到显示在我们的mainactivity中显示出来，用intent来传递，所有Albuns 这个类要集成Parcelable 接口

	data class Albums(var folderNames: String, var imagePath: String, var imgCount: Int, var isVideo: Boolean) : Parcelable {
	    constructor(parcel: Parcel) : this(
	            parcel.readString(),
	            parcel.readString(),
	            parcel.readInt(),
	            parcel.readByte() != 0.toByte()) {
	    }
	
	    override fun writeToParcel(parcel: Parcel, flags: Int) {
	        parcel.writeString(folderNames)
	        parcel.writeString(imagePath)
	        parcel.writeInt(imgCount)
	        parcel.writeByte(if (isVideo) 1 else 0)
	    }
	
	    override fun describeContents(): Int {
	        return 0
	    }
	
	    companion object CREATOR : Parcelable.Creator<Albums> {
	        override fun createFromParcel(parcel: Parcel): Albums {
	            return Albums(parcel)
	        }
	
	        override fun newArray(size: Int): Array<Albums?> {
	            return arrayOfNulls(size)
	        }
	    }
	}

###添加一个新的欢迎Activity 并添加下面的代码
在这个activity中，首先在oncreate方法中访问外部存储器，我们要求用户允许应用程序访问存储在设备上的图像。

然后用户需要对read_external_storag授予权限，我们可以获取图像存储和发送图像的名称以及显示所有文件夹的动图像总数，并且在Recyclerview上显示出来。


	// SplashActivity.kt
	
	internal var SPLASH_TIME_OUT = 800
	
	override fun onCreate(savedInstanceState: Bundle?) {
	    super.onCreate(savedInstanceState)
	
	    setContentView(R.layout.activity_splash)
	
	    Handler().postDelayed(
	            {
	                 // check if user has grannted permission to access device external storage.
	                 // if not ask user for access to external storage.
	                if (!checkSelfPermission()) {
	                    requestPermission()
	                } else {
	                   // if permission granted read images from storage.
	                   //  source code for this function can be found below.
	                    loadAllImages()
	                }
	            }, SPLASH_TIME_OUT.toLong())
	}

### 添加权限代码

	private fun requestPermission() {
	    ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.READ_EXTERNAL_STORAGE), 6036)
	}


	private fun checkSelfPermission(): Boolean {
	
	    if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED) {
	        return false
	    } else
	        return true
	}

如果用户已授予或拒绝权限，此方法将被调用。

	override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
	    when (requestCode) {
	        6036 -> {
	            if (grantResults.size > 0) {
	                var permissionGranted = grantResults[0] == PackageManager.PERMISSION_GRANTED
	                if (permissionGranted) {
	
	    // Now we are ready to access device storage and read images stored on device.
	
	                    loadAllImages()
	                } else {
	                    Toast.makeText(this, "Permission Denied! Cannot load images.", Toast.LENGTH_SHORT).show()
	                }
	            }
	        }
	    }
	    super.onRequestPermissionsResult(requestCode, permissions, grantResults)
	}

如果我们允许通过权限，我们可以把获取的图片资源传递到MainActivity.kt中

通过数据类（模型类）对象的意图打包。

这个函数会调用另一个方法getallshownimagespath（上下文）会返回类型的专辑列表。

并通过回归专辑列表通过意图的主要活动。

主要活动将显示所有文件夹，例如图像和视频列表（相机、Instagram、WhatsApp图像）在RecyclerView显示。

	fun loadAllImages() {
	    var imagesList = getAllShownImagesPath(this)
	    var intent = Intent(this, MainActivity::class.java)
	    intent.putParcelableArrayListExtra("image_url_data", imagesList)
	    startActivity(intent)
	    finish()
	}

在上面代码中，我们有数据对象，我们需要把这些数据传递到另一个活动界面里面，因此我们要用序列化来传递数据。

###从内部存储器加载图像

在这段代码我们阅读所有的文件夹，图片和视频类应用mediastore。

In this code snippet we are reading all folders which has images and videos using MediaStore class.

返回的结果列表 ArrayList<Albums>.


	private fun getAllShownImagesPath(activity: Activity): ArrayList<Albums> {
	
	    val uri: Uri
	    val cursor: Cursor
	    var cursorBucket: Cursor
	    val column_index_data: Int
	    val column_index_folder_name: Int
	    val listOfAllImages = ArrayList<String>()
	    var absolutePathOfImage: String? = null
	    var albumsList = ArrayList<Albums>()
	    var album: Albums? = null


​	
​	    val BUCKET_GROUP_BY = "1) GROUP BY 1,(2"
​	    val BUCKET_ORDER_BY = "MAX(datetaken) DESC"
​	
	    uri = android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI
	
	    val projection = arrayOf(MediaStore.Images.ImageColumns.BUCKET_ID,
	            MediaStore.Images.ImageColumns.BUCKET_DISPLAY_NAME,
	            MediaStore.Images.ImageColumns.DATE_TAKEN,
	            MediaStore.Images.ImageColumns.DATA)
	
	    cursor = activity.contentResolver.query(uri, projection, BUCKET_GROUP_BY, null, BUCKET_ORDER_BY)
	
	    if (cursor != null) {
	        column_index_data = cursor.getColumnIndexOrThrow(MediaStore.MediaColumns.DATA)
	        column_index_folder_name = cursor
	                .getColumnIndexOrThrow(MediaStore.Images.Media.BUCKET_DISPLAY_NAME)
	        while (cursor.moveToNext()) {
	            absolutePathOfImage = cursor.getString(column_index_data)
	            Log.d("title_apps", "bucket name:" + cursor.getString(column_index_data))
	
	            val selectionArgs = arrayOf("%" + cursor.getString(column_index_folder_name) + "%")
	            val selection = MediaStore.Images.Media.DATA + " like ? "
	            val projectionOnlyBucket = arrayOf(MediaStore.MediaColumns.DATA, MediaStore.Images.Media.BUCKET_DISPLAY_NAME)
	
	            cursorBucket = activity.contentResolver.query(uri, projectionOnlyBucket, selection, selectionArgs, null)
	            Log.d("title_apps", "bucket size:" + cursorBucket.count)
	
	            if (absolutePathOfImage != "" && absolutePathOfImage != null) {
	                listOfAllImages.add(absolutePathOfImage)
	                albumsList.add(Albums(cursor.getString(column_index_folder_name), absolutePathOfImage, cursorBucket.count, false))
	            }
	        }
	    }
	    return getListOfVideoFolders(albumsList)
	}
	
	// This function is resposible to read all videos from all folders.
	private fun getListOfVideoFolders(albumsList: ArrayList<Albums>): ArrayList<Albums> {
	
	    var cursor: Cursor
	    var cursorBucket: Cursor
	    var uri: Uri
	    val BUCKET_GROUP_BY = "1) GROUP BY 1,(2"
	    val BUCKET_ORDER_BY = "MAX(datetaken) DESC"
	    val column_index_album_name: Int
	    val column_index_album_video: Int
	
	    uri = android.provider.MediaStore.Video.Media.EXTERNAL_CONTENT_URI
	
	    val projection1 = arrayOf(MediaStore.Video.VideoColumns.BUCKET_ID,
	            MediaStore.Video.VideoColumns.BUCKET_DISPLAY_NAME,
	            MediaStore.Video.VideoColumns.DATE_TAKEN,
	            MediaStore.Video.VideoColumns.DATA)
	
	    cursor = this.contentResolver.query(uri, projection1, BUCKET_GROUP_BY, null, BUCKET_ORDER_BY)
	
	    if (cursor != null) {
	        column_index_album_name = cursor.getColumnIndexOrThrow(MediaStore.Video.Media.BUCKET_DISPLAY_NAME)
	        column_index_album_video = cursor.getColumnIndexOrThrow(MediaStore.Video.Media.DATA)
	        while (cursor.moveToNext()) {
	            Log.d("title_apps", "bucket video:" + cursor.getString(column_index_album_name))
	            Log.d("title_apps", "bucket video:" + cursor.getString(column_index_album_video))
	            val selectionArgs = arrayOf("%" + cursor.getString(column_index_album_name) + "%")
	
	            val selection = MediaStore.Video.Media.DATA + " like ? "
	            val projectionOnlyBucket = arrayOf(MediaStore.MediaColumns.DATA, MediaStore.Video.Media.BUCKET_DISPLAY_NAME)
	
	            cursorBucket = this.contentResolver.query(uri, projectionOnlyBucket, selection, selectionArgs, null)
	            Log.d("title_apps", "bucket size:" + cursorBucket.count)
	
	            albumsList.add(Albums(cursor.getString(column_index_album_name), cursor.getString(column_index_album_video), cursorBucket.count, true))
	        }
	    }
	    return albumsList
	}

到目前为止我们做了什么？

我们已经了解了在Android中开发图像库应用程序需要做些什么，为此目的


1. 在我们的图库应用程序中添加了加载和显示图像的运行时权限要求。
2. 我们已经完成了加载图片的路径从Android设备内部存储使用mediastore和光标。
3. 如何通过序列化的数据类对象从一个活动到另一个活动界面
4. 在我们的项目学会了如何使用数据类（Albums.KT）。

现在创建另一个活动的名字mainactivity.kt类项目中。

我们要做的主要活动

1. 我们将收到来自splashactivity中相册对象。
2. RecyclerView item 点击事件监听，是对接口的监听实现的。
3. 创建的item 的layout文件，来显示所有的文件数据。


创建 maniActivity 的layout 文件

	// activity_main.xml
	
	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:fitsSystemWindows="true">
	
	    <android.support.v7.widget.Toolbar
	        android:id="@+id/my_toolbar"
	        android:layout_width="match_parent"
	        android:layout_height="?attr/actionBarSize"
	        android:background="?attr/colorPrimary"
	        android:elevation="4dp"
	        app:titleTextColor="@color/colorIcons" />
	    
	    <android.support.v4.widget.DrawerLayout
	        android:id="@+id/drawer_layout"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:layout_below="@+id/my_toolbar"
	        android:fitsSystemWindows="true">
	
	        <!-- Your contents -->
	        <include layout="@layout/include_main_content"></include>
	
	        <android.support.design.widget.NavigationView
	            android:id="@+id/navigation"
	            android:layout_width="wrap_content"
	            android:layout_height="match_parent"
	            android:layout_gravity="start"
	            android:onClick="toast"
	            app:menu="@menu/my_navigation_menu"
	            app:theme="@style/NavigationDrawerStyle" />
	    </android.support.v4.widget.DrawerLayout>
	</RelativeLayout>

=   


	//include_main_content.xml 
	
	<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:id="@+id/main_content"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">


​	
​	    <android.support.v7.widget.RecyclerView
​	        android:id="@+id/rvAlbums"
​	        android:layout_width="match_parent"
​	        android:layout_height="match_parent"
​	        android:clipToPadding="false" />
​	
	    <android.support.design.widget.FloatingActionButton
	        android:id="@+id/fab_camera"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:layout_gravity="bottom|right"
	        android:layout_margin="16dp"
	        android:src="@drawable/ic_photo_camera_white_24dp"
	        app:layout_anchor="@id/rvAlbums"
	        app:layout_anchorGravity="bottom|right|end"
	        app:layout_behavior="com.title_apps.canalpic.util.ScrollAwareFABBehavior" />
	
	</android.support.design.widget.CoordinatorLayout>
	
	//MainActivity.kt
	
	override fun onCreate(savedInstanceState: Bundle?) {
	    super.onCreate(savedInstanceState)
	    setContentView(R.layout.activity_main)
	
	    savedState = savedInstanceState
	
	    if (savedState != null)
	        folder_name = savedInstanceState!!.getString("folder_name")
	
	    setSupportActionBar(my_toolbar)
	    // Enable the Up button
	    supportActionBar!!.setDisplayHomeAsUpEnabled(true)
	    supportActionBar!!.setHomeAsUpIndicator(resources.getDrawable(R.drawable.ic_menu_white_24dp))
	
	    setupNavigationView()
	
	    var extra = intent.extras;
	    if (extra != null) {
	        var extraData = extra.get("image_url_data") as ArrayList<Albums>
	        select_fragment(extraData)
	    }
	
	    drawer_layout_listener()
	    supportActionBar!!.setTitle("Folders")
	}
	
	// here we are sending folder name on which user has clicked, for example if user has
	// clicked on downloads folder from list of all folders in our gallery app we will pass that
	// folder name to our next activity which will load and display all images from that folder.
	
	override fun onItemClick(position: String, isVideo: Boolean) {
	
	    var bundle = Bundle()
	    bundle.putString("folder_name", position)
	        var intent = Intent(this, AlbumActivity::class.java)
	        intent.putExtra("folder_name", position)
	        startActivity(intent)
	}

在我们的Android Gallery应用中如何使用Glide 来显示图片

	private var folder_name: String = ""
	
	public fun select_fragment(imagesList: ArrayList<Albums>) {
	
	    val options = RequestOptions()
	            .diskCacheStrategy(DiskCacheStrategy.RESOURCE).override(160, 160).skipMemoryCache(true).error(R.drawable.ic_image_unavailable)
	    val glide = Glide.with(this)
	
	    val builder = glide.asBitmap()
	    rvAlbums?.layoutManager = GridLayoutManager(this, 2)
	
	    rvAlbums?.setHasFixedSize(true)
	
	    // AlbumFoldersAdapter.kt is RecyclerView Adapter class. we will implement shortly.
	    rvAlbums?.adapter = AlbumFoldersAdapter(imagesList, this, options, builder, glide, this)


​	
​	    rvAlbums?.addOnScrollListener(object : RecyclerView.OnScrollListener() {
​	        override fun onScrolled(recyclerView: RecyclerView?, dx: Int, dy: Int) {
​	            super.onScrolled(recyclerView, dx, dy)
​	        }
​	
	        override fun onScrollStateChanged(recyclerView: RecyclerView?, newState: Int) {
	            super.onScrollStateChanged(recyclerView, newState)
	            when (newState) {
	                RecyclerView.SCROLL_STATE_IDLE -> glide.resumeRequests()
	                AbsListView.OnScrollListener.SCROLL_STATE_TOUCH_SCROLL, AbsListView.OnScrollListener.SCROLL_STATE_FLING -> glide.pauseRequests()
	            }
	        }
	    }
	    )
	
	    fab_camera?.setOnClickListener(object : View.OnClickListener {
	        override fun onClick(p0: View?) {
	            launchCamera()
	        }
	    }
	    )
	}

NavigationView and drawerLayout 的点击监听 方法

	// drawer layout click listener in Kotlin source code.
	private fun drawer_layout_listener() {
	
	    drawer_layout.addDrawerListener(object : DrawerLayout.DrawerListener {
	        override fun onDrawerStateChanged(newState: Int) {
	        }
	
	        override fun onDrawerSlide(drawerView: View?, slideOffset: Float) {
	        }
	
	        override fun onDrawerClosed(drawerView: View?) {
	            supportActionBar!!.setHomeAsUpIndicator(resources.getDrawable(R.drawable.ic_menu_white_24dp))
	        }
	
	        override fun onDrawerOpened(drawerView: View?) {
	            supportActionBar!!.setHomeAsUpIndicator(resources.getDrawable(R.drawable.ic_keyboard_backspace_white_24dp))
	        }
	    }
	    )
	}
	
	// Navigation item click listener Kotlin source code.
	private fun setupNavigationView() {
	
	    navigation.setNavigationItemSelectedListener(object : NavigationView.OnNavigationItemSelectedListener {
	        override fun onNavigationItemSelected(item: MenuItem): Boolean {
	            drawer_layout.closeDrawer(Gravity.START)
	            when (item.itemId) {
	                R.id.nav_all_folders -> {
	                                 }
	                R.id.nav_hidden_folders -> {
	                                 }
	            }
	            return false
	        }
	    })
	}

现在我们将为recyclerview 的AlbumsFolderAdapter.kt添加代码

###为MainActivity RecyclerView Adapter创造的布局文件

	// list_layout.xml
	// layout file for RecyclerView adapter.
	
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:card_view="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content">
	
	    <android.support.v7.widget.CardView
	        android:id="@+id/card_view"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:layout_gravity="center"
	        android:layout_margin="@dimen/five_dp"
	        android:elevation="3dp"
	        card_view:cardCornerRadius="@dimen/zero_dp">
	
	        <RelativeLayout
	            android:layout_width="wrap_content"
	            android:layout_height="match_parent"
	            android:background="?attr/selectableItemBackgroundBorderless">
	
	            <ImageView
	                android:id="@+id/thumbnail"
	                android:layout_width="match_parent"
	                android:layout_height="@dimen/album_cover_height"
	                android:scaleType="centerCrop" />
	
	            <TextView
	                android:id="@+id/title"
	                android:layout_width="wrap_content"
	                android:layout_height="wrap_content"
	                android:layout_below="@id/thumbnail"
	                android:paddingLeft="@dimen/ten_dp"
	                android:paddingRight="@dimen/ten_dp"
	                android:paddingTop="@dimen/ten_dp"
	                android:text="Camera"
	                android:textColor="@color/colorPrimaryText"
	                android:textSize="@dimen/fifteen_dp" />
	
	            <RelativeLayout
	                android:layout_width="wrap_content"
	                android:layout_height="wrap_content"
	                android:layout_below="@id/title">
	
	                <TextView
	                    android:id="@+id/photoCount"
	                    android:layout_width="wrap_content"
	                    android:layout_height="wrap_content"
	                    android:paddingBottom="@dimen/five_dp"
	                    android:paddingLeft="@dimen/ten_dp"
	                    android:paddingRight="@dimen/six_dp"
	                    android:textColor="@color/colorSecondaryText"
	                    android:textSize="@dimen/twelve_dp" />
	            </RelativeLayout>
	        </RelativeLayout>
	    </android.support.v7.widget.CardView>
	</LinearLayout>


现在创建另一个类的名字AlbumFoldersAdapter.kt，是我们的recyclerview适配器

	class AlbumFoldersAdapter(val albumList: ArrayList<Albums>, val context: Context, val options: RequestOptions, val glide: RequestBuilder<Bitmap>, val glideMain: RequestManager, val inOnItemClick: IOnItemClick) : RecyclerView.Adapter<AlbumFoldersAdapter.ViewHolder>() {
	
	 override fun onViewRecycled(holder: ViewHolder?) {
	 if (holder != null) {
	 //glideMain.clear(holder.itemView.thumbnail)
	 // glide.clear(holder.itemView.thumbnail)
	 //Glide.get(context).clearMemory()
	 // holder?.itemView?.thumbnail?.setImageBitmap(null)
	 }// Glide.clear(holder?.itemView?.thumbnail)
	 super.onViewRecycled(holder)
	
	}
	
	override fun onViewDetachedFromWindow(holder: ViewHolder) {
	 if (holder != null) {
	 // glideMain.clear(holder.itemView.thumbnail)
	 //Glide.get(context).clearMemory()
	 // holder?.itemView?.thumbnail?.setImageBitmap(null)
	 
	}
	
	super.onViewDetachedFromWindow(holder)
	 }
	
	override fun getItemCount(): Int {
	 return albumList.size
	 }
	
	override fun onBindViewHolder(holder: ViewHolder?, position: Int) {
	 holder?.bindItems(albumList.get(position), glide, options, inOnItemClick, albumList.get(position).isVideo)
	
	holder?.itemView?.title?.setText(albumList.get(position).folderNames)
	 if (albumList.get(position).isVideo)
	 holder?.itemView?.photoCount?.setText("" + albumList.get(position).imgCount + " videos")
	 else
	 holder?.itemView?.photoCount?.setText("" + albumList.get(position).imgCount + " photos")
	 }
	
	override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
	 val v = LayoutInflater.from(parent.context).inflate(R.layout.list_layout, parent, false)
	 return ViewHolder(v)
	 }
	
	class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
	
	fun bindItems(albumList: Albums, glide: RequestBuilder<Bitmap>, options: RequestOptions, inOnItemClick: IOnItemClick, isVideo: Boolean) {
	 glide.load(albumList.imagePath).apply { options }.thumbnail(0.4f)
	 .into(itemView.thumbnail)
	
	itemView.setOnClickListener(object : View.OnClickListener {
	 override fun onClick(p0: View?) {
	 inOnItemClick.onItemClick(albumList.folderNames, isVideo)
	     }
	  })
	}}}

添加处理recyclerview项单击监听器接口。

	interface IOnItemClick {
	fun onItemClick(position: String, isVideo: Boolean)
	}

www
为AlbumsActivity.kt创造的布局文件

	// activity_album.xml
	
	<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:id="@+id/main_content"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	
	    <android.support.v7.widget.Toolbar
	        android:id="@+id/my_album_toolbar"
	        android:layout_width="match_parent"
	        android:layout_height="?attr/actionBarSize"
	        android:background="?attr/colorPrimary"
	        android:elevation="4dp"
	        app:titleTextColor="@color/colorIcons" />
	
	    <android.support.v7.widget.RecyclerView
	        android:id="@+id/rvAlbumSelected"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:layout_marginTop="?attr/actionBarSize"
	        android:clipToPadding="false" />
	
	</android.support.design.widget.CoordinatorLayout>


	AlbumsActivity.kt
	
	override fun onCreate(savedInstanceState: Bundle?) {
	    super.onCreate(savedInstanceState)
	    setContentView(R.layout.activity_album)
	
	    setSupportActionBar(my_album_toolbar)
	    // Enable the Up button
	    supportActionBar!!.setDisplayHomeAsUpEnabled(true)
	
	    val folder_name = intent.getStringExtra("folder_name")
	    supportActionBar!!.setTitle("" + folder_name)
	    val isVideo = intent.getBooleanExtra("isVideo", false)
	    init_ui_views(folder_name, isVideo)
	
	}
	var adapter: SingleAlbumAdapter? = null
	
	private fun init_ui_views(folderName: String?, isVideo: Boolean?) {
	
	    val options = RequestOptions()
	            .diskCacheStrategy(DiskCacheStrategy.RESOURCE).override(160, 160).skipMemoryCache(true).error(R.drawable.ic_image_unavailable)
	    val glide = Glide.with(this)
	    val builder = glide.asBitmap()
	
	        rvAlbumSelected.layoutManager = GridLayoutManager(this, 2)
	    rvAlbumSelected?.setHasFixedSize(true)
	    adapter = SingleAlbumAdapter(getAllShownImagesPath(this, folderName, isVideo), this, options, builder, glide, this)
	    rvAlbumSelected?.adapter = adapter
	
	    rvAlbumSelected?.addOnScrollListener(object : RecyclerView.OnScrollListener() {
	        override fun onScrolled(recyclerView: RecyclerView?, dx: Int, dy: Int) {
	            super.onScrolled(recyclerView, dx, dy)
	        }
	
	        override fun onScrollStateChanged(recyclerView: RecyclerView?, newState: Int) {
	            super.onScrollStateChanged(recyclerView, newState)
	            when (newState) {
	                RecyclerView.SCROLL_STATE_IDLE -> glide.resumeRequests()
	                AbsListView.OnScrollListener.SCROLL_STATE_TOUCH_SCROLL, AbsListView.OnScrollListener.SCROLL_STATE_FLING -> glide.pauseRequests()
	            }
	        }
	    }
	    )
	}
	
	// Read all images path from specified directory.
	
	private fun getAllShownImagesPath(activity: Activity, folderName: String?, isVideo: Boolean?): MutableList<String> {
	
	    val uri: Uri
	    val cursorBucket: Cursor
	    val column_index_data: Int
	    val listOfAllImages = ArrayList<String>()
	    var absolutePathOfImage: String? = null
	
	    val selectionArgs = arrayOf("%" + folderName + "%")
	
	    uri = android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI
	    val selection = MediaStore.Images.Media.DATA + " like ? "
	
	    val projectionOnlyBucket = arrayOf(MediaStore.MediaColumns.DATA, MediaStore.Images.Media.BUCKET_DISPLAY_NAME)
	
	    cursorBucket = activity.contentResolver.query(uri, projectionOnlyBucket, selection, selectionArgs, null)
	
	    column_index_data = cursorBucket.getColumnIndexOrThrow(MediaStore.MediaColumns.DATA)
	
	    while (cursorBucket.moveToNext()) {
	        absolutePathOfImage = cursorBucket.getString(column_index_data)
	        if (absolutePathOfImage != "" && absolutePathOfImage != null)
	            listOfAllImages.add(absolutePathOfImage)
	    }
	    return listOfAllImages.asReversed()
	}

  Album Activity中Item 里面的监听点击事件

	override fun onItemClick(position: String, isVideo: Boolean) {
	    val intent = Intent(this, PhotoActivity::class.java)
	    intent.putExtra("folder_name", position)
	    startActivity(intent)
	}

SingleAlbumAdapter 是为了 AlbumActivity.kt中列表处理适配器

	class SingleAlbumAdapter(val albumList: MutableList<String>, val context: Context, val options: RequestOptions, val glide: RequestBuilder<Bitmap>, val glideMain: RequestManager, val inOnItemClick: IOnItemClick) : RecyclerView.Adapter<SingleAlbumAdapter.ViewHolder>() {


​	
​	    override fun onViewRecycled(holder: ViewHolder?) {
​	        if (holder != null) {
​	        }
​	        super.onViewRecycled(holder)
​	    }
​	
	    override fun onViewDetachedFromWindow(holder: ViewHolder) {
	        if (holder != null) {
	
	        }
	        super.onViewDetachedFromWindow(holder)
	    }
	
	    override fun getItemCount(): Int {
	        return albumList.size
	    }
	
	    override fun onBindViewHolder(holder: ViewHolder?, position: Int) {
	        holder?.bindItems(albumList.get(position), glide, options, inOnItemClick)
	    }
	
	    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
	        val v = LayoutInflater.from(parent.context).inflate(R.layout.list_single_album_layout, parent, false)
	        return ViewHolder(v)
	    }
	
	    class ViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
	        fun bindItems(albumList: String, glide: RequestBuilder<Bitmap>, options: RequestOptions, inOnItemClick: IOnItemClick) {
	
	            glide.load(albumList).apply { options }.thumbnail(0.4f)
	                    .into(itemView.thumbnail)
	
	            itemView.setOnClickListener(object : View.OnClickListener {
	                override fun onClick(p0: View?) {
	                    inOnItemClick.onItemClick(albumList, false)
	                }
	            })
	        }
	    }
	}

 Single Album Adapter的layout文件

	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:card_view="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content">
	
	    <android.support.v7.widget.CardView
	        android:id="@+id/card_view"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:layout_gravity="center"
	        android:layout_margin="@dimen/five_dp"
	        android:elevation="3dp"
	        card_view:cardCornerRadius="@dimen/zero_dp">
	
	        <RelativeLayout
	            android:layout_width="wrap_content"
	            android:layout_height="match_parent"
	            android:background="?attr/selectableItemBackgroundBorderless">
	
	            <ImageView
	                android:id="@+id/thumbnail"
	                android:layout_width="@dimen/album_cover_height"
	                android:layout_height="@dimen/album_cover_height"
	                android:scaleType="centerCrop" />
	        </RelativeLayout>
	    </android.support.v7.widget.CardView>
	</LinearLayout>

现在让我们在 Detail Activity中添加显示单个图片显示
	
	// SingleActivity.kt
	
	override fun onCreate(savedInstanceState: Bundle?) {
	    super.onCreate(savedInstanceState)
	    setContentView(R.layout.activity_photo)
	
	    setSupportActionBar(toolbar)
	    // Enable the Up button
		// 返回键的 出力
	    supportActionBar!!.setDisplayHomeAsUpEnabled(true)
	    supportActionBar!!.setDisplayShowTitleEnabled(false)
	
	    val folder_name = intent.getStringExtra("folder_name")
	    Glide.with(this).load(folder_name).into(imageFullScreenView)
	
	    Handler().postDelayed(Runnable
	    {
	        if (supportActionBar != null)
	            appbar.animate().translationY(-appbar.bottom.toFloat()).setInterpolator(AccelerateInterpolator()).start()
	        isAppBarShown = false
	    }, 1500)


​	
​	}


	// activity_photo.xml
	
	<?xml version="1.0" encoding="utf-8"?>
	<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:background="@android:color/black"
	    tools:context="com.title_apps.canalpic.screens.detail.PhotoActivity">
	
	    <android.support.design.widget.AppBarLayout
	        android:id="@+id/appbar"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
	        android:background="#93000000">
	
	        <android.support.v7.widget.Toolbar
	            android:id="@+id/toolbar"
	            android:layout_width="match_parent"
	            android:layout_height="?attr/actionBarSize"
	            android:elevation="4dp"
	            app:titleTextColor="@color/colorIcons" />
	    </android.support.design.widget.AppBarLayout>


​	
​	    <ImageView
​	        android:id="@+id/imageFullScreenView"
​	        android:layout_width="wrap_content"
​	        android:layout_height="wrap_content"
​	        android:layout_gravity="center" />
​	
	</android.support.design.widget.CoordinatorLayout>

##结论

你已经有了完整的图像画廊的Android应用程序，你学会了Kotlin的语法，Glide ，recyclerview，navigationview，Drawer Layout在Kotlin和他们的点击的听众，

你已经学会了如何创建和使用Kotlin的数据类，如何使用kotin创建和使用recyclerview及其recyclerview的适配器。

推荐文章

[如何kotlin预约添加本地的广告在Android设备上](http://developine.com/integrate-firebase-advance-native-admob-ads-android-kotlin-tutorial/)




## 结束

1，上面glide 的方法的升级，所有和我的demo里面有些不一样。
2，界面颜色和原作者不一致
3，代码都是从上面抄写下来的，并有一些自己的处理。
4，上面没有写kotlin如何添加 ，可以搜索一下， 也可以用androidstudio 3.0 直接添加 ，勾选就可以了。
5，没有对AndroidManifest.xml中，添加 这个所有activity 获取的添加 以及权限的添加。
6，同比自己翻译要难的，毕竟自己英文不是很好，知道代码也知道大概意思，仔细看英文也明白的差不多了。 



代码请点击[galleryKetlin](https://github.com/smm113522/galleryKetlin)


<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=298 height=52 src="http://music.163.com/outchain/player?type=2&id=482395261&auto=1&height=32"></iframe>