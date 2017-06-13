---
layout: post
title:  "What is the ASP.NET Core Request Pipeline?"
date:   2017-06-12 18:57:03 -0500
categories: dotnet
---

Let's talk about the ASP.NET Core request pipeline within the scope of a new sample ASP.NET Core application. The current version of ASP.NET Core is 1.1. Information in this post will be pertaining to that verison.

Open up Visual Studio and create a new empty ASP.NET Core Web Application:
![New empty project][newproject]

We see that our empty project only has two classes, Program.cs and Startup.cs. Lets review each of them.

### Program.cs

```cs
// Program.cs
namespace SampleApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseKestrel()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .UseApplicationInsights()
                .Build();

            host.Run();
        }
    }
}
```

Program.cs is the starting point of our application. It looks like a typical console application. That's because ASP.NET Core is a console application, which calls certain ASP.NET libraries. Our entry point Main method runs the application. Since we want to run a web application, we need to host it. So an instance of WebHostBuilder is initialized. 

A hosted web app needs a web server. ASP.NET Core ships with [two HTTP web servers][webservers], WebListener (Windows-only) and Kestrel (cross-platform). Kestrel is the default. 

By using Visual Studio, we default to IIS Express as the web host. Calling UseIISIntegration means IIS Express functions as a reverse proxy server for Kestrel. If we intend to deploy our web app on a Windows server then IIS should be used to manage and proxy requests to Kestrel. Deploying on another platform like Linux would use Apache, for example, to proxy requests to Kestrel. It's possible to self host a web app with Kestrel alone but using IIS has its advantages such as filtering requests, managing the SSL layer, restarting the app after it crashes, etc.

The content root holds the base path of an app's content, like its views and its web content. The default directory is the base path for the executable hosting the app, which is the current directory.

[Application Insights][applicationinsights] is an analytics platform that monitors the performance and usage of an ASP.NET Core web app. It's added on to every new web app by default.

Calling UseStartup with some kind of start up type is important as that class contain additional configurations telling the web app what needs to be done when it initially runs. By default the start up type is the Startup class given to us automatically with a new project but we can freely define our own if needed.

Finally, the web host instance is built and ran.

[newproject]: /assets/images/2017-06-12-aspdotnet-core-pipeline/01_new_project.jpg
[webservers]: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/
[applicationinsights]: https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started

**To Be Continued...**