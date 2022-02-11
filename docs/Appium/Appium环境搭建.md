## Appium 安装配置

**1. 安装Node.js**
这里给出 Node.js 的下载地址 : `https://nodejs.org/zh-cn/`

下载安装包安装,一直点击下一步就ok啦！     
安装完成后,在终端中输入node -v,显示版本号则表示安装成功    
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429211915201.png)
 
**2. 安装JDK，配置环境变量**         
官网地址：`https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html`    
安装完记得安装路径，下面配置环境变量要用        
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429213510676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429213720933.png) 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429213936828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)    

创建环境变量`%JAVA_HOME%\bin` 加入到Path路径下.     

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429214202554.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)    

在命令行输入`java`和`javac`,出现下面的界面即安装成功.



![在这里插入图片描述](https://img-blog.csdnimg.cn/2020042921433898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429214415140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

**3. 安装SDK**

下载地址：`https://www.androiddevtools.cn/index.html`

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429215647860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

安装

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429225824753.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429225921894.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429230043558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429230245497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429230359133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429230621661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429230932321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

这四个插件是必装的，打钩后点击安装。

如果这个页面没有打开，可以去安装目录下的`SDK Mansger.exe`文件

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429231439733.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

这里会下载会儿，有点慢~

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430001831160.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

这是安装后的目录，下面可以配置环境变量了。

- 添加环境变量
  
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430002129197.png)


- 配置Path
  - 添加`%ANDROID_HOME%\tools`
  - 添加`%ANDROID_HOME%\platform-tools`


  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430002446931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)
- 验证
  
  打开命令行，输入`adb bersion`，显示以下信息，则表示安装成功

```
C:\Users\Administrator>adb version
Android Debug Bridge version 1.0.41
Version 29.0.6-6198805
Installed as D:\software\android sdk\platform-tools\adb.exe
```

**4. 安装 appium**

下载地址：`https://link.zhihu.com/?target=http%3A//appium.io/`

双击.exe文件进行安装

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200430003005594.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FseXNvbl9qbQ==,size_16,color_FFFFFF,t_70)

等待安装完成即可。
