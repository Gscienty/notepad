#Katana 概览#
*本文档节选并主观翻译自asp.net中的一篇文章，原链接：[An Overview Of Project Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana)*

运用ASP.NET平台已经超过十年了，十多年里，利用ASP.NET平台开发的Web站点和服务数不胜数。随着Web应用开发策略的变革，ASP.NET也随即推出了与之相匹配的开发技术，例如ASP.NET MVC 和 Wab API。当下，Web应用开发正处于云计算的革命浪潮之中，Katana项目提供给了ASP.NET开发者一系列的
基础开发组件。能够使项目变得**灵活**、**轻便**、**轻量**。Katana可以优化您的ASP.NET应用。


###对传统模式的挑战###
现在的体系是成熟的，功能丰富的，和具备完善的程序开发模型，然而，这些显著丰富的特点带来了显著的挑战。  
首先，平台是一个紧密的整体，与System.Web.dll高度耦合在一起。（例如在Web forms平台下的HTTP核心对象）。  
其次，ASP.NET包含在巨大的.NET Framework的一部分之中，这就意味着在两次发行期间需要间隔很长一段时间。在这期间Web开发模式可能会有翻天覆地的变化，这会导致ASP.NET很难紧跟Web开发的潮流。  
最后，System.Web.dll很少有能与之搭配的托管软件，仅有IIS能够与之搭配。

###革命的步伐： ASP.NET MVC 和 ASP.NET Web API###

Web开发发生了太多的变化。目前，Web开发逐渐倾向于将Web应用开发成一系列小的模块、倾向于组件化，而不是一个庞大的体系平台。很多组件的发布更新频率也变得越来越快。
要使的ASP.NET紧跟Web的步伐，就得对平台的要求越来越小，降低耦合度，使功能模块化，而不是全部基于一个庞大丰富的平台。因此ASP.NET开发团队进行了一系列的变革，使得ASP.NET更亲和于可添加的Web组件而不是一个单一的平台框架。

一个最早的变化就是被人熟知的MVC设计模式，使得Web开发框架变得像Ruby on Rails。这种风格能够在在能够保持标记与业务逻辑的分离情况下，充分给予了开发者在她的开发应用中视图标记的控制权。

另一个在Web应用开发中主要的变化是从动态的、服务端生成的Web页面过渡到静态初始标记与静态标记通过客户端脚本在页面中生成动态节点，利用Ajax请求Web API的方式实现（例如AngularJS）