---
layout: post
title: 再见 Rails
---

林语堂说，读书就像找情人，不喜欢了就换。我今天要讲的也是一个跟曾经的最爱的技术 Rails 分手的故事，而且会是一种非常没良心的决绝，老话说：提起裤子不认人！

### 时代变了

其实我公开叛逃 Rails ，不是在本文啦，2015年9月 [ruby-china.org 的一个帖子的回复](https://ruby-china.org/topics/27500) 里面，我就明确表过态了：

>从今年开始就再也没有碰过 Rails 了，感觉 Rails 已经老了，该退休了。十年前 REST http request/response 的模式看起来那么美，但是现在是实时应用的时代了

>35楼 @lgn21st 对这个是完全赞同的，但是“实时性”的部分应该是内嵌到一个框架的最深处的，成为框架的默认。所以，过时的不仅仅是 Rails ，还有 Ajax/Pjax/turbolink 等这些所有基于非实时框架做成的实时 Hack 。

>世界变了...到了今天，web 应用应该是”无刷新为默认“的，也就是”类原生应用为默认“，道理很简单：网速上来了，用户体验要在平滑性上来拼了。Rails 从基本架构上不是为这个时代而生，所以改进也没有必要了，慢慢淡出娱乐圈才是正道。

>说起”编程快乐“，这个依然是最重要的。但是用用 meteor 就会发现，Rails 输掉的同时是两个极端：易用性和强大性。


### 社区活跃度几乎是一切

框架没有了足够的社区就没有了足够的讨论，也就是说您有问题卡住了，也根本搜不到答案了。

看看 Github 上 rails 的  star 数目，几个月都不怎么动了。看看 nodejs 社区这些小老虎们  react meteor ... 。您可能会说人民群众都是跟风，好，再看看 Rails 社区大牛们的言论：

- Github 的作者 Tom （ Talk at Startup School 2012 ） ： [I don't like  Rails ，you can tell the world](https://youtu.be/mGTpU5XUAA8?t=188) 。

- [ Why I wouldn’ t use rails for a new company](https://ruby-china.org/topics/27500)

- [http://podcast.crater.io/](http://podcast.crater.io/) 作者 Josh Owens  十年 Rails 开发者，Peter 很早就是他的粉丝了，现在也是全职做 meteor 了。

- [Jeffery Way](http://laracasts.com) 那天在 podcast 里提到，在美国圈子里一提起 Rails 很多人都是：恶心。但是 Jeffery 还是替 Rails 说话的：Rails changed everything! 

### 拥抱新技术得大于失，很值

Stay on cutting edge 的成本是有的，因为要踩坑。但是作为技术人员收获是远大于损失的。

成熟稳定的这些好处大家是公认的。但是新框架之所以流行，就是因为新技术中包含革命性的内容，这些内容是不可能通过老技术迭代达成的。Rails/PHP/Django 这些基于老的 request/response 架构的框架，最近手忙脚乱的往实时性方面强化，但是由于底层架构限制，越改越乱套。

### 总结

Rails 该隐退了，谁会是未来，不敢说，但是我现在押宝在 Meteor + React 。
