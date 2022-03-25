# Angular note

NhanNguyen

some little note by myself to remind what did i learn

---

## Things worth to review:

| No. | Content                                                               |
| --- | --------------------------------------------------------------------- |
|     |                                                                       |
| 0   | [Interview Training](#interview)                             |
| 0.1   | [How Angular actually work - change detection](#how-angular-actually-work)                             |
| 1   | [Overview - Basic](#section-01-the-basic)                             |
|     | [Data Binding](#data-binding)                                         |
|     | [Directive](#directive)                                               |
| 2   | [Component Communication](#component-communication)                   |
|     | [UserRef-Reference to HTML Element](#reference)                       |
|     | [ngOnInit](#ngOnInit)                                                 |
| 3   | [Directive Dive Deeper](#section-07-directive-dive-deeper)            |
|     | [Create your own Directives](#your-own-directive)                     |
|     | [Structural Directive](#structural-directive)                         |
| 4   | [Services & Dependency Injection](#services-and-dependency-injection) |
|     | [Hierarchical Injector of Dependency Injector](#hierachical-injector) |
| 5   | [Routing](#routing)                                                   |
|     | [Route params](#route-params)                                         |
|     | [Route Observable](#route-observable)                                 |
|     | [Query Params](#query-params)                                         |
|     | [Nested Route](#nested-route)                                         |
|     | [Handling strange route](#handling-strange-route)                     |
|     | [Protected Route - Guards](#guards)                                   |
| 6   | [Observable - dive deeper](#observable)                               |
| 7   | [Form](#form)                                                         |
| 8   | [Pipes](#pipes)                                                       |
| 9   | [HTTP Requests](#http-requests)                                       |
| 10  | [Authentication](#authentication)                                     |
| 11  | [NgRx](#NgRx)                                                         |
| 12  | [Async Pipe](#async-pipe)                                             |
| 13  | [Map, switchMap, mergeMap](#map-switchmap-mergemap)                   |

# How Angular actually work

[Extremely helpful article about how `Change Detection` work](https://blog.angular-university.io/how-does-angular-2-change-detection-really-work/#:~:text=Angular%20change%20detection%20is%20a,and%20its%20HTML%20template%20view.)

`Change Detection` is mechanism that `Angular` use for dectecting changes in the DOM.

It patch/overide the default browser APIs ( such as addEventListener apis), and add `change detection` method into this custom addEventListener, by doing this, when everytime the DOM event was triggered, it will triggered the `change detection` and update the UI.

The process of pathching a custom event into default browser API is doing by a ship-in external library in Angular called `Zone.js`

# Section 01 - THE BASIC

`Angular` just only load index.html - and the `ts` file was read first when the application was loaded

`Angular` use `component` to build web functionality - and use `module` to `bundle component` into `packages`

If you want `angular` and `ts` aware one component - you have to `declare` and `import` it in `the module app `

```
@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.scss']
})

Export class ...Component{}
```

`Component` in `Angular` just a Class => You must use `@Component decorator` to make `Angular` recognize this is a `Component`

The `component` and `module` must have a `template` or `templateURL`.

Style can declare as `external file (styleUrls)` or internal file `style`

`Selector` can be another type like attribute (`[app-server]`)or class (`.app-server`). But `best practice` is `app-server` because later angular have `build in attribute selector`

## Interface and Class

`Interface` and `Class` are `blueprint` of the house, we build (implement) the house (by new keyword in class) with this `blueprint` contract

The `contract` meaning we have to declare the `property` or `method` we specify in this `Interface or Class` (if we use `?` operator after property or method => It will become optional)

### Difference between interface and class

`Interface` is a place we defined what property or method must be used in the class that implemented this interface

`Class` is where the actual functionality happen - where we actual implement the method

```
export interface post {
  title: string;
  content: string;
  id?: string;
}
```

## Data binding

### String Interpolation and Property binding

Out put data or logic from TS to HTML

`String Interpolation` is way to binding data from TS to HTML by `{{ expression }}` - remember just only `expression` result in a `string` can pass into `Interpolation` => number will be force into a `string`

`Property binding` is way to binding attribute in HTML tag => dynamic change DOM element when value change

```
<button class="btn btn-success" [disabled]="isTrue">OK</button>
```

### Event binding

React to event that user do in HTML

using `(event without On)` to react to event that user interact with HTML

### Two-way binding

[(ngModal)] will react to change in our `input` and update variable

## Directive

<img src="./img/4.PNG" width="900">

`Directive` is an `instruction` to the `DOM`

Ex: We `directive` / `instruct` where the `component` should place in `template structure`

There are other `directive` built in `Angular` => We use it

`Directive` in `Angular` use `attribute selector` to perform.

### Structural directive

```
<htmlTag *ngIf></htmlTag>
```

=> `*` in `*ngIf` is important because it inform for `Angular` know it will change the DOM. And `directive` it self is ngIf.

### Attribute directive

This does `not add or remove` element in the DOM like `structural directive`
=> It just change the `appearance` element

```
[ngStyle]="{backgroundColor: red}"
```

=> `[]`: attribute binding / `ngStyle`: attribute directive

=> We are use `attribute binding` to `bind` a `property` of the `directive`

```
[ngClass]="{ class: condition expression }"
```

=> Dynamic add or remove class

# Section 03

## Model

`Model` is the `blueprint` of object that we use in a lot of place in our app.

`Model` is a `TypeScript class` it be instantiated and defined how the object create based on this setup should look like

=> Like a `blueprint` => Every instances (objects) will create base on this `blueprint`

# Section 05 - Component Communication

## Parent to child (@Input) (Lec.67)

We use `@Input decorator` to make `property from child` expose to `parent component`

```
  @Input() childElement:
    { name: string;
      type: string;
      content: string
    };
```

In parent component we bind `this child property` with the `property of parent`

```
<app-server-element
    *ngFor="let element of serverElements"
    [childElement]="element"
></app-server-element>

```

## Child to parent (@Output) (Lec.69)

We create our `event` in `child component` - and `emit` it to `parent component` with `data`

`$event` is a `protected argument` => It always reference to the same data we pass in the function

## Cross-component communication

We use `subject` - a special type of `observable` 

> Use eventEmitter in child - parent communication

> Use subject for cross-component communication

## View Encapsulation (Lec.71)

`Angular` made each `css class` have scope to one specific `component`

=> Because each element in HTML template have `unique attribute` of that `component`
=> It make sure only element have that `attribute` will receive the `style` of that `component`

=> `Angular` use technique to `encapsulation` css style

=> You can change default behavior of `encapsulation` in `Angular` by

```
encapsulation: ViewEncapsulation.None
```

## Reference - refer to DOM node

### By local reference

We use `#` as a reference to HTML node
=> It reference can be use only in `Angular HTML Template`
=> We pass it through `TS File` by function

```
<form>
  <input
    type="text"
    id="name"
    #referenceToName
  />
  <button (click)="onClickHandler(referenceToName)"></button>
</form>

```

=> Our `onClickHandler` function will receive `input` node

### By @ViewChild

```
@ViewChild() ('localReference') nameInput: ElementRef;
```

It create a connection between element and property in `TS File`

=> It will allow us to access `ElementRef.nativeElement` where input value of element place

### By @ViewContent

To `reference` to the `content` that `inject` into our component

## Children Property in Angular

`<ng-content></ng-content>` is a special `hook` in `Angular`

=> It work like `children property` in `React`

## Component LifeCircle in React

<img src="./img/1.png" width="900">

`Lec.77` - `Rewatch`

### Constructor

### ngOnchanges

### ngOnInit

This method will run when `component` initialize and `prepare` to appear into the `DOM` => but not into the `DOM`
=>

### ngDoCheck

### ngAfterContentInit

### ngAfterContentChecked

### ngAfterViewInit

### ngAfterViewChecked

### ngOnDestroy

---

---

# Section 07 - Directive Dive Deeper

## Your own Directive

We can create our own `directive` and we `inject` it to our element

`Angular` will `inject` our `directive` to `element` and give us the `reference` to this `DOM NODE`

=> It is `BEST PRACTICE` to use `Renderer2` to access `DOM` because in some circumstances `(services worker)` the DOM `wasn't create` by the browser and access directly to the `DOM` will `cause some bug.`

```
import { Directive, ElementRef, OnInit, Renderer2 } from "@angular/core";

@Directive({
  selector: "[highlightElement]",
})
export class BasicHighLightDirective implements OnInit {
  @Input() defaultColor: string;

  elementRef: ElementRef;
  renderer: Renderer2;
  constructor(elementRef: ElementRef, renderer: Renderer2) {
    this.elementRef = elementRef;
    this.renderer = renderer;
  }

  ngOnInit() {
    this.renderer.setStyle(
      this.elementRef.nativeElement,
      "background-color",
      "green"
    );
  }
}

```

We can also `bind property to directive` like `element` by `placing property in the same place of directive placed`

```
<p highlightElement [defaultColor]="'yellow'"></p>
```

## Structural Directive

### The \* meaning

There is no `*` operator in `Angular`

=> The `*` is just a way to write syntactic sugar code for:

```
<ng-template [ngIf]='condition'>
  <!-- Code Render Conditionally -->
</ng-template>
```

=> It allow us to write \*ngIf inside the element we want to render

---

---

# Section 09 - Services and Dependency Injection

`Services` is where we centralize our code for more purposes - for reuse this code in others component

`Services` is where we use for cross-component communication, fetching data from backend,...

`Dependency` is what our component depend on

`Dependency Injection` is the way `Angular` provide to us to `inject` `the instances` of `Services Class` into `our component`

=> It will `automatically` create the `instances` of `service class` for us

```
servicesInstance: servicesClass;

constructor( servicesInstance: servicesClass ) {
  this.servicesInstance = servicesInstance;
}

```

=> This snippet above tell `angular` what service instances we want

=> When `angular` create our component => It will know what we want but `not know` how to give us such `instances`

=> We provide it for `angular` by:

```
providers: [servicesClass]
```

=> It tell `angular` `how to create this services` - `which instance services we want`

## Hierarchical Injector

<img src="./img/2.png" width="900">

If we provide `service` in `Parent Component` it will available in all `Child Component`. And if we still declare it in `Child Component` `Services` will be override.

=> It mean `Parent's Service` and `Child's Service` is no longer the same.

    => Provide `service` in `AppModule` it will be available in all `child component` and `services`

    => Provide `service` in `AppComponent` it will be available in all `child component` but `not services`

## Inject service into service

We have to attach `metadata` to our services, if we want to make it `injectable`

```
import { Injectable } from '@angular/core'

@Injectable()
```

=> It tell `angular` it service can be `injectable`

---

---

# Routing

<img src="./img/5.PNG" width="900">

First step we have to create our `Routes` with `Angular`

```
const appRoutes = [
  {path: 'home', component: HomeComponent}
  {path: 'user', component: UserComponent}
  {path: 'server', component: ServerComponent}
]
```

Second, we have to register our `Route` with `RouterModule`

```
Imports: [RouterModule.forRoot(appRoutes)]
```

Third, we have to tell `Angular` where our `Route` should place by `<router-outlet></router-outlet> directive`

```
<router-outlet></router-outlet>
```

## Router Link === Link

Add a link without reload

<a
routerLink='/home'
[routerLinkOption]="{exact: true}"

> </a>

=> `[routerLinkOption] directive` make page load with `exactly path`

## Relative path and Absolute path in Router Link

### Relative path

<a
routerLink='home'
[routerLinkOption]="{exact: true}"

> </a>

=> It will `continue` of the `current path` => localhost:4200/home/home

### Absolute path

<a
routerLink='/home'
[routerLinkOption]="{exact: true}"

> </a>

=> It will take the `root path` and continue => localhost:4200/home

## Navigating page in our code

=> We use `Router` service => We inject `Router` service into our component

```
constructor(private router: Router)
this.router.navigate(['servers or /servers']) // same result => to localhost:4200/servers
```

NOTE: this route we use in `navigate method` is `relative to root component`, if we want this method to relative to where this method sit in, you have to specify `the component in which navigate method should relative to`

```
constructor(private router: Router, private route: ActivatedRoute)
this.router.navigate(['servers'], {relativeTo: this.route}) // to localhost:4200/servers/servers
```

## Route params

We using `router service` to retrieve `route params`

```
constructor(private router: Router)

ngOnInit(){
  this.routeID = this.router.snapshot.params['id']
}

```

## Route Observable

## Subscribe to change route in same component

Lec.135

If we change `route params` when already in this `route` => we have to `subscribe to that change`, otherwise `Angular` will not recreate variable because we are in this route

```
constructor(private activatedRoute: ActivatedRoute){}
ngOnInit(){
  this.activatedRoute.params.
    subscribe(
      (params:Params)=>{this.id = params['id']}
    )
}
```

=> This is `Observable` (one kind of Promise) but it can unsubscribe.

`UNDER THE HOOD`: `Angular` will unsubscribe it automatically when component be `destroy (unmount)` to reduce memory leak (You have to do it manually in your own Observable)

## Query Params

### Add query params and fragment

In HTML, we bind `[queryParams]` and `[fragment]` to `[routerLink]` directive

```
<a
  [routerLink]="['/servers', 5,'edit']"
  [queryParams]="{allowEdit: 1}"
  fragment='loading'
>
</a>
```
> localhost:4200/servers/5/edit?allowEdit=1#loading

In code, In TS we use `Router Service` to navigate our route

```
constructor(private router: Router)

this.router.navigate(['/server', id , 'edit'], {
  queryParams: {allowEdit: 1},
  fragment: 'loading',
})
```
> localhost:4200/servers/5/edit?allowEdit=1#loading

=> To `access` query params => We use `activatedRoute services` to retrieve through `snapshot` or subscribe to `activatedRoute observable`

```
this.activatedRoute.snapshot.params);
console.log(this.activatedRoute.snapshot.fragment);
```

```
this.activatedRoute.params.subscribe((params: Params) => {
    console.log(params.id);
});

this.activatedRoute.fragment.subscribe((fragment) => {
    console.log(fragment);
});
```

## Children Route and Router Outlet

### Nested Route

Lec.139 - 140

## Handling error and redirect route

### Handling strange route

Lec.140 - 142

## Outsourcing Route Configuration

=> Outsourcing our route config into other `Module` => Help easy to maintain

---

## Guards

### Control to access a route

Lec.146 - Lec.147

We use `canActive & canActivateChild interface` to protect our `route`

Only `canActivate & canActivateChild method` in our `Guard Service` return true in where we defined our `Route` => It will have access to our route

### Control to leave a route

---

---

# Observable

`Observable` like an upgrade version of `Promise`

=> It can `handle asynchronous task` by `subscribe` to it `observable`

Advanced:

- `Observable` can be `subscribe` to an a stream of value from HTTP Respond over time, while `Promise` just only receive only one time.
- `Observable` can `unsubscribe` to an former event - to avoid memory leak.
- `Built-in Observable` is `unsubscribe` by `Angular` - you do not need to do that

<img src="./img/3.PNG" width="900">

## EventEmitter - Subject - Behavior Subject - Observable

> Don't use EventEmitter, let's use Subject, they are a bit more efficient behind the scenes

### Subject vs Obervable

`Observable` is a `passive` type - we recieve value in `observer` and handle this value.

`Subject` is a `active` type - we call `next` to emit value, and then we subscribe to it to recieve value.

`EventEmitter` we call `emit` to emit value.

> Subject is some kind similar to EvenEmitter, but more efficient behind the scenes

[Subject and Observale] (https://fpt-software.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/14466304#overview)

<img src="./img/8.PNG" width="900">

### EventEmitter vs. Subject

      Subject.next() does the same like EventEmitter.emit(): It emits an event.

      Angular's EventEmitter extends RxJS' Subject; the implementation differences are not relevant,thus both could be used interchangeably.

      The official docs use ...

      ● EventEmitter in connection with @Output() and event binding in direct child/parent communication, and

      ● Subjects in connection with Subscriptions in cross component communication.

      From a technical point of view this is just a convention. And it's good to follow this convention (even though it might just be based on historical reasons), using different tools for different tasks.

      In this way we can always recognize at first sight in which way the event will be received on the other end (with @Output() and event binding or with a Subscription).

      Important:

      If you would use an EventEmitter with a Subscription, you would have to unsubscribe in the same way like when using a Subject.

### Subject vs. BehaviorSubject vs. Observable

      1.

      Imagine subscriptions to a normal newspaper:

      ● All subscribers get the same edition (same edition number) at the same time.

      ● The first edition a new subscriber gets is the first one which is published after he subscribed - and not edition no. 1, of course.

      ● The publisher publishes his editions regardless of whether there are subscribers (in theory at least).

      This is how RxJS Subjects work.

      2.

      ● Imagine now that additionally each subscriber gets a special welcome present at the time when he subscribes: the latest edition that was emitted before (which implies that there is an initial edition already existent before someone can subscribe).

      This is how RxJS BehaviorSubjects work.

      3.

      Now imagine subscriptions to a personal course.

      ● When someone subscribes, he gets edition no. 1 in the first week after his subscription, edition no. 2 in the second week, and so on.

      ● In this way the subscribers don't get the same edition at the same time. They get the editions depending on their individual date of subscription, and it may even be that they get editions with different personalized tasks.

      ● If no one subscribes, no editions will be emitted.

      This is how RxJS Observables work.

---

### Observable vs BehaviorSubject

https://stackoverflow.com/questions/39494058/behaviorsubject-vs-observable

---

---

# Form

## Template-driven Form

Template driven forms are forms where we write logic, validations, controls etc, in the template part of the code (html code). The template is responsible for setting up the form, the validation, control, group etc.

=> Everything in the template

`Angular FormModule` will construct the object contain key - value pair of input template - and give it for us

`Template-driven` approach is mean that everything you do, you do in the template. Everything you change about this form - you change in the template.

## Reactive Form

`Angular` is autodetecting all form and create a `form object` contain all the information about this form for us (include validation)

In `Template-driven Approach` we use local reference to reference to this `Form` created by `Angular`

In `Reactive Approach` we instruct `Angular` the way to create this `Form Group`

---

---

# Pipes

Is a `function` that `interact` with `our data in HTML Template`

We change our data `before` it appear in HTML but `not directly change property`

There are two type of `pipes`: `built-in` and `custom` pipes

```
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: "filter",
  pure: false, //Typically, we are not using this property unless we want pipe calculation run every time we change value of input //
})
export class FilterPipe implements PipeTransform {
  transform(value: any, filterString: string, propName: string): any {
    if (filterString.length === 0 || filterString === "") {
      return value;
    }

    return value.filter((item) => {
      return item.status === filterString;
    });
  }
}


```

This is `pile` should look like. We implement `PileTransform interface` and use `transform method` to re-reduce the `current value`

`Pure property: false` meaning we want to re-calculate the pipe every time the value change - default it not re-calc because of costing of performance

---

---

# HTTP Requests

We use `HttpClientModule` to make Http request

`Pipe` to `transform` the data from `firebase` before `subscribe`

```
In Service
  constructor(private http: HttpClient) {}

private getData() {
    return this.http
      .get("https://project-e8993-default-rtdb.firebaseio.com/posts.json")
      .pipe(
        map((data) => {
          const arrayData = [];
          for (const key in data) {
            if (key) {
              arrayData.push({ ...data[key], id: key });
            }
          }
          return arrayData;
        })
      )
  }
```

```
In Component

ngOnInit(){
  this.service.getData().subscribe((data) => {
        console.log(data);
      });
}
```

---

---

# Authentication

## What is authentication?

`Authentication` is a way to `prevent` user access our `data` in server without `JSON Web Token`

When we `login` or do `authentication task`. The server will sent us `Token` and we save that `Token` in `Local Storage`

And every `HTTP Action` sending to server have to have attached that `Token` to `authenticate`

---

JWT: `JSON Web Token` is `encoded string` with a encoded algorithm only back end know how to decode it to ensure the security of our website => No one can access the data without have the permission of our back end

---

---

# NgRx

## NgRx Effect

When we create an effect it will listen to every action we dispatch and filter what action we specify and run asynchronous code, after that:

- In map => it wrap the new action we create in to an observable and automatically dispatch this action
- In catchError => we have to use `of operator` to wrap our newly created action to a new observable

---
---

# Async Pipe

Async pipe is a way to subcribe to observable like we do manually by calling subscribe method.

The angular async pipe allows the subscription to observables inside of the angular template syntax. It also takes care of unsubscribing from observables automatically.

Why would we use this rather than use subcribe to an observable manually?

=> Because if we subcribe manually we must be unsubscribe manually. With async pipe it will unsubscrible automatically when the component is destroyed.

- Async pipe with interpolation

```
<p>{{ observable | async }}</p>
```
- Async pipe with ngIf directive

```
<p *ngIf="(observable$ | async) > 5">{{ observable$ | async }}</p>
```
- Async pipe with ngFor directive

```
<p *ngFor="let item of items | async">{{ item }}</p>
```

---

---

# map, switchMap, mergeMap

We should read this article to find out more about Map, MergeMap, SwitchMap.

> https://dev.to/bhagatparwinder/map-vs-mergemap-vs-switchmap-5gee

```map```: applies a given project function to each value emitted by the source Observable, and emits the resulting values as an Observable.


    // lets create our data first
    const data = of([
      {
        brand: 'porsche',
        model: '911'
      },
      {
        brand: 'porsche',
        model: 'macan'
      },
      {
        brand: 'ferarri',
        model: '458'
      },
      {
        brand: 'lamborghini',
        model: 'urus'
      }
    ]);

    // get data as brand+model string. Result: 
    // ["porsche 911", "porsche macan", "ferarri 458", "lamborghini urus"]
    data
      .pipe(
        map(cars => cars.map(car => `${car.brand} ${car.model}`))
      ).subscribe(cars => console.log(cars))


```mergeMap``` and ```switchMap``` help us doesn't need to subscribe to both outer and inner observable 

=> It mean we do not end up with multiple nested subcriber for observable.

```mergeMap```: use to combine outer observable with inner observable => only emitted one observable 

in other word

```mergeMap```: is a combination of ```map``` and ```merge``` 

> it map through the original array and do the API call, after that merge the observable of map and the observable of API call

=> It does not cancel old subscription of inner observable when outer observable was emitting => It will receive a stream of value

```swtichMap```: use to combine outer observable with inner observable => only emitted latest value of observable stream

> switchMap does what mergeMap does but with a slight twist. switchMap will subscribe to all the inner Observables inside the outer Observable but it does not merge the inner Observables. It instead switches to the latest Observable and passes that along to the chain.

---

---


## What is Module?

`Module` is a centralized container of Angular application, where we can group/contain related the component, services, directives and pipes of that module.

We split Angular application into multiple `Module`, each `Module` dedicated to an application domain, it provide us the easier for maintaining our application

We can `import` what we `need` into `our module`, and `export` what we want to `share` to `other module`.

## MetaData and Decorator

We turn a class `AppModule` into an `Angular Module` just by using the `NgModule decorator`.

We add `decorator` `@NgModule()` and `metadata object` in class to provide extra information to `Angular`, help `Angular` recognize this is an module.

## What is Component?

Component is a basic UI building block of `Angular Application`

Component consit of 3 main part: 

  A HTML Template - The View: 

  A Typescript class - The code: support the view

  A CSS style - the styling

  A Medadata: provide some additional information of the class for `Angular`


## What is directive?

We use `Directive` to add some behavior to element template.

`Directive` is just a class, we use `@Directive() decorator` to tell `Angular` this is a `Directive`

There is two kind of build-in directive:

- Structural Directive: *ngIf, *ngFor => changing the DOM

- Attribute Directive: [ngStyle] => changing the styling

## What is life cycle in Angular?

Directive and Component in `Angular` has a lifecycle, it provide us a change to jump into every changes of component.

Angular creates it, renders it, creates and renders its children, checks it when its data-bound properties change, and destroys it before removing it from the DOM.

## What is Services in Angular?

`Services` is where we centralized our business logic for reuseable purpose. We can use our logic in services at multiple component.

`Services` is also a place where we do some asynchronous task like calling value from APIs.

`Services` is also use for cross-component communication.

`Services` is place we put business logic - it reduce the work load of component - reuseable - testing

## Why dependency injection?

[Dependency Injection in Angular ( this is a good resources outside there)](https://www.youtube.com/watch?v=OFPIGlxunL0)

<img src="./img/9.png" width="900">

It violate first principle of SOLID - one class should be single responsibility

Hard for testing - becuase when we change Engine, the Car class change also

## What is dependency injection in Angular?

`Dependency Injection` is a pattern allow us to `inject our dependency into the contructor of our component`, it tell `Angluar` that we need the `intances` of this `dependency`, `Angular` will go to `Inversion of Control ( Injector)` and create this `intances` and sent it to our `component`.

> It help our component separate with this dependency - it help us do not worries about the implementation of this dependency - easy to maintaining, testing and increase scalablebility

In `Angular`, we inject our services ( dependency) to our component, and `Angular` will provide the intances of this services ( dependency) for our component.

## How Component can use Services as an Dependency?

We register our `Services` into the `Injector` by add a `@Injector decorator` or we provide in rootModule

<img src="./img/10.png" width="900">

## What is Routing in Angular?

In SPA, we change what user sees by showing or hiding portions of the view/display that corespond to component, rather than going out to the server and request a new page.

And we allow user to move/navigate from one view to another view -> it call routing 

We predefined AppRoutes variable, and register it to AppRoutingModule

## What is Observalbe and Observer in Angular?

`Observable` is like an upgrade version of Promise, we use to handle asynchronous task.

`Observable` is a stream of data come from HTTP Respond. We subscribe to `Observable` to receive value of it.

`Observer` is object we passed into `subscribe method`, it contain a set of subscibe call back function  allow us to have three way to handle data:

- Handle Data - Next

- Handle Error - Error

- Handle the Completion - Complete

We write our code to decide what happen with the data whenever observable emitted this data

`Observer` is the consumer of the delivered value from `Observable`

`The Subscrible method` include an `Observer`


<img src="./img/6.png" width="900">

<img src="./img/7.png" width="900">

## What is Subject in Angular?

`Subject` is special kind of `Observable`, we call call `next` in subject to `emit` value. We can `subcribe` to it like an `Observable`

> In cross-component communication ( services), using subject is more efficient thatn EventEmitter

## What is template varialble?

`Template Variable` is allow us to use data from one part ( or element) of a template in another part ( another element) of the template

In other words, it help us to get the refer to the DOM of one element

```
<input #phone placeholder="phone number" />

<!-- lots of other elements -->

<!-- phone refers to the input element; pass its `value` to an event handler -->
<button type="button" (click)="callPhone(phone.value)">Call</button>
```

## What is <ng-template> <ng-container>?

`<ng-template>` is a place we put html code inside, and `Angular` does not consider to render it into the DOM

We can use `template variable` to add a reference to `<ng-template>`, and using `structural directive` ( like *ngIf, *ngFor, *ngSwitch) to dynamic render it to the DOM

`<ng-container>` is where we render the `<ng-template>` through `*ngTemplateOutlet=""` directive.

```
<ng-template #sayHelloTemplate>
  <p> Say Hello</p>
</ng-template>

<ng-container *ngTemplateOutlet="sayHelloTemplate">
</ng-container> 
 
```

## What is TemplateReference and @Viewchild()?

`@ViewChild()` is use for add a reference to HTML Element in the Typescript file, and we can manipulate this HTML Element in the code

`TemplateRef` is a return type of `@ViewChild()`, it hold the reference to the DOM element

## What is <ng-content></ng-content>?

`<ng-content></ng-content>` is used output the content inside selector tag of component to the component html

In AppComponent.html

```

<div>
  <app-header></app-header>
</div>

<app-home>
  <div>
    <h2>Home Page</h2>
  </div>
</app-home>

```
In AppHome.html

```
<section>
  <ng-content></ng-content>
</section>
```

=> Everything between <app-home></app-home> will goes to where <ng-content></ng-content> place