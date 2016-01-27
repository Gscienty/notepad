#Katana 概览#
*本文档节选并主观翻译自asp.net中的一篇文章，原链接：[An Overview Of Project Katana](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana)*

运用ASP.NET平台已经超过十年了，十多年里，利用ASP.NET平台开发的Web站点和服务数不胜数。随着Web应用开发策略的变革，ASP.NET也随即推出了与之相匹配的开发技术，例如ASP.NET MVC 和 Wab API。当下，Web应用开发正处于云计算的革命浪潮之中，Katana项目提供给了ASP.NET开发者一系列的
基础开发组件。能够使项目变得**灵活**、**轻便**、**轻量**。Katana可以优化您的ASP.NET应用。


##对传统模式的挑战##
现在的体系是成熟的，功能丰富的，和具备完善的程序开发模型，然而，这些显著丰富的特点带来了显著的挑战。  
首先，平台是一个紧密的整体，与System.Web.dll高度耦合在一起。（例如在Web forms平台下的HTTP核心对象）。  
其次，ASP.NET包含在巨大的.NET Framework的一部分之中，这就意味着在两次发行期间需要间隔很长一段时间。在这期间Web开发模式可能会有翻天覆地的变化，这会导致ASP.NET很难紧跟Web开发的潮流