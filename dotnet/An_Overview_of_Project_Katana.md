#An Overview of Project Katana#

The net result was a mature, feature-rich runtime and developer programming model.
Firstly, the framework was monolithic, with logically disparate units of functionality being tightly coupled in the same System.Web.dll assembly.
Sencondly, ASP.NET was was included as a part of the large .NET Framework, which meant that the time between releases was on the order of years.

built ASP.NET Web API such that it had no dependencies on any of the core framework types found in System.Web.dll, This enabled two things:

1. it meant that ASP.NET Web API could evolve in a completely self-contained manner.
2. because there were no external dependencies to System.Web.dll, and therefore no dependencies to IIS, ASP.NET Web API included th capability to run in a custom host(for example, a console application, Windows service, etc.)

A modern Web application generally supports static file serving, dynamic page generation, Web API, and more recently real-time/push notifications. Expecting that each of these services should be run and managed independently was simply not realistic.

##The Open Web Interface for .NET##

* New components could be more easily developed and consumed
* Applications could be more easily ported between hosts and potentially entire platform/operating systems.

The resulting abstraction consists of two core elements:
1. The environment dictionary. This data structure is responsible for storing all of the state necessary for processing an HTTP request and response, as well as any relevant server state.
The environment dictionary is defined as follows:
``
IDictionary<String, Object>
    ``
    the required dictionary keys for an HTTP request, for example:
    - owin.RequestBody (A Stream with the request body, if any. Stream.Null MAY be used as a placeholder if there is no request body)
    - owin.RequestHeaders (A IDictionary<String, String[]>
        of request header)
        - owin.RequestMethod (A string containing the HTTP request method of the request)
        - owin.RequestPath (A string containing the request path. The path MUST be relative the "root" of the application delegate)
        - owin.RequestPathBase (A string containing the portion of the request path corresponding the "root" of the application delegate)
        - owin.RequestProtocol (A string containing the protocol name and version)
        - owin.RequestQueryString (A string containing the query string component of the HTTP request URI, without the leading "?". The value MAY be an empty string)
        - owin.RequestScheme (A string containing the URI scheme used for the request)
        2. The application delegate.This is a function signature which serves as the primary interface between all components in an OWIN application. The definition for the application delegate is as follows:
        ``
        Func<IDictionary<String, Object>, Task>;
``

OWIN is a specification. Its goal is not to be the next Web framework, but rather a specification for how Web frameworks and Web servers interact.

##Owin.dll##
This library contains a single interface, *IAppBuilder*, which formalizes and codifies the startup sequence. While not required in order to build OWIN servers, the IAppBuilder interface provides a concrete reference point, and it is used by the Katana project components.

#Project Katana#
Whereas both the OWIN specification and Owin.dll are community owned and community run open source efforts, the Katana project represents the set of OWIN components that, while still open source, are built and released by Microsoft. These components include both infrastructure components, as well as functional components, and bindings to frameworks.

The project has the following three high level goals:
* Portable - Components should be able to be easily substituted for new components as the become available.
* Modular/flexible - Katana project components should be small and focused, giving control over to the application developer in determining which components to use in her application.
* Lightweight/performant/scalable - Katana application can consume fewer computing resources, and as a result, handle more load, than with other types of servers and frameworks.

#Getting Started with Katana Components#

we will install the Microsoft.Owin.Host.SystemWeb NuGet package which provides an OWIN server that runs in the ASP.NET request pipeline into the project

One of those dependencies is *Microsoft.Owin*, a library which provides several helper types and methods for developing OWIN applications.

for example:
``
public class Startup{
    public void Configuration(IAppBuilder app){
        app.Run(context=>{
           context.Response.ContentType = "text/plain";
           return context.Response.WriteAsync("Hello World");
        });
    }
}
``

##Switching Host##

By default, the previous "hello world" example runs in the ASP.NET request pipeline, which uses System.Web in the context of IIS.

Moving form a Web-server host to a command line host requires simply adding the new server and host dependencies to project's output folder and then starting the host. we will use the Katana HttpListener-based server. Similarly to the other Katana components, these will be acquired from NuGet using the following command:
``
install-package OwinHost
``

#Katana Architecture#
The Katana component architecture divides an application into four logical layers, as depicted below: *host,server,middleware,application*. The component architecture is factored in such a way that implementations of these layers can be easily substituted, in many cases, without requiring recompilation of the application.

|------------|
|Application |
|Middleware  |
|Server      |
|Host        |
|------------|
##Host##
Responsible:

* Managing thee underlying process.
* Orchestrating the workflow that results in the selection of a server and the construction of an OWIN pipeline through which requests will be handled.

At present, there are 3 primary hosting options for Katana-based applications:

**IIS/ASP.NET**:Using the standard HttpModule and HttpHandler types, OWIN pipelines can run on IIS as a part of an ASP.NET request flow. ASP.NET hosting support is enabled by installing the *Microsoft.AspNet.Host.SystemWeb* NuGet package into a Web application project.
Additionally, beacuse IIS acts as both a host and a server, the OWIN server/host distinction is conflated in this NuGet package, meaning that if using the SystemWeb host, a developer cannot substitute an alternate server implementation.

**Custom Host**:The Katana component suite gives a developer the ability to host applications in her own custom process, whether that is a console application, Windows service, etc.

``

    static void Main()
    {
        var baseAddress = new Url("http://localhost:5000");
        var config = new HttpSelfHostConfiguration(baseAddress);
        config.Routes.MapHttpRoute("default", "{controller}");
        using (var svr = new HttpSelfHostServer(config))
        {
            svr.OpenAsync().wait();
            Console.WriteLine("Press Enter to quit.");
            Console.ReadLine();
        }
    }
``

The self-host setup for a Katana application is similar:

``

    static void Main(String[] args)
    {
        const String baseUrl = "http://localhost:5000";
        using (WebApplication.Start<Startup>(new StartOptions { Url = baseUrl }))
        {
            Console.WriteLine("Press Enter to quit.");
            Console.ReadKey();
        }
    }

    public class StartUp
    {
        public void Configuration(IAppBuilder app)
        {
            var config = new HttpConfiguration();
            config.Routes.MapHttpRoute("default", "{controller}");
            app.UseWebApi(config);
        }
    }
``

NOTE: how to find Configuration method?? need "the article"(from An Overview of Project Katana).
However, the code required to start a Katana self-host process looks strikingly similar to the code that you may be using today in ASP.NET self-host applications.

**OwinHost.exe**: While some will to write a custom process to run Katana Web applications, many would prefer to simply launch a pre-built executable that can start a server and run their application.

##Server##
While the host is responsible for starting and maintaining process within which the application runs, thee responsibility of the server is to **open a network socket**, **listen for requests**, and **send them through the pipeline of OWIN components specified by the user**(this pipeline is specified in the application developer's Startup class). Currently, the Katana project includes two server implementations:

**Microsoft.Owin.Host.SystemWeb**: As previously mentioned, IIS in concert with the ASP.NET pipeline acts as both a host and a server. Therefore, when choosing this hosting option, IIS both manages host-level concerns such as process activation and listens for HTTP requests. For ASP.NET Web applications, it then sends the requests into the ASP.NET pipeline. The Katana SystemWeb host registers an ASP.NET HttpModule and HttpHandler to intercept requests as they flow through the HTTP pipeline eand send then through the user-specified OWIN pipeline.
**Microsoft.Owin.Host.HttpListener**: As its name indicates, this Katana server uses the .NET framework's HttpListener class to open a socket and send requests into a developerspecified OWIN pipeline.This is currently the default server selection for both the Katana self-host API and OwinHost.exe

##Middleware/framwork##

As previously mentioned, when the server accepts a request from a client, it is responsible for passing it through a pipeline of OWIN components, which are specified by the developer's startup code. These pipeline components are known as middleware.

Katana supports a handful of conventions and helper types for middleware components. The must common of these is the *OwinMiddleware* class. A custom middleware component built using this class would look similar to the following:

``

    public class LoggerMiddleware : OwinMiddleware
    {
        private readonly ILog _logger;
        public LoggerMiddleware(OwinMiddleware next, ILog logger) : base(next)
        {
            _logger = logger;
        }

        public override async Task Invoke(IOwinContext context)
        {
            _logger.LogInfo("Middleware begin");
            await this.Next.Invoke(context);
            _logger.LogInfo("Middle end");
        }
    }
``

This class derives from *OwinMiddleware*, implements a constructor than accepts an instance of the next middleware in the pipeline as one of its arguments, and then passes it to the base constructor.

At runtime, the middleware is executed via the overridden Invoke method.

The middleware class can be easily added to the OWIN pipeline in the application startup code as follows:
``

    public class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            app.Use<LoggerMiddleware>(new TraceLogger());
        }
    }
``

Because the Katana infrastructure simply builds up a pipeline of OWIN middleware components, and because the components simply need to support the application delegate to participate in the pipeline, middleware components can range in complexity from simple loggers to entire frameworks like ASP.NET, Web API, or SignalR(makes developing real-time web functionality. SignalR allows bi-directional communication between server and client).

The Katana infrastructure will build the pipeline of middleware components based on the order in which they were added to the IAppBuilder object in the Configuration method. This enables powerful scenarios where a middleware component can process requests for a pipeline that includes multiple components and frameworks.

##Application##

OWIN and the Katana project should not be thought of as a new application programming model, but rather as an abstraction to decouple application programming models and frameworks from server and hosting infrastructure.