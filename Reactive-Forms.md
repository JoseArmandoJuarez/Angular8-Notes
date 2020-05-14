# Reactive Forms

* Creating a sign up form.

> sign-up.component.ts
```TypeScript
    // Important imports
    import { FormGroup, FormControl } from '@angular/forms';

    /* Abstract control - In OOP languages we have this concept called inheritence, so if
     you have multiple classes that should have some common behavior and properties,
     instead of implementing this common behavior and common properties in multiple places,
     can defined them once in a parent or base class and then have those other classes inherit
     this properties and behavior form their base class.
    */
    /* So in Angular Abstract control is the base class for form control and form group.
    */

    export class SignupFormComponent {
        form = new FormGroup({
            'username': new FormControl(),
            'password': new FormControl()
        })
    }
```
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div class="form-group">
            <label for="username">Username</label>
            <input formControlName="username" id="username" type="text" class="form-control">
        </div>
        <div class="form-group">
            <label for="password">Password</label>
            <input formControlName="username" id="password" type="text" class="form-control">
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```
* NOTE: Dont forget to import FormsModule and ReactiveFormsModule to app.module.ts

---

## Adding Validation to reactive forms

* When you want to add validation to your form we dont use HTML. 
Must add them when creating Form control objects.

> sign-up.component.ts
```TypeScript
    // Important imports
    import { FormGroup, FormControl, Validators } from '@angular/forms';

    export class SignupFormComponent {
        form = new FormGroup({
            /* 
                setting the initial value to an empty string and 
                then we can pass multiple validation in a array.
            */
            'username': new FormControl('', [
                Validators.required,
                Validators.minLength(3)
            ]),
            'password': new FormControl('', Validators.required)
        })
    }

    get username() {
        return this.form.get('username');
    }
```
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div class="form-group">
            <label for="username">Username</label>
            <input formControlName="username" id="username" type="text" class="form-control">
            <div *ngIf="username.touched && username.invalid" class="alert alert-danger">
                <div *ngIf="username.errors.required">Username is required.</div>
                <div *ngIf="username.errors.minlength">
                    Username should be minimum {{ username.errors.minlength.requiredLength }} characters.
                </div>
            </div>
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```

---

## Implementing Custome Validation

* Create a ts file called username.validators.ts to have all validators in one place.

> username.validators.ts
```TypeScript
    import { AbstractControl, ValidatonErrors } from '@angular/forms';

    export class UsernameValidators {
        // in order to access this from the outside add the static decorator
        static cannotContainSpace(control: AbstractControl) : ValidatonErrors | null {
            // if we have a space in the username then return validation errors.
            if((control.value as string).indexOf(' ') >= 0){
                return { cannotContainSpace: true }
            } else {
                return null;
            }
        }
    }
```
> sign-up.component.ts
```TypeScript
    // Important imports
    import { FormGroup, FormControl, Validators } from '@angular/forms';

    export class SignupFormComponent {
        form = new FormGroup({
            'username': new FormControl('', [
                Validators.required,
                Validators.minLength(3),
                UsernameValidators.cannotContainSpace // make sure to add the validators here
            ]),
            'password': new FormControl('', Validators.required)
        })
    }

    get username() {
        return this.form.get('username');
    }
```
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div class="form-group">
            <label for="username">Username</label>
            <input formControlName="username" id="username" type="text" class="form-control">
            <div *ngIf="username.touched && username.invalid" class="alert alert-danger">
                <div *ngIf="username.errors.required">Username is required.</div>
                <div *ngIf="username.errors.minlength">
                    Username should be minimum {{ username.errors.minlength.requiredLength }} characters.
                </div>
                <!--add the custome validator like this-->
                <div *ngIf="username.errors.cannotContainSpace">
                    Username cannot contain space.
                </div>
            </div>
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```

---

## Asynchronous Operations and Asynchronous Validations

* There are going to be times when you need to call the server to validate a giving value.
* For example to see if a username is taken or not.
* Calling the server is what we call as **Asynchronous oparation**
    * When calling the server there's going to be a call delay. It can be half a second,
    3 seconds it depends on the connection speed. In situtaions like that the process 
    that is executed our code, doesn't want to block while waiting for the result coming back from the server because if that process blocks, the user cannot interact with the browser.
    So that process is going to call the serve behind the scene, and when the result is ready is going to display to the user.
    * Another example of a **Asynchronous oparation** oparation is a Timer function.

> username.validators.ts
```TypeScript
    import { AbstractControl, ValidatonErrors } from '@angular/forms';

    export class UsernameValidators {

        // return type is a promise
        static shouldBeUnique(control: AbstractControl) : Promise<ValidationErrors | null> {
            return new Promise((resolve, reject) =>{
                setTimeOut(() => {
                    if(control.value === 'John'){
                        return { shouldBeUnique: true }
                    } else{
                        return null;
                    }
                }, 2000)
            })
        }
    }
```
> sign-up.component.ts
```TypeScript
    // Important imports
    import { FormGroup, FormControl, Validators } from '@angular/forms';

    export class SignupFormComponent {
        form = new FormGroup({
            'username': new FormControl('', [
                Validators.required,
                Validators.minLength(3),
                UsernameValidators.cannotContainSpace, // make sure to add the validators here
                UsernameValidators.shouldBeUnique
            ]),
            'password': new FormControl('', Validators.required)
        })
    }

    get username() {
        return this.form.get('username');
    }
```
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div class="form-group">
            <label for="username">Username</label>
            <input formControlName="username" id="username" type="text" class="form-control">
            <div *ngIf="username.touched && username.invalid" class="alert alert-danger">
                <div *ngIf="username.errors.required">Username is required.</div>
                <div *ngIf="username.errors.minlength">
                    Username should be minimum {{ username.errors.minlength.requiredLength }} characters.
                </div>
                <!--add the custome validator like this-->
                <div *ngIf="username.errors.cannotContainSpace">
                    Username cannot contain space.
                </div>
                <div *ngIf="username.errors.shouldBeUnique">
                    Username is already taken.
                </div>
            </div>
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```

---

## Adding a loader 
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div class="form-group">
            <label for="username">Username</label>
            <input formControlName="username" id="username" type="text" class="form-control">
            
            <div *ngIf="username.pending">Checknig for uniqueness...</div>

            <div *ngIf="username.touched && username.invalid" class="alert alert-danger">
            <!--Validation-->
            </div>
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```

---

## Validation Upon Submit
* This is useful when creating a log in page.
> sign-up.component.html
```HTML
    <form [formGroup]="form" (ngSubmit)="logIn()">
        <div *ngIf="form.errors" class="alert alert-danger">
            <!--Validation errors go here-->
        </div>
        <!---->
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```
> sign-up.component.ts
```TypeScript
    logIn(){
        this.form.setErrors({
            inValidLogin: true;
        });
    }
```

---

## Nested FormGroups
> sign-up.component.ts
```TypeScript
    // Important imports
    import { FormGroup, FormControl, Validators } from '@angular/forms';

    export class SignupFormComponent {
        form = new FormGroup({
            // Use a nested FormGroup when you have a form with multiple subgroups
            account: new FormGroup({
                'username': new FormControl('', Validators.required),
                'password': new FormControl('', Validators.required)
            })
        })
    }

    get username() {
        return this.form.get('account.username');
    }
```
> sign-up.component.html
```HTML
    <form [formGroup]="form">
        <div formGroupName="account"> <!--Add formGroupName-->
            <div class="form-group">
                <label for="username">Username</label>
                <input formControlName="username" id="username" type="text" class="form-control">
                <div *ngIf="username.touched && username.invalid" class="alert alert-danger">
                    <div *ngIf="username.errors.required">Username is required.</div>
                    <div *ngIf="username.errors.minlength">
                        Username should be minimum {{ username.errors.minlength.requiredLength }} characters.
                    </div>
                    <!--add the custome validator like this-->
                    <div *ngIf="username.errors.cannotContainSpace">
                        Username cannot contain space.
                    </div>
                    <div *ngIf="username.errors.shouldBeUnique">
                        Username is already taken.
                    </div>
                </div>
            </div>
        </div>
        <button class="btn btn-primary" type="submit">Sign Up</button>
    </form>
```

---

## Form Array and Form Builder

```HTML
    <form>
        <input type="text" class="form-control" (keyup.enter)="addTopic(topic)" #topic>
        <ul class="list-group">
            <li *ngFor="let topic of topics.controls" (click)="removeTopic(topic)" class="list-group-item">
                {{ topic.value }}
            </li>
        </ul>
    </form>
```
```TypeScript
    import { FormArray, FormGroup, FormControl, FormBuilder, Validators } from '@angular/forms';
    
    // form = new FormGroup({
    //   name: new FormControl('', Validators.required),
    //   contact: new FormGroup({
    //     email: new FormControl(),
    //     phone: new FormControl(),
    //   }),
    //   topics: new FormArray([])
    // });

    form;
    constructor(fb: FormBuilder) {
        this.form = fb.group({
        name: ['', Validators.required],
        contact: fb.group({
            email: [],
            phone: []
        }),
        topics: fb.array([])
        });
    }



    addTopic(topic: HTMLInputElement) {
        this.topics.push(new FormControl(topic.value))
        topic.value = '';
    }

    removeTopic(topic: FormControl) {
        let index = this.topics.controls.indexOf(topic);
        this.topics.removeAt(index);
    }

    get topics() {
        return this.form.get('topics') as FormArray;
    }
```