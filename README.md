# QxqSDK

提供各种基类、工具类、图片选择器(包含裁剪功能)、Android6.0动态权限管理、利用RxJava+retrofit2+okHttp实现网络请求和文件上传下载、应用检查更新、RecyclerView万能Adapter、第三方分享、登录、NDK图片压缩等功能，极大的提升了开发的效率<br><br>

qxqsdk demo请移步https://github.com/qxq5434/qxqsdkExample/<br><br>

1.3.4版本添加RecyclerView的BaseAdapter、自定义轮播广告控件、第三方登录（QQ和微信）、第三方分享（QQ和微信），详情请参考下面的介绍<br><br>


1.4.1版本升级完成。<br>
主要对sdk代码的设计模式进行优化<br>
升级描述请见https://github.com/qxq5434/QxqSDK/blob/master/1.4.1升级讲解 <br><br>


1.4.3版本升级<br>
修改了一些bug<br>
修改轮播广告第一次加载时会显示默认图标<br><br>


1.4.5版本升级<br>
优化RecyclerView的BaseAdapter<br>
优化QxqDialogUtils<br>
添加控件ColorTrackTextView(变色字体)<br>
修改最小版本只能是大于等于21的bug<br><br>

1.4.6版本更新<br>
添加NDK图片压缩功能,采用JPEG的压缩算法,调用C代码去进行图片压缩<br>
RecyclerView的BaseAdapter添加瀑布流列表，添加数据全部加载完后的显示操作<br><br>

```java
compile 'com.github.qxq.library:qxqsdk:1.4.6'
```

建议升级到最新版本,避免有些方法没有或有些方法过时
-------
如果小伙伴有什么好的建议请发邮件至qxq5434@sina.com，谢谢<br>
------

Android Studio依赖方法
-------

在工程的build.gradle中添加如下代码,一步轻松搞定

```java
compile 'com.github.qxq.library:qxqsdk:1.4.6'
```
基类
-------

* QxqBaseActivity <br>
Activity的基类

* QxqBaseFragment <br>
Fragment的基类

* QxqBaseSwipeBackActivity <br>
带有滑动返回功能的Activity的基类<br>
```java
//在主Activity中继承该类并需要在onCreate中调用一下代码
SwipeBackHelper.getCurrentPage(this)
                .setSwipeBackEnable(false);
SwipeBackHelper.getCurrentPage(this).setDisallowInterceptTouchEvent(true);
```


* QxqBaseMVPActivity <br>
MVP架构的Activity基类

* QxqBaseMVPSwipeBackActivity <br>
带有滑动返回的MVP架构的Activity基类

继承以上基类需实现以下四个抽象方法<br>
```java

/**
 * 初始化布局
 */
protected abstract void initLayout();

/**
 * 初始化数据
 */
protected abstract void initData();

/**
 * 设置控件事件
 */
protected abstract void initListener();

/**
 * 设置界面视图
 */
protected abstract void setContentView();

```
基类自带一个加载方法<br>
```java
//加载
showLoadingDialog(this,"正在加载...");
//注:this必须为Activity

//关闭
if(dlg != null)
   dlg.dismiss();

```
基类自带Activity跳转方法<br>
```java
startAnimActivity(TestActivity.class,new String[]{"参数名1","参数名2"},new String[]{"参数值1","参数值2"});
```
基类自带Activity返回方法<br>
```java
backAnimActivity();
```
基类自带权限管理方法(Activity)
```java
//你程序用到的权限集
String[] mPermissionList = new String[]{
                Manifest.permission.READ_EXTERNAL_STORAGE,
                Manifest.permission.CALL_PHONE};
checkPermission(mPermissionList);
```
1、工具类
-------
* ToastUtil
```java

msg:String类型的Toast文本
strRes:在string.xml中定义的文本的id

showShortToast(String msg);
showShortToast(int strRes);

showLongToast(String msg);
showLongToast(int strRes);

showToast(String msg,int duration);

调用方法
ToastUtil.getInstance().showToast();

```

* LogUtil
```java
调用方法
1、在程序的Application中初始化LogUtil
   LogUtil.init(true);
   //参数：当参数为true时打印log，否则不打印log   当应用在调试阶段设置为true，当应用正式上线后设置为false
   
2、LogUtil.getInstance().i(String tag, String desc);

```


* DialogUtil
```java

调用方法
DialogUtil.getInstance()//注:这里的this必须为activity不能是Context
             .setTitle("")//diaolog标题
             .setMessage("")//diaolog描述
             .setBtn1Text("")//第一个按钮的文本
             .setBtn2Text("")//第二个按钮的文本
             .setSetCancelable(false)//设置按返回键时dialog是否消失
             .setSetCanceledOnTouchOutside(false)//设置点击dialog以外区域时dialog是否消失
             .setBtn1Listener(new DialogInterface.OnClickListener{})//设置第一个按钮的点击回调
             .setBtn2Listener(new DialogInterface.OnClickListener{})//设置第二个按钮的点击回调
             .showDialog(this);

```

* QxqUtils

详情请见https://github.com/qxq5434/QxqSDK/blob/master/QxqUtils


2、图片选择器
-------
 
* 多图选择 <br>
```java
PhotoPickUtil.newInstance().startPhotoPickToList(getActivity(),mPickData);
//传入最多可选择多少张
PhotoPickUtil.newInstance().startPhotoPickToList(getActivity(),9,mPickData);
mPickData:存放选择图片的数组
```
* 单图选择 <br>
```java
PhotoPickUtil.newInstance().startPhotoPickToOne(getActivity());
//传入需要裁切的图片的长宽比
PhotoPickUtil.newInstance().startPhotoPickToOne(getActivity(),2,3);
```
* 回调函数 <br>

* 查看图片 <br>
```java
//传入最大显示多少张,传入选择的图片集合，点击的第几张图片
PhotoPickUtil.newInstance().startPhotoPickToSeeImage(getActivity(),9,mPickData,0);
```


```java
//在程序的onActivityResult中调用如下回调函数
PhotoPickUtil.newInstance().onActivityResult(requestCode,requestCode,data, new PhotoPickResult() {
                @Override 
                public void OneImage(String path) {
                  //path:选择的单张图片的地址
                }
                @Override
                public void ListImage(ArrayList<ImageInfo> arrayList) {
                  //arrayList:选择的图片数组
                }
            });
```

* 注册相关Activity
```xml
<activity
    android:name="com.qxq.photopick.PhotoPickActivity"
    android:configChanges="orientation|keyboardHidden"
    android:screenOrientation="portrait"
    >
</activity>
<activity
    android:name="com.qxq.photopick.PhotoPickDetailActivity"
    android:configChanges="orientation|keyboardHidden"
    android:screenOrientation="portrait"
    />
<activity
    android:name="com.qxq.photopick.CropImageUIActy"
    android:configChanges="orientation|keyboardHidden"
    android:screenOrientation="portrait"
    />

```

QxqHttpUtil
-------

运用RxJava+Retrofit2+OkHttp完成以下功能

>功能

>>get、post网络请求

>>文件上传

>>文件下载


在你的Application中初始化QxqHttpUtil
```java

QxqHttpUtil.getInstance().setBaseParam("your baseurl");
QxqHttpUtil.getInstance().setBaseParam("your baseurl",int timeout);//timeout:访问超时时间设置

```

3、网络请求
-------

* GET请求

>参数
>>url:你需要请求的url地址


>>OnHttpCallBackListener:请求完成后的回调函数


```java
QxqHttpUtil.getInstance().get("your url",
                        new OnHttpCallBackListener() {
                            @Override
                            public void onComplete(String json) {
                                QxqLogUtil.onBind().i("TAG","json..."+json);
                            }

                            @Override
                            public void onError(String error) {
                                QxqLogUtil.onBind().i("TAG","error..."+error);
                            }
                        });
```
* POST请求

>参数
>>url:你需要请求的url地址

>>map:你需要请求的参数集

>>OnHttpCallBackListener:请求完成后的回调函数

```java
QxqHttpUtil.getInstance().post("your url", map, new OnHttpCallBackListener() {
                    @Override
                    public void onComplete(String json) {
                        QxqLogUtil.onBind().i("TAG","json..."+json);
                    }

                    @Override
                    public void onError(String error) {
                        QxqLogUtil.onBind().i("TAG","error..."+error);
                    }
                });
```

* POST请求上传json

>参数

>>url:你需要请求的url地址

>>json:你需要传递的json

>>OnHttpCallBackListener:请求完成后的回调函数

```java
 QxqHttpUtil.getInstance().postToJson("your url", "your json", new OnHttpCallBackListener() {
                    @Override
                    public void onComplete(String json) {
                        QxqLogUtil.onBind().i("TAG","json..."+json);
                    }

                    @Override
                    public void onError(String error) {
                        QxqLogUtil.onBind().i("TAG","error..."+error);
                    }
                });

```


4、文件下载
-------

```java

 QxqHttpUtil.getInstance().downloadBuilder()
            .setDownLoadUrl("your file download url")
            .setDownLoadFilePath("/testDownLoad")//文件下载后存放的文件夹
            .setDownLoadFileName("test.apk")//文件下载后的名字
            .setDownLoadListener(new OnDownLoadListener() {
                @Override
                public void onSuccess() {
                    QxqToastUtil.onBind().showLongToast("下载完成!");
                    dialog.dismiss();
                }
                @Override
                public void onFailure(String error) {
                    QxqToastUtil.onBind().showLongToast("下载失败!"+error);
                }
                @Override
                public void onLoading(long l, long l1) {
                    //文件下载进度
                    int progress = ((int) ((l1 / (float) l) * 100));
                }
            })
            .startDownload();

```

5、文件上传
-------

如果上传的是图片(设置setUpLoadIsImage(true))，sdk会自动根据图片大小进行压缩，做到完全一步到位


* 单个文件上传
```java

 QxqHttpUtil.getInstance().uploadBuilder()
            .setUpLoadFilePath("file path")//需要上传的文件路径
            .setUpLoadFileName("")//服务器的文件参数名
            .setUpLoadUrl("")//上传的地址
            .setUpLoadIsImage(true)//设置上传的文件是否是图片，默认为true
            .setUpLoadListener(new OnUpLoadListener() {
                @Override
                public void onSuccess() {
                    QxqToastUtil.onBind().showLongToast("上传成功!");
                }

                @Override
                public void onFailure(String error) {
                    QxqToastUtil.onBind().showLongToast("上传失败!"+error);
                }

                @Override
                public void onLoading(long total, long progress) {
                    //进度
                    int progress2 = ((int) ((total / (float) progress) * 100));
                }
            })
            .startUploadFile();

```

* 多文件上传
```java

Map<String,File> map = new HashMap<String, File>();
map.put("image0",new File(""));
map.put("image1",new File(""));
map.put("image2",new File(""));

QxqHttpUtil.getInstance().uploadBuilder()
           .setUpLoadFiles(map)//需要上传的文件集
           .setUpLoadUrl("url")//上传的地址
           .setUpLoadIsImage(true)//设置上传的文件是否是图片，默认为true
           .setUpLoadListener(new OnUpLoadListener() {
               @Override
               public void onSuccess() {
                   QxqToastUtil.onBind().showLongToast("上传成功!");
               }

               @Override
               public void onFailure(String error) {
                   QxqToastUtil.onBind().showLongToast("上传失败!"+error);
               }

               @Override
               public void onLoading(long total, long progress) {
                   //进度
                   int progress2 = ((int) ((total / (float) progress) * 100));
               }
           })
           .startUploadFiles();

```

* 文件上传 (from表单格式，适用于注册或者修改个人信息用户头像和用户信息一起上传)
```java
String url = "your upload url";
Map<String ,String> map = new HashMap<String, String>();
map.put("id","123");
map.put("nickname","test");

QxqHttpUtil.getInstance().uploadBuilder()
           .setUpLoadFileName("icon")//服务器图片参数名
           .setUpLoadFilePath("file path")//你需要上传的文件路径
           .setUpLoadMap(map)//你的用户信息
           .setUpLoadUrl(url)//你的上传地址
           .setUpLoadIsImage(true)//设置上传的文件是否是图片，默认为true
           .setUpLoadListener(new OnUpLoadListener() {
               @Override
               public void onSuccess() {
                   QxqToastUtil.onBind().showLongToast("上传成功!");
               }

               @Override
               public void onFailure(String error) {
                   QxqToastUtil.onBind().showLongToast("上传失败!"+error);
               }

               @Override
               public void onLoading(long total, long progress) {
                   //上传的进度
                   int progress2 = ((int) ((total / (float) progress) * 100));
               }
           })
           .startUploadInfo();
```

6、程序检查更新
-------
```java
注:服务器返回的json数据应为如下格式
{
    "data": {
        "version": "1.0",
        "url": "your apk downlod url",
        "message": "your update message"
    }
}

UpdateManager.init().setContext(getActivity())
                        .setUpdateUrl("your url")//你的更新地址
                        .setUpdateFilePath("")//设置下载后apk的存放路径
                        .setUpdateFileName("")//设置下载后apk的名字
                        .getVersion();
```

以下是qxq1.3.1版本更新的类容 <br>
com.github.qxq.library:qxqsdk:1.3.1 <br>


-------

7、RecyclerView的BaseAdapter
-------

新建一个TestAdapter类继承QxqBaseRecyclerViewAdapter <br>
实现父类的几个抽象方法
```java

@Override
public RecyclerView.ViewHolder getHolder(ViewGroup parent,int viewType) {
	if(viewType == ITEM_TYPE_CONTENT1){
		return new TestHolder(mContext, parent,R.layout.item1,onRecyclerViewListener);
	}else if(viewType == ITEM_TYPE_CONTENT2){
		return new Test2Holder(mContext, parent,R.layout.item2,onRecyclerViewListener);
	}
	return null;
}

@Override
public void onMyBindViewHolder(RecyclerView.ViewHolder holder, int position, List payload) {
	//如果payload为空,正常加载
	if(payload.isEmpty()){
		if(holder instanceof TestHolder){
			((TestHolder) holder).bindData(getItem(position),bool);
		}else if(holder instanceof Test2Holder){
			((Test2Holder) holder).bindData(getItem(position));
		}
	}else{//否则,刷新你需要刷新的控件
		TestHolder holder1 = (TestHolder) holder;
		holder1.name.setText("1234");
	}
}
public static final int ITEM_TYPE_CONTENT1 = 1;
public static final int ITEM_TYPE_CONTENT2 = 2;
//根据需要跟换item
@Override
public int getItemType(int position) {
	if(position % 2 == 0){
		return ITEM_TYPE_CONTENT1;
	}else{
		return ITEM_TYPE_CONTENT2;
	}
}
```
新建一个TestHolder类继承QxqBaseViewHolder <br>
在TestHolder里面实现你的业务逻辑 <br>

详情请见QxqSDKExample <br>

```java

adapter = new TestAdapter(getActivity());
adapter.setIsLoadMore(true);//设置是否需要加载更多
adapter.setPageCount(10);//设置你每页需要显示多少行
adapter.setIsShowNoDataMessage(true);//是否显示数据加载完后“没有更多数据”的文本
adapter.setNoDataTextColor(R.color.green);//数据加载完后显示文本的字体颜色
adapter.setDataLoadOver("没有更多数据了!");//数据加载完后显示的文本文字
adapter.setOnDataLoadOverListener(new OnDataLoadOverListener() {//点击数据加载完成后文本的回调
	@Override
	public void loadOver() {
		QxqToastUtil.getInstance(getActivity()).showLongToast("哈哈哈哈h");
	}
});
adapter.setOnRecyclerViewListener(onItemClickListener);
//如果你需要用RecyclerView实现ListView的效果
LinearLayoutManager layoutManager = new LinearLayoutManager(getActivity());
//如果你需要用RecyclerView实现GridView的效果
GridLayoutManager layoutManager = new GridLayoutManager(getActivity(), 3);
//如果你需要用RecyclerView实现瀑布流的效果
StaggeredGridLayoutManager layoutManager = new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL);
adapter.setLoadMore(mRecyclerView, layoutManager, new QxqBaseRecyclerViewAdapter.RecyclerViewLoadMoreCallBack() {
    @Override
    public void loadMore() {
        page ++;
        //请求数据
    }
});
mRecyclerView.setLayoutManager(layoutManager);
mRecyclerView.setHasFixedSize(true);
mRecyclerView.setItemAnimator(new DefaultItemAnimator());
mRecyclerView.addItemDecoration(new DividerItemDecoration(getActivity(), LinearLayoutManager.VERTICAL,R.color.index_item_grey));//LinearLayoutManager调用
SpacesItemDecoration decoration=new SpacesItemDecoration(16);//GridLayoutManager和StaggeredGridLayoutManager调用
mRecyclerView.addItemDecoration(decoration);
mRecyclerView.setAdapter(adapter);

adapter.update(list,page);

```


8、轮播广告
-------

该控件分为Activit和Fragment两种

```xml
//fragment
<fragment
	android:layout_width="match_parent"
	android:layout_height="250dp"
	android:id="@+id/fragment_cycle_viewpager_content"
	android:name="com.qxq.view_pager.QxqFragmentCycleViewPager"
	/>
//activity
<fragment
	android:layout_width="match_parent"
	android:layout_height="250dp"
	android:id="@+id/fragment_cycle_viewpager_content"
	android:name="com.qxq.view_pager.QxqActivityCycleViewPager"
	/>
```
```java

QxqActivityCycleViewPager cycleViewPager = (QxqActivityCycleViewPager)getFragmentManager().findFragmentById(R.id.fragment_cycle_viewpager_content);
//QxqFragmentCycleViewPager cycleViewPager = (QxqFragmentCycleViewPager)getChildFragmentManager().findFragmentById(R.id.fragment_cycle_viewpager_content);
QxqViewPagerUtil.onBind()
		.with(cycleViewPager)//添加控件
		.setIsActivity(true)//设置当前是不是在Activity
		.setWheel(true)//设置是否轮播
		.setTime(3000)//设置轮播切换时间(毫秒)
		.setPointImageRes(R.mipmap.icon_point,R.mipmap.icon_point_pre)//设置切换的点的资源图片
		.setData(datas)//设置数据
		.setImageCycleViewListener(new ImageCycleViewListener() {
			@Override
			public void onImageClick(int postion, View imageView) {
				QxqToastUtil.onBind().showLongToast("第"+(postion + 1)+"个");
			}
		})
		.show(getApplicationContext());
```


9、第三方登录（QQ和微信）
-------
```java
//在程序的Application中添加如下代码
QxqLoginUtil.init(this);
QxqLoginUtil.setQQ("1104911867","vZTng1q5fKbLQI08");
QxqLoginUtil.setWeiXin("wx023daf39180f3578","d4624c36b6795d1d99dcf0547af5443d");
```

* QQ登录

```java
 QxqLoginUtil.onBind(getActivity()).loginToQQ( new QxqLoginCallBack() {
                    @Override
                    public void onError(String error) {

                    }

                    @Override
                    public void onCancel(String str) {

                    }

                    @Override
                    public void onStart(String str) {

                    }

                    @Override
                    public void onComplete(String json) {
                        QxqLogUtil.onBind().i("json",json);
                    }
                });
				
				
//需要实现onActivityResult
QxqLoginUtil.onBind(this).onActivityResult(requestCode,resultCode,data);
//注:如果是在Fragment中调用的登录功能，请在fragment的容器Activity中实现onActivityResult
```


* 微信登录
```java
QxqLoginUtil.onBind(getActivity()).loginToWeiXin(new QxqLoginCallBack() {
                    @Override
                    public void onError(String error) {
                        QxqLogUtil.onBind().i("ERROR",error+"===");
                    }

                    @Override
                    public void onCancel(String str) {

                    }

                    @Override
                    public void onStart(String str) {

                    }

                    @Override
                    public void onComplete(String json) {
                        QxqLogUtil.onBind().i("json",json+"---");
                    }
                });
//请在自己程序中包名目录下创建wxapi文件夹，新建一个名为WXEntryActivity的activity继承WXCallbackActivity。这里注意一定是包名路径下，例如我的包名是com.qxqsdk（com.qxqsdk.wxapi）
```

在onDestroy方法中调用QxqLoginShareUtil.onBind(getActivity()).release();释放资源

10、第三方分享
-------

注：第三方分享和第三方登录的配置方法一样，如已经配置了第三方登录分享就不用重新配置

```java
//分享纯文字
QxqLoginShareUtil.onBind(getActivity()).shareToText(SHARE_TYPE.WEIXIN_CIRCLE, "content",callBack);
//分享纯图片
//图片格式支持网页地址、资源文件、bitmap
QxqLoginShareUtil.onBind(getActivity()).shareToImage(SHARE_TYPE.WEIXIN_CIRCLE, "content",R.drawable.ic_launcher,callBack);
//分享网页
QxqLoginShareUtil.onBind(getActivity()).shareToUrl(SHARE_TYPE.WEIXIN_CIRCLE,"http://www.baidu.com/","title" ,"content",R.drawable.ic_launcher,callBack);

QxqLoginShareCallBack callBack = new QxqLoginShareCallBack() {
	@Override
	public void onError(String error) {
		QxqToastUtil.onBind().showLongToast(error);
	}

	@Override
	public void onCancel(String str) {
		QxqToastUtil.onBind().showLongToast(str);
	}

	@Override
	public void onStart(String str) {
		QxqToastUtil.onBind().showLongToast(str);
	}

	@Override
	public void onComplete(String json) {
		QxqToastUtil.onBind().showLongToast(json);
	}
};

```

11、QxqDialogUtil添加showProgressDialog()方法
-------
```java
//显示进度条dialog
ProgressDialog dialog = QxqDialogUtil.getInstance().progressDialog().setMessage("正在获取用户信息...").showProgressDialog(this);
//关闭dialog
dialog.dismiss();
```


12、优化RecyclerView的BaseAdapter
-------
1、添加了刷新某一个item的update(int position)方法
2、在QxqBaseRecyclerViewAdapter中添加一个抽象方法onMyBindViewHolder(RecyclerView.ViewHolder holder, int position,List payload),
TestAdapter需要实现,主要方便使用者继承后的扩展<br>
3、可以进行局部刷新,notifyItemChanged(position,"update");
```java
//1、进行局部刷新 notifyItemChanged(position,"update");
//2、不进行局部刷新 notifyItemChanged(position);
/**
*
* payload 主要用在局部刷新上面
*
*/
@Override
public void onMyBindViewHolder(RecyclerView.ViewHolder holder, int position, List payload) {
	if(payload.isEmpty()){//如果payload为空，则正常刷新
		if(holder instanceof TestHolder){
			((TestHolder) holder).bindData(getItem(position));
		}
	}else{//如果payload不等于空,进行局部刷新
		TestHolder holder1 = (TestHolder) holder;
		holder1.name.setText("1234");
	}
}

```
13、变色字体
-------
```xml
  <com.qxq.views.ColorTrackTextView
            android:id="@+id/text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="测试测试"
            />
```
```java
//设置朝向
mCttv.setDirection(ColorTrackTextView.Direction.DIRECTION_RIGHT);
//设置变色进度（0-1）
mCttv.setCurrentProgress(1 - progress);
```

14、图片压缩
-------
```java
//请在异步中进行图片压缩处理
/**
 * 本地方法 JNI处理图片
 *
 * @param bitmap
 *            bitmap
 * @param width
 *            宽度
 * @param height
 *            高度
 * @param quality
 *            图片质量 100表示不变 越小就压缩越严重
 * @param fileName
 *            文件路径的byte数组
 * @param optimize
 *            是否采用哈弗曼表数据计算
 * @return "0"失败, "1"成功
 */
String codeString = ImageUtils.compressBitmap(bitmap, bitmap.getWidth(), bitmap.getHeight(), 40, afterCompressImgFile.getAbsolutePath().getBytes(), true);
                       
/**
 * 尺寸和质量压缩 没有oom处理
 *
 * @param compressFilepath
 *            原始文件路径
 * @param filePath
 *            目标文件路径
 */					   
ImageUtils.compressBitmap(tempCompressImgPath, afterCompressImgFile.getPath());

/**
 * 尺寸和质量压缩 没有oom处理
 *
 * @param bitmap
 *            bitmap
 * @param filePath
 *            文件路径
 */
ImageUtils.compressBitmap(bimap, afterCompressImgFile.getPath());
```

