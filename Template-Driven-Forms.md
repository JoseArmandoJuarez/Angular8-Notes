# Template-Driven Forms
* You use forms to log in, submit a help request, place an order, book a flight and perform countles other data-entry tasks.

## Building a Bootstrap Form

* First in the terminal create a component -> ng g c contact-form
* Install Bootstrap
    * In the terminal
    ```
        npm install bootstrap --save
    ```
    * In your style.css
    ```CSS
        @import "~bootstrap/dist/css/bootstrap.css";
    ```

> contact-form.component.html
```HTML
    <form>
        <div class="form-group">
            <label for="firstName">First Name</label>
            <input id="firstName" type="text" class="form-control">
        </div>
        <div class="form-group">
            <label for="comment">Comment</label>
            <textarea id="comment" cols="30" rows="10" class="form-control"></textarea>
        </div>
        <button class="btn btn-primary"></button>
    </form>
```

### Form Control
* For each input of the form you must create an instance of the control class. With the control 
class you can check the value stored in the input field, see if the input field has been touched 
or untouched, see if the value has changed, if its valid or not and validation errors. (For each 
field in the form you must need a control class).

### Form Group
* Represents a group of group controls in a form. For example you have a form group for the shipping address and another for biling address.

---

## ngModel
* The ngModel is a directive which binds input, select and textarea, and stores the required user value 
    in a variable and we can use that variable whenever we require that value. It also is used 
    during validations in a form. ngModelController which is created and exposed by this directive.
* The ngModel is responsible for:
    * Binding the view into the model, which other directives such as input, textarea or select require.
    * Providing validation behavior (i.e. required, number, email, url)
    * Keeping the state of the control (i.e. Valid/Invalid , dirty/pristine, touched/untouched, validation errors).
    * Setting related css classes on the element (ng-valid, ng-invalid, ng-touched, ng-untouched ...) and animations.
    * Registering the control with its parent form.

### Example
> control-form.component.html
```HTML
     <form>
        <div class="form-group">
            <label for="firstName">First Name</label>
            <!--Need ngModel and name="" for this to work-->
            <!--Create a log() method to see what happenning-->
            <!--Must create variable that contains the value ngModel and pass it to the log-->
            <input ngModel name="firstName" #firstName="ngModel" (change)="log(firstName)" id="firstName" type="text" class="form-control">
        </div>
        <button class="btn btn-primary"></button>
    </form>
```
> control-form.component.ts
```TypeScript
    log(x){
        console.log(x);
    }
```

> Output
```
    * Type something in the input field and press tab and check the console and you should get:
    
    ngModel {//...}

    * And ngModel is an object with properties such as: asyncValidator, control, dirty, disabled, enabled, errors and much more.
```

---

## Adding Validation

> contact-form.component.html
```HTML
    <form>
        <div class="form-group">
            <label for="firstName">First Name</label>
            <!--Add form requirements such as minimumLength, maxLength, pattern and required which is the most important-->
            <input ngModel name="firstName" #firstName="ngModel" (change)="log(firstName)" 
                    id="firstName" type="text" class="form-control" 
                    minlength="3" maxlength="10" pattern="banana" required>

            <!--If the input field (which is out variable in the input that contains the value)
             is touched and user does not write a valid input then show the errors. -->
            <div class="alert alert-danger" *ngIf="firstName.touched && !firstName.valid">
                <div *ngIf="firstName.errors.required">
                    First Name is Required.
                </div>
                <div *ngIf="firstName.errors.minlength">
                    First Name should be minimum {{ firstName.errors.minlength.requiredLength }} characters.
                </div>
                <div *ngIf="firstName.errors.pattern">
                    First Name does not match the pattern.
                </div>
            </div>
        </div>
    </form>
```

---

## ngForm

* Creates a top-level FormGroup instance and binds it to a form to track aggregate form value and validation status.

```HTML
    <!--definie variable  and set it to ngForm-->
    <form #f="ngForm" (ngSubmit)="submit(f)">
        <!---->
        <button class="btn btn-primary">Submit</button>
    </form>
```

```TypeScript
    submit(f){
        console.log(f);
    }
```
> output
```
    * type valid input into the input field and press submit.
    * check the console and you should get an object: NgForm{ //... }
```

----

## ngModelGroup
* Creates and binds a FormGroup instance to a DOM element.
* Use this directive to validate a sub-group of your form separately 
    from the rest of your form, or if some values in your domain model 
    make more sense to consume together in a nested object.
```HTML
    <form #f="ngForm" (ngSubmit)="submit(f)">
        <!--Can get a reference by creating a template variable like #contact-->
        <div ngModelGroup="contact" #contact="ngModelGroup">
            <div class="form-group">
                <label for="firstName">First Name</label>
                <input ngModel name="firstName" #firstName="ngModel" (change)="log(firstName)" 
                        id="firstName" type="text" class="form-control" 
                        minlength="3" maxlength="10" pattern="banana" required>

                <div class="alert alert-danger" *ngIf="firstName.touched && !firstName.valid">
                    <div *ngIf="firstName.errors.required">
                        First Name is Required.
                    </div>
                    <div *ngIf="firstName.errors.minlength">
                        First Name should be minimum {{ firstName.errors.minlength.requiredLength }} characters.
                    </div>
                    <div *ngIf="firstName.errors.pattern">
                        First Name does not match the pattern.
                    </div>
                </div>
            </div>
        </div>
       
        <button class="btn btn-primary">Submit</button>
    </form>
    
```

---

## Disabling the submit button

* You want to enable the button only when all inputs are valid

```HTML
    <form #f="ngForm" (ngSubmit)="submit(f)">
        <!--Checking the validations a whole-->
        <button class="btn btn-primary" [disabled]="!f.valid">Submit</button>
    </form>
    
```