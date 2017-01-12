# Running Orchard2 as a service
Orchard 2 by default is compiled to be portable. It provides pros but also cons. 
One of the cons is we cannot use methods available in .NET Core for executing a program as a service because those are platform dependent.
This limitation can be overcome thanks to [dasMulli/dotnet-win32-service|https://github.com/dasMulli/dotnet-win32-service] library.

In this repo you can find the extra files needed for executing Orchard 2 as a service.
+ *ProgramAsAService.cs*: It replaces default Orchard2 Program.cs.
+ *OrchardCmsWin32Service.cs*:  It implements IWin32Service.
 alternate Program.cs file called ProgramAsAService.cs It replaces default Program.cs.

 Add those two files within the Orchard.Cms.Web project in your Orchard 2 solution.

Next in project.json you need to do following changes:
+ Add these two dependencies:
`"DasMulli.Win32.ServiceUtils": "1.0.1"`
`"Microsoft.Extensions.Configuration.CommandLine": "1.0.0"`
+ Add this entry `"excludeFiles": [ "Program.cs" ]` to buildoptions -> compile to ignore default Command class.

Arguments available to execute Orchard 2 through command line:
  --register-service	Registers and starts the program as a windows service. All additional arguments will be passed to ASP.NET Core's WebHostBuilder.
	--service-name:<your service name>     Replace default service name (Orchard 2). Service will run using a virtual account named NT SERVICE\\<serviceName>.
    --service-display-name:<your service name>     Replace default service display-name (Orchard 2)
    --service-description:<your service description>     Replace default service description
  --unregister-service      Removes the windows service created by --register-service.
  
After registering the service don't forget to provide read/write permissions in Orchard2 folder for vitual user NT SERVICE\\<serviceName> 

##Samples

Running Orchard 2 standalone:
`dotnet Orchard.Cms.Web.dll --urls http://localhost`

Registering Orchard 2 as a service:            
`dotnet Orchard.Cms.Web.dll --register-service --urls http://localhost`

Registering Orchard 2 as a service customizing its name:            
`dotnet Orchard.Cms.Web.dll --register-service --urls http://localhost`
