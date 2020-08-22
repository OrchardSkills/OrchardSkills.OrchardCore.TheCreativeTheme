# Orchard Core Creative Theme

## Designng a Theme for Orchard Core

# [YouTube Video](https://youtu.be/Hc6cTf211Cs)

[![OrchardSkillsYouTubeThumbNailTheme](https://user-images.githubusercontent.com/59172485/90962686-1e5aee80-e46f-11ea-8ad1-cf486f5e9003.png)](https://youtu.be/Hc6cTf211Cs)

# Introduction

An Orchard theme defines an application's appearance and is used to customize the look and feel of an Orchard website. A theme can override the style sheets, images, layouts, or content templates provided by any Orchard module. In addition, a theme can contain code that overrides targeted code in a module. We will show you how to create a theme from scratch. It is intended to be an introduction to theme development and has been kept simple by design.



![Orchard-Core-Theme-001](https://user-images.githubusercontent.com/59172485/90962605-a8ef1e00-e46e-11ea-9ca0-06928a60f752.png)

Launch your favorite browser and browse to https:/startbootstrap.com/creative. Download the template to your computer.



![Orchard-Core-Theme-002](https://user-images.githubusercontent.com/59172485/90962606-aa204b00-e46e-11ea-9bbf-be9ad8ceb73c.png)

Launch Visual Studio and create a new solution. Select ASP.NET Core Web Solution.



![Orchard-Core-Theme-003](https://user-images.githubusercontent.com/59172485/90962607-aab8e180-e46e-11ea-8009-eb66dfeb86c4.png)

For the Project name enter MyOrchardCoreCMS and then press the "Create" button.



![Orchard-Core-Theme-004](https://user-images.githubusercontent.com/59172485/90962609-abea0e80-e46e-11ea-9a84-bd08d5c100a8.png)

Select "Empty" and then press the "Create" button.



![Orchard-Core-Theme-005](https://user-images.githubusercontent.com/59172485/90962610-ad1b3b80-e46e-11ea-8b97-38b79e36a26d.png)

Right click on the solution and select "Add" then "New Item...".



![Orchard-Core-Theme-006](https://user-images.githubusercontent.com/59172485/90962611-adb3d200-e46e-11ea-9fbd-1b02c0e8f716.png)

Select a "Test File" and then click on the "Add" button.



![Orchard-Core-Theme-007](https://user-images.githubusercontent.com/59172485/90962613-adb3d200-e46e-11ea-8cb6-f1976f07a82e.png)

Rename it to "NuGet.config".



![Orchard-Core-Theme-008](https://user-images.githubusercontent.com/59172485/90962614-ae4c6880-e46e-11ea-9530-bfe6dd8d2abc.png)

Add the following configuration. Note this for RC2.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="NuGet" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
  <activePackageSource>
    <add key="All" value="(Aggregate source)" />
  </activePackageSource>
</configuration>
```



![Orchard-Core-Theme-009](https://user-images.githubusercontent.com/59172485/90962616-aee4ff00-e46e-11ea-837a-a11286a00576.png)

Exit Visual Studio.



![Orchard-Core-Theme-010](https://user-images.githubusercontent.com/59172485/90962617-aee4ff00-e46e-11ea-849b-d4ae858ab2c2.png)

Relaunch Visual Studio and Select your solution.



![Orchard-Core-Theme-011](https://user-images.githubusercontent.com/59172485/90962618-b0162c00-e46e-11ea-8fb7-ac6d725db2ab.png)

Click on the "Project". Add the following code: Note this is for RC2. Adding the logger for logging functionality.

```
  <ItemGroup>
    <PackageReference Include="OrchardCore.Application.Cms.Targets" Version="1.0.0-rc2-13450" />
    <PackageReference Include="OrchardCore.Logging.NLog" Version="1.0.0-rc2-13450" />
  </ItemGroup>
```



![Orchard-Core-Theme-012](https://user-images.githubusercontent.com/59172485/90962619-b0aec280-e46e-11ea-82c9-38caf8e1a9b2.png)

Added services.AddOrchardCms();



![Orchard-Core-Theme-013](https://user-images.githubusercontent.com/59172485/90962620-b1475900-e46e-11ea-8923-e9ba848c290b.png)

Add app.UseOrchardCore().



Here is the complete code listing of Startup.cs.

```
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

namespace OrchardSkills.OrchardCore.OrchardCMS
{
    public class Startup
    {
        // This method gets called by the runtime. Use this method to add services to the container.
        // For more information on how to configure your application, visit https://go.microsoft.com/fwlink/?LinkID=398940
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddOrchardCms();
        }

        // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.UseOrchardCore();

        }
    }
}

```

Also for reference, here is the complete listing of Program.cs

```
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using OrchardCore.Logging;

namespace OrchardSkills.OrchardCore.OrchardCMS
{
    public class Program
    {
        public static Task Main(string[] args)
            => BuildHost(args).RunAsync();

        public static IHost BuildHost(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureLogging(logging => logging.ClearProviders())
                .ConfigureWebHostDefaults(webBuilder => webBuilder
                    .UseStartup<Startup>()
                    .UseNLogWeb()
                ).Build();
    }
}

```

Note: Notice the logger being set up.



You will also need add the file "NLog.config" to the web application project. 

```
<?xml version="1.0" encoding="utf-8" ?>
<!--
Level Typical Use
Fatal Something bad happened; application is going down
Error Something failed; application may or may not continue
Warn  Something unexpected; application will continue
Info  Normal behavior like mail sent; user updated profile etc.
Debug For debugging; execute query; user authenticated, session expired
Trace For trace debugging; begin method X, end method X
-->
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      autoReload="true"
      internalLogLevel="Warn"
      internalLogFile="App_Data/logs/internal-nlog.txt">

  <extensions>
    <add assembly="NLog.Web.AspNetCore"/>
    <add assembly="OrchardCore.Logging.NLog"/>
  </extensions>

  <targets>
    <!-- file target -->
    <target xsi:type="File" name="file"
            fileName="${var:configDir}/App_Data/logs/orchard-log-${shortdate}.log"
            layout="${longdate}|${orchard-tenant-name}|${aspnet-traceidentifier}|${event-properties:item=EventId.Id}|${logger}|${uppercase:${level}}|${message} ${exception:format=ToString,StackTrace}"
        />

    <!-- console target -->
    <target xsi:type="Console" name="console" />

  </targets>

  <rules>
    <!-- all warnings and above go to the file target -->
    <logger name="*" minlevel="Warn" writeTo="file" />

    <!-- the hosting lifetime events go to the console -->
    <logger name="Microsoft.Hosting.Lifetime" minlevel="Info" writeTo="console" />
  </rules>
</nlog>

```

Run the application by pressing the green play button.



![Orchard-Core-Theme-014](https://user-images.githubusercontent.com/59172485/90962622-b1475900-e46e-11ea-8b25-ef4d126b779c.png)

Congratulations the web application is running.



![Orchard-Core-Theme-015](https://user-images.githubusercontent.com/59172485/90962623-b1dfef80-e46e-11ea-92db-a3d090357fe8.png)

Now lets create the theme project. Right click on the solution, select "Add" and then "New Project...".



![Orchard-Core-Theme-016](https://user-images.githubusercontent.com/59172485/90962626-b2788600-e46e-11ea-924c-4cb2785aae0c.png)

Select the "Class Library (.NET Core)" and press the "Next" button.



![Orchard-Core-Theme-017](https://user-images.githubusercontent.com/59172485/90962627-b3111c80-e46e-11ea-8b75-8f02abfce59a.png)

Enter the Project name and select the "Create" button.



![Orchard-Core-Theme-018](https://user-images.githubusercontent.com/59172485/90962628-b3a9b300-e46e-11ea-8ab8-a6cef7d12542.png)

Right click on the "Class1" class and select rename. Rename it to "Startup", click the check boxes and then press Apply. 



![Orchard-Core-Theme-019](https://user-images.githubusercontent.com/59172485/90962629-b4424980-e46e-11ea-93ca-ccbd002e7426.png)

Click on the "TheCreateTheme" project and modify to the following: Notre this is for RC2.

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <AddRazorSupportForMvc>true</AddRazorSupportForMvc>
  </PropertyGroup>
  
  <ItemGroup>
    <Compile Remove="wwwroot\node_modules\**" />
    <EmbeddedResource Remove="wwwroot\node_modules\**" />
    <None Remove="wwwroot\node_modules\**" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="OrchardCore.Theme.Targets" Version="1.0.0-rc2-13450" />
    <PackageReference Include="OrchardCore.DisplayManagement" Version="1.0.0-rc2-13450" />
    <PackageReference Include="OrchardCore.ResourceManagement.Abstractions" Version="1.0.0-rc2-13450" />
  </ItemGroup>

  <ItemGroup>
    <EmbeddedResource Remove="**\*.less;**\*.scss;" />
    <EmbeddedResource Remove="wwwroot\gulpfile.js" />
    <EmbeddedResource Remove="wwwroot\index.html" />
    <EmbeddedResource Remove="wwwroot\package-lock.json" />
    <EmbeddedResource Remove="wwwroot\package.json" />
  </ItemGroup>
</Project>

```



![Orchard-Core-Theme-020](https://user-images.githubusercontent.com/59172485/90962630-b4dae000-e46e-11ea-92e5-bb5da3956178.png)

Modify the "Startup.cs" file to as follows: Note this is for RC2.

```

using Microsoft.Extensions.DependencyInjection;
using OrchardCore.Modules;
using OrchardCore.ResourceManagement;

namespace OrchardCore.Themes.TheCreativeTheme
{
    public class Startup : StartupBase
    {
        public override void ConfigureServices(IServiceCollection serviceCollection)
        {
            serviceCollection.AddScoped<IResourceManifestProvider, ResourceManifest>();
        }
    }
}

```



![Orchard-Core-Theme-021](https://user-images.githubusercontent.com/59172485/90962631-b5737680-e46e-11ea-94bf-f664004aecac.png)

Right click on the web application project and select "Add Reference..."



![Orchard-Core-Theme-022](https://user-images.githubusercontent.com/59172485/90962632-b5737680-e46e-11ea-98dc-a2b78589626b.png)

Select the "TheCreateTheme" to add it as a project reference.



![Orchard-Core-Theme-023](https://user-images.githubusercontent.com/59172485/90962633-b60c0d00-e46e-11ea-98cd-854c41eb919f.png)

Clone the "OrchardCore project" at https://github.com/OrchardCMS/OrchardCore. Copy the file and folders from the "TheAgencyTheme" from the directory "OrchardCore/src/OrchardCore.Themes/TheAgencyTheme". Right click and create a new folder.



![Orchard-Core-Theme-024](https://user-images.githubusercontent.com/59172485/90962635-b6a4a380-e46e-11ea-82ae-aada58dc2fcc.png)

Rename it to "wwwroot".



![Orchard-Core-Theme-025](https://user-images.githubusercontent.com/59172485/90962637-b73d3a00-e46e-11ea-8616-1eb1c1ab38b3.png)

Do to the directory where you downloaded the creative theme template.

 

![Orchard-Core-Theme-026](https://user-images.githubusercontent.com/59172485/90962640-b7d5d080-e46e-11ea-930a-9494096c2e17.png)

Paste the files and folders in the "wwwroot" folder.



![Orchard-Core-Theme-027](https://user-images.githubusercontent.com/59172485/90962641-b86e6700-e46e-11ea-87ff-447561c35bca.png)

In the "Views" folder, rename the "Layout.liquid" file to something else. Copy the index.html file from the template and rename it to "Layout.liquid".



![Orchard-Core-Theme-028](https://user-images.githubusercontent.com/59172485/90962642-b906fd80-e46e-11ea-839b-b6b09987886a.png)

Modify the Layout to liquid to the following:  Node the code has been refactored for RC2.

```
<!DOCTYPE html>
<html lang="en">

<head>

  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <meta name="description" content="">
  <meta name="author" content="">

  <title>Creative - Start Bootstrap Theme</title>

  <!-- Font Awesome Icons -->
  <link href="/TheCreativeTheme/vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">

  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css?family=Merriweather+Sans:400,700" rel="stylesheet">
  <link href='https://fonts.googleapis.com/css?family=Merriweather:400,300,300italic,400italic,700,700italic' rel='stylesheet' type='text/css'>

  <!-- Plugin CSS -->
  <link href="/TheCreativeTheme/vendor/magnific-popup/magnific-popup.css" rel="stylesheet">

  <!-- Theme CSS - Includes Bootstrap -->
  <link href="/TheCreativeTheme/css/creative.min.css" rel="stylesheet">

</head>

<body id="page-top">

  <!-- Navigation -->
  <nav class="navbar navbar-expand-lg navbar-light fixed-top py-3" id="mainNav">
    <div class="container">
      <a class="navbar-brand js-scroll-trigger" href="#page-top">Start Bootstrap</a>
      <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarResponsive">
        <ul class="navbar-nav ml-auto my-2 my-lg-0">
          <li class="nav-item">
            <a class="nav-link js-scroll-trigger" href="#about">About</a>
          </li>
          <li class="nav-item">
            <a class="nav-link js-scroll-trigger" href="#services">Services</a>
          </li>
          <li class="nav-item">
            <a class="nav-link js-scroll-trigger" href="#portfolio">Portfolio</a>
          </li>
          <li class="nav-item">
            <a class="nav-link js-scroll-trigger" href="#contact">Contact</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- Masthead -->
  <header class="masthead">
    <div class="container h-100">
      <div class="row h-100 align-items-center justify-content-center text-center">
        <div class="col-lg-10 align-self-end">
          <h1 class="text-uppercase text-white font-weight-bold">Your Favorite Source of Free Bootstrap Themes</h1>
          <hr class="divider my-4">
        </div>
        <div class="col-lg-8 align-self-baseline">
          <p class="text-white-75 font-weight-light mb-5">Start Bootstrap can help you build better websites using the Bootstrap framework! Just download a theme and start customizing, no strings attached!</p>
          <a class="btn btn-primary btn-xl js-scroll-trigger" href="#about">Find Out More</a>
        </div>
      </div>
    </div>
  </header>

  <!-- About Section -->
  <section class="page-section bg-primary" id="about">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-lg-8 text-center">
          <h2 class="text-white mt-0">We've got what you need!</h2>
          <hr class="divider light my-4">
          <p class="text-white-50 mb-4">Start Bootstrap has everything you need to get your new website up and running in no time! Choose one of our open source, free to download, and easy to use themes! No strings attached!</p>
          <a class="btn btn-light btn-xl js-scroll-trigger" href="#services">Get Started!</a>
        </div>
      </div>
    </div>
  </section>

  <!-- Services Section -->
  <section class="page-section" id="services">
    <div class="container">
      <h2 class="text-center mt-0">At Your Service</h2>
      <hr class="divider my-4">
      <div class="row">
        <div class="col-lg-3 col-md-6 text-center">
          <div class="mt-5">
            <i class="fas fa-4x fa-gem text-primary mb-4"></i>
            <h3 class="h4 mb-2">Sturdy Themes</h3>
            <p class="text-muted mb-0">Our themes are updated regularly to keep them bug free!</p>
          </div>
        </div>
        <div class="col-lg-3 col-md-6 text-center">
          <div class="mt-5">
            <i class="fas fa-4x fa-laptop-code text-primary mb-4"></i>
            <h3 class="h4 mb-2">Up to Date</h3>
            <p class="text-muted mb-0">All dependencies are kept current to keep things fresh.</p>
          </div>
        </div>
        <div class="col-lg-3 col-md-6 text-center">
          <div class="mt-5">
            <i class="fas fa-4x fa-globe text-primary mb-4"></i>
            <h3 class="h4 mb-2">Ready to Publish</h3>
            <p class="text-muted mb-0">You can use this design as is, or you can make changes!</p>
          </div>
        </div>
        <div class="col-lg-3 col-md-6 text-center">
          <div class="mt-5">
            <i class="fas fa-4x fa-heart text-primary mb-4"></i>
            <h3 class="h4 mb-2">Made with Love</h3>
            <p class="text-muted mb-0">Is it really open source if it's not made with love?</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Portfolio Section -->
  <section id="portfolio">
    <div class="container-fluid p-0">
      <div class="row no-gutters">
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/1.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/1.jpg" alt="">
            <div class="portfolio-box-caption">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/2.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/2.jpg" alt="">
            <div class="portfolio-box-caption">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/3.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/3.jpg" alt="">
            <div class="portfolio-box-caption">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/4.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/4.jpg" alt="">
            <div class="portfolio-box-caption">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/5.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/5.jpg" alt="">
            <div class="portfolio-box-caption">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
        <div class="col-lg-4 col-sm-6">
          <a class="portfolio-box" href="/TheCreativeTheme/img/portfolio/fullsize/6.jpg">
            <img class="img-fluid" src="/TheCreativeTheme/img/portfolio/thumbnails/6.jpg" alt="">
            <div class="portfolio-box-caption p-3">
              <div class="project-category text-white-50">
                Category
              </div>
              <div class="project-name">
                Project Name
              </div>
            </div>
          </a>
        </div>
      </div>
    </div>
  </section>

  <!-- Call to Action Section -->
  <section class="page-section bg-dark text-white">
    <div class="container text-center">
      <h2 class="mb-4">Free Download at Start Bootstrap!</h2>
      <a class="btn btn-light btn-xl" href="https://startbootstrap.com/themes/creative/">Download Now!</a>
    </div>
  </section>

  <!-- Contact Section -->
  <section class="page-section" id="contact">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-lg-8 text-center">
          <h2 class="mt-0">Let's Get In Touch!</h2>
          <hr class="divider my-4">
          <p class="text-muted mb-5">Ready to start your next project with us? Give us a call or send us an email and we will get back to you as soon as possible!</p>
        </div>
      </div>
      <div class="row">
        <div class="col-lg-4 ml-auto text-center mb-5 mb-lg-0">
          <i class="fas fa-phone fa-3x mb-3 text-muted"></i>
          <div>+1 (202) 555-0149</div>
        </div>
        <div class="col-lg-4 mr-auto text-center">
          <i class="fas fa-envelope fa-3x mb-3 text-muted"></i>
          <!-- Make sure to change the email address in anchor text AND the link below! -->
          <a class="d-block" href="mailto:contact@yourwebsite.com">contact@yourwebsite.com</a>
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-light py-5">
    <div class="container">
      <div class="small text-center text-muted">Copyright &copy; 2019 - Start Bootstrap</div>
    </div>
  </footer>

  <!-- Bootstrap core JavaScript -->
  <script src="/TheCreativeTheme/vendor/jquery/jquery.min.js"></script>
  <script src="/TheCreativeTheme/vendor/bootstrap/js/bootstrap.bundle.min.js"></script>

  <!-- Plugin JavaScript -->
  <script src="/TheCreativeTheme/vendor/jquery-easing/jquery.easing.min.js"></script>
  <script src="/TheCreativeTheme/vendor/magnific-popup/jquery.magnific-popup.min.js"></script>

  <!-- Custom scripts for this template -->
  <script src="/TheCreativeTheme/js/creative.min.js"></script>

</body>

</html>

```





![Orchard-Core-Theme-029](https://user-images.githubusercontent.com/59172485/90962644-b99f9400-e46e-11ea-8ec2-6f98956f511f.png)

Modify the recipe file to the following: Node the recipe has been refactored for RC2.

```
{
  "name": "The Creative Theme",
  "displayName": "The Creative Theme",
  "description": "Orchard Skills CreativeTheme.",
  "author": "Orchard Skills",
  "website": "https://OrchardSkills.com",
  "version": "1.0.0-rc2",
  "issetuprecipe": true,
  "categories": [ "default" ],
  "tags": [ "Creative" ],

  // The variables are evaluated the first time they are accessed, and reused across steps
  "variables": {
    "landingContentItemId": "[js:uuid()]"
    // "now": "[js: new Date().toISOString()]"
  },

  "steps": [
    {
      "name": "feature",
      "disable": [],
      "enable": [
        // SaaS
        "OrchardCore.HomeRoute",
        "OrchardCore.Admin",
        "OrchardCore.Diagnostics",
        "OrchardCore.DynamicCache",
        "OrchardCore.Features",
        "OrchardCore.Navigation",
        "OrchardCore.Recipes",
        "OrchardCore.Resources",
        "OrchardCore.Roles",
        "OrchardCore.Settings",
        "OrchardCore.Themes",
        "OrchardCore.Users",

        // Content Management
        "OrchardCore.Alias",
        "OrchardCore.Autoroute",
        "OrchardCore.Html",
        "OrchardCore.ContentFields",
        "OrchardCore.ContentPreview",
        "OrchardCore.Contents",
        "OrchardCore.Contents.FileContentDefinition",
        "OrchardCore.ContentTypes",
        "OrchardCore.CustomSettings",
        "OrchardCore.Deployment",
        "OrchardCore.Deployment.Remote",
        "OrchardCore.Feeds",
        "OrchardCore.Flows",
        "OrchardCore.Layers",
        "OrchardCore.Lists",
        "OrchardCore.Liquid",
        "OrchardCore.Markdown",
        "OrchardCore.Media",
        "OrchardCore.Menu",
        "OrchardCore.Queries",
        "OrchardCore.Title",
        "OrchardCore.Templates",
        "OrchardCore.Widgets",

        // Themes
        "TheCreativeTheme",
        "TheAdmin",
        "SafeMode"
      ]
    },
    {
      "name": "themes",
      "admin": "TheAdmin",
      "site": "TheCreativeTheme"
    },
    {
      "name": "settings",
      "HomeRoute": {
        "Action": "Display",
        "Controller": "Item",
        "Area": "OrchardCore.Contents",
        "ContentItemId": "[js: variables('landingContentItemId')]"
      },
      "LayerSettings": {
        "Zones": [ "Footer" ]
      }
    },
    {
      "name": "ContentDefinition",
      "ContentTypes": [
        {
          "Name": "LiquidPage",
          "DisplayName": "Liquid Page",
          "Hidden": false,
          "Settings": {
            "ContentTypeSettings": {
              "Creatable": true,
              "Draftable": true,
              "Versionable": true,
              "Listable": true
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "LiquidPage",
              "Name": "LiquidPage",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "2"
                }
              }
            },
            {
              "PartName": "AutoroutePart",
              "Name": "AutoroutePart",
              "Settings": {
                "AutoroutePartSettings": {
                  "AllowCustomPath": true,
                  "Pattern": "{{ Model.ContentItem | display_text | slugify }}",
                  "ShowHomepageOption": true
                },
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "LiquidPart",
              "Name": "LiquidPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "3"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "HtmlWidget",
          "DisplayName": "Html",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "HtmlWidget",
              "Name": "HtmlWidget",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Page",
          "DisplayName": "Page",
          "Settings": {
            "ContentTypeSettings": {
              "Creatable": true,
              "Draftable": true,
              "Versionable": true,
              "Listable": true,
              "Securable": true
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Page",
              "Name": "Page",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "3"
                }
              }
            },
            {
              "PartName": "AutoroutePart",
              "Name": "AutoroutePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                },
                "AutoroutePartSettings": {
                  "AllowCustomPath": true,
                  "Pattern": "{{ Model.ContentItem | display_text | slugify }}",
                  "ShowHomepageOption": true
                }
              }
            },
            {
              "PartName": "FlowPart",
              "Name": "FlowPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "2"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "LandingPage",
          "DisplayName": "Landing Page",
          "Settings": {
            "ContentTypeSettings": {
              "Creatable": true,
              "Draftable": true,
              "Versionable": true,
              "Listable": true,
              "Securable": true
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "LandingPage",
              "Name": "LandingPage",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "8"
                }
              }
            },
            {
              "PartName": "AutoroutePart",
              "Name": "AutoroutePart",
              "Settings": {
                "AutoroutePartSettings": {
                  "AllowCustomPath": true,
                  "Pattern": "{{ Model.ContentItem | display_text | slugify }}",
                  "ShowHomepageOption": true
                },
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            },
            {
              "PartName": "BagPart",
              "Name": "Services",
              "Settings": {
                "ContentTypePartSettings": {
                  "DisplayName": "Services",
                  "Description": "Provides a collection behavior for your content item.",
                  "Position": "3"
                },
                "BagPartSettings": {
                  "ContainedContentTypes": [
                    "Service"
                  ]
                }
              }
            },
            {
              "PartName": "BagPart",
              "Name": "Portfolio",
              "Settings": {
                "BagPartSettings": {
                  "ContainedContentTypes": [
                    "Project"
                  ]
                },
                "ContentTypePartSettings": {
                  "DisplayName": "Portfolio",
                  "Description": "Provides a collection behavior for your content item.",
                  "Position": "4"
                }
              }
            },
            {
              "PartName": "BagPart",
              "Name": "About",
              "Settings": {
                "ContentTypePartSettings": {
                  "DisplayName": "About",
                  "Description": "A collection of milestones",
                  "Position": "5"
                },
                "BagPartSettings": {
                  "ContainedContentTypes": [
                    "Milestone"
                  ]
                }
              }
            },
            {
              "PartName": "BagPart",
              "Name": "Team",
              "Settings": {
                "ContentTypePartSettings": {
                  "DisplayName": "Team",
                  "Description": "Team members",
                  "Position": "6"
                },
                "BagPartSettings": {
                  "ContainedContentTypes": [
                    "TeamMember"
                  ]
                }
              }
            },
            {
              "PartName": "BagPart",
              "Name": "Clients",
              "Settings": {
                "ContentTypePartSettings": {
                  "DisplayName": "Clients",
                  "Description": "Provides a collection behavior for your content item.",
                  "Position": "7"
                },
                "BagPartSettings": {
                  "ContainedContentTypes": [
                    "Client"
                  ]
                }
              }
            }
          ]
        },
        {
          "Name": "Service",
          "DisplayName": "Service",
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Service",
              "Name": "Service",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "2"
                }
              }
            },
            {
              "PartName": "HtmlBodyPart",
              "Name": "HtmlBodyPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Project",
          "DisplayName": "Project",
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Project",
              "Name": "Project",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "2"
                }
              }
            },
            {
              "PartName": "HtmlBodyPart",
              "Name": "HtmlBodyPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Milestone",
          "DisplayName": "Milestone",
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Milestone",
              "Name": "Milestone",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            },
            {
              "PartName": "HtmlBodyPart",
              "Name": "HtmlBodyPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "2"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            }
          ]
        },
        {
          "Name": "TeamMember",
          "DisplayName": "Team Member",
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "TeamMember",
              "Name": "TeamMember",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Client",
          "DisplayName": "Client",
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Client",
              "Name": "Client",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Container",
          "DisplayName": "Container",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Container",
              "Name": "Container",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            },
            {
              "PartName": "FlowPart",
              "Name": "FlowPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "1"
                }
              }
            }
          ]
        },
        {
          "Name": "Blockquote",
          "DisplayName": "Blockquote",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Blockquote",
              "Name": "Blockquote",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "ImageWidget",
          "DisplayName": "Image",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Image",
              "Name": "Image",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "LiquidWidget",
          "DisplayName": "Liquid",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "LiquidPart",
              "Name": "LiquidPart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Image",
          "DisplayName": "Image",
          "Settings": {
            "ContentTypeSettings": {
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Image",
              "Name": "Image",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            },
            {
              "PartName": "TitlePart",
              "Name": "TitlePart",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Paragraph",
          "DisplayName": "Paragraph",
          "Settings": {
            "ContentTypeSettings": {
              "Draftable": true,
              "Versionable": true,
              "Securable": true,
              "Stereotype": "Widget"
            }
          },
          "ContentTypePartDefinitionRecords": [
            {
              "PartName": "Paragraph",
              "Name": "Paragraph",
              "Settings": {
                "ContentTypePartSettings": {
                  "Position": "0"
                }
              }
            }
          ]
        }
      ],
      "ContentParts": [
        {
          "Name": "HtmlWidget",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "HtmlField",
              "Name": "Content",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Content",
                  "Editor": "Multiline",
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "MenuItem",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "TextField",
              "Name": "Link",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Link",
                  "Position": "0"
                }
              }
            }
          ]
        },
        {
          "Name": "Service",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "TextField",
              "Name": "IconClass",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Icon Class",
                  "Position": "0"
                },
                "TextFieldSettings": {
                  "Hint": "The icon css class from font-awesome. e.g., fa-laptop, fa-shopping-cart, fa-lock"
                }
              }
            }
          ]
        },
        {
          "Name": "Project",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "MediaField",
              "Name": "Image",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Image",
                  "Position": "1"
                },
                "MediaFieldSettings": {
                  "Required": true,
                  "Multiple":  false
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "Category",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Category",
                  "Position": "0"
                },
                "TextFieldSettings": {
                  "Hint": "The category that is displayed below the title"
                }
              }
            }
          ]
        },
        {
          "Name": "Milestone",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "TextField",
              "Name": "Date",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Date",
                  "Position": "0"
                }
              }
            },
            {
              "FieldName": "MediaField",
              "Name": "Image",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Image",
                  "Position": "1"
                },
                "MediaFieldSettings": {
                  "Required": true,
                  "Multiple": false
                }
              }
            }
          ]
        },
        {
          "Name": "TeamMember",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "TextField",
              "Name": "Occupation",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Occupation",
                  "Position": "0"
                }
              }
            },
            {
              "FieldName": "MediaField",
              "Name": "Picture",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Picture",
                  "Position": "1"
                },
                "MediaFieldSettings": {
                  "Required": true,
                  "Multiple": false
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "Twitter",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Twitter",
                  "Position": "2"
                },
                "TextFieldSettings": {
                  "Hint": "Twitter handle, e.g. @BillGates"
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "Facebook",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Facebook",
                  "Position": "3"
                },
                "TextFieldSettings": {
                  "Hint": "Facebook handle"
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "LinkedIn",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "LinkedIn",
                  "Position": "4"
                },
                "TextFieldSettings": {
                  "Hint": "LinkedIn handle"
                }
              }
            }
          ]
        },
        {
          "Name": "Client",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "MediaField",
              "Name": "Logo",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Logo",
                  "Position": "0"
                },
                "MediaFieldSettings": {
                  "Required": true,
                  "Multiple": false
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "Url",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Url",
                  "Position": "1"
                }
              }
            }
          ]
        },
        {
          "Name": "Blockquote",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "TextField",
              "Name": "Quote",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Quote",
                  "Position": "0",
                  "Editor": "TextArea"
                }
              }
            }
          ]
        },
        {
          "Name": "Image",
          "Settings": {
            "ContentPartSettings": {
              "Attachable": true
            }
          },
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "MediaField",
              "Name": "Media",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Image",
                  "Position": "0"
                },
                "MediaFieldSettings": {
                  "Required": true,
                  "Multiple": false
                }
              }
            },
            {
              "FieldName": "TextField",
              "Name": "Caption",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Caption",
                  "Position": "1"
                },
                "TextFieldSettings": {
                  "Hint": "A description of the image used as title or alternate text"
                }
              }
            }
          ]
        },
        {
          "Name": "Paragraph",
          "Settings": {},
          "ContentPartFieldDefinitionRecords": [
            {
              "FieldName": "HtmlField",
              "Name": "Content",
              "Settings": {
                "ContentPartFieldSettings": {
                  "DisplayName": "Content",
                  "Position": "0",
                  "Editor": "Wysiwyg"
                }
              }
            }
          ]
        }
      ]
    },
    {
      "name": "content",
      "Data": [
        {
          "ContentItemId": "[js: uuid()]",
          "ContentType": "HtmlWidget",
          "DisplayText": "Footer",
          "Latest": true,
          "Published": true,
          "Owner": "[js: parameters('AdminUsername')]",
          "Author": "[js: parameters('AdminUsername')]",
          "LayerMetadata": {
            "Layer": "Always",
            "Zone": "Footer",
            "RenderTitle": false,
            "Position": 10
          },
          "HtmlWidget": {
            "Content": {
              "Html": "[file:text('Snippets/footer.html')]"
            }
          }
        }
      ]
    },
    {
      "name": "layers",
      "Layers": [
        {
          "Name": "Always",
          "Rule": "true",
          "Description": "The widgets in this layer are displayed on any page of this site."
        },
        {
          "Name": "Homepage",
          "Rule": "isHomepage()",
          "Description": "The widgets in this layer are only displayed on the homepage."
        }
      ]
    },
    {
      "name": "Content",
      "data": [
        {
          "ContentItemId": "[js:uuid()]",
          "ContentItemVersionId": "[js:uuid()]",
          "ContentType": "Menu",
          "DisplayText": "Main Menu",
          "Latest": true,
          "Published": true,
          "Owner": "[js: parameters('AdminUsername')]",
          "Author": "[js: parameters('AdminUsername')]",
          "MenuPart": {},
          "TitlePart": {
            "Title": "Main Menu"
          },
          "MenuItemsListPart": {
            "MenuItems": [
              {
                "ContentType": "LinkMenuItem",
                "ContentItemId": "[js:uuid()]",
                "LinkMenuItemPart": {
                  "Name": "Services",
                  "Url": "~/#services"
                }
              },
              {
                "ContentType": "LinkMenuItem",
                "ContentItemId": "[js:uuid()]",
                "LinkMenuItemPart": {
                  "Name": "Portfolio",
                  "Url": "~/#portfolio"
                }
              },
              {
                "ContentType": "LinkMenuItem",
                "ContentItemId": "[js:uuid()]",
                "LinkMenuItemPart": {
                  "Name": "About",
                  "Url": "~/#about"
                }
              },
              {
                "ContentType": "LinkMenuItem",
                "ContentItemId": "[js:uuid()]",
                "LinkMenuItemPart": {
                  "Name": "Team",
                  "Url": "~/#team"
                }
              },
              {
                "ContentType": "LinkMenuItem",
                "ContentItemId": "[js:uuid()]",
                "LinkMenuItemPart": {
                  "Name": "Contact",
                  "Url": "~/#contact"
                }
              }
            ]
          },
          "AliasPart": {
            "Alias": "main-menu"
          }
        },
        {
          "ContentItemId": "[js:uuid()]",
          "ContentItemVersionId": "[js:uuid()]",
          "ContentType": "LandingPage",
          "DisplayText": "[js: parameters('SiteName')]",
          "Latest": true,
          "Published": true,
          "Owner": "[js: parameters('AdminUsername')]",
          "Author": "[js: parameters('AdminUsername')]",
          "LandingPage": {},
          "AutoroutePart": {
            "Path": "home-page",
            "SetHomepage": true
          },
          "TitlePart": {
            "Title": "[js: parameters('SiteName')]"
          },
          "Services": {
            "ContentItems": [
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Service",
                "DisplayText": "E-Commerce",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Service": {
                  "IconClass": {
                    "Text": "fa-shopping-cart"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Minima maxime quam architecto quo inventore harum ex magni, dicta impedit."
                },
                "TitlePart": {
                  "Title": "E-Commerce"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Service",
                "DisplayText": "Responsive Design",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Service": {
                  "IconClass": {
                    "Text": "fa-laptop"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Minima maxime quam architecto quo inventore harum ex magni, dicta impedit."
                },
                "TitlePart": {
                  "Title": "Responsive Design"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Service",
                "DisplayText": "Web Security",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Service": {
                  "IconClass": {
                    "Text": "fa-lock"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Minima maxime quam architecto quo inventore harum ex magni, dicta impedit."
                },
                "TitlePart": {
                  "Title": "Web Security"
                }
              }
            ]
          },
          "Portfolio": {
            "ContentItems": [
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Threads",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/01-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Illustrations"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Threads"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Explore",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/02-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Graphic Design"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Explore"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Finish",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/03-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Identity"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Finish"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Lines",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/04-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Branding"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Lines"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Southwest",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/05-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Web Design"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Southwest"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Project",
                "DisplayText": "Window",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Project": {
                  "Image": {
                    "Paths": [
                      "/portfolio/06-full.jpg"
                    ]
                  },
                  "Category": {
                    "Text": "Photography"
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Est blanditiis dolorem culpa incidunt minus dignissimos deserunt repellat aperiam quasi sunt officia expedita beatae cupiditate, maiores repudiandae, nostrum, reiciendis facere nemo!"
                },
                "TitlePart": {
                  "Title": "Window"
                }
              }
            ]
          },
          "About": {
            "ContentItems": [
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Milestone",
                "DisplayText": "Our Humble Beginnings",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Milestone": {
                  "Date": {
                    "Text": "2009-2011"
                  },
                  "Image": {
                    "Paths": [
                      "/about/1.jpg"
                    ]
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sunt ut voluptatum eius sapiente, totam reiciendis temporibus qui quibusdam, recusandae sit vero unde, sed, incidunt et ea quo dolore laudantium consectetur!"
                },
                "TitlePart": {
                  "Title": "Our Humble Beginnings"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Milestone",
                "DisplayText": "An Creative is Born",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Milestone": {
                  "Date": {
                    "Text": "MARCH 2011"
                  },
                  "Image": {
                    "Paths": [
                      "/about/2.jpg"
                    ]
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sunt ut voluptatum eius sapiente, totam reiciendis temporibus qui quibusdam, recusandae sit vero unde, sed, incidunt et ea quo dolore laudantium consectetur!\r\n"
                },
                "TitlePart": {
                  "Title": "An Creative is Born"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Milestone",
                "DisplayText": "Transition to Full Service",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Milestone": {
                  "Date": {
                    "Text": "DECEMBER 2012"
                  },
                  "Image": {
                    "Paths": [
                      "/about/3.jpg"
                    ]
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sunt ut voluptatum eius sapiente, totam reiciendis temporibus qui quibusdam, recusandae sit vero unde, sed, incidunt et ea quo dolore laudantium consectetur!"
                },
                "TitlePart": {
                  "Title": "Transition to Full Service"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Milestone",
                "DisplayText": "Phase Two Expansion",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Milestone": {
                  "Date": {
                    "Text": "JULY 2014"
                  },
                  "Image": {
                    "Paths": [
                      "/about/4.jpg"
                    ]
                  }
                },
                "HtmlBodyPart": {
                  "Html": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sunt ut voluptatum eius sapiente, totam reiciendis temporibus qui quibusdam, recusandae sit vero unde, sed, incidunt et ea quo dolore laudantium consectetur!\r\n"
                },
                "TitlePart": {
                  "Title": "Phase Two Expansion"
                }
              }
            ]
          },
          "Team": {
            "ContentItems": [
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "TeamMember",
                "DisplayText": "Kay Garland",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "TeamMember": {
                  "Occupation": {
                    "Text": "Lead Designer"
                  },
                  "Picture": {
                    "Paths": [
                      "/team/1.jpg"
                    ]
                  },
                  "Twitter": {
                    "Text": "@BillGates"
                  },
                  "Facebook": {
                    "Text": "BillGates"
                  },
                  "LinkedIn": {
                    "Text": "williamhgates"
                  }
                },
                "TitlePart": {
                  "Title": "Kay Garland"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "TeamMember",
                "DisplayText": "Larry Parker",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "TeamMember": {
                  "Occupation": {
                    "Text": "Lead Marketer"
                  },
                  "Picture": {
                    "Paths": [
                      "/team/2.jpg"
                    ]
                  },
                  "Twitter": {
                    "Text": "@BillGates"
                  },
                  "Facebook": {
                    "Text": "BillGates"
                  },
                  "LinkedIn": {
                    "Text": "williamhgates"
                  }
                },
                "TitlePart": {
                  "Title": "Larry Parker"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "TeamMember",
                "DisplayText": "Diana Pertersen",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "TeamMember": {
                  "Occupation": {
                    "Text": "Lead Developer"
                  },
                  "Picture": {
                    "Paths": [
                      "/team/3.jpg"
                    ]
                  },
                  "Twitter": {
                    "Text": "@BillGates"
                  },
                  "Facebook": {
                    "Text": "BillGates"
                  },
                  "LinkedIn": {
                    "Text": "williamhgates"
                  }
                },
                "TitlePart": {
                  "Title": "Diana Pertersen"
                }
              }
            ]
          },
          "Clients": {
            "ContentItems": [
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Client",
                "DisplayText": "Envato",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Client": {
                  "Logo": {
                    "Paths": [
                      "/logos/envato.jpg"
                    ]
                  },
                  "Url": {
                    "Text": "#"
                  }
                },
                "TitlePart": {
                  "Title": "Envato"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Client",
                "DisplayText": "Design Moto",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Client": {
                  "Logo": {
                    "Paths": [
                      "/logos/designmodo.jpg"
                    ]
                  },
                  "Url": {
                    "Text": "#"
                  }
                },
                "TitlePart": {
                  "Title": "Design Moto"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Client",
                "DisplayText": "ThemeForest",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Client": {
                  "Logo": {
                    "Paths": [
                      "/logos/themeforest.jpg"
                    ]
                  },
                  "Url": {
                    "Text": "#"
                  }
                },
                "TitlePart": {
                  "Title": "ThemeForest"
                }
              },
              {
                "ContentItemId": "[js:uuid()]",
                "ContentItemVersionId": "[js:uuid()]",
                "ContentType": "Client",
                "DisplayText": "Creative",
                "Latest": false,
                "Published": false,
                "Owner": null,
                "Author": "[js: parameters('AdminUsername')]",
                "Client": {
                  "Logo": {
                    "Paths": [
                      "/logos/creative-market.jpg"
                    ]
                  },
                  "Url": {
                    "Text": "#"
                  }
                },
                "TitlePart": {
                  "Title": "Creative"
                }
              }
            ]
          }
        }
      ]
    },
    {
      "name": "Templates",
      "Templates": {
        "Content__LandingPage": {
          "Description": "A template for the Landing Page content type",
          "Content": "[file:text('Snippets/landingpage.liquid')]"
        }
      }
    }

  ]
}

```



![Orchard-Core-Theme-030](https://user-images.githubusercontent.com/59172485/90962645-ba382a80-e46e-11ea-84e9-eefed46125ec.png)

Run the application by pressing the green play button.



![Orchard-Core-Theme-031](https://user-images.githubusercontent.com/59172485/90962646-bad0c100-e46e-11ea-924c-db4c937f90d2.png)

Enter in the site name, select the "Creative" Recipe, enter in your credentials and then press the "Finish Setup" button.



![Orchard-Core-Theme-032](https://user-images.githubusercontent.com/59172485/90962647-bb695780-e46e-11ea-93af-a4239d1df5c1.png)

Congratulations you have just create an Orchard Core theme.



# Conclusion

With any Bootstrap template it's easy to create an Orchard Core Theme that can be selected, during the set up process, by using the liquid scripting language. 



# GitHub

The complete source code is located [here](https://github.com/OrchardSkills/OrchardSkills.OrchardCore.TheCreativeTheme).

