---
layout: post
title: "断言与前置条件"
description: ""
categories: ["Swift"]
tags: ['swift']
---
{% include JB/setup %}


<p>断言与前置条件（Assertions & Preconditions）跟其它语言类似，都是用于程序执行的条件判断，当条件判断为false时，则后续代码将不再执行，从而抛出异常，以保证后续代码的执行安全。
二者完全是实现相同功能，只是应用场景有所不同而以，assertions用于开发阶段使用，也就是说使用xcode编译时，如果编译设置为Debug版本，则assertions会被正常编入该版本，如果设置为Release版本，则assertions语句将不会编入。xcode默认设置为联机调试过程为Debug版本，发布为Release版本。</p>
<!--more-->
我们先看一下assertions的定义：
{% highlight swift %}
func assert(condition: @autoclosure () -> Bool, _ message: @autoclosure () -> String = default, file: StaticString = default, line: UWord = default){
      #if DEBUG
          if !condition() {
              println("assertion failed at \(file):\(line): \(message)")
              abort()
          }
      #endif
}
{% endhighlight %}
从中，我们可以看到其实assert仅仅是添加了一个DEBUG宏进行了编译条件的判断而以。第一个参数是条件，第二个参数是一个可选的描述字符串，后面两个参数默认是调用者的上下文位置.

> 贴心提示：
>  StaticString是一种内部使用的String,其实字符串类型有几种类型，包括：  
>  String ; Character Unicode 字符 ;   
>  UnicodeScalar 相当于 C 中的 wchar_t ;  
>  CString 用于表示 C 中的 const char * ;   
>  StaticString 静态字符串，内部使用，例如 fatalError  

搞明白了什么是断言与前置条件，我们来实际应用一下：
{% highlight swift %}

let x = 1
assert(x<0)
println("assert passed,\(x) is less than zero")

precondition(x < -9 )
println("precondition passwd ,\(x) less than -9")

{% endhighlight %}
> * 当前编译版本为DEBUG版本时，可以看到结果会输出： <span style="color: red;">assertion failed: : .......</span>
> * 当前编译版本为Release版本时，输出为：<span style="color: red;">assert passed,1 is less than zero</span> 
> * 切换方式：Product -> Scheme -> Edit Scheme -> Run  -> info -> Build Configuration

综上，那什么时候使用Assert与precondition呢？

1. 当使用一个不确定是否越界的数组下标时；
2. 当接收到的值无法确定是否符合要求时；
3。 由于Swift提供了Option选项，但是后续的代码需要其值不能为nil时；

参考：  
本文主要用于一个知识的归纳总结，过程中可能会引用到其它地方的文字或代码，如有侵权请及时联系我，在此对写作过程中参考了的文章作者表示感谢！ 

> * http://www.raywenderlich.com/82572/swift-generics-tutorial
> * http://numbbbbb.gitbooks.io/-the-swift-programming-language-/content/