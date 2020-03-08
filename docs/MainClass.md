#主类、Mod信息与代理
##mcmod.info
你的Mod开发环境已经构建完成，接下来我们编辑mcmod.info。此时你看到的内容应该是这样的：  
![image.png](https://i.loli.net/2020/03/08/8Oue15tHFPU4mbL.png)（不知道为啥，用代码块不能换行，所以只能用图片了）  

第3行"modid"是这篇介绍所链接的modid。如果这个Mod没有加载，那么这篇介绍将会被忽略（必填）（注意：在之后我们将统一用`ExampleMod`来作为modid）  

第4行"name"是Mod显示的名字（必填）  

第5行"description"是对Mod的简单介绍，一般一两句即可  

第6行"version"是这个Mod的版本，通常以"MC版本-MOD主版本号.API主版本号.次版本号.修订号"来命名版本  

第7行"url"是这个Mod主页的链接  

第8行"updateUrl"有定义但没有实际用途，现在被updateJSON取代了  

第9行"authorList"是Mod作者的列表  

第10行"credits"可以包含任何你想感谢的东西

以上以及更多内容参考自[Forge中文文档](https://mcforge-ko.readthedocs.io/zh/latest/)

---
##Mod主类
现在mcmod.info已经编辑好了，接下来我们编辑主类。  
打开com.example.examplemod.ExampleMod类，可以看到里面有许多东西，我们边编码边来分析。    
---
###什么是@Mod?
> 这是一个注解，它会告诉Forge Mod Loader(FML)这个类(Class)是一个Mod的入口点。它可以包含不同的关于这个mod的元数据(Metadata)。它也同样指示了这个类将会收到 @EventHandler 事件。  

（`@Mod` : 如果一个类被打上这个注解，那么这个类将会成为整个Mod的主类，所有代码都先从这个类开始执行。  
（如果有学过Java肯定知道主类要有一个`main`方法，但是Forge Mod使你不需要这个方法也能直接启动，因为Forge Mod开发过程中真正的主类其实是`GradleStart`和`GradleStartServer`））  
下面是@Mod的属性表：  
| 属性  | 类型 | 默认值 | 简介                                                                                                                                                |
| ------- | ------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| modid   | String | 必填 | 指定mod的唯一标识符。 它必须是小写的，并且将被截断为64个字符的长度。                                                   |
| name    | String | ""     | 指定mod的对用户友好的名称。                                                                                                               |
| version | String | ""     | 指定mod的版本. 它只能是被点分隔开的数字, 应该符合 [语义化版本](https://semver.org/lang/zh-CN/) 标准。 即使useMetadata 设为 true, 也建议在这里设置版本 |  
---
##开始正式编写Mod
首先在`private static Logger logger;`这行代码前面打上`@Instance`注解，括号内填上`ExampleMod.MODID`，  
然后`private static ExampleMod instance`。  
这样就保存了ExampleMod的实例。  

接着创建两个新类，分别是`com.example.examplemod.proxy.ClientProxy`和`com.example.examplemod.proxy.CommonProxy`。  
在`CommonProxy`中添加如下代码:  

     public void preInit(FMLPreInitializationEvent event) {

     }
     public void init(FMLInitializationEvent event) {
     
     }
而在`ClientProxy`中添加如下代码:

    @Override
    public void preInit(FMLPreInitializationEvent event) {
        super.preInit(event);
    }

    @Override
    public void init(FMLInitializationEvent event) {
        super.init(event);
    }
注意：`ClientProxy`继承于`CommonProxy`。

---
你以为做完了吗？No, 我们还有一些事情要做。  
在`private static ExampleMod instance;`的下面，  
`private static Logger logger;`的上面打上`@SidedProxy`注解，并分别指定  
`clientSide`为`com.example.examplemod.proxy.ClientProxy`，  
`serverSide`为`com.example.examplemod.proxy.CommonProxy`。  
你还需要在主类的`preInit`和`init`方法中分别写上`proxy.preInit(event);`和`proxy.init(event);`。  
现在最基本的已经完成了，不过你还可以添加一些东西，比如在`preInit`方法中写上`logger.info("preInit");`，这样在Mod预加载阶段时，你可以在控制台里看到"preInit"。  
每一次做完了一些东西，当你认为可以了之后，可以运行`Minecraft Client`以查看效果。
---
##示例代码
###ExampleMod.java
    package com.example.examplemod;

    import com.example.examplemod.proxy.CommonProxy;
    import net.minecraft.init.Blocks;
    import net.minecraftforge.fml.common.Mod;
    import net.minecraftforge.fml.common.Mod.EventHandler;
    import net.minecraftforge.fml.common.SidedProxy;
    import net.minecraftforge.fml.common.event.FMLInitializationEvent;
    import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;
    import org.apache.logging.log4j.Logger;

    @Mod(modid = ExampleMod.MODID, name = ExampleMod.NAME, version = ExampleMod.VERSION)
    public class ExampleMod
    {
    public static final String MODID = "examplemod";
    public static final String NAME = "Example Mod";
    public static final String VERSION = "1.0";
    
    @Mod.Instance(ExampleMod.MODID)
    private static ExampleMod instance;

    @SidedProxy
            (clientSide = "com.example.examplemod.proxy.ClientProxy",
             serverSide = "com.example.examplemod.proxy.CommonProxy"
            )
    private static CommonProxy proxy;
    private static Logger logger;

    @EventHandler
    public void preInit(FMLPreInitializationEvent event)
    {
        logger = event.getModLog();
        logger.info("preInit");
        proxy.preInit(event);
    }
    @EventHandler
    public void init(FMLInitializationEvent event)
    {
            logger.info("DIRT BLOCK >> {}", Blocks.DIRT.getRegistryName());
            proxy.init(event);
    }
    }
---
###CommonProxy.java
    package com.example.examplemod.proxy;

    import net.minecraftforge.fml.common.event.FMLInitializationEvent;
    import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

    public class CommonProxy {
    public void preInit(FMLPreInitializationEvent event) {
    }
    public void init(FMLInitializationEvent event) {
    }
    }
---
###ClientProxy.java
    package com.example.examplemod.proxy;

    import net.minecraftforge.fml.common.event.FMLInitializationEvent;
    import net.minecraftforge.fml.common.event.FMLPreInitializationEvent;

    public class CommonProxy {
    public void preInit(FMLPreInitializationEvent event) {
    }
    public void init(FMLInitializationEvent event) {
    }
    }

---
下一期我们将添加物品并为其进行添加一些功能