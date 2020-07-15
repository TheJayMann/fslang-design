The purpose of this RFC document is to develop a shared plan of record for the delivery of FSharp.Core in NuGet package form as a netstandard2.0 library.

Suggestion thread: https://github.com/fsharp/fslang-suggestions/issues/893

## Background
Historically FSharp.Core.dll was compiled and delivered by Microsoft as a .NET 4.x binary installed under "Reference Assemblies". The DLL was signed and strong-named by Microsoft. Way, way back in time it was called fslib.dll.

Subsequently, FSharp.Core.dll has been recompiled for "trimmed down" platforms: portable profiles, Xamarin profiles, .NET Core, .NET Standard and .NET Framework. There is even a version of FSharp.Core for the "Xamarin iOS TV" profile.... When provided by Microsoft, these were signed, strong-named and installed under "Reference Assemblies" on Windows. For other F# compilation environments like Xamarin, Mono, Cloud Sharper, FSharp.Formatting, Azure Notebooks, Azure Functions there was a bit of a mess (made considerably worse by the separation of sigdata/optdata files, now addressed)

The F# Core Engineering Group created a NuGet package called FSharp.Core, partly to avoid a proliferation of "homebrew" packages appearing at that time.  For the last 3 years management and distribution of this nuget package has occured at the fsharp OSS site: https://github.com/dotnet/fsharp

Today
For the last 3 years the package has been released as a signed nuget package supporting both net45+ and netstandard2.0 TFMs.

## implementation
The implementation is easy.  Today at the fsharp oss project site: https://github.com/dotnet/fsharp, the fsharp.core nuget package and signed dll's are built.  We will remove the build for the desktop clr and all multi-fsharp.core testing support.

This will work well, because the Windows desktop and netstandard FSharp.Core dll's have identical public surfaces.  And net472 automagically modiifies references to netstandard to load the specific net4X version of the api.

## Assumptions
For the purposes of this RFC we will assume that:

# Scenarios
Please contribute scenarios to the list below.

1. General purpose library developer targetting usage on both NetCoreApp *, and net472+ frameworks uses F# to build an library.
When the FSharp.Core targets netstandard 2.0  only then a nuget package with a single netstandard2.0 will work well, the developer will be able to build libraries that target all coreclr frameworks from netcoreapp2.0 up, and all desktop frameworks from net472 to net48.

2. Application developer targetting windows development build a command line application  uses F# to build an application.
The developer can target Windows using net472 and net48, or the coreclr with all netcoreapps from 2.0 up

3. Application developer targetting linux / mac development build a command line application  uses F# to build an application.
The developer can target Windows linux / mac or windows using the coreclr with all netcoreapps from 2.0 up

4. Application developer targetting windows / linux / mac using net5 uses F# to build an app.
The developer can target Windows linux / mac or windows using the coreclr with all net5 up

5. General purpose library developer targetting usage on net45+ frameworks uses F# to build an library.
netstandard2.0 is not well supported on versions of dotnet earlier than 4.7.2, in this circumstance the developer should reference the FSharp.Core nuget package version 4.7.3 or earlier to get the net45 version of FSharp.Core.  New language features that rely on an updated FSharp.Core will not work.

5. Application developer targetting usage on net45+ frameworks uses F# to build an application.
netstandard2.0 is not well supported on versions of dotnet earlier than 4.7.2, in this circumstance the developer should reference the FSharp.Core nuget package version 4.7.3 or earlier to get the net45 version of FSharp.Core.  New language features that rely on an updated FSharp.Core will not work.

# Alternatives

1.  Continue targetting multiple frameworks
Instead of building against a single netstandard2.0 framework we additionally target netstandard 2.1, net5 ... etc

Response:

Future frameworks are targetting release specific framework rids, I.e. netcoreapp3.0, netcoreapp3.1, net5 ... beyond.
If we chose to target specific frameworks with FSharp.Core developerslibrary developers 281299 would be responsible for picking, developing and deploying rid specific FSharp.Cores.  Right now FSharp.Core doesn't need any rid specific code and so we will ship a single FSharp.Core with the widest reach possible.
