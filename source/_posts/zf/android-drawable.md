# Android 图片资源文件夹drawable用法解析
## 一、drawable文件夹分类：
![资源图片文件夹名称](http://vfile.meituan.net/xgfe/a33182c8a98e7e46587b62a5f61d7c4244105.jpg)
 mipmap:主要放应用程序的icon图片资源

## 二、屏幕密度的获取：

```
float xdpi = getResources().getDisplayMetrics().xdpi;
float ydpi = getResources().getDisplayMetrics().ydpi;
```

## 三、屏幕密度dpi范围分布情况：

|     dpi范围        |      密度       |
| --- | --- |
| 0dpi    ~ 120dpi  |     ldpi        |
| 120dpi ~ 160dpi   | mdpi            |
| 160dpi ~ 240dpi   | hdpi            |
| 240dpi ~ 320dpi   | xhdpi           |
| 320dpi ~ 480dpi   | xxhdpi          |
| 480dpi ~ 640dpi   | xxxhdpi         |


		
## 四、系统是如何选择最适合本手机的图片资源的呢？
#### 原理：
比如当前手机屏幕密度为xhdpi，那么系统首先会到 drawable-xhdpi文件下中找有没有对应的图片，如果有就用这里面的图片，如果没有，系统会自动往更高分辨率的文件夹中找，比如xxhdpi，如果找到了就用它，如果还是没有找到，接着往更高的分辨率文件夹中找，如果直到最后还是没有找到的话，系统会继续往 drawable-nodpi中找（nodip中的图片系统是不会做任何处理的，既不会压缩也不会拉伸），如果还没有找到紧接着会在低一级的文件夹中找，比如drawable-hdpi,直到找到为止。
#### 总结：
图片搜索路径：当前dpi的文件夹->更高一级的文件夹-···->drawable-nodpi->更低一级的文件夹。

## 五、实验验证：
A、如果把图片放到了比手机屏幕密度更高的文件夹下会出现什么情况呢？
B、反之，如果把图片放到比手机屏幕分辨率更低的文件夹下又会出现什么情况？

下面我们针对上面两种情况分别做实验，实验结果如下：
### 实验条件：
手机分辨率：1080x1920
图片分辨率：270x480
屏幕密度：403dpi，处于320dpi~480dpi之间，所以属于xxhdpi范围。

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
   >

    <ImageView
        android:id="@+id/image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/android_logo"
        />
</LinearLayout>
```

### 实验步骤：

#### 1、把图片放到drawable-xxhdpi文件夹中，效果如下：
![1](http://vfile.meituan.net/xgfe/77ab5a85332b977509a77097a0110a1813492.jpg)

原因分析：由于测试手机分辨率是1080x1920像素的，而这张图片的分辨率是270x480像素的，刚好是手机分辨率的四分之一，因此从上图中也可以看出，android_logo图片的宽和高大概都占据了屏幕宽高的四分之一左右，大小基本是比较精准的。

#### 2、把图片放到drawable-xhdpi中，看看会发生什么？效果如下：
![2](http://vfile.meituan.net/xgfe/dc16759d37b26f53c48ba31aa0731e5b15066.jpg)

为什么图片会变大?
原因分析：这是因为，系统在比手机屏幕密度更小的drawable-xhdpi文件夹中找到此张图片，系统会认为drawable-xhdpi里的文件是为了更小分辨率的手机准备的，所以为了适配更大屏幕密度的手机系统会自动把图片做放大操作。
#### 3、我们可以继续验证上面的结论，把图片放到更小密度的文件夹drawable-mdpi中，又会发生什么现象呢？
![3](http://vfile.meituan.net/xgfe/f617703138ee651afa9e3d46c64c3a6520706.jpg)



没错，图片继续被放大了！而且放大的效果更加明显，这是因为把图片置于了更低的屏幕密度的地方，系统会认为存放在这个地方的图片要适配更高的屏幕密度的手机必须放到的合适的比例才能显示正常。

#### 4、转变一下思路，把图片放到比屏幕密度更大的文件夹中，会发生什么现象呢？下面我们把图片放到drawable-xxxhdpi中，请看效果：
![4](http://vfile.meituan.net/xgfe/cf227e85522e0c163ed7ea38834d047c11738.jpg)

原因分析：
同理，因为图片处于更高分辨率的文件夹中，系统会认为这张图片足够大，放在一个较低分辨率的手机中会显示不全，所以系统自动做了压缩，以适配更低分辨率的手机展示。

## 六、总结：
通过以上的demo，我们大致了解了Android系统是如何找到最佳适配图片资源的。现实开发过过程中，UI同学不可能会给每个密度的文件夹都提供一套图片，所以我们尽量让UI提供一套适配较高屏幕密度的图片，比如drawable-xxhdpi。再高其实也没必要，因为市面上xxxhdpi的手机较少，大部分都是属于xxhdpi的。再者，xxhdpi的图片遇到较低分辨率的手机，系统会稍作压缩，其实这对图片质量不会造成太大影响，而且内存开销也会降低。反之，如果UI提供较低的图片资源，一方面系统会把图片做拉伸，从而影响显示效果，另外也会增加内存消耗。

