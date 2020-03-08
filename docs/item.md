#添加物品
主类在之前已经配置好了，接下来就该添加一个物品了。
如果你只是想做一个无任何功能的物品，你完全可以直接实例化一个`item`对象。但我们要做的是一个有功能的物品，所以...
##添加第一个物品
我们拿`Bubble`来当例子。  
由于大部分的物品都继承于`Item`类，所以我们也要写一个类来继承它。  
创建`com.example.examplemod.common.item.ItemBubble`类并继承`Item`类。  
接着我们添加一段代码：`private static String NAME = "bubble";`。这段代码定义了一个字符串常量，在后面可以直接调用它。  
然后再添加一段代码：  

    public ItemBubble() {
        this.setRegistryName(name);
        this.setUnlocalizedName(ExampleMod.MODID+"."+name);
        this.setCreativeTab(CreativeTabs.MISC);
    }
`setRegistryName`:设置物品的注册名称  
`setUnlocalizedName`:设置物品的未本地化名称，这个值要记住，翻译时用得到  
`setCreativeTab`:设置物品的创造模式物品栏位置

这时一个物品已经做出来了。但是游戏还不知道这个物品，因此我们需要对这个物品进行注册。

---
##注册物品
要注册物品，首先创建`com.example.examplemod.register.ItemsRegister`类。  
接着，对ItemsRegister类打上`EventBusSubscriber`注解。  
然后，粘贴以下代码：

    public static final Item BUBBLE = new ItemBubble();
    
    public ItemsRegister() {
        MinecraftForge.EVENT_BUS.register(this);
    }

    @SubscribeEvent
    public static void registerItems(RegistryEvent.Register<Item> event) {
        event.getRegistry().registerAll(
                BUBBLE
        );
    }
`public static final Item BUBBLE = new ItemBubble();`:定义一个物品。其它物品也可以类似于这样
`event.getRegistry().registerAll();`：注册所有在括号里的物品  

最后我们只需要在`CommonProxy`类中的的`preInit`方法中`new ItemsRegister()`即可。

现在物品已经成功注册了，运行游戏看一下效果：
![image.png](https://i.loli.net/2020/03/08/Ep8eNIfdmGD9iRK.png)  
已经可以在创造模式物品栏的杂项中找到了。
