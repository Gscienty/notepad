##[Microsoft.Owin](https://msdn.microsoft.com/zh-cn/library/microsoft.owin(v=vs.111).aspx)##
该命名空间包含基本HTTP协议内实体结构。
内容包括：

1. 表示Cookie选项。
2. 提供表单数据
3. 表示请求与响应头的包装
4. 提供强类型访问器并包装上下文
5. 表示OWIN中间件
6. 表示OWIN请求实体以及相应实体

##[Microsoft.Owin.Builder](https://msdn.microsoft.com/zh-cn/library/microsoft.owin.builder(v=vs.111).aspx)##
该命名空间包含web应用生成类

核心：   
**AppBuilder类**，实现IAppBuilder接口的标准类  

- 属性：**IDictionary<string, object> Properties**， 包含在处理过程阶段，经组件添加、修改、查看的的所有属性。可理解为环境字典。
- 方法：**IAppBuilder Use(object middleware, params object[] args)**，用于添加中间件节点到OWIN处理过程管道中。Use中指明的中间件的排列顺序为它们添加的顺序。  
参数middleware指明安置在处理管道指定节点处的行为。  
如果参数middleware是一个代理类型（Delegate），其执行触发点为上一个处理节点的“下一个应用”调用方法。如果该代理执行携带参数，则需要将携带的参数填充到args数组之中。  
如果参数middleware是一个类型（Type），其公共构造方法将被上一个处理节点的“下一个应用”调用方法执行。其返回的对象必须包含一个公共的Invoke方法。若该构造方法携带参数，则需要将携带的参数填充到args数组之中。  
其返回值为IAppBuilder本身（方便链式编程）
- 方法：**IAppBuilder New()**，用于构造一个新的IAppBuilder实例，该方法用于在你的处理流程中构建树状结构而非线性管道。新实例将共享原实例中的Properties（环境字典），但是会构造一个新的、空的中间件列表（处理管道）。