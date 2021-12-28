# Dating App - .NET5 / Angular
* Building a Wep App with Dotnet 5, Angular and Entity Framework.
* Following along the course: https://www.udemy.com/course/build-an-app-with-aspnet-core-and-angular-from-scratch/learn/lecture/22400262?start=0#overview

---

## Section2 - Building a walking skeleton Part One - API
* We will be using the template `Solution File (sln)` from Dotnet Templates, this is a container for our projects. Check all templates running `dotnet new -l`.
* We are going to create a template as well, one that we are going to use is the `ASP .NET Core Web API`.
* Lets create a solution file. This can be used to open up our project in Visual Studio / VSCode:
  * `$ dotnet new sln`
* Create the API project: 
  * `$ dotnet new webapi -o API`
* Now we want to add our API Project into our Solution:
  * `$ dotnet sln add API`
* Before starting our API, we need to make sure our browser trust the certificate that is provided with dotnet SDK:
  * `$ dotnet dev-certs https --trust`
* Start your API by:
  * `cd API`
  * `$ dotnet run` / If you want to refresh the application on file changes use `$ dotnet watch run`
* Creating the User abstraction at `Entities/AppUser.cs`.   
### Entity Framework
* We want now to use **Entity Framework**. We will install a package into our application so that we can make use of entity framework. To facilitate the search and find packages, install a plugin ccalled `NuGet Gallery`.
  * Now press `CTRL + SHIFT + P` and search for `Nuget Gallery` and click it. There you type `entityFramework` and find the one for `SQLite`. Also, choose the same version as your DotNet runtime installed, and tick the box `API.csproj`.
  * Now we added a new class on `Data/DataContext.cs` to define our `DataContext` class that inherits `DbContext`. Now we need to add `DataContext` to our `Startup.cs` on `ConfigureServices`:
    ```C#
            public void ConfigureServices(IServiceCollection services)
            {
                services.AddDbContext<DataContext>(options =>
                {
                    options.UseSqlite("Connection string");
                });
                services.AddControllers();
                services.AddSwaggerGen(c =>
                {
                    c.SwaggerDoc("v1", new OpenApiInfo { Title = "API", Version = "v1" });
                });
            }
    ```
    * Now we need to define our connection string at configuration level. Open `appsettings.Development.json` and paste the following:
    ```
    "ConnectionStrings": {
        "DefaultConnection": "Data source=datingapp.db"
    },
    ```
    * Now we need to install [dotnet-ef](https://www.nuget.org/packages/dotnet-ef/) to perform .NET Command-Line Interface commands, such as `dotnet ef migrations add`: `$ dotnet tool install --global dotnet-ef --version 5.0.4`
    * Before applying migrations, we will need to add `Microsoft.EntityFrameworkCore.Design`. Go to Nuget Gallery and install it there so it get's added to `API.csproj`.
    * Now we apply migrations: `$ dotnet ef migrations add InitialCreate -o Data/Migrations`
    * Now that we have done migration, we will go ahead and update our database: `dotnet ef database update`.
    * We want to look at our db now, we will install the `SQLite` plugin, click `CTRL + SHIFT + P` and then `SQLITE -> Open DB`. The use the SQLITE Explorer at the left bar of VSCode.   

---

## Section3 - Building a walking skeleton Part Two - Angular
* Update Node to version `v16.13.x`.
* Install the Angular 12 globally: `$ npm install -g @angular/cli@12`
* Type `$ ng --version` to check the Angular version we have.
* Lets create an Angular Project: `$ ng new client --strict false`
  * `y` for Angular routing
  * `CSS` for stylesheet
* Install the `Angular Language Service` plugin. Then, on VSCode settings set `"angular.enable-strict-mode-prompt"` to `False`.
* Go to the cliente and start the client: `$ cd client`, `$ ng serve`.
* We are now editing the main component at `client/src/app/app.components.ts`. By doing that, we can open `client/src/app/app.components.html` and use the data defined on `app.components.ts` with interpolation:   
```javascript
// app.components.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'The Dating App!';
}
```   

```html
<!-- app.components.html -->
<h1>{{title}}</h1>
```
* Three addons to install for Angular on Vscode:
  * **Angular Language Service**
  * **Angular Snippets**
  * **Bracket Pair Colorizer 2**
### Make http requesto to Angular component
* We need to add `HttpClientModule` to `app.module.ts` on `imports` section.
* Now we will use our `app.component.ts` to fetch the data and then display on the page:
  * On `app.components.ts` add `constructor(private http: HttpClient) { }` to `export class AppComponent{}`
### Adding CORS support in the API
* Go to `API/Startup.cs` and add CORS to the `ConfigureServices` and then at `Configure`.
### Displaying the fetched users in the browser
* At `app.components.html` edit this:
```html
<h1>{{title}}</h1>

<ul>
    <li *ngFor="let user of users">{{user.id}} - {{user.userName}}</li>
</ul>
```   
### Adding Bootstrap and Font-Awsome
* Angular Bootstrap: `valor-software.com/ngx-bootstrap`, we will be installing **Bootstrap 4** for sake of compatibility with code in the project. 
* On `client/` run: `$ ng add ngx-bootstrap`.
* We will need some icons aswell: `$ npm install font-awesome`