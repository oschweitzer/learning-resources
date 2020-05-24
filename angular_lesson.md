# Angular

- [Angular](#angular)
  - [Prerequisites](#prerequisites)
  - [Introduction](#introduction)
  - [Basics](#basics)
    - [Bootstrap](#bootstrap)
    - [Modules](#modules)
  - [Components](#components)
    - [Template](#template)
    - [Databinding](#databinding)
      - [String interpolation](#string-interpolation)
      - [Property binding](#property-binding)
      - [Event binding](#event-binding)
      - [Two-way-databinding](#two-way-databinding)
    - [Components & databinding](#components--databinding)
      - [Input](#input)
      - [Output](#output)
      - [Lifecycle hooks](#lifecycle-hooks)
    - [Views](#views)
      - [View encapsulation](#view-encapsulation)
      - [Local reference](#local-reference)
      - [ViewChild](#viewchild)
      - [Component projection](#component-projection)
      - [ContentChild](#contentchild)
  - [Directives](#directives)
    - [Built-in directives](#built-in-directives)
      - [*ngIf](#ngif)
      - [*ngFor](#ngfor)
      - [ngClass](#ngclass)
    - [Custom directives](#custom-directives)
      - [Renderer](#renderer)
      - [Host events](#host-events)
      - [Host binding](#host-binding)
      - [Custom structural directive](#custom-structural-directive)
    - [Directives & Input](#directives--input)
  - [Services](#services)
    - [Dependency injector](#dependency-injector)
  - [Routing](#routing)
    - [Navigation](#navigation)
      - [Navigating programmatically](#navigating-programmatically)
    - [Route parameters](#route-parameters)
      - [Query parameters and fragments](#query-parameters-and-fragments)
        - [Passing the parameters](#passing-the-parameters)
        - [Fetching the parameters](#fetching-the-parameters)
    - [Child routes](#child-routes)
    - [Redirection & wildcard](#redirection--wildcard)
    - [Guards](#guards)
  - [Observables](#observables)
  - [Pipes](#pipes)
  - [Authentication](#authentication)
  - [Links and references](#links-and-references)
    - [Video course](#video-course)
    - [Documentation](#documentation)

## Prerequisites

- JavaScript and TypeScript knowledge

## Introduction

**[Angular](https://angular.io/)** is a Javascript (with wide TypeScript support) framework allowing to create **reactive Single Page Applications ([SPA](https://en.wikipedia.org/wiki/Single-page_application))**.

A SPA consists in having one HTML page and only update some elements of this page using JavaScript allowing to have a much faster application.

> :warning: Don't mistake Angular and AngularJS. AngularJS was the first version of Angular and is not so much used. Angular refers to the framework from version 2 to the last one (version 9). There are a lot of breaking changes between AngularJS and Angular (plus the fact that Angular supports TypeScript), so, when searching information about Angular, you can add `-angularjs` in your search engine.

> Angular versioning policy is that they release a new Angular version every 6 months. So, even if Angular 2 is a complete rewrite of AngularJS, there a far less changes between Angular 2 and 4 (version 3 was skipped), 4 and 5, 5 and 6, etc. To make it easier, Angular team provides tools to migrate your code to the next version.

## Basics

### Bootstrap

After creating a new Angular application with the [CLI](https://cli.angular.io/), you can serve it using the `ng serve` command. What will happened ? here's the simplified workflow:

Basically, when starting an Angular application, Angular will start the `main.ts` file which bootstraps the `AppModule` class which is the root module of any Angular application. In this module, the root component is referenced through the `bootstrap` property (it's an array of components). In the root component, Angular will see the root component selector `app-root` (HTML tag linked to a Angular component) and, when running the single page file (the `index.html` file) it will know which component refers to the `app-root` HTML tag.

**main.ts**

```typescript
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule) // bootstraps the root module AppModule
  .catch(err => console.error(err));
```

**app.module.ts**

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent] // Here we tell Angular components which should be bootstrapped
})
export class AppModule { }
```

**app.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root', // selector used in index.html file
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {}
```

**index.html**

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>My Angular app</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>
```

### Modules

Modules are bundle of functionalities linked your business logic. An Angular module can contain multiple components, directives, services... (those are the main elements of the Angular framework and we will get though them). Angular modules can be imported in the root module, or in other modules.

An Angular module is a class decorated with the `@NgModule` decorator. This decorator takes an object as an argument to describe the module. The main properties of this object (each property is an array) are:

- `declarations`: Registers the module components (Angular will not scan all your files, so we have to tell Angular which components we want to integrate).
- `imports`: Allows to add other modules in this module.
- `exports`: Allows to make some elements (components, directives and pipes but not services) available outside of this module scope.
- `providers`: Specify the injectable objects that are available for dependency injection. Elements specified here could be injected in this module components, services, directives or pipes.
- `bootstrap`: Tells Angular which components it should be aware of when the application starts (which component should Angular recognize in the `index.html` file). Most of the time, this property is only used in the `AppModule` with `[ AppComponent ]` as a value.
- `entryComponents`: Here you specify the entry components. An entry component is a component that Angular should load but that's not used in an HTML template. It is the case when you use Angular routing.

> :warning: An Angular module is **not** a JavaScript ES6 module !

## Components

Components are probably the key concept of the Angular framework. An Angular component contains data and logic associated with an HTML template, and it is used to build the User Interface (UI). It allows to structure your UI and your code. A **component** is represented by a class decorated with the `@Component` decorator.

This `@Component` decorator needs some configuration in order for Angular to understand how to use it. Here are some common `@Component` properties:

- `selector`: HTML tag which will be used to use the component in another HTML template. You can refer the selector by attribute (using `selector: '[app-my-component]'`) or by class (using `selector: '.app-my-component'`)
- `template`: HTML inline code for the component template.
- `templateUrl`: Path to the HTML template of this component (I would recommend to use this instead of `template`).
- `styles`: Array of strings in which you specify CSS code.
- `styleUrls`: Array of CSS files path (I would recommend to use this instead of `styles`).
- `providers`: List of providers this component requires.

> Each component should have its own directory, named with the component name, and the TypeScript file should respect the naming convention `<your_component_name>.component.ts`.

### Template

Each component has an **HTML template** used to structure and display content, at the end, the HTML code is the only thing the user will see. This template looks like regular HTML but it also contain some Angular template syntax which can modify the HTML.

```html
<button (click)="onSave($event)">Save</button>
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button>
<form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>
```

### Databinding

**Databinding** is another key concept in Angular. It basically allows communication between your component TypeScript code and your component HTML template. There a few way of communicating between these two entities.

For example, if we want to output data from our TypeScript code in our HTML code, we can use **[string interpolation](#string-interpolation)**. If we want to set a value to a HTML tag, we will use **[property binding](#property-binding)**.

But we might want to communicate in the other way, from the template ot the TypeScript code. For example, if we have a button on the template, we would want the event triggered by this button to refer to a function implemented in the TypeScript code. To do that we will use **[event binding](#event-binding)**.

Finally, we also might want to communicate in both ways at the same time. To do that, we will use the **[two-way-binding](#two-way-databinding)**.

#### String interpolation

String interpolation is an Angular syntax allowing to display data in string format. Angular uses the double curly braces syntax to do that `{{ myVariable }}`. Inside those curly braces, you can only write expressions resolving in a string, so block expressions or multi-line expressions are not accepted. Note that you can use something that can easily converted to a string (a number for example).

**test.component.ts**

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    name: string = 'Gandalf';
}
```

**test.component.html**

```html
<p>My name is {{ name }}</p>
```

#### Property binding

**Property binding** allows to bind a HTML tag property to a variable or function defined in your TypeScript code. This is done by adding square brackets `[]` to the HTML property we want to bind and assign it a value/function (we assign TypeScript code).

```html
<button [disabled]="!allowValidation">Validate</button>
```

> `disabled` is a native property of the `button` HTML tag.

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    allowValidation: boolean = false;
}
```

#### Event binding

**Event binding** allows to bind to [DOM events](https://developer.mozilla.org/en-US/docs/Web/Events). This is done by adding DOM events  between parenthesis on the targeted HTML elements. You can pass the payload of DOM events to the bind function by using the `$event` argument.

```html
<button (click)="onValidation()">Validate</button>
<input (keyup)="onKey($event)">
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    onValidation() {
        console.log('Validation');
    }

    onKey(event: any) { // without type info
        console.log(event);
  }
}
```

#### Two-way-databinding

**Two-way-binding** is the combination of property and event binding. The syntax is also combined, we will use `[()]` on a property. A common use case is to bind a property using the Angular directive `ngModel`. In the example below, the `name` property of the component is set to default value. that means the `input` element will be populated with this value. But it goes the other way too, if we edit the value in the `input` widget, the value of name will be updated too. Basically, this allows your variables to always represent what is displayed in the view.

```html
<input type="text" [(ngModel)]="name">
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    name: string = 'Gandalf';
}
```

### Components & databinding

#### Input

We saw earlier how we can do property binding inside a component. But what if I need to pass some data from one component to another component property ?  To do that, simply use the `@Input` decorator on the property to bind and assign the data in the parent component using property binding.

**parent.component.html**

```html
<app-child
  [name]="A name"
>
</app-child>
```

**child.component.ts**

```typescript
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
    @Input() name: string; // value will come from the parent component
    // You can specify as argument of the Input decorator an alias for the property to bind.
    // @Input('customName') name: string;
    // Then, in the parent component => <app-child [customName]="'A name'"></app-child>
}
```

#### Output

We saw how to send data from a parent component to a child component, but what if we want to do the opposite, send data from the child to the parent ? We can do that by using the `@Output` decorator and the event binding. What we want is the parent to listen to an event and the child to send an event to communicate and send data.

**child.component.ts**

```typescript
import { Component, Output, OnInit, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {
    @Output() dataEmitter = new EventEmitter<any>(); // we instantiate an EventEmitter that will allow us to emit data

    constructor() {}

    ngOnInit(): void {
       this.dataEmitter.emit('Some data');
    }
}
```

**parent.component.ts**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-parent',
  templateUrl: './parent.component.html',
  styleUrls: ['./parent.component.css']
})
export class ParentComponent {
    onData(data: string): void {
        console.log(data);
    }
}
```

parent.component.html

```html
<app-child
  (dataEmitter)="onData($event)" // event binding on the child component property
>
</app-child>
```

#### Lifecycle hooks

Once a new component is instantiated, Angular goes through a couple of different phases in this creation process. Angular provides some functions that allow us to hook into these phases and do some logic. These hooks are interfaces that a component can implement.

- `ngOnChanges`: Called after a bound input property (decorated with `@Input`) changes. Can be executed multiple times.
- `noOnInit`: Called one the component is initialized. This does not mean that we can see the component, it is not added to  the DOM yet but some initialization (properties for example) has been done. Keep in mind that this function is executed after the constructor.
- `ngDoCheck`: Called during every change detection run. Whenever Angular needs to check if something changed (so this function is called a lot).
- `ngAfterContentInit`: Called after content (ng-content) has been projected into view.
- `ngAfterContentChecked`: Called every time the projected content has been checked.
- `ngAfterViewInit`: Called after the component's view (and child views) has been initialized (rendered you could say).
- `ngAfterViewChecked`: Called every time the view (and child views) have been checked.
- `ngOnDestroy`: Called once the component is about to be destroyed.

### Views

#### View encapsulation

If you apply CSS style to an HTML element (`<p>` tag for example), you will see that only the corresponding HTML elements of your component will have that CSS style applied. Well, this is not how it is handled normally in a web browser, so Angular is doing something here.

Angular is doing what the [shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM) is doing in some web browsers, it will encapsulate your views. To do that, Angular is not using the shadow DOM but it will add a unique identifier to your HTML elements (see image below). It basically emulates a DOM.

![Angular view encapsulation](https://codecraft.tv/courses/angular/components/templates-styles-view-encapsulation/images/encapsulation-emulated-html.png)

You can overwrite that on your component with the `encapsulation` property.

```typescript
import { Component, ViewEncapsulation } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css'],
  encapsulation: ViewEncapsulation.None // ViewEncapsulation is an enum with three values: Emulated (default behaviour), None and ShadowDom.
})
export class TestComponent {}
```

#### Local reference

A local reference can be placed on any HTML element and will hold the whole HTML element with all its properties. To create a local reference, simply use the `#` symbol and then a name for the reference. A local reference can only be used in the template (HTML file) not in the TypeScript code (but you will see that you can later).

```html
<div>
  <span>Name:</span>
  <input type="text" #customName>
</div>
```

#### ViewChild

In order to access local references from your TypeScript component code, you can use the `@ViewChild` decorator. This decorator must be applide on a property and can take the local reference name as an argument.

```html
<div>
  <span>Name:</span>
  <input type="text" #customName>
</div>
```

```typescript
import { Component, OnInit, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css'],
})
export class TestComponent implements OnInit {

    @ViewChild('customName', {
        static: true // static must be true in order to access the property in the ngOnInit hook.
    }) customNameInput: ElementRef;

    ngOnInit() {
        console.log(customNameInput.nativeElement.value)
    }
}
```

> :warning: You should not use this technique to set values and manipulate the DOM, Angular has enough tools to do that.

> ViewChild property will not be accessible in lifecycle hooks until the ngAfterViewInit hook.

#### Component projection

Angular, by default, will not care about the content you put between a component selector (`<app-test>Everything in here is ignored...</app-test>`). But, to build reusable component it could be nice to put logic in here. To do that, we are going to use the `ng-content` built-in directive, which allows to do component projection. Basically the content between the component selector, will replace the `ng-content` directive.

**child.component.html**

```html
<div>
  <ng-content></ng-content>
</div>
```

**parent.component.html**

```html
<app-child>
  <p>My text</p>
</app-child>
```

#### ContentChild

In the same as with the `@ViewChild` decorator, we can access a local reference from projected content (content that will replace the `ng-content` directive). To do that, simply use the `@ContentChild` instead.

**child.component.html**

```html
<div>
  <ng-content></ng-content>
</div>
```

**parent.component.html**

```html
<app-child>
  <p #projectedText>My text</p>
</app-child>
```

**child.component.ts**

```typescript
import { Component, Output } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent implements OnInit {

    @ContentChild('projectedText') content: ElementRef;

    constructor() {}
}
```

> ContentChild property will not be accessible in lifecycle hooks until the ngAfterContentInit hook.

## Directives

**Directives** are instructions in the DOM. We could say that components are a kind of directives, directives with a template. There are also directives without templates. Angular provides some built-in directives but you can build your own directives too.

In Angular there are three kinds of directives:

- Components: a component is a directive with a template.
- Structural directives: a structural directive changes the DOM by adding or removing elements.
- Attribute directives: an attribute directive changes the appearance or behavior of an element, component or another directive.

### Built-in directives

#### *ngIf

One of the most used built-in directive is the `*ngIf` directive. This directive is used to add conditions to say if we want an HTML element to be displayed or not. `*ngIf` has a `*` in its name because it's a structural directive, it changes the structure of the DOM.

```html
<p *ngIf="isOk">OK</p>
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    isOk = true;
}
```

You can use the `else` condition to display something when your condition is false. To do that, you must add a local reference on a `ng-template` tag and then use this local reference after the `else` expression.

```html
<p *ngIf="isOk; else notOk">OK</p>
<ng-template #notOk>
  <p>Not OK</p>
</ng-template>
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    isOk = true;
}
```

> The `*` before ngIf will actually be transform into something like: 

```html
<ng-template [ngIf]="isOk">
  <p>OK</p>
</ng-template>
```

#### *ngFor

`*ngFor` is a structural directive too (notice the `*` before the name). It allows to loop over a collection and stamp out one element (the element in which the directive lays) per item in the collection.

```html
<ul>
    <li *ngFor="let name of names">{{ name }}</li>
</ul>
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    names: string[] = ['Gandalf', 'Frodo', 'Sam'];
}
```

#### ngClass

`ngClass` allows us to dynamically add or remove CSS classes. This directive only works with property binding, we have to put `[]` around this directive. To apply CSS classes on an element, the syntax is an object with properties. The key of each property will be the CSS class name and the value will be a condition (resolving to a boolean) saying whether to apply the class CSS code or not.

```html
<p [ngClass]="{ redText: counter < 0 }">{{ counter }}</p>
```

```css
.redText {
    color: red;
}
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent {
    counter = -1;
}
```

### Custom directives

A custom directive is a class decorated with the `@Directive` decorator. Like components (remember a component is a directive), a directive needs configuration, and one of the most important argument is the selector.

The selector works the same way as for components, it's a string that will be used on HTML elements. A selector can use the [CSS attribute syntax](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors) if you want your directive to be placed on a HTML element. If that's the case, you can then use Angular injection system to get access to the element your directive sits on.

```typescript
import {Directive, ElementRef, OnInit} from '@angular/core';

@Directive({
  selector: '[testDirective]' // CSS attribute selector
})
export class MyComponentDirective implements OnInit {
  // By putting the element in the directive constructor, Angular will inject the ElementRef for you
  constructor(private elementRef: ElementRef) {}

  ngOnInit() {
    console.log(this.elementRef);
  }
}
```

```html
<!-- We can use our directive as an HTML attribute on any HTML element -->
<div testDirective></div>
```

> You can omit the `[]` on the selector and use the directive as an independent HTML element (in the same way we do with a component)

#### Renderer

You can access your element properties directly in a directive (style properties for example), but it's not always the best practice to do so. You might have some use cases where you have no DOM, because Angular is not running only in browsers, it can run on [service workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) for example. Angular provides a class, a service that allows us to manipulate elements without having to touch the DOM directly. This service is the Angular Renderer, called `Renderer2` (`Renderer` has been deprecated and removed since Angular 9).

```typescript
import {Directive, ElementRef, OnInit, Renderer2} from '@angular/core';

@Directive({
  selector: '[testDirective]'
})
export class MyComponentDirective implements  OnInit {
  constructor(private elementRef: ElementRef, private renderer: Renderer2) {}

  ngOnInit(): void {
    this.renderer.setStyle(this.elementRef.nativeElement, 'color', 'blue');
  }
}
```

```html
<p testDirective>My test</p>
```

#### Host events

We might want to make our directives a little bit more interactive, maybe we would want to react to events occurring on the element a directive sits on for example.

To do that, we will use a decorator called `@HostListener` on a method of the directive. This decorator needs a [DOM event](https://developer.mozilla.org/en-US/docs/Web/Events) name as an argument.

```typescript
import {Directive, ElementRef, HostListener, OnInit, Renderer2} from '@angular/core';

@Directive({
  selector: '[testDirective]'
})
export class MyComponentDirective implements  OnInit {
  constructor(private elementRef: ElementRef, private renderer: Renderer2) {}

  ngOnInit(): void {}

  // argument of @HostListener must be a valid DOM event
  @HostListener('mouseenter') onMouseEnter(eventData: Event) {
    // When the mouse pointer will enter on the HTML element, the text color of this element will become blue.
    this.renderer.setStyle(this.elementRef.nativeElement, 'color', 'blue');
  }
}
```

#### Host binding

Angular also provides a decorator to bind a DOM element property to a property of the directive. This decorator is called `@HostBinding`. This decorator replaces the renderer in a way. You can access any DOM element attributes with `@HostBinding`, simply write the attribute name as an argument of the decorator. Please note that attributes with dashes in their name should be written in camel case when passed as a decorator argument.

```typescript
import {Directive, ElementRef, HostBinding, HostListener, OnInit, Renderer2} from '@angular/core';

@Directive({
  selector: '[testDirective]'
})
export class MyComponentDirective implements  OnInit {

  // You can
  @HostBinding('style.color') textColor: string = 'black';

  constructor(private elementRef: ElementRef, private renderer: Renderer2) {}

  ngOnInit(): void {}

  @HostListener('mouseenter') onMouseEnter(eventData: Event) {
    // this.renderer.setStyle(this.elementRef.nativeElement, 'color', 'blue');
    this.textColor = 'blue';
  }
}
```

#### Custom structural directive

We can also create some custom structural directive. To do that, and because the `*` before the directive will transform it to a `ng-template`, we will need access to the template code inside `ng-template`. This is done by injecting the template (of type `TemplateRef`) in the directive constructor. The other thing we might want is an access to the view to know where to render the template for example. This is done by injecting a reference to the view container (of type `ViewContainerRef`).

```typescript
import {Directive, Input, OnInit, TemplateRef, ViewContainerRef} from '@angular/core';

  // This directive is the opposite of ngIf.
@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective implements  OnInit {

  // here we are using a setter with the same name as the directive (this is very important!)
  @Input() set appUnless(condition: boolean) {
    if (!condition) {
      this.vcRef.createEmbeddedView(this.templateRef); // display the template in the newly created view
    } else {
      this.vcRef.clear(); // remove the view
    }
  }

  constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef) {}

  ngOnInit(): void {}
}

```

```html
<p *appUnless="counter < 0">My test</p>
```

### Directives & Input

With property binding and the `@Input` decorator, you can pass data to a directive as well.

```typescript
import {Directive, ElementRef, HostListener, Input, OnInit, Renderer2} from '@angular/core';

@Directive({
    selector: '[testDirective]'
  })
  export class MyComponentDirective implements  OnInit {

    @Input() textColor: string;

    constructor(private elementRef: ElementRef, private renderer: Renderer2) {}

    ngOnInit(): void {}

    @HostListener('mouseenter') onMouseEnter(eventData: Event) {
      this.renderer.setStyle(this.elementRef.nativeElement, 'color', this.textColor);
    }
  }
```

```html
<!-- Note that the '' are important since we are passing a string. -->
<!-- A shortcut to avoid using '' is:  textColor="yellow" -->
<p testDirective [textColor]="'yellow'">My test</p>
```

## Services

Services act as central business unit,somewhere where can centralize your code in. They are often used to handle data whereas components are use to display data basically.

Services in Angular are classes decorated with the `@Injectable` decorator. This decorator tells Angular that this class will be injected somewhere using **dependency injection**. Dependency injection is a design pattern, a specific form of **[inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control)** where basically a client doesn't want to know how to construct a service, it just want to use it so it will delegate the creation of this service to an injector.

```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class MyService {

  doSomething() { /* ... */ }
}
```

### Dependency injector

In Angular, a dependency is something a class of ours will depend on, a service is a dependency of a component for example.

The dependency injector will inject an instance of this dependency into our component automatically. What we need to do is to inform Angular that we need this dependency. To do that, you simply need to pass the dependency in your constructor and specify the proper type. Once done that, the last step is to provide the wanted dependency. To do that, you can add it to the component or to the module.

> We can also inject a service in an another service.

The Angular dependency injector is a hierarchical injector, that means if we provide a service in some place in our application, in a component for example, Angular knows how to create an instance of that service for **this component and all its child components**. This component and all its child components will receive **the same instance of the service**.

There are other places where we can provide a service.

- AppModule: The same instance of the service will be available application wide.
- AppComponent: The same instance of the service will be available for all components (but not for other services).
- Any other component: The same instance of the service will be available this component and all its child components.

> Remember that this service propagation goes only down.

**Custom component level**

```typescript
import {Component} from '@angular/core';
import {MyService} from './my-service.service';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css'],
  providers: [MyService], // we provide the service here
})
export class MyComponent {

  constructor(private myService: MyService) {} // we inject the service here
}
```

**Module level**

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import {MyService} from '../services/my-service.service';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ],
  providers: [MyService] // service is available for all elements of the module (components, services, other modules)
})
export class MyModule { }
```

> For the AppModule level, you can put your service in the `providers` array but, you can also put an argument to the `@Injectable` decorator: `@Injectable({ providedIn: 'root' })`. It's the same thing, except that the last syntax will allow the service to be lazy loaded and will lead to better performances.

## Routing

Angular provides a router allowing to change the URL of your application, but with still keep the SPA aspect. It will change a lot of parts of this single page to make it look like another page.

To create such routes, first we need to declare them. Angular provides a `Routes` type that is an array of object describing the routes.
Then, to tell Angular that we want these routes, import the `RouterModule` and add the routes as an argument to the `forRoot` function.
FInally, in the template where you want to use the routes (usually in the `app.component.html` file), add the `router-outlet` directive.

**app.module.ts**

```typescript
const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'test',
    component: MyComponent
  }
];

@NgModule({
  declarations: [
    AppComponent,
    MyComponent,
    HomeComponent,
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes), // we tell Angular which routes we want
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

**app.component.html**

```html
<router-outlet></router-outlet>
```

### Navigation

Accessing view through the URL bar is nice but not very useful. What we would want is to be able to navigate through our application with links.
To do that, Angular provides a special directive `routerLink`. It's an attribute directive that you can put on an anchor HTML element `<a></a>`.

We will then use property binding to pass the route we want to access with this link.

> Calling the routes with a simple `href` on anchor element will work but it will also reload the page at each click (this is not a good way to do navigation).

```html
<h1>Navigation</h1>
<ul>
  <li><a routerLink="/">Home</a></li>
  <li><a routerLink="/test">My component</a></li>
</ul>
<router-outlet></router-outlet>
```

Here we are using the shortcut for passing strings with property binding. but we could have also written `<li><a [routerLink]="'/test'">My component</a></li>` or use an array `<li><a [routerLink]="['/test']">My component</a></li>`.

> Beginning slashes `/` tells the router that this path is an absolute path (it will start after `<http://hostname:port>`). If you omit the beginning slash that means you want a relative path (which is the case for nested routes for example).

#### Navigating programmatically

Navigating with links is quite common, but what if we want to navigate with our code (in a button click event handler for example) ? To do that, first inject the `Router` object in your component/service/directive constructor. Then, in your handler, you can use the `navigate` method of the `Router` object and pass the wanted path as an argument.

```typescript
import { Component } from '@angular/core';
import {Router} from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  constructor(private router: Router) {}

  onGoToComponentClick() {
    this.router.navigate(['/test']); // navigate method takes an array
  }
}
```

```html
<button (click)="onGoToComponentClick()">Go to my component</button>
```

The `navigate` method doesn't know where we are, so to handle relative path, you will have to use the second argument of this method, which is an object with a few options. The option we are looking ofr is the `relativeTo` property. Here we want to tell Angular that the path passed as the first argument is relative to the current path. To get the current path, you can inject the `ActivatedRoute` object which is the currently active route when the component is loaded.

```typescript
import {Component} from '@angular/core';
import {ActivatedRoute, Router} from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  constructor(
    private router: Router,
    private activatedRoute: ActivatedRoute
  ) {}

  onGoToComponentClick() {
    this.router.navigate(['myNestedRoute'], {
      relativeTo: this.activatedRoute
    }); // this will allow to navigate to a path like /test/myNestedRoute if the AppComponent is accessed through the /test route.
  }
}
```

### Route parameters

There might be cases where you want to add dynamic parameter to your routes. To do that, just add a `:` and the name of your dynamic parameter in the route path definition

```typescript
const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'test/:id', // here we are adding a dynamic parameter named id to our route
    component: MyComponent
  }
];
```

To fetch such a parameter, again inject the `ActivatedRoute`. It will allow us to get the parameter from the URL.

```typescript
import {Component, OnInit} from '@angular/core';
import {ActivatedRoute} from '@angular/router';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css'],
})
export class MyComponent implements OnInit {

  constructor(private activatedRoute: ActivatedRoute) {}

  ngOnInit() {
    console.log(this.activatedRoute.snapshot.params['id']); // we assume that the route parameter was named 'id'
  }
}
```

The issue with this code is that it will work well when the component is initialized, but it will not update if the component already exists. This would be the case if in your component you have a link to itself but with other parameters for example. To solve this issue, we will use the `params` property of the `ActivatedRoute` which is an [Observable](#observables). So, by subscribing to this Observable you will get the updated parameters of the route.

```typescript
import {Component, OnInit} from '@angular/core';
import {ActivatedRoute, Params} from '@angular/router';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css'],
})
export class MyComponent implements OnInit {

  constructor(private activatedRoute: ActivatedRoute) {}

  ngOnInit() {
    console.log(this.activatedRoute.snapshot.params['id']);
    this.activatedRoute.params.subscribe((params: Params) => {
      console.log(params['id']);
    });
  }
}
```

#### Query parameters and fragments

##### Passing the parameters

You may want to pass **[query parameters](https://en.wikipedia.org/wiki/Query_string)** in your URL. Again, you can do that in the HTML template if you want the user to navigate, or in the TypeSCript code if you want to navigate programmatically.

In the HTML file, you can use property binding on `[queryParams]` (`[queryParams]` is an input of the `routerLink` directive, it's not a directive by itself). This input accepts JavaScript object, key-value pairs for each query parameter.

In the Typescript code, we will use the injected `Router` again. In the same way as we saw with the `relativeTo` property, we are going to us the second argument of the `navigate()` method and use the property `queryParams`.

If you want to use **[fragments](https://en.wikipedia.org/wiki/Fragment_identifier)** in your URL, it's the same thing as for the query parameters, use the `[fragment]` property binding in HTML and `fragment` property for the `navigate()` method.

**Example with user interaction navigation**

```html
<h1>Navigation</h1>
<ul>
  <li><a routerLink="/">Home</a></li>
  <li><a [routerLink]="['test', 5]" [queryParams]="{ myParam: 'nice param' }" [fragment]="'section'">My component</a></li>
  <!-- The route will be '/test/5?myParam=nice%20param#section'. %20 is the code for a space -->
</ul>
<router-outlet></router-outlet>
```

**Example with programmatic navigation**

```typescript
import {Component} from '@angular/core';
import {Router} from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  constructor(
    private router: Router,
  ) {}

  onGoToComponentClick() {
    this.router.navigate(['/test', 5], {
    queryParams: {
      myParam: 'nice param'
    },
      fragment: 'section'
    });
  }
}
```

##### Fetching the parameters

Again to retrieve these parameters, we will use the `ActivatedRoute` and whether the `snapshot.queryParams` and `snapshot.fragment` properties or the `queryParams` and `fragment` Observables.

```typescript
import {Component, OnInit} from '@angular/core';
import {ActivatedRoute, Params} from '@angular/router';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css'],
})
export class MyComponent implements OnInit {

  constructor(private activatedRoute: ActivatedRoute) {}

  ngOnInit() {
    console.log(this.activatedRoute.snapshot.queryParams);
    console.log(this.activatedRoute.snapshot.fragment);

    this.activatedRoute.queryParams.subscribe((params: Params) => {
      console.log(params);
    });
    this.activatedRoute.fragment.subscribe((params: Params) => {
      console.log(params);
    });
  }
}
```

### Child routes

Sometimes you would want to have nested routes, child routes. To do that, in the routes definition, simply use the `children` property which is an array of `Routes`.

```typescript
const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'test',
    component: MyComponent,
    children: [
      {
        path: ':id',
        component: MyComponent
      },
      {
        path: ':id/edit',
        component: EditMyComponent
      }
    ]
  }
];
```

### Redirection & wildcard

On most application, if a user wants to access a route that doesn't exist, it will be redirected or a 404 error view will be displayed.

To redirect a route to another, simply use the `redirectTo` property instead of the `component` property in the routes definition. To catch all the non wanted routes, you can use the `**` (wildcard) as a path. :warning: Be careful of the routes order, the wildcard path should be the last route in the routes array, because otherwise, all the routes below the wildcard will be ignored and you will always be redirected.

```typescript
const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'test',
    component: MyComponent,
    children: [
      {
        path: ':id',
        component: MyComponent
      },
      {
        path: ':id/edit',
        component: EditMyComponent
      }
    ]
  },
  {
    path: 'not-found',
    component: NotFoundComponent
  },
  {
    path: '**', // Must be the last route
    redirectTo: '/not-found'
  }
];
```

### Guards

A **route guard** is code, logic that you want to execute before a component is loaded or before you leave a component. Guards are often used to protect some routes with authentication. A guard is basically a service that will be provided in a module. The Angular router provides  multiple guards interfaces like `CanActivate`, `CanActivateChild` or `CanDeactivate`.

**auth-guard.service.ts**

```typescript
import { Injectable } from '@angular/core';
import {ActivatedRouteSnapshot, CanActivate, RouterStateSnapshot, UrlTree} from '@angular/router';
import {Observable} from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor() { }

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean | UrlTree> | Promise<boolean | UrlTree> | boolean | UrlTree {
    /* do some logic here */
  }
}
```

**app.module.ts**

```typescript
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {RouterModule, Routes} from '@angular/router';
import {AppComponent} from './app.component';
import {AuthGuard} from './auth-guard.service';
import {HomeComponent} from './home/home.component';
import {MyComponent} from './my-component/my-component.component';

const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent
  },
  {
    path: 'test',
    canActivate: [AuthGuard], // this wall apply the guard on this route and all its children
    component: MyComponent,
    children: [
      {
        path: ':id',
        component: MyComponent
      },
      {
        path: ':id/edit',
        component: EditMyComponent
      }
    ]
  },
];

@NgModule({
  declarations: [
    AppComponent,
    MyComponent,
    HomeComponent,
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes),
  ],
  providers: [AuthGuard], // don't forget to provide the guard, it's a service
  bootstrap: [AppComponent]
})
export class AppModule { }
```

> If you want to preload data before accessing a route (in order to avoid displaying an empty view for example), you can use the [`Resolve` guard](https://angular.io/guide/router#resolve-pre-fetching-component-data).

## Observables

## Pipes

## Authentication

## Links and references

### Video course

- https://www.udemy.com/course/the-complete-guide-to-angular-2 (is often on offer)

### Documentation

- [Official Angular documentation](https://angular.io/docs)