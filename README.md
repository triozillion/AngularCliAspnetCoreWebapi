
Working with Angular/Cli + ASP.NET Web API + VSCode !
===================


This sample is meant as starting steps to create a standard ASP.NET WEB API project using dotnet Cli command and integrate with Angualr library with the help of @Angular/Cli command to make use of all the cli features.


 **Step 1: Install NodeJs:**
-----------------------------

 - Download and install node js from https://nodejs.org/en/download/

 **Step 2: Install @Angular/Cli:**
-----------------------------------

 - Download and install @Angular/Cli using  **npm install -g @angular/cli**

 **Step 3: Install Visual Studio Code:**
-----------------------------------------

- Download and install latest version of Visual Studio Code from https://code.visualstudio.com/docs/setup/windows

 **Step 4: Create @Angular/Cli command line project :**
--------------------------------------------------------

> - Open Visual Studio Code
> - **#File -> Open Folder** (select/ create folder)
> - Navigate to **#View-> Integrated Terminal** 
> - Create Angular project template **:> ng new projectName**   ( e.g ng new AngularAspnetWebapi)
> - Navigate to project folder **#File -> Open Folder -> new directory path** ( e.g AngularAspnetWebapi )

 **Step 5: Create VSCode dotnet cli asp.net web api project :**
----------------------------------------------------------------

> - Your @ new directory ( e.g AngularAspnetWebapi)
> - Navigate to  **#View-> Integrated Terminal** 
> - Crate dotnet web api project **:> dotnet new webapi**  - this command will create web api project at selected folder location 
> - run **npm install** ( this will take some time to download npm packages)
> - don't build the project ( its better to compile and launch application using - **launch.json and tasks.json**  )
 

 **Step 6: Change config file - link web api and angular cli output :**
------------------------------------------------------------------------

> - Navigate & open .angular-cli.json file 
> - Modify output directory path - "outDir":"dist" to **"outDir": "wwwroot"**
```
  // Original/Old config file
  {
    "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
    "project": {
      "name": " AngularAspnetWebapi "
    },
    "apps": [
      {
        "root": "src",
        "outDir": "dist",
        "assets": [
          "assets",
          "favicon.ico"
        ],
        ...
```
```
  // Updated config file
  {
    "$schema": "./node_modules/@angular/cli/lib/config/schema.json",
    "project": {
      "name": " AngularAspnetWebapi "
    },
    "apps": [
      {
        "root": "src",
        "outDir": "wwwroot",
        "assets": [
          "assets",
          "favicon.ico"
        ],
        ...
```
> - build angular project using **:>ng build** ( angular output at wwwroot folder)

**Step 7: Create Angular component:**
----------------------------------------

 ASP.NET Web API project will create default controller under Controller folder 
```
 [Route("api/[controller]")]
    public class ValuesController : Controller
    {
        // GET api/values
        [HttpGet]
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }
```
Navigate to Integrated Terminal & execute command  **:>ng generate component value**   or **:>ng g c value**  this command will create component and update the module for you
```
		  create src/app/value/value.component.html (24 bytes)
		  create src/app/value/value.component.spec.ts (621 bytes)
		  create src/app/value/value.component.ts (265 bytes)
		  create src/app/value/value.component.css (0 bytes)
		  update src/app/app.module.ts (394 bytes)
```
**Access web api** (communicate angular and asp.net web api)
``` 
 // Defualt generated code - valueComponent 
 import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-value',
  templateUrl: './value.component.html',
  styleUrls: ['./value.component.css']
})
export class ValueComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```
update **value.component.ts** file with following 

>        add Http import --- import { Http  } from '@angular/http'; 
>        create new variable values --- value = {};
>        call web Api  ---  this.http.get(`<api path>`).subscribe( response => this.value = response.json());
 
```
// Updated code 
import { Component, OnInit } from '@angular/core';
import { Http  } from '@angular/http';

@Component({
  selector: 'app-value',
  templateUrl: './value.component.html',
  styleUrls: ['./value.component.css']
})
export class ValueComponent implements OnInit {
  value = {};
  constructor(private http: Http) { }
  
  ngOnInit() {
  }
  getValues() {
      this.http.get(`http://localhost:5000/api/values`).subscribe( response => this.value = response.json());
    }
}
```
Update **value.component.html** with following 
```
// Old code
<p>
  value works!
</p> 
```
```
// New code 
<div>
  <button (click)="getValues()">Load values</button>
  {{ value | json }}
</div>

```

 **Step 8: Angular Routing:**
 -----------------------------

Setup angular routing for new component 
Go to **app.module.ts** file and update following 

```
// Old code
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { ValueComponent } from './value/value.component';

@NgModule({
  declarations: [
    AppComponent,
    ValueComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```
// new code 

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';
import { ValueComponent } from './value/value.component';
import { RouterModule, Routes } from '@angular/router';
import { HttpModule } from '@angular/http';

const appRoutes: Routes = [
    {path: '', redirectTo: 'value', pathMatch: 'full' },
    { path: 'value',  component: ValueComponent },
    { path: '**', redirectTo: ''}
  ];


@NgModule({
  declarations: [
    AppComponent,
    ValueComponent
   ],
  imports: [
    BrowserModule,
    HttpModule,
    RouterModule.forRoot(appRoutes)
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```




Update **app.component.html** file
```
<!--  Old code -->
<!--The content below is only a placeholder and can be replaced.-->
<div style="text-align:center">
  <h1>
    Welcome to {{ title }}!
  </h1>
  <img width="300" alt="Angular Logo" src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNTAgMjUwIj4KICAgIDxwYXRoIGZpbGw9IiNERDAwMzEiIGQ9Ik0xMjUgMzBMMzEuOSA2My4ybDE0LjIgMTIzLjFMMTI1IDIzMGw3OC45LTQzLjcgMTQuMi0xMjMuMXoiIC8+CiAgICA8cGF0aCBmaWxsPSIjQzMwMDJGIiBkPSJNMTI1IDMwdjIyLjItLjFWMjMwbDc4LjktNDMuNyAxNC4yLTEyMy4xTDEyNSAzMHoiIC8+CiAgICA8cGF0aCAgZmlsbD0iI0ZGRkZGRiIgZD0iTTEyNSA1Mi4xTDY2LjggMTgyLjZoMjEuN2wxMS43LTI5LjJoNDkuNGwxMS43IDI5LjJIMTgzTDEyNSA1Mi4xem0xNyA4My4zaC0zNGwxNy00MC45IDE3IDQwLjl6IiAvPgogIDwvc3ZnPg==">
</div>
<h2>Here are some links to help you start: </h2>
<ul>
  <li>
    <h2><a target="_blank" rel="noopener" href="https://angular.io/tutorial">Tour of Heroes</a></h2>
  </li>
  <li>
    <h2><a target="_blank" rel="noopener" href="https://github.com/angular/angular-cli/wiki">CLI Documentation</a></h2>
  </li>
  <li>
    <h2><a target="_blank" rel="noopener" href="https://blog.angular.io/">Angular blog</a></h2>
  </li>
</ul>

```
```
<!-- component navigation placeholder -->
<div class='container-fluid'>
  <div class='row'>
     <div class='col-sm-12 body-content'>
          <router-outlet></router-outlet>
      </div>
  </div>
</div>
```


 **Step 9: Update startup.cs file :**
 -----------------------------

Add **UseDefaultFiles** ( web server look for default file index.html ) and **UseStaticFile** ( web server look for wwwroot folder ) to Startup.cs middleware 

```
//Old code
 if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            app.UseMvc();
``` 

```
//New code

 if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            app.UseDefaultFiles();
            app.UseStaticFiles();
            app.UseMvc();

```


 **Step 10: Execute and Run project :**
 -----------------------------

There are many way to configure build configuration below is the simplest one 

1) Compile angular project using **:>ng build** 

2) Navigate Debug -> Start debugging 
      > it will open command palette..
      > Select **.Net Core**
      > It will create launch.json and tasks.json  or it will open assistance "Required assets to build and debug are missing from"<folder>". add them? -- click on **Yes** 

Click on **Load values** button will connect to api and fetch value.    




