# Angular Fundementals

## Componenets
* A component encapsulates the data, the HTML markup and he logic for a view, which is the area of the screen the user sees.

* How to create a component?
    * In the terminal or in CP add ng g c name-of-component
        * ng - angular
        * g - generate 
        * c - component
    * This creates a folder inside src/app folders with the name of the component you created. Inside of that component it creates 4 files, a CSS file, HTML file, spec file which is for writing unit tests for the component and finally a TypeScript file. (It also updates the app.modules.ts file).

## Modules
* A module is a container for a group of related  components. Every angular app has at least one module which we call App module (app.module.ts).
* As your module grows you want to break it into smaller more maintainable Modules. 

---

## How to use a component
1. Create a component
2. Register the component in a module. Normally when you create a component it automatically adds it to the app.module.ts but if not then register the component yourself.
3. Add an element in an HTML markup.

* Directives
    * **Directive** changes the appearance or behavior of a DOM element.
    * An example would be string interpolation {{ title }}

> courses.component.ts
```TypeScript
    
    import {Component} from '@angular/core';

    @Component({
        selector: 'courses' // Add <courses></courses> to your app.compoent.html
        templateUrl: './courses.component.html', // Here you link your .html file to your .ts file
        styleUrls: ['./courses.component.css'] // Here you link your .css file to your .ts file
    })

    export class CoursesComponent {
        title = "Courses";
        courses = ['Angular', 'Vue', 'Svelte']
    }
```

> courses.component.html
```html
    <h1>{{ title }}</h1> <!-- Here use interpolation to add the title -->
    <ul>
        <li *ngFor="let course of courses"> <!-- Using a for loop to loop through the courses array -->
            {{ courses }}
        </li>
    </ul>
```

> app.component.html (external template for our app component) Anything you want to render in the browser must go here.
```html
    <courses></courses>
```

> Register the component to your module (app.module.ts)
```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { AppComponent } from './app.component';

    import { CoursesComponent } from './courses.component';

    @NgModule({
        // Here you add all he components that are part of this module
        declarations: [ 
            AppComponent,
            CoursesComponent
        ],
        // Imports other modules with the components, directives, and pipes that components in the current module need.
        imports: [
            BrowserModule
        ],
        // Provides services that the other application components can use.
        providers: [],
        bootstrap: [AppComponent]
    })
    export class AppModule { }
```

## Services and Dependency injection
* How to create a service?
    * In the terminal or in CP add ng g s name-of-service
        * ng - angular
        * g - generate
        * s - service
    * This creates two files a spec.ts for testing and .ts file for your functionality

* What is a service?
    * A **service** provides some very useful functions. These functions can be call from any component of Angular. This helps to devide the application into smaller pieces. 

* What is Dependency Injection?
    *  **Dependency injection** is a technique in which an object receives other objects that it depends on. These other objects are called dependencies.

> courses.service.ts
```TypeScript
    export class CoursesService{
        getCourses(){
            return ['Angular', 'Vue', 'Svelte'];
        }
    }
```

> courses.component.ts
```TypeScript
    
    import { Component } from '@angular/core';
    import { CoursesService } from './courses.service';

    @Component({
        selector: 'courses'
        templateUrl: './courses.component.html',
        styleUrls: ['./courses.component.css']
    })

    export class CoursesComponent {
        title = "Courses";
        courses;

        // Add your service as a parameter in the constructor
        // Witht this when angular is going to create an instance of our component
        // it looks at this constructor, it sees that this constructor has a dependency. 
        // This dependency is of type courses service 
        constructor(service: CoursesService){
            // Here its saying from CoursesService get me getCourses();
            this.courses = service.getCourses();
        }
    }
```

> last step make sure to register the service in the providers array.
```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { AppComponent } from './app.component';

    import { CoursesComponent } from './courses.component';
    import { CoursesService } from './courses.service';

    @NgModule({
        declarations: [ 
            AppComponent,
            CoursesComponent
        ],
        imports: [
            BrowserModule
        ],
        // Provides services that the other application components can use.
        providers: [
            CoursesService
        ],
        bootstrap: [AppComponent]
    })
    export class AppModule { }
```