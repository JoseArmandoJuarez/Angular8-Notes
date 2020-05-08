# Displaying Data and Handling Events

## Property Binding
* {{ title }} - this is known as interpolation. Behind the scenes when Angular compiles our templates, it translates these interpolations into what we call **property binding**.
* With **property binding** we bind a property of a DOM element like src, to a field or a property in our component.

```HTML
    <img src="{{ image }}" />
```

* Example of **Property Binding**
```HTML
    <img [src]="image" />
```

* When to use **Interpolation** or the **Square bracket syntax**?
    * Interpolation - Works well for adding dynamic values between headings or if you are using divs, spans, paragraphs or where ever you want to render text.
    * If I want to use **Property Binding** for the Heading then it gets messy.
    ```HTML
    <!--Interpolation-->
    <h1>{{ title }}</h1>

    <!--Property Binding-->
    <h1 [textContent]="title"></h1>
    ```

---

## Attribute Binding

```TypeScript
    import { Component } from '@angular/core';

    @Component({
        selector: 'courses',
        template: `
            <table>
                <tr>
                    <td [colspan]="colSpan"></td>
                </tr>
            </table>
        `
    })

    export class CoursesComponent{
        colSpan = 2
    }
```

* If you check the console in your browser you will get an error.

    ```
        Unabled Promise rejection: Template parse error: 
            Can't bind to 'colspan' since it isn't a known property of 'td'...
    ```
* Why do we get this error?
    * We have HTML attributes that don't have a representation in the DOM (colSpan is an example of that). When we define the HTML markup and create an actual DOM object for the <td></td> that DOM object does not have a property called colspan, and thats why we get that error. So colspan isn't a known property of td.
        * so If you want to bind this attribute of the td element you need the **Attribute** syntax.
        ```HTML
            <td [attr.colspan]="colSpan"></td>
        ```
---

## Adding Bootstrap
* What is bootstrap?
    * Is a CSS library that gives your applications a modern look and feel.

* Install bootstrap
    ```
    npm install bootstrap --save
    ```

* What this does, is that first it downloads bootstrap and stores it into node modules folder, but the safe flag also adds bootstrap as a dependency in package.json

* How to check which version of bootstrap you have? 
    * Go to your package.json and find "bootstrap": "^3.3.7"  <- Yours might be a different verion
    ```
        ^3.3.7
    ```
    * ^ - can use the most recent major version
    * 3 - major number
    * 3 - minor number
    * 7 - patch number

* Next import it in the style.css
    ```CSS
        /* find the path where bootstrap.css is located */
        @import "~bootstrap/dist/css/bootstrap.css";
    ```

---

## Class Binding
* Here is how to set the class attribute without a binding in plain HTML:
    ```HTML
        <button class="btn btn-primary active">Save</button>
    ```
* You can add or remove CSS class names from an element's class attribute with a class binding.

    > app.component.html
    ```HTML
        <button class="btn btn-primary" [class.active]="isActive">Save</button>
    ```
    > app.component.ts
    ```TypeScript
        // If isActive = true true then the [class.active]="isActive" will be added to the button 
        // If isActive = false then [class.active]="isActive" will be removed from the button
        isActive = true;
    ```

---

## Style Binding
* This is how to set a style attibute without binding in plain HTML

```HTML
    <button class="btn" style="background-color: red;" >Go!</button>
```

* With Style binding you can do so much more

> app.component.html
```HTML
    <!-- If isActive = true then backgroundColor is blue 
        If isActive = false then backgroundColor is red -->
    <button class="btn" [style.backgroundColor]="isActive ? 'blue' : 'red'" >Go!</button>
```
> app.component.ts
```TypeScript
    isActive = true;
 ```

---

## Event Binding
*   When a user interacts with an application in a form, a mouse click, or a mouseover, it generates an event.
> app.component.html
```HTML
    <button class="btn btn-secondary" (click)="onSave()">Save</button>
```
> app.component.ts
```TypeScript
    onSave() {
        console.log("Button was clicked");
    }
```
* Now if we want to see what type of event occurred whe we interact with the button we need to pass the $event as a parameter in the onSave function and in the call back function. Then Check the console in the browser to see the events that occurred.

> app.component.html
```HTML
    <button class="btn btn-secondary" (click)="onSave($event)">Save</button>
```
> app.component.ts
```TypeScript
    onSave($event) {
        console.log("Button was clicked", $event);
    }
```
> In the console you should get something like this and all the properties that you see are part of the $event object.
```JavaScript
    > MouseEvent {isTruested: true, screenX: 50, screenY: 274, clientX: 50, clientY: 110...}
```

--- 

## Event Filtering and Template Variables
* User actions such as clicking a link, pushing a button, and entering text.
    ```HTML
        <input (keyup.enter)="onKeyUp()" />
    ```
    ```TypeScript
        onKeyUp(){
            console.log("ENTER was pressed!");
        }
    ```

* Now lets say you want to see in the console what we type in the input field:
    ```HTML
        <!--This #email is a variable will reference this input fiel. This is known as Template Variable-->
        <input #email (keyup.enter)="onKeyUp(email.value)" />
    ```
    ```TypeScript
        onKeyUp(email){
            console.log(email);
        }
    ```
---

## Two-Way Binding
* Is the synchronization (Two things happening at the same time) between the model and the view. * When data in the model changes, the view reflects the change, and when data in the view changes, the model is updated as well.

* [(ngModel)]
    * I can bind the value of an input field to a variable.
    * If the user changes the value inside the input field, the Angular property will also change its value.


```HTML
    <!--One Way Binding-->
    <input [value]="email" (keyup.enter)="email = $event.target.value; onKeyUp()"/>
    
    <!--Two Way Binding ngModel is used for implementing two way binding-->
    <input [(ngModel)]="email" (keyup.enter)="onKeyUp()"/>
```
```TypeScript
    email = "me@example.com";
    onKeyUp() {
        console.log(this.email);
    }
```

* YOU will get an ERROR in the console because ngModel is part of a library called FormsModuel. So you will need to import it in app.module.ts and add it to the imports array.
> app.module.ts
```TypeScript
    import { FormsModule } from '@angular/forms';

    @NgModule({
        //...
        imports: [
            FormsModule
        ]
        //...
    })
```  

---

## Pipes and Custome pipes
* We use pipes to format data.
* Examples of built in pipes
    * uppercase
    * lowercase
    * number
    * currency
    * percent
* and you can also create custome pipes

```HTML
    <p>{{ courseDetails.title | uppercase}}</p>
    <p>{{ courseDetails.students | number }}</p>
    
    <!--The first number is the number of integer digits
    Then you can give it a minimum or maximum number of digits after the decimal point-->
    <p>{{ courseDetails.rating | number:'1.2-2' }}</p> 
    
    <!--You can change the currency to USD, CAD, EUR...
        true - to indicate to display the currency symbol-->
    <p>{{ courseDetails.price | currency:'AUD':true:'3.2-2' }}</p>
    <p>{{ courseDetails.releaseDate | date:'shortDate'}}</p>
```

```TypeScript
    courseDetails = {
        title: "Angular is awesome",
        students: 30123,
        rating: 4.9745,
        price: 190.95,
        releaseDate: new Date(2016, 3, 1)
    }
```

> output
```
    ANGULAR IS AWESOME
    30,123
    4.97
    A$190.95
    5/8/2020 

```

### Custome Pipes
* You can create your own pipes
* create a file called summary.pipe.ts

> app.component.html
```HTML
    <p>{{ text | summary: 20 }}</p>
```
> app.component.ts
```TypeScript
    text = `
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
    Nam pretium felis convallis arcu viverra mollis vitae vel nunc. 
    Sed posuere, erat vitae interdum laoreet, lacus metus venenatis odio, 
    non imperdiet nibh ex id nisi. Nulla facilisi. Ut nec velit a quam 
    dictum aliquam ac et ipsum. Fusce convallis, elit eu porta consectetur, 
    tellus dui iaculis nisl, scelerisque aliquam justo ipsum sed turpis. 
    Vestibulum ultricies vel metus aliquet vestibulum. Suspendisse faucibus 
    finibus orci sit amet bibendum. Praesent cursus augue non viverra consectetur. 
    Mauris nec gravida augue. Vestibulum sagittis molestie orci, 
    quis porttitor libero posuere quis.
    `
```
> summary.pipe.ts
```TypeScript
    import { Pipe, PipeTransform } from '@angular/core';

    @Pipe({
        name: 'summary' //keyword ass the name of the pipe
    })

    /* 
    what is implements PipeTransform?
    With this we are telling typescript compiler that 
    this class should have the exact same shape as the PipeTransform interface.
    */
    export class SummaryPipe implements PipeTransform {
        
        // value - passing the text
        // limit - is the limit we assign to the pipe in the html file {{ text | summary: 20 }}
        // ? - this question mark means that the limit can be optional
        transform(value: string, limit?: number) {
            // if value is false then return null
            if (!value) {
                return null;
            }

            // checking if we included a limit if not then the limit is 50
            // but because we included a limit (20) then the limit is 20
            let actualLimit = (limit) ? limit : 50;
            // return the value with the limit
            return value.substr(0, actualLimit) + '...';
        }
    }
```
> make sure to register the summary.pipe.ts in app.module.ts

```TypeScript
    //Import summary.pipe.ts
    import { SummaryPipe } from './summary.pipe';

    //And make sure to add it to your declarations arra
    @NgModule({
        declarations:[
            SummaryPipe
        ],
        //...
    })
```