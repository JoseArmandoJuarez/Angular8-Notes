# Building Reusable Components

## Component API
* API - Application Programming Interface
* Is a set of routines, protocols and tools for building software aplications.

## Input Properties
* The Input decorator marks a class field as an input property and supplies configuration metadata. The input property is bound to a DOM property in the tamplate. During change detection, Angular automatically updates the data property witht the DOM property's value.

    ### EXAMPLE

    > app.component.html
    ```HTML
        <!-- Binding isFavorite to post.isFavorite from app.component.html
            [isFavorite] corresponds to isFavorite from 
            favorite.component.ts-->
        <favorite [isFavorite]="post.isFavorite"></favorite>
    ```
    >favorite.component.html
    ```HTML
        <fa-icon *ngIf="isSelected" [class.redStar]="isSelected" [icon]="faStarHalf" (click)="onClick()"></fa-icon>
        <fa-icon *ngIf="!isSelected" [class.blueStar]="!isSelected" [icon]="faStar" (click)="onClick()"></fa-icon>
    ```

    > favorite.component.ts
    ```TypeScript
        import { Input } from '@angular/core';

        @Component({
            // ...
        })
        export class FavoriteComponent implements OnInit{
            // Input decorator
            // 'isFavorite' corresponds to [isFavorite] in app.component.html
            @Input('isFavorite') isSelected: boolean;

            onClick(){
                this.isSelected = !this.isSelected;
            }
        }
    ```
    > app.component.ts
    ```TypeScript
        // imports...

        @Component({
            // ...
        })

        export class AppCompoent{
            post = {
                title = "Title",
                isFavorite = true
            }   
        } 
    ```
---

## Output Properties and passing event Data
* Similar to the Input Property. It marks a class field as an output property and supplies configuration metadata. The DOM property bound to the output property is automatically updated during change detection.

    ### EXAMPLE
    * We want to get notified when the user clicks on the component (The icon).

    > app.component.html
    ```HTML
        <!--Creating a custom event and it will be called when ever the user clicks on the star-->
        <favorite [isFavorite]="post.isFavorite" (change)="onFavoriteChanged($event)"></favorite>
    ```
    > app.component.ts
    ```TypeScript
        // imports...
        import { FavoriteChangedEventArgs } from './favorite/favorite.component.ts';
        @Component({
            // ...
        })

        export class AppCompoent{
            post = {
                title = "Title",
                isFavorite = true
            }

            onFavoriteChanged(eventArgs: FavoriteChangedEventArgs){
                console.log("Favorite changed: ", eventArgs);
            }   
        } 
    ```

    > favorite.component.ts
    ```TypeScript
        import { Input, Output, EventEmmitter } from '@angular/core';

        @Component({
            // ...
        })
        export class FavoriteComponent{
            @Input('isFavorite') isSelected: boolean;
            // Signifying the type of event value we are emitting, our change output is of type EventEmitter.
            @Output('change') click = new EventEmitter(); 

            onClick(){
                this.isSelected = !this.isSelected;
                // Notifies others that something happened.(To raise or publish an event)
                this.change.emit({newValue: this.isSelected}); 
            }
        }

        // This interface represents the shape of the object that is passed 
        // along with the change event of our favorite component.
        export interface FavoriteChangedEventArgs {
            newValue: boolean
        }
    ```

---

## Styles
* 3 ways to add styles to a component. Just remember the method that you use last in the @Component decorator is the one that will be affective.

    1. Create a styleUrls: [] arrays
        * Inside the array you can have 1 or more css files.
            ```TypeScript
                styleUrls: ['./header.component.css',
                            './body.component.css',
                            './footer.component.css'
                            ]        
            ```
    2. create a styles array (Make sure to user the back tick ` to be able to write multiple lines).
        ```TypeScript
            styles: [`
                //Styles go here
            `]
        ```
    
    3. You can also use the style tag (<style></style>) in the HTML file.
        ```HTML
            <style>
                <!--Styles go here-->
            </style>
        ```
        * NOTE: If you end up writing the styles in the HTML file, it will overwrite the styles you have in the .ts file.

--- 

## View Encapsulation

* Shadow DOM - A specification that enables DOM tree and style encapsulation.
    * Allows us to apply scoped styles to elements without bleeding out to the outer world.
    ### Example
    ```Javascript
        // getting a reference to an element in the html document
        var el =  document.querySelector('favorite');

        // Using the innerHTML property to set its inner html.
        // Setting the heading to color red.
        // The only problem with this implementation it leaks outside of this element
        // so if there is an h1 somewhere else is going to be red as well.
        el.innerHTML = `
            <style> h1{ color: red } </style>
            <h1>Hello</h1>
        `;
    ```
    * Solution using .createShadowRoot() so the h1 style will be scoped only through this element.
    ```Javascript
        var el =  document.querySelector('favorite');
        var root = el.createShadowRoot();
        root.innerHTML = `
            <style> h1{ color: red } </style>
            <h1>Hello</h1>
        `;
    ```

---

## ngContent and ngContainer

> panel.component.html
```HTML
    <div class="card">
        <div class="card-header">
            <ng-content select=".heading"></ng-content>
        </div>
        <div class="card-body">
            <ng-content select=".body"></ng-content>
        </div>
    </div>
```
> app.component.html
```HTML
<bootstrap-panel>
    <div class="heading">
        <h1>Heading</h1>
    </div>
    <div class="body">
        <h2>Body</h2>
        <p>Some content goes here...</p>
    </div>  
</bootstrap-panel>
```
* NOTE: You dont need a selector if you have **only one** ng-content

* If I want to render something without putting it inside a div or another kind of html element use **ng-container** element.

```HTML
    <bootstrap-panel>
        <ng-container class="heading">
            <h1>Heading</h1>
        </ng-container>
        
        <div class="body">
            <h2>Body</h2>
            <p>Some content goes here...</p>
        </div>
    </bootstrap-panel>
```