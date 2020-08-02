# 干啥的？
## 啥玩意啊
Controller 顾名思义，控制器。  
单独提出来控制器，可能不知道是干啥的。  
但是，机器人，它最主要的功能是啥？   
处理消息。   
无论其他的，什么申请啊，禁言啊，撤回啊，那都是次要的。  
处理消息，是最主要的目的。

相比于其他机器人框架，消息和其他的事件都是事件。  
YuQ，在这里和他们有些不同。  
YuQ 不仅可以通过事件处理消息，还可以通过路由匹配，更精确，更方便的处理消息。  
这就是 Controller 部分存在的意义。  

尽其所能，降低消息处理的难度与复杂度。

## 咋回事啊
很多情况下，我们的机器人都会处理很多很多的指令。  
有些是娱乐性指令，有些可能是功能性指令，有些则可能是群管类的。  

在其他的框架下，我们可能会针对这些指令，写出无数的 if-else 判断。  
这些机械化的东西，真的没有意义，而且性能也很差。  
偶尔心血来潮的时候想写点东西，耐心都被这些重复性的，机械化的，没有丝毫体验的劳动消磨光了。  

## 那咋整啊
YuQ 在这里引入了类似于 Web MVC 的那套理论。  
将消息按一定的内容（空格，换行）进行分割，作为路由，然后匹配到相关方法。

举个例子：  
"群 撤回监控 启用"  
通过空格分割，我们就得到了"群"、"撤回监控"、"启用"，这三个元素。  
就像 Web 中请求的地址 /api/user/info 一样。  
然后我们就得到了一个多级地址，可以理解为 Web 中的 URL，就像：群/撤回监控/启用  
熟悉 RESTful 的同学，可能会熟悉，URL，中，某个内容是可以当做参数的。  
这里的启用应该就会被解析为一个参数。  
那我们的代码就应该是：
```Java
@Path("群")
@GroupController
public class DemoController {

    @Action("撤回监控 {flag}")
    public String recall(boolean flag) {
        if (flag) return "撤回监控启用！";
        return "撤回监控禁用！";
    }

}
```
```Kotlin
@Path("群")
@GroupController
class TestPathGroupController {
    
    @Action("撤回监控 {flag}")
    fun recall(flag: Boolean) = if (switch) "撤回监控启用！" else "撤回监控禁用！"
    
}
```
`@GroupController` 注解，声明了这是一个 Controller，并且是用来处理群消息的。  
`@Path_` 注解，声明了这个 Controller 的一级路由为"群"。  

到 recall 方法，`@Action` 注解声明了这是一个具体处理某个消息的处理器。  
想其他的 Web MVC 一样，YuQ 的 Controller 也支持零个到多个参数，只需要将你需要的参数写下来。  
YuQ 就会将你需要的参数带到你的方法里面。

这里，我们在 `Action` 注解中，用 `{flag}` 指定了一个参数 `flag`。  
它的含义就是 "群 撤回监控 启用" 这段话中的启用，就会被映射到 flag 参数。  
因为我们要求的参数类型是 `boolean`，所以 "启用" 这个内容，就会被 YuQ 智能的转化为一个 `boolean` 类型的值，并带入到您的 Action 中。

然后 recall 返回的一个 String 对象，被 YuQ 接收，并且组成一个 Message，向消息源发送。

就本例子而言，您在群里发送"群 成员监控 启用"。  
机器人就会给您一个回应："撤回监控启用！"这一结果。

通过这种方式，我们可以让自己从繁杂的指令判断中解放出来，专注于功能的开发。
YuQ 竭尽所能的减少一切机械化的重复劳动。