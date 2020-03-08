#构建环境
##1.JDK下载
若想制作Mod，必须先构建好开发环境。关于JDK的下载，就不必多说了，大家应该都会。  
_注意：JDK版本必须为8，否则可能会出问题_  
JDK8安装好后，需要配置JAVA_HOME，配置的方法请自行搜索
---
##2.选择IDE
Java运行时环境已经安装完成，接下来就是选择IDE（Integrated Development Environment，集成开发环境）了。这里推荐使用IntelliJ IDEA。  
![image.png](https://i.loli.net/2020/03/07/IEoZKgqejMTJb9d.png)
选择右边免费的社区版下载并安装。
---
##3.Forge MDK包
IDEA安装好后，该下载Forge MDK包了。点击左侧的1.12.2，点击下载MDK。注意版本号为1.12.2-14.23.5.2847
![image.png](https://i.loli.net/2020/03/07/ZEbznyDOTpMSf6u.png)
包下载好后，将其解压到**All English路径**（如果不是全英文路径你会遇到一些奇怪的问题）
---
##4.构建开发环境
现在，我们可以打开IDEA了。点击Open，选择你解压到的地方。之后IDEA应该会自动构建。Please stand by...  
如果你遇到这种情况：
![image.png](https://i.loli.net/2020/03/07/6nyfhXagzZ4vEDF.png)  
那么请点击Upgrade Gradle wrapper to 4.8.1 version and re-import the project（注意：这个过程特别耗RAM，建议RAM至少4GB的电脑进行开发（当然RAM只有2GB的也行，只不过你会像我一样卡死））  
  
另外也请跟着以下步骤做：按下Ctrl+Alt+Shift+S-->指定Project SDK为1.8  

点击Terminal，输入：  
`gradlew setupDecompWorkspace`  
这个过程非常耗时间，建议大家搜索Lss233's.Mirror();  
**如果出现BUILD FAILED请重新输入该指令**  

`gradlew idea`  
**如果出现BUILD FAILED请重新输入该指令**  

`gradlew genIntellijRuns`  
**如果出现BUILD FAILED请重新输入该指令**  

当出现BUILD SUCCESSFUL的时候，差不多就大功告成了。  
注：BUILD SUCCESSFUL长这个样子：![image.png](https://i.loli.net/2020/03/07/bn4ypjoAvCeKWSw.png)  

`gradlew genIntellijRuns`还是有几率NPE（NullPointerException，空指针异常）的。因此，如果出现这种情况，我们需要手动配置启动方式，否则每次都要输入`gradlew runClient`来进行测试是非常麻烦的。  

要手动配置启动方式，首先需要点击右上角的`Add Configuration`，然后点击`+`号，选择Application。接着跟着图片进行操作。
![image.png](https://i.loli.net/2020/03/07/hFe4kwHmvdY9QGp.png)  
`Minecraft Server`同理
![image.png](https://i.loli.net/2020/03/07/oqO57fkBSIPwRDd.png)  
（注意：在指定工作路径时，要新建文件夹（如果是跟着图片的话））  
到此为止，工作环境已经配置好了，下一期我们将配置主类和Mod信息