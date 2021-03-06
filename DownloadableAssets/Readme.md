# Localization Admin UI and Sample Support Files
.NET Core no longer allows content as part of NuGet packages, and these zip files provide the additional content files you can add to your project.

This folder contains two sets of support files in zipped format:

* **Localization Administration UI for .NET Core**  
This Zip file contains the HTML, CSS, Scripts and Images to run the Localization Administration UI for ASP.NET Core. 
<small>[Download](https://github.com/RickStrahl/Westwind.Globalization/blob/master/DownloadableAssets/LocalizationAdministrationHtml_AspNetCore.zip?raw=true)</small>

* **Sample Page and Resources**   
This zip file contains a sample page and resources you can play with when first setting up Westwind.Globalization in a ASP.NET Core site.  <small>[Download](https://github.com/RickStrahl/Westwind.Globalization/blob/master/DownloadableAssets/LocalizationSample_AspNetCore.zip?raw=true)</small>

> #### Requires ASP.NET Core MVC
> Both of these addons rely on features of **ASP.NET MVC** in order to run. Make sure your ASP.NET Core app has ` app.UseMvc()` as part of its startup code. Note: It's only these addons that require MVC - **running** `Westwind.Globalization` and `Westwind.Globalization.AspNetCore` in a .NET Core or ASP.NET Core application doesn't require MVC - it's only the Admin UI and sample that do.


## Setting up the Localization Admin UI
To set up the Localization Admin UI is a two step process:

* Download [LocalizationAdministration_AspNetCore.zip](https://github.com/RickStrahl/Westwind.Globalization/blob/master/DownloadableAssets/LocalizationAdministrationHtml_AspNetCore.zip?raw=true)
* Unzip the package into the **root folder** of your ASP.NET Core MVC application
* Recompile your project
* Setup your Configuration  
Set the `ConnectionString` and `ResourceTableName` to an existing database and a tablename that you want to create, either in `DbResourceConfiguration.json` or `appsettings.json` (see docs)
* Open the Admin Page at `localizationAdmin/`
* Click on **Create Table**
* You should see some sample resources in the admin interface
* Click **Import


### What the Zip File Contains
The zip file provides the Administration UI client side assests and .NET Resources required to run the Admin form.

* **Copies Admin UI HTML and Resources**  
Copies files into `wwwroot/LocalizationAdmin`, which contains the client side Localization Admin UI HTML and assets into your application. **You can move this folder** to any location you want. If you generate your `wwwroot` folder as part of a client side build, you can move the `LocalizationAdmin` folder into the client side build directory.

* **Copies Localization Admin Resources into Properties**   
Copies `Properties\LocalizationAdminForm.resx` and `Properties\LocalizationAdminForm.de.resx` files into the `Properties` folder of your project, these are the resources required for the LocalizationAdmin backend application which serves JavaScript resources to the client.

### What it Runs
Once you run the application and access teh admin UI, `Westwind.Globalization.AspNetCore` exposes two api services which use their own set of hard coded routes:

* **api/LocalizationAdministration/XXXXXXX** 
These calls calls handle the Localization Administration backend for showing and editing  resources. 

* **api/JavaScriptLocalizationResources**  
Handles JavaScript resource localization by serving resources from the server as a JavaScript object.

### Securing the Administration UI
By default the Localization Administration UI is **always accessible**!

In order to configure access to the API you can implement a startup configuration delegate with the signature of `Func<ControllerContext, bool>` which needs to return `true` or `false` depending on whether you want to allow access. At minimum you probably want to check for authenticated users, or group membership.

Here's how you can do this as part of the ASP.NET Core Startup in the `Configure()` method of Startup:

```cs
services.AddWestwindGlobalization(opt =>
{                
    // the default settings comme from DbResourceConfiguration.json if exists
    // Customize opt with  any customizations here...

    // *** THIS ***
    // Set up security for Localization Administration form
    opt.ConfigureAuthorizeLocalizationAdministration(controllerContext =>
    {
        // return true or false whether this request is authorized
        return controllerContext.HttpContext.User.Identity.IsAuthenticated;
    });
});

// if checking for users you need to hook up ASP.NET Auth of some sort
services.AddAuthentication(...);
```

The code you run here can be anything at all, but typically you will check access against your authentication scheme defined as part of your application.

> Note: In ASP.NET Core Authentication features only work if you have actually hooked up an Authentication mechanism as part of the application in `Startup` with `app.AddAuthentication()`.
More info can be on the [ASP.NET Docs Authentication Topics](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/)

### Loading Admin UI Resources into the Database
The zip file contains the Resx resources for the admin UI and adds them to your Properties folder. In order to use those resources you have to import them into 


## Setting up ResourceTest Sample Page
The ResourceTest Sample page allows you to check out Westwind Globalization in your application and to easily check and see if the localization configuration features are set up properly.

To install:

* Download the [zip file](https://github.com/RickStrahl/Westwind.Globalization/blob/master/DownloadableAssets/LocalizationSample_AspNetCore.zip?raw=true)
* Unzip in the root folder of your ASP.NET Core MVC application
* Recompile your project

### What it Does
The zip file copies a sample page and resources:

* `Pages/ResourceTest.aspx`
* Copies several Resource files into `Properties` folder


### Using the Sample
The best way to check this out is to 
