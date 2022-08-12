ViewsDllVsSlnPostProject
========================

This repo demonstrates the next issue.

Prerequisites
-------------

There are 2 project:

* An ASP.NET MVC project
* An ASP.NET Core project

There is `.sln` level "build dependency" between them:

`ASP.NET MVC` --postProjet--> `ASP.NET Core`

Issue
-----

If you build the `.sln` using Solution build (aka outside of IDE, from CLI with `msbuild.exe`),  
then the `ASP.NET Core` project's `.Views.dll` is copied into the `ASP.NET MVC` project's output folder.

Note, that only the `.Views.dll` is copied, the main output dll (the CSC build result dll) of the .NET Core project is not copied - which is the expected behavior.

### Why is this a problem?

A .NET Core dll is copied among .NET Framework dlls.  
When a tool (like Owin) running on the .NET Framework runetime tries to load all the dlls from this folder, it will throw a (misleading) Exception like:

	Could not load file or assembly 'System.Runtime, Version=5.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The system cannot find the file specified

Expected behavior
-----------------

"Dependant" projects, marked as `postProject` of "dependee" projects should not consume any build output from the "dependee" projects.  
The "relation" `postProject` should only affect the build order.

Issue repro using this repo
---------------------------

1. NuGet restore
2. Build the `.sln` using `msbuild.exe`. (Try to use `build.bat`)

You will see the `.Views.dll` is copied here:

	\WebApplication_NetFramework\bin\WebApplication_NetCore.Views.dll 
