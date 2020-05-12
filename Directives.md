# Structural Directives
* Structural directives are responsible for HTML layout. They shape or reshape the DOM's structure, typically by adding, removing or manipulating elements.
* Structural directives are easy to recognize since they lead with an *.

## ngIF
* ngIf is the simple structural directive and the easiest to understand.
* It takes a boolean expression (true or false) and makes an entire chunk of the DOM appear or disapear.

> app.component.html
```HTML
    <!--Simple Syntax-->
    <!--If courses length is greater than 0 the show List of Courses-->
    <div *ngIf="courses.length > 0">
        List of Courses
    </div>
    <!--If courses length is equal to 0 then NO courses yet-->
    <div *ngIf="courses.length === 0">
        NO courses yet
    </div>



    <!--Using else-->
    <!--noCourses corresponds to #noCourses (Notice: No need to add the # here)-->
    <div *ngIf="courses.length > 0; else noCourses">
        List of Courses
    </div>
    <!--Using the ng-template tag to simply define a template-->
    <ng-template #noCourses>
        NO Courses yet
    </ng-template>



    <!--Using then and else-->
    <div *ngIf="courses.length > 0; then coursesList; else noCourses">
    </div>
    <ng-template #coursesList>
        List of Courses
    </ng-template>
    <ng-template #noCourses>
        NO Courses yet
    </ng-template>

```
> app.component.ts
```TypeScript
    courses = ['Course 1', 'Course 2'];
```

----

## Hidden Property
* Can also use the **Hidden Property** to show or hide part of a page.

> app.component.html
```HTML
    <!--Show only if this condition is false-->
    <div [hidden]="courses.length === 0">
        List of Courses
    </div>
   <!--Show this only if the condition is false-->
    <div [hidden]="courses.length > 0">
        NO courses yet
    </div>
```

> app.component.ts
```TypeScript
    courses = ['Course 1', 'Course 2'];
```

* NOTE: The main difference between the **Hidden Property** and ***ngIf** is that the hidden property shows both divs in the DOM and the *ngIf removes the template that is condition is false.

---

## ngSwitchCase
* Similar to *ngIf but with ngSwitchCase you can have multiple conditions.
> app.component.html
```HTML
    <ul>
        <li>
            <a (click)="viewMode = 'map'">Map View</a>    
        </li>
        <li>
            <a (click)="viewMode = 'list'">List View</a>
        </li>
    </ul>

    <div [ngSwitch]="viewMode">
        <div *ngSwitchCase="'map'">Map View Content</div>
        <div *ngSwitchCase="'list'">List View Content</div>
        <div *ngSwitchDefault>OtherWise</div>
    </div>
```
> app.component.ts
```TypeScript
    // to render the *ngSwitchDefault change the map to something else
    viewMode = "map";
```

---

## ngFor and Change Detection

* To render a list of objects

> app.component.ts
```TypeScript
    courses = [
        {id: 1, name: 'Course 1'},
        {id: 2, name: 'Course 2'},
        {id: 3, name: 'Course 3'},
        {id: 4, name: 'Course 4'}
    ];

    onAdd(){
        this.courses.push({id: 5, name: 'Course 5'});
    }

    onRemove(course){
        // finding the index of the object
        let index = this.courses.indexOf(course);
        // then we go to that index and delete that object
        this.courses.splice(index, 1);
    }
```
> app.component.html
```HTML
    <!--Render the names-->
    <ul>
        <li *ngFor="let course of courses">
            {{ course.name }}
        </li>
    </ul>

    <!--Render with index-->
    <ul>
        <li *ngFor="let course of courses; index as i">
            {{i}} - {{ course.name }}
        </li>
    </ul>

    <!--Render even rows-->
    <ul>
        <li *ngFor="let course of courses; even as isEven">
            {{ course.name }} <span *ngIf="isEven">(EVEN)</span>
        </li>
    </ul>

    <!--Change detection-->
    <button (click)="onAdd()">Create</button>
    <ul>
        <li *ngFor="let course of courses">
            {{ course.name }}
            <button (click)="onRemove(course)">REMOVE</button>
        </li>
    </ul>
```
> Output
```
    * Course 1
    * Course 2
    * Course 3
    * Course 4

    
    * 0 - Course 1
    * 1 - Course 2
    * 2 - Course 3
    * 3 - Course 4
    

    * Course 1 (EVEN)
    * Course 2 
    * Course 3 (EVEN)
    * Course 4 
```

---

## ngFor and Trackby
* When passing an array (list) of objects to *ngFor this directive renders it to the DOM. But the problem is that when changing some elements of this array, this elements are completly rendered again. So we can use trackby to only render the ones that are new to the array. 
> app.component.ts
```TypeScript
    courses;
    
    loadCourses() {
        this.courses = [
        { id: 1, name: 'course 1' },
        { id: 2, name: 'course 2' },
        { id: 3, name: 'course 3' },
        { id: 4, name: 'course 4' },
        { id: 5, name: 'course 5' }
        ];
    }

    trackCourse(index, course){
        return course ? course.id: undefined;
    }    
```
> app.component.html
```HTML
    <button (click)="loadCourses()">Load Courses</button>
    <ul>
        <li *ngFor="let course of courses; trackBy: trackCourse">
            {{ course.name }}
        </li>
    </ul>
```
---
## ngClass
* no need to repeat class binding multiple times

```HTML
    <!--Multiple class binding-->
    <span [class.glyphicon-star]="isSelected"
          [class.glyphicon-star]="isSelected" 
          (click)="onClick()">
    </span>

    <!--Using ngClass-->
    <span [ngClass]="{
        'glyphicon-star': 'isSelected',
        'glyphicon-star-empty': '!isSelected',
    }"
    (click)="onClick()">
    </span>
```

---

## ngStyle

* An attribute directive that updates styles for the containing HTML element.

* Example
    * If **canSave is true** then the button will have: 
        * backgroundColor of blue
        * color of the text is white
        * fontWeight is bold
    * If **canSave is false** then the button will have: 
        * backgroundColor of gray
        * color of the text is black
        * fontWeight is normal
```HTML
    <!--Without ngStyle-->
    <button
        [style.backgroundColor]="canSave ? 'blue': 'gray'"
        [style.color]="canSave ? 'white': 'black'"
        [style.fontWeight]="canSave ? 'bold': 'normal'"
    >Save</button>

    <!--Using ngStyle-->
    <button
        [ngStyle]="{
            'backgroundColor': canSave ? 'blue': 'gray',
            'color': canSave ? 'white': 'black',
            'fontWeight': canSave ? 'bold': 'normal'
        }"
    >Save</button>
```

```TypeScript
    canSave = true;
```

---

## Safe Traversal Operator

>app.component.ts
```TypeScript
    task = {
        title: 'Review applications',
        assignee: {
        name: 'John Smith'
        }
    }
```
>app.component.html
```HTML
    <!--This will render John Smith but what if name is empty
    then we will get an error in the console and we dont want that so
    we can either use ngIf or the safe traversal operator (?)-->
    <span>{{task.assignee.name}}</span> 

    <!--Only display assignee if it has a value-->
    <span *ngIf="task.assignee">{{task.assignee.name}}</span> 
    
    <!--usign safe Traversal operator by using the ? 
    So if the assignee is null the there wont be an error-->
    <span >{{task.assignee?.name}}</span>
```

---
## Creating Custome Directives
* Sometimes you want to have control over the behaviour of DOM elements
* Example - The user inserts a name in uppercase but you want to format the name in lowercase.

1. Create a directive file
    * ng g d input-format
    * This will create two fiels: input-format.directive.ts and input-format.directive.spe.ts

>app.component.html
```HTML
    <input type="text" [appInputFormat]="'uppercase'">
```
>input-format.directive.ts
```TypeScript
    // Need to import HostListener that allows us to subscribe to the event raised 
    // from the DOM element that is the DOM element that is Hosting this directive. 
    // (The DOM that has this attribute).

    // ElementRef - is a service in Angular that gives us access to the DOM object.
    import { Directive, HostListener, ElementRef, Input } from '@angular/core';

    @Directive({
    selector: '[appInputFormat]'
    })
    export class InputFormatDirective {

        @Input('appInputFormat') format;

        constructor(private el: ElementRef) { }

        // blur is used when user clicks of the input field
        @HostListener('blur') onBlur() {
            // get the input value
            let value: string = this.el.nativeElement.value;

            // only if we want to format name into lowercase else format name into uppercase
            if (this.format == 'lowercase') {
                this.el.nativeElement.value = value.toLowerCase();
            } else {
                this.el.nativeElement.value = value.toUpperCase();
            }
        }
    }
```