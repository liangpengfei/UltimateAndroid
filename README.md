UltimateAndroid
==========================

###Version:0.5.0


[![Build Status](https://drone.io/github.com/cymcsg/UltimateAndroid/status.png)](https://drone.io/github.com/cymcsg/UltimateAndroid/latest)
<br>

###UltimateAndroid is a rapid development framework for developing your apps


### 快速开发Android应用框架  UltimateAndroid

[English Introduction](#english_introduction)

[中文简介](#chinese_introduction)

[English Tutorials](https://github.com/cymcsg/UltimateAndroid/blob/master/Tutorials/TableofContents_eng.md)

[中文详细教程](https://github.com/cymcsg/UltimateAndroid/blob/master/Tutorials/TableofContents_zh.md)

[Ui Demo scrennshot](#demo_of_ui)

[Try Demo](#sample)


###Update Ui for Material Design!
### Support Android Studio 1.0
##### You can see javadoc [here](http://blog.marshalchen.com/UltimateAndroid/ultimateandroiddoc/index.html)



#####Welcome to fork and pull request.
My Email is ``cymcsg # gmail.com``


[Update Log](https://github.com/cymcsg/UltimateAndroid/blob/master/updateLog.md)


<h2 ><a name="english_introduction">Introduction</h2>  

---

#####UltimateAndroid is a rapid development framework for developing your apps.UltimateAndroid framework contains many features like View Injection,ORM,Asynchronous Networking and Image Loader,User scenario testing,over 100 Ui effects etc.And there are also many useful features like WebViewUtils,DaoUtils,Https Utils, CryptographyUtils etc.The framework will be added  more feature in the future. 



#####The framework is like flask(a web development framework) which contains some other opensource project like [Butter Knife][1],[Asynchronous Http Client for Android][2], [Universal Image Loader for Android][3] and many other which I said at the end of Readme or in the updatelog.



Up to now,I have only write the demo of most parts of UI modules and View Injection.The demo is something boring,but you can see many kinds of UI modules.The DemoOfUi's screenshots are below,and you can download the apk directly.

[Demo of Ui's screenshot is here.](#demo_of_ui)





Demo Manual
-----



###Quick Setup（Basic Usage）
1.
#####Gradle way(Recommended):

Core framework:
```groovy
repositories {
        jcenter()
    }
dependencies {
    ...
    compile 'com.marshalchen.ultimateandroid:ultimateandroid:0.5.0'
}
```
Ui framework:
```groovy
repositories {
        jcenter()
    }
dependencies {
    ...
    compile 'com.marshalchen.ultimateandroid:ultimateandroidui:0.5.0'
}
```


#####Normal way:
If you use gradle way and you want to edit it at the same time,you can use ``Import Non-Android Studio Project``(need Android Studio 1.0).
If you use normal way,you can pay attention that UltimateAndroid  depends on appcompat, UltimateAndroidUi  depends on UltimateAndroid,and the DemoOfUi is depends on UltimateAndroidUi. However,if you want to use UltimateAndroid without the UIModules,you can just depend on UltimateAndroid and this will make the app more flexible.

2.As the function of View Injection which uses ButterKnife,you should config your IDE before you can compile the project.***Most of  IDEs require additional configuration in order to enable annotation processing for Butter Knife,
or you can see [IntelliJ IDEA Configuration for Butter Knife ][101] or [Eclipse Configuration for butter Knife][102].***

***Notice:The latest version of the framework needs Android Sdk  of Version 21.If you use gradle project, you should use android studio which version should be 1.0.0+***

3.View Injection:

  ``Example:``

``` java
  class ExampleActivity extends Activity {
  @InjectView(R.id.title) TextView title;
  @InjectView(R.id.subtitle) TextView subtitle;
  @InjectView(R.id.footer) TextView footer;

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    ButterKnife.inject(this);
    // TODO Use "injected" views...
  }
}
```

Another use is simplifying the view holder pattern inside of a list adapter.

``` java
public class MyAdapter extends BaseAdapter {
@Override public View getView(int position, View view, ViewGroup parent) {
    ViewHolder holder;
    if (view != null) {
      holder = (ViewHolder) view.getTag();
    } else {
      view = inflater.inflate(R.layout.whatever, parent, false);
      holder = new ViewHolder(view);
      view.setTag(holder);
    }
    holder.name.setText("John Doe");
    // etc...
    return view;
    }
    static class ViewHolder {
    @InjectView(R.id.title) TextView name;
    @InjectView(R.id.job_title) TextView jobTitle;
    public ViewHolder(View view) {
    ButterKnife.inject(this, view);
    }
    }
}
```

4.Asynchronous Network:
  Use asynchronous utils,you do not need to use an addtional Thread to visit network.

``` java
HttpUtilsAsync.get("http://www.google.com", new AsyncHttpResponseHandler() {

  @Override
  public void onStart() {
      // called before request is started
  }

  @Override
  public void onSuccess(int statusCode, Header[] headers, byte[] response) {
      // called when response HTTP status is "200 OK"
  }

  @Override
  public void onFailure(int statusCode, Header[] headers, byte[] errorResponse, Throwable e) {
      // called when response HTTP status is "4XX" (eg. 401, 403, 404)
  }

  @Override
  public void onRetry(int retryNo) {
      // called when request is retried
}
});
```

Post request:

``` java
HttpUtilsAsync.post("http://www.google.com", new AsyncHttpResponseHandler() {

  @Override
  public void onStart() {
      // called before request is started
  }

  @Override
  public void onSuccess(int statusCode, Header[] headers, byte[] response) {
      // called when response HTTP status is "200 OK"
  }

  @Override
  public void onFailure(int statusCode, Header[] headers, byte[] errorResponse, Throwable e) {
      // called when response HTTP status is "4XX" (eg. 401, 403, 404)
  }

  @Override
  public void onRetry(int retryNo) {
      // called when request is retried
}
});
```

5.Display Images:
  If you have already use or extend CommonApplication,you can use like this:
  ```ImageLoader.getInstance().displayImage((imageUri, imageView));```

  Or for some advantage usage:

``` java
imageLoader.displayImage(imageUri, imageView, displayOptions, new ImageLoadingListener()  {
    @Override
    public void onLoadingStarted(String imageUri, View view) {
        ...
    }
    @Override
    public void onLoadingFailed(String imageUri, View view, FailReason failReason) {
        ...
    }
    @Override
    public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
    ...
    }
    @Override
    public void onLoadingCancelled(String imageUri, View view) {
     ...
     }
}, new ImageLoadingProgressListener() {
 @Override
 public void onProgressUpdate(String imageUri, View view, int current, int total) {
 ...
 }
});
```

``` java
  // Load image, decode it to Bitmap and return Bitmap to callback
  ImageSize targetSize = new ImageSize(120, 80); // result Bitmap will be fit to this size
  imageLoader.loadImage(imageUri, targetSize, displayOptions, new   SimpleImageLoadingListener() {
    @Override
    public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
        // Do whatever you want with Bitmap
    }
  });
```

Acceptable URIs examples:

``` java
String imageUri = "http://site.com/image.png"; // from Web
String imageUri = "file:///mnt/sdcard/image.png"; // from SD card
String imageUri = "content://media/external/audio/albumart/13"; // from content provider
String imageUri = "assets://image.png"; // from assets
String imageUri = "drawable://" + R.drawable.image; // from drawables (only images,   non-9patch)
```

**NOTE**: Use drawable:// only if you really need it! Always consider the native way to load drawables - ImageView.setImageResource(...) instead of using of ImageLoader.


6.ORM:

 The Orm Module of the framework contains both [GreenDao](https://github.com/greenrobot/greenDAO) and [ActiveRecord](https://github.com/pardom/ActiveAndroid).

 You can choose either of them freely.


Please set the ides before you run the demo apps.



##UI Modules
* Support animations for Android 2.3
* EnhanceListView for listview which can swipe to dismiss the item
* Shimmer and Titanic to let the textview more amazing
* SmoothProgressBar to let the progress bar like Gmail or Google +
* Flip View for implementing flipping between views as seen in the popular Flipboard application
* PhotoView to help produce an easily usable implementation of a zooming Android ImageView
* PagerSlidingTabStrip and  ViewPagerIndicator to help customing View Pager more easily.
* SwipeBackLayout to help you finish a activity by swipe the screen.
* Material Design
* 
  ``And there are more than 100 UI modules which I do not mention here.``


 <h2 ><a name="chinese_introduction"></a>简介</h2>

---
####框架目前主要包含的功能有View Injection,ORM,异步网络请求和图片加载，自动化脚本测试,磁盘LRU等功能.同时提供了类似于TripleDes、Webview快速设置、Cryptography处理、String处理,Https处理等常用工具类，还有超过100多种UI控件效果。并且这些功能正在逐步增加中。

#####UltimateAndroid框架是如图flask框架（python）那样包含了许多其他的开源项目的框架，比如 [Butter Knife][1],[Asynchronous Http Client for Android][2], [Universal Image Loader for Android][3] 还有许多我在 Readme 或者 updatelog中提到的.

欢迎各种fork与提意见。

如果大家有需要的功能，欢迎随时提意见。

使用DemoOfUI的时候,需要注意UltimateAndroid 依赖 appcompat.UltimateAndroidUi 依赖 UltimateAndroid.DemoOfUi 依赖 UltimateAndroidUi.如果你不需要使用UiModule的话，可以直接依赖UltimateAndroid，这样体积会更纤细。Demo的Apk文件可以直接下载使用.

[UI截图在这里](#demo_of_ui)


#####Welcome to fork.


###QQ交流群：341970175（请注明Android开发）



Demo 使用方法
--------------

Demo依赖于appcompat 和 UltimateAndroid，你可以在IDE或者配置文件里面添加一下依赖。

###快速入门（基础使用）
#####Gradle way(Recommend):

Main framework:
```groovy
repositories {
        jcenter()
    }
dependencies {
    ...
    compile 'com.marshalchen.ultimateandroid:ultimateandroid:0.5.0'
}
```
Ui framework:
```groovy
repositories {
        jcenter()
    }
dependencies {
    ...
    compile 'com.marshalchen.ultimateandroid:ultimateandroidui:0.5.0'
}
```


#####Normal way:

1.传统的library和gradle 的library在不同的文件夹中。如果你使用gradle方式,你可以拷贝 "ultimateandroid.aar" 到你的项目中 ，添加``  compile(name:'ultimateandroid', ext:'aar')``到gradle file中 .如果你使用gradle方式，并且想同时修改框架，可以用android studio的``Import Non-Android Studio Project``(需要Android Studio1.0)。如果使用普通方式，需要注意UltimateAndroid 依赖 appcompat.UltimateAndroidUi 依赖 UltimateAndroid.DemoOfUi 依赖 UltimateAndroidUi.如果你不需要使用UiModule的话，可以直接依赖UltimateAndroid，这样体积会更纤细。

2.Demo的Apk文件可以直接下载使用.由于框架使用了View Injection，**大部分IDE需要开启annotation的编译（使用了Butter Knife），如果不清楚如何开启可以看一下[IntelliJ IDEA Configuration for Butter Knife ][101] or [Eclipse Configuration for butter Knife][102].**

***注意：UltimateAndroid框架需要API21版本的Android SDK来进行编译,如果使用了gradle的project，android studio的版本需要大于1.0.0***

3.视图注入：

 Example:

``` java
class ExampleActivity extends Activity {
@InjectView(R.id.title) TextView title;
@InjectView(R.id.subtitle) TextView subtitle;
@InjectView(R.id.footer) TextView footer;

@Override public void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.simple_activity);
  ButterKnife.inject(this);
  // TODO Use "injected" views...
  }
}
```

Another use is simplifying the view holder pattern inside of a list adapter.

``` java
public class MyAdapter extends BaseAdapter {
@Override public View getView(int position, View view, ViewGroup parent) {
    ViewHolder holder;
    if (view != null) {
      holder = (ViewHolder) view.getTag();
    } else {
      view = inflater.inflate(R.layout.whatever, parent, false);
      holder = new ViewHolder(view);
      view.setTag(holder);
    }
    holder.name.setText("John Doe");
    // etc...
    return view;
    }
    static class ViewHolder {
    @InjectView(R.id.title) TextView name;
    @InjectView(R.id.job_title) TextView jobTitle;
    public ViewHolder(View view) {
    ButterKnife.inject(this, view);
    }
    }
}
```

4.异步网络请求:
  使用异步网络请求工具，你不需要在额外的声明Thread来进行网络请求。

``` java
HttpUtilsAsync.get("http://www.google.com", new AsyncHttpResponseHandler() {

  @Override
  public void onStart() {
      // called before request is started
  }

  @Override
  public void onSuccess(int statusCode, Header[] headers, byte[] response) {
      // called when response HTTP status is "200 OK"
  }

  @Override
  public void onFailure(int statusCode, Header[] headers, byte[] errorResponse, Throwable e) {
      // called when response HTTP status is "4XX" (eg. 401, 403, 404)
  }

  @Override
  public void onRetry(int retryNo) {
      // called when request is retried
}
});
```

Post request:

``` java
HttpUtilsAsync.post("http://www.google.com", new AsyncHttpResponseHandler() {

  @Override
  public void onStart() {
      // called before request is started
  }

  @Override
  public void onSuccess(int statusCode, Header[] headers, byte[] response) {
      // called when response HTTP status is "200 OK"
  }

  @Override
  public void onFailure(int statusCode, Header[] headers, byte[] errorResponse, Throwable e) {
      // called when response HTTP status is "4XX" (eg. 401, 403, 404)
  }

  @Override
  public void onRetry(int retryNo) {
      // called when request is retried
}
});
```

5.显示图片:
  如果你已经使用或继承了CommonApplication，你可以如下使用:
  ```ImageLoader.getInstance().displayImage((imageUri, imageView));```

  或者高级使用:

``` java
imageLoader.displayImage(imageUri, imageView, displayOptions, new ImageLoadingListener()  {
    @Override
    public void onLoadingStarted(String imageUri, View view) {
        ...
    }
    @Override
    public void onLoadingFailed(String imageUri, View view, FailReason failReason) {
        ...
    }
    @Override
    public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
    ...
    }
    @Override
    public void onLoadingCancelled(String imageUri, View view) {
     ...
     }
}, new ImageLoadingProgressListener() {
 @Override
 public void onProgressUpdate(String imageUri, View view, int current, int total) {
 ...
 }
});
```

``` java
// Load image, decode it to Bitmap and return Bitmap to callback
ImageSize targetSize = new ImageSize(120, 80); // result Bitmap will be fit to this size
imageLoader.loadImage(imageUri, targetSize, displayOptions, new   SimpleImageLoadingListener() {
  @Override
  public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
      // Do whatever you want with Bitmap
  }
});
```

可以使用的URI格式:

``` java
String imageUri = "http://site.com/image.png"; // from Web
String imageUri = "file:///mnt/sdcard/image.png"; // from SD card
String imageUri = "content://media/external/audio/albumart/13"; // from content provider
String imageUri = "assets://image.png"; // from assets
String imageUri = "drawable://" + R.drawable.image; // from drawables (only images,   non-9patch)
```

**NOTE**: Use drawable:// only if you really need it! Always consider the native way to load drawables - ImageView.setImageResource(...) instead of using of ImageLoader.


6.ORM:

 Orm模块包括[GreenDao](https://github.com/greenrobot/greenDAO) and [ActiveRecord](https://github.com/pardom/ActiveAndroid).

 你可以自由选择两者中的一个。


##UI 模块
* 支持Android2.3 上面的动画效果
* Listview的滑动删除
* 动态的textview
* 类似Gmail和Google+的进度条
* 类似FlipBoard的翻页效果
* 放大缩写图片的模块
* 影视效果的图片
* 更方便的定制ViewPager
* 滑动后退
* Material Design

  ``还有超过100多种其他模块没有提到``



Warning

Some stuff is just implemented to showcase its functionality and so they are not optimized for performance and they are not done with "best practice" in mind.


<h2 ><a name="demo_of_ui">Some  Demo of Ui:</h2>  

![tutorials2](http://blog.marshalchen.com/images/tutorial2-12.gif)


![tutorials2](http://blog.marshalchen.com/images/tutorial2-1.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-2.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-3.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-4.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-5.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-6.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-7.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-8.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-9.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-10.gif)

![tutorials2](http://blog.marshalchen.com/images/tutorial2-11.gif)


<br>
<h2 ><a name="sample">Sample</h2>  

You can clone the project and compile it yourself (it includes a sample), or you can check it out already compiled at Google Play


[![Google Play](http://developer.android.com/images/brand/en_generic_rgb_wo_45.png)](https://play.google.com/store/apps/details?id=com.marshalchen.common.demoofui)

[Download demo apk directly](http://blog.marshalchen.com/assets/demoui_of_ultimateandroid.apk)

> Notice that it might not be the latest version





The UltimateAndroid use many opensource program and I am very grateful to the author of them.
The opensource program which I use:

1.JakeWharton's [Butter Knife][1] for View Injection.

2.loopj's [Asynchronous Http Client for Android][2].

3.nostra13's [Universal Image Loader for Android][3]

4.greenrobot's [greenDAO][4]

5.pardom's [ActiveAndroid][5]

6.JakeWharton's [DiskLruCache][6]

7.Issacw0ng's [SwipeBackLayout][7]

8.[google-gson][8]

9.RobotiumTech's [robotium][9]

10.JakeWharton's [NineOldAndroids][10]

11.JakeWharton's [Android-ViewPagerIndicator][11]

12.[RippleEffect](https://github.com/traex/RippleEffect)

13.[Material Menu](https://github.com/balysv/material-menu)

14.[FloatingActionButton](https://github.com/futuresimple/android-floating-action-button)      
15.[FloatingActionButtonWithListView](https://github.com/makovkastar/FloatingActionButton)

16.[Android View Animations](https://github.com/daimajia/AndroidViewAnimations)

17.[Trinea](https://github.com/Trinea/android-common)

18.[ListviewAnimations](https://github.com/nhaarman/ListViewAnimations)

19.[PagerSlidingTabStrip](https://github.com/astuetz/PagerSlidingTabStrip)

20.[Rebound](https://github.com/facebook/rebound)

21.[Titanic](https://github.com/RomainPiel/Titanic)

22.[FaceCrop](https://github.com/Todd-Davies/ProgressWheel)  

23.[KenBurns](https://github.com/flavioarfaria/KenBurnsView)  

There are some other projects which I write in [Update Log](https://github.com/cymcsg/UltimateAndroid/blob/master/updateLog.md).


If there's anything I forgot to mention,I would be very appreciated for helping me notice it.





License
--------

    Copyright 2014 Marshal Chen

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


 [1]: https://github.com/JakeWharton/butterknife
 [2]: https://github.com/loopj/android-async-http
 [3]: https://github.com/nostra13/Android-Universal-Image-Loader
 [4]: https://github.com/greenrobot/greenDAO
 [5]: https://github.com/pardom/ActiveAndroid
 [6]: https://github.com/JakeWharton/DiskLruCache
 [7]: https://github.com/Issacw0ng/SwipeBackLayout
 [8]: https://code.google.com/p/google-gson/
 [9]: https://github.com/RobotiumTech/robotium
 [10]:https://github.com/JakeWharton/NineOldAndroids
 [11]:https://github.com/JakeWharton/Android-ViewPagerIndicator
 [12]:https://github.com/RomainPiel/Shimmer-android
 [101]:http://jakewharton.github.io/butterknife/ide-idea.html
 [102]:http://jakewharton.github.io/butterknife/ide-eclipse.html
 [13]:https://github.com/moagrius/TileView/tree/master
=======







