# iRonRouter
a Router on iOS,random transfer / iOS里的路由，实现任意页面跳转

<p>
  
## 使用方法-Usage

参考viewController<br>
<pre><code>
1、#import "iRonRouter.h" <br>
2、[[iRonRouter sharedInstance] getViewControllerWithName:@"controllerName" andParam:@{@"aParam":@"aaa",@"bParam":@"bbb"}];
</code></pre>

## 原理-Principle

1、主要是利用runtime里面的 NSClassFromString（）方法，获取viewController<br>
<pre><code>
    Class class = NSClassFromString(VCNameString);<br>
    UIViewController *controller = [[class alloc] init];<br>
</code></pre>

2、在我们的自定义viewController中，写一个初始化参数的方法 initVCParam: <br>
<pre><code>
    SEL selector = NSSelectorFromString(@"initVCParam:");<br>
    //如果没定义初始化参数方法，直接返回，没必要在往下做设置参数的方法<br>
    if(![controller respondsToSelector:selector]){<br>
        return controller;<br>
    } else {<br>
        NSMutableDictionary *dict = [NSMutableDictionary dictionaryWithDictionary:paramdic];<br>
        // URLKEY  页面唯一路径标识别<br>
        [dict setValue:vcName forKey:@"URLKEY"];<br>
        /* 忽略警告的情况下，执行初始化参数的方法 <br>
         * 因为ARC下，是在编译阶段自动加上释放内存的代码，但是这里的方法是运行时动态加的，所以没加释放内存的代码，有可能内存溢出<br>
         */<br>
        SuppressPerformSelectorLeakWarning( [controller performSelector:selector withObject:dict]);<br>
        return controller;<br>
    }<br>
 </code></pre>

</p>
