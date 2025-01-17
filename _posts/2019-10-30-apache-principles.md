---
layout: article
title: Apache架构师带给你的原则
---
本文作者是Srinath Perera，Apache架构师，是Axis2, WSO2 Stream Processor的设计师，看看他带给我们的原则，特别喜欢这些老程序员
经过了岁月历练之后带给我们的尊尊教诲.
     
     
[原文](https://medium.com/hackernoon/first-do-no-harm-30-principles-that-helped-me-avoid-fly-by-architecture-reviews-e8952ac632a)



我把最后的总结提前了：

```
Finally, let me talk about something that I have changed my mind about over time. In an ideal world, 
a platform must be composed of orthogonal components — components where each handles one aspect (e.g., security, messaging, registry, mediation, analytics). 
A system built with such features would be optimal.

Unfortunately, it is hard to get to that state. It is even hard to stay there. It could be a mistake to enforce this rigidly, 
especially at the initial state of new features where simple features can cascade into big changes because we try to make everything orthogonal. 
Sometimes we find that the feature we added was not useful after all and then all the additional work is spent for nothing. Finally, 
if this lead to negotiations between multiple teams, the feature might never get done.
With hindsight, now I am willing to live with duplication when trying to remove it lead to significant complexity. The cure can be worse than the disease.

```


## Basic


原则1：KISS(Keep it simple,sutpid) 和保持每件事情都尽可能的简单。用最简单的解决方案来解决问题。

 

原则2：YAGNI(You aren’t gonna need it)-不要去搞一些不需要的东西，需要的时候再搞吧。


 
原则3：爬，走，跑。换句话说就是先保证跑通，然后再优化变得更好，然后继续优化让其变得伟大。迭代着去做事情，敏捷开发的思路。对于每个功能点，创建里程碑（最大两周），然后去迭代。



原则4：创建稳定、高质量的产品的唯一方法就是自动化测试。所有的都可以自动化，当你设计时，不妨想想这一点。

 
原则5：时刻要想投入产出比（ROI）。就是划得来不。


原则6：了解你的用户，然后基于此来平衡你需要做哪些事情。不要花了几个月时间做了一个devops用户界面，最后你发现那些人只喜欢命令行。此原则是原则5的一个具体表现。


原则7：设计和测试一个功能得尽可能的独立。当你做设计时，应该想想这一条。从长远来看这能给你解决很多问题，否则你的功能只能等待系统其他所有的功能都就绪了才能测试，这显然很不好。有了这个原则， 你的版本将会更加的顺畅。


原则8：不要搞花哨的。我们都喜欢高端炫酷的设计。最后我们搞了很多功能和解决方案到我们的架构中，然后这些东西根本不会被用到。


## Choosing Features

原则9：不可能预测到用户将会如何使用我们的产品。所以要拥抱MVP（Minimal Viable Product），最小可运行版本。这个观点主要思想就是你挑几个很少的使用场景，然后把它搞出来，然后发布上线让用户使用，然后基于体验和用户反馈再决定下一步要做什么。


原则10：尽可能的做较少的功能。当有疑问的时候，就不要去做，甚至干掉。很多功能从来不会被使用。最多留个扩展点就够了。
 

原则11：等到有人提出再说（除非是影响核心流程，否则就等到需要的时候再去做）。


原则12：有时候你要有勇气和客户说不。这时候你需要找到一个更好的解决方案来去解决。记住亨利福特曾经说过的 ：”如果我问人们他们需要什么，他们会说我需要一匹速度更快的马”。记住：你是那个专家，你要去引导和领导。要去做正确的事情，而不是流行的事情。最终用户会感谢你为他们提供了汽车。

## Server Design and Concurrency

原则13：要知道一个server是如何运行的，从硬件到操作系统，直到编程语言。优化IO调用的数量是你通往最好架构的首选之路。

 

原则14：要了解Amdhal同步定律。在线程之间共享可变数据会让你的程序变慢。只在必要的时候才去使用并发的数据结构，只在必须使用同步（synchronization）的时候才去使用同步。如果要用锁，也要确保尽可能少的时间去hold住锁。如果要在加锁后做一些事情，要确保自己在锁内会做哪些事情。

 

原则15：如果你的设计是一个无阻塞且事件驱动的架构，那么千万不要阻塞线程或者在这些线程中做一些IO操作，如果你做了，你的系统会慢的像骡子一样。


## Distributed Systems

原则16：无状态的系统的是可扩展的和直接的。任何时候都要考虑这一点，不要搞个不可扩展的，有状态的东东出来，这是起码的。


原则17：保证消息只被传递一次，不管失败，这很难，除非你要在客户端和服务端都做控制。试着让你的系统更轻便（使用原则18）。你要知道大部分的承诺exactly-once-delivery的系统都是做了精简的。


原则18：实现一个操作尽可能的幂等。这样的话就比较好恢复，而且你还处于至少一次传递（at least once delivery）的状态。


原则19：知道CAP理论。可扩展的事务（分布式事务）是很难的。如果可能的的话，尽可能的使用补偿机制。RDBMS事务是无法扩展的。

原则20：分布式一致性无法扩展，也无法进行组通信，也无法进行集群范围内的可靠通信。理想情况下最大的节点限制为8个节点。


原则21：在分布式系统中，你永远无法避免延迟和失败。


## User Experience

原则22：要了解你的用户和清楚他们的目标。他们是新手、专家还是偶然的用户？他们了解计算机科学的程度。极客喜欢扩展点，开发者喜欢示例和脚本，而普通人则喜欢UI。

 

原则23：最好的产品是不需要产品手册的。

 

原则24：当你无法在两个选择中做决定的时候，请不要直接把这个问题通过提供配置选项的方式传递给用户。这样只能让用户更加的发懵。如果连你这个专家都无法选择的情况下，交给一个比你了解的还少的人这样合适吗？最好的做法的是每次都找到一个可行的选项；次好的做法是自动的给出选项，第三好的做法是增加一个配置参数，然后设置一个合理的默认值。

 

原则25：总是要为配置设置一个合理的默认值。

 

原则26：设计不良的配置会造成一些困扰。应该总是为配置提供一些示例值。

 

原则27：配置值必须是用户能够理解和直接填写的。比如：不能让用户填写最大缓存条目的数量，而是应该让用户填写可被用于缓存的最大内存。

 

原则28：如果输入了未知的配置要抛出错误。永远不要悄悄的忽略。悄悄的忽略配置错误往往是找bug花了数小时的罪魁祸首。


## Hard Problems

原则29：梦想着新的编程语言就会变得简单和明了，但往往要想真正掌握会很难。不要轻易的去换编程语言。


原则30：复杂的拖拉拽的界面是艰难的，不要去尝试这样的效果，除非你准备好了10人年的团队。