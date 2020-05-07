# TypeScript Fundementals

## Variables

* Types of variable (primitive variables)
```TypeScript
    let a: number;
    let b: boolean;
    let c: string;
    let d: any;
    let e: number[] = [1,2,3];
    let f: any[] = [1, true, 'a', false];

    const ColorRed = 0;
    const ColorGreen = 1;
    const ColorBlue = 2;

    // Enums allow us to to declare a set of named constants
    // a collection of related values that can be a numeric or string value
    enum Color {Red=0, Green=1, Blue=2, Purple=3}
    let backgroundColor = Color.Red;
```
---

## Type Assertions

* Type assertion is a way to tell the compiler what is the type of a variable.

```TypeScript
    let message;
    message = 'abc';
    // 2 wyas to to do Type Assertions!
    //#1 preffix the variable with angle brakets and add the type like string
    let endsWithC = (<string>message).endsWith('c');
    
    //#2 alternativeWay
    let alternativeWay = (message as string).endsWith('c');
```

---

## Functions

### Arrow functions
* What is the use of an Arrow function?
    * Arrow functions make it much easier to work with. They can be use to define  anonymous functions in a simpler way.

    ```JavaScript
    let doLog = function(message){
        console.log(message);
    }

    // Using an arrow function
   let dolog = (message) => {
        console.log(message);
    }
    ```

### Interface
* Instead of writing multiple parameters in your function, create an interface to have all your parameters.

```JavaScript
    // The name of your interface has to be PastelCase
    Interface LabeledValue{
        label: string;
        size: number
    }

    function printLabel (labelObj: LabeledValue) {
        console.log(labelObj.label)
    }

    let myObj = {
        size: 10,
        label: "size 10 Object"
    };

    printLabel(myObj);
```

---

## Classes / Objects / Constructors

* **Class** - Group variables (properties) and functions (methods) that are highly related
* **Cohesion** - Things that are related should be part of one unit (Go Together).
* **Objects** - An object is an instance which contains set of key value pairs. Such as a human is a class and the object are name, height, age...
* **Constructor** - Its purpose is to help prepare the creation of a new instance of the class.

> class and objects
```TypeScript
    class Point{
        x:number;
        y:number;

        draw(){
            console.log('X: ' + this.x + ', Y: ' + this.y)
        }
    }

    let point = new Point(); // creates a new object
    point.x = 1;
    point.y = 2;
    point.draw();
```
> Class, Objects and Constructor
```TypeScript
    class Point{
        x:number;
        y:number;

        // the ? means optional
        constructor(x?:number, y?: number){
            this.x = x;
            this.y = y;
        }

        draw(){
            console.log('X: ' + this.x + ', Y: ' + this.y)
        }
    }

    let point = new Point(); creates a new object
    point.draw();
```

---

## Access modifiers in constructor parameters

* Such as public, private, and protected
    * **Public** - By default, members (properties and methods) of TypeScript class are public. No need to prefix members with the public keyword.
    * **Private** - A private memeber cannot be accessed outside of its containing class.
    * **Protected** - A protected member cannot be accessed outside of its containing class.

```TypeScript
    class Point{

        constructor(private x?:number, private y?: number){
        }

        draw(){
            console.log('X: ' + this.x + ', Y: ' + this.y)
        }
    }

    let point = new Point(1, 2); creates a new object
    point.draw();
```

---

## Properties
* The property looks like a field from the outside, but internally its really a method in the class. But accurately its either one method which is a **getter** or a **setter** or a combination of a **getter** and a **setter**.

```TypeScript
    class Point{

        constructor(private x?:number, private y?: number){
        }

        draw(){
            console.log('X: ' + this.x + ', Y: ' + this.y)
        }

        get X(){
            return this.x;
        }

        set X(value){
            if(value < 0){
                throw new Error('value cannot be less than 0.');
            }
            this.x = value;
        }
    }

    let point = new Point(1, 2); // creates a new object
    let x = point.X;
    point.X = 10;
    point.draw();
```

----

## Modules
* In order to use a class somewhere else in the program, must export it from the outside (Use the export keyword).

> main.ts
```TypeScript
    import {LikeComponent} from './like.component';

    let component = new LikeComponent(10, true);
    component.onClick();
    console.log(`likesCount: ${component.likesCount}, isSelected: ${component.isSelected}`);
```

> like.component.ts
```TypeScript
    export class LikeComponent{

        constructor(private _likesCount: number, private _isSelected:boolean){
        }

        onClick(){
            if(this.isSelected){
                this._likesCount--;
                this._isSelected = false;
            }
            else{
                this._likesCount++;
                this._isSelected = true;
            }

            //Can also do this
            // this.likesCount += (this.isSelected) ? -1 : +1
            // this.isSelected = !this.isSelected;
        }

        get likesCount(){
            return this._likesCount;
        }

        get isSelected(){
            return this._isSelected;
        }
        
    }
```

---