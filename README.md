# fsharp-dnx-templates
Visual Studio templates for creating DNX projects with F#.

## Getting started
[FSharpDNXConsoleApp](https://github.com/jbfp/fsharp-dnx-templates/tree/master/src/FSharpDNXConsoleApp) is a template that contains a reference to the real template ([FSharpConsoleApp](https://github.com/jbfp/fsharp-dnx-templates/tree/master/src/FSharpConsoleApp).) It might be possible to combine the templates, but I just copied the approach the ASP.NET guys took with [the official C# ASP.NET 5 templates](https://github.com/aspnet/Templates).
[FSharpConsoleApp](https://github.com/jbfp/fsharp-dnx-templates/tree/master/src/FSharpConsoleApp) is the actual contents of the project (`project.json`, `Program.fs`, the `.xproj` file`.)

* [FSharpConsoleApp](https://github.com/jbfp/fsharp-dnx-templates/tree/master/src/FSharpConsoleApp) goes into `..\Microsoft Visual Studio 14.0\Common7\IDE\AspNetProjectTemplates\1033\`
* [FSharpDNXConsoleApp](https://github.com/jbfp/fsharp-dnx-templates/tree/master/src/FSharpDNXConsoleApp) goes into `..\Microsoft Visual Studio 14.0\Common7\IDE\ProjectTemplates\FSharp\1033`

To get Visual Studio to register the templates, you can run `devenv.exe /installvstemplates`. (`devenv.exe` is in the `..\Microsoft Visual Studio 14.0\Common7\IDE\` folder.)

## FAQ
### Where can I find the `FSharp.Dnx` dependency?
The `FSharp.Dnx` dependency can be found in the fsprojects NuGet feed: https://www.myget.org/F/fsprojects/api/v3/index.json
You can include it in your NuGet.Config file:
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
    <add key="fsprojects" value="https://www.myget.org/F/fsprojects/api/v3/index.json" />
    <add key="api.nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
</configuration>
```

### Why is the `Program` type a class with a `Main` method and not a simple F# function like the standard F# Console Application template?
When DNX loads, [it looks for a class called `Program` with a method called `Main`](https://github.com/aspnet/dnx/blob/7ac7929aa575e17b3c271e4a7a0c164418de0395/src/Microsoft.Dnx.Runtime.Sources/Impl/EntryPointExecutor.cs#L70-L111).
You can do
```F#
namespace ConsoleApplication1

type Program () =
    member x.Main (argv: string array) =
        printfn "%A" argv
        0
```
but you can also do
```F#
module MyModule

type Program () =
    member x.Main (argv: string array) =
        printfn "%A" argv
        0
```
or
```F#
module Program

let Main (argv: string array) =
    printfn "%A" argv
    0
```
If you choose to use a Program class, you can do dependency injection, e.g. inject an `IApplicationEnvironment`.
