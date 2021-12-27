# Dating App - .NET5 / Angular
* Building a Wep App with Dotnet 5, Angular and Entity Framework.
* Following along the course: https://www.udemy.com/course/build-an-app-with-aspnet-core-and-angular-from-scratch/learn/lecture/22400262?start=0#overview

### Section2 - Building a walking skeleton Part One - API
* We will be using the template `Solution File (sln)` from Dotnet Templates, this is a container for our projects. Check all templates running `dotnet new -l`.
* We are going to create a template as well, one that we are going to use is the `ASP .NET Core Web API`.
* Lets create a solution file. This can be used to open up our project in Visual Studio / VSCode:
  * `dotnet new sln`
* Create the API project: 
  * `dotnet new webapi -o API`
* Now we want to add our API Project into our Solution:
  * `dotnet sln add API`