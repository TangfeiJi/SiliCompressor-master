最近公司做海外项目中的活体检测会对用户进行活体检测并上传视频到服务器，存在海外手机和网络的问题就要求对视频进行压缩后上传，其实吧我个人认为视频拍摄完17s才2m也不大，后来找了三方的工具压缩silicompressor。它的优点和其他对比：

FFmpeg 压缩效率低，时间长，使用繁琐，增大apk体积         silicompressor完胜。轻巧占用小压缩速度快前提看你设置的压缩帧率。并且可以压缩图片等等。

Gradle引入方法不建议使用会出现apk文件名重名装不上

1.Gradle

implementation'com.iceteck.silicompressorr:silicompressor:2.2.1'

2.添加相关权限（手机得动态申请权限）

<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

3.使用

使用很简单，直接调用相关方法传入文件路径就能获得压缩之后新文件的路径

<1> 视频压缩（需要在子线程中使用）

压缩视频文件并返回新视频的文件路径（参数传入原视频videoPath和压缩后destinationDirectory存放的文件夹，返回压缩后图片绝对路径）。横屏视频的outWidth宽度   outHeight高度   bitrate比特率（码率）越高数据大  体积越大一般450000

StringfilePath=SiliCompressor.with(Context).compressVideo(videoPath, destinationDirectory，outWidth，outHeight，bitrate);

StringfilePath=SiliCompressor.with(Context).compressVideo(videoPath, destinationDirectory);默认720  480 450000

<2> 图片压缩（需要在子线程中使用）

压缩图像并返回新图像的文件路径

StringfilePath=SiliCompressor.with(Context).compress(imagePath, destinationDirectory);

压缩图像并在删除源图像时返回新图像的文件路径

StringfilePath=SiliCompressor.with(Context).compress(imagePath, destinationDirectory,true);

压缩图像可绘制并返回新图像的文件路径

StringfilePath=SiliCompressor.with(Context).compress(R.drawable.icon);

压缩图像并返回新图像的位图数据

BitmapimageBitmap=SiliCompressor.with(Context).getCompressBitmap(imagePath);

压缩图像并在删除源图像的同时返回新图像的位图数据

BitmapimageBitmap=SiliCompressor.with(Context).getCompressBitmap(imagePath,true);

如果有的项目和你这样引用一致（会出现apk文件名称相同  打包后安装失败  也就是provider命名重名了，改名字得module导入修改）以下是module里的AndroidManifest.xml



Module的导入（apk文件名称相同其实导入后就会取你项目的applicationId 无须设置能关联你项目）

1.把下载好的项目module通过Import module导入到项目中（注意module的sdk版本小于等于主项目版本）

在项目Gradle中加入implementation project(':silicompressor')
2.在工程Gradle中加入

classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.4'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.4.1'
3.增加相关的权限（手机得动态申请权限）

<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>

<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

4.用法和上述的一样记住在子线程中使用

说一说设置压缩的比例：方法都一样

filePath = SiliCompressor.with(mContext).compressVideo(paths[0], paths[1], 720, sizeWidth, 400000);