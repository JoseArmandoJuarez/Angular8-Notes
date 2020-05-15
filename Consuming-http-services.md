# Consuming Http Services

* A Fake Http Service that you can use as the back end for an application.
    * http://jsonplaceholder.typicode.com/

1. First thing to consume Http Services go to **app.module.ts** and import HttpModule and add it to the imports array. Also provide the HttpClient class in the dependencies array.

## Getting Data

```TypeScript
    import { HttpModule } from '@angular/http';

    imports:[
        HttpModule
    ]
    prodivers:[
        HttpClient
    ]
```

2. Create a component (posts.component.ts) and in the constructo add http: httpClient. This Http class is used to either get the data, or save the data.

> posts.component.ts
```TypeScript
    import { HttpClient } from '@angular/common/http';

    export class PostsComponent {

    posts = any[];

    constructor(private http: HttpClient) {
        // Here im getting data using the .get()
        http.get('http://jsonplaceholder.typicode.com/posts')
            // Here the Subscribe will subscribe to this observavle which is the get()
            // and when the result is ready it will notify us
            .subscribe(response => {
                // Will give us an array of posts (Check the console in the browser)
                console.log(response.json()); 
                this.posts = response.json();
            }) 
    }
```
> posts.component.html
```HTML
    <!--This is a bootstrap list-->
    <ul class="list-group">
        <li *ngFor="let post of posts" class="list-group-item">
            {{ post.title }}
        </li>
    </ul>
```

* Types of HTTP Requests:
    * GET - Get data
    * POST - Create data
    * PUT - Update data
    * DELETE - Delete data

---

## Creating Data

1. Create an input field
> posts.component.html
```HTML
    <input (keyup.enter)="createPost(title)" #title type="text" class="form-control">
```

2. add the url to a variable
> posts.component.ts
```TypeScript
    import { HttpClient } from '@angular/common/http';

    export class PostsComponent {

        posts = any[];
        private url = 'http://jsonplaceholder.typicode.com/posts';

        constructor(private http: HttpClient) {
            http.get(this.url).subscribe(response => { this.posts = response.json(); }) 
        }

        createPost(title: HTMLInputElement){
            let post = { title: input.value };
            input.value = '';
            // JSON.stringify() method converts a JavaScript object or value to a JSON string
            this.http.post(this.url, JSON.stringify(post))
                .subscribe(response => {
                    console.log(response.json());
                    post['id'] = response.json().id;
                    // adding the post at the begging of the list
                    this.posts.splice(0, 0, post);
                });
        }
    }
```

---

## Update Data

* add a button
> posts.component.html
```HTML
    <input (keyup.enter)="createPost(title)" #title type="text" class="form-control">
    <ul class="list-group">
        <li *ngFor="let post of posts" class="list-group-item">
            <button class="btn btn-primary btn-sm" (click)="updatePost(post)">UPDATE</button>
            {{ post.title }}
        </li>
    </ul>
```

> posts.component.ts
```TypeScript
    import { HttpClient } from '@angular/common/http';

    export class PostsComponent {

        posts = any[];
        private url = 'http://jsonplaceholder.typicode.com/posts';

        constructor(private http: HttpClient) {
            http.get(this.url).subscribe(response => { this.posts = response.json(); }) 
        }

        createPost(title: HTMLInputElement){
            // ...
        }

        updatePost(post){
            /*
            Use patch only to update a few properties in an object. So instead of sending the 
            complete representation of the object like put those to the server, we only send the 
            properties that should be modified by using patch.
            */
            // this.http.patch(this.url, JSON.stringify(post))
            this.http.patch(this.url +  '/' + post.id, JSON.stringify({isRead: true})).
                subscribe(response => {
                    console.log(response.json());
                })
            
        }
    }
```

---

## Deleting Data

> posts.component.html
```HTML
    <input (keyup.enter)="createPost(title)" #title type="text" class="form-control">
    <ul class="list-group">
        <li *ngFor="let post of posts" class="list-group-item">
            <button class="btn btn-primary btn-sm" (click)="deletePost(post)">DELETE</button>
            {{ post.title }}
        </li>
    </ul>
```

> posts.component.ts
```TypeScript
    import { HttpClient } from '@angular/common/http';

    export class PostsComponent {

        posts = any[];
        private url = 'http://jsonplaceholder.typicode.com/posts';

        constructor(private http: HttpClient) {
            http.get(this.url).subscribe(response => { this.posts = response.json(); }) 
        }

        createPost(title: HTMLInputElement){
            // ...
        }

        updatePost(post){
            // ...   
        }

        deletePost(post){
            this.http.delete(this.url + '/' + post.id)
                .subscribe(
                    let index = this.post.indexOf(post);
                    this.post.splice(index, 1)
                )
        }
    }
```

---

## OnInit
* A lifecycle hook that is called after Angular has initialized all data-bound of a directive.
* Define on ngOnInit() method to handle any additional initialization tasks.

> posts.component.ts
```TypeScript
    import { HttpClient } from '@angular/common/http';
    import { OnInit } from '@angular/core';

    // implements is used for compiling time checking.
    export class PostsComponent implements OnInit {

        posts = any[];
        private url = 'http://jsonplaceholder.typicode.com/posts';

        constructor(private http: HttpClient) {
            
        }

        ngOnInit(){
            // An interface that we refer to as a lifecycle hooks
            this.http.get(this.url).subscribe(response => { this.posts = response.json(); }) 
        }
    }
```

### Lifecycle Hooks
* When Angular, creates a component, renders it, creates and renders its children, 
destroys a component this are examples of lidecycle events.
* Examples:
    * OnInit
    * OnChange
    * DoCheck
    * AfterContentInit
    * ...

