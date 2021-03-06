﻿## Owin Sample with F# 

It can be hosted in either IIS or as a Self-Host app. You can use this project as a
template or reference for creating server-side portion of a
[single-page application](http://en.wikipedia.org/wiki/Single-page_application) (SPA).

![F# Owin Sample Project](http://i.imgur.com/yJL8nLZ.png)

### References

 * [Getting Started with OWIN and Katana](http://www.asp.net/aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana)
 * [How to create a pure F# ASP.NET Web API project](http://blog.ploeh.dk/2013/08/23/how-to-create-a-pure-f-aspnet-web-api-project/)

### Download

[![Web Application Server](http://i.imgur.com/PA5swd0.png)](http://visualstudiogallery.msdn.microsoft.com/c7ea6e81-b383-40e4-899c-4a5ab9d68f02)

http://visualstudiogallery.msdn.microsoft.com/c7ea6e81-b383-40e4-899c-4a5ab9d68f02

#### OWIN Startup (startup.fs)

```fsharp
namespace Server

open Owin
open Microsoft.Owin

type Startup() =
    member x.Configuration(app: Owin.IAppBuilder) =
        app.Run(fun ctx ->
            ctx.Response.ContentType <- "text/plain"
            ctx.Response.WriteAsync("Owin Sample with F#")
        ) |> ignore

[<assembly: OwinStartup(typeof<Startup>)>]
do ()
```

You can plug [SignalR](http://www.asp.net/signalr) and/or [Nancy](http://nancyfx.org/)
framework into this class and start from there. And the front-end part of the SPA can
be developed in a separate project and hosted in CDN (ex.: [angular-seed](https://github.com/angular/angular-seed)).

Also see: [Owin Startup Class Detection](http://www.asp.net/aspnet/overview/owin-and-katana/owin-startup-class-detection)

#### Project settings for local debugging

![OwinHost](http://i.imgur.com/nlVcWxa.png)

```xml
  <PropertyGroup>
    ...
    <StartProgram>$(SolutionDir)packages\OwinHost.3.0.0-beta1\tools\OwinHost.exe</StartProgram>
    <StartArguments>-d "$(ProjectDir)"</StartArguments>
  </PropertyGroup>
```

#### Project settings for web publishing

```xml
  <PropertyGroup>
    ...
    <ProjectGuid>...</ProjectGuid>
    <ProjectTypeGuids>{349c5851-65df-11da-9384-00065b846f21};{f2a71f9b-5d33-465a-a702-920d77279786}</ProjectTypeGuids>
    ...
  </PropertyGroup>
  ...
  <PropertyGroup>
    <VisualStudioVersion Condition="'$(VisualStudioVersion)' == ''">10.0</VisualStudioVersion>
    <VSToolsPath Condition="'$(VSToolsPath)' == ''">$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)</VSToolsPath>
  </PropertyGroup>
  <Import Project="$(VSToolsPath)\WebApplications\Microsoft.WebApplication.targets" Condition="'$(VSToolsPath)' != ''" />
  <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\WebApplications\Microsoft.WebApplication.targets" Condition="false" />
```