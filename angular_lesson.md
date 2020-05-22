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
    - [Directives](#directives)
      - [Built-in directives](#built-in-directives)
      - [*ngIf](#ngif)
        - [ngFor](#ngfor)
        - [ngClass](#ngclass)
      - [Custom directives](#custom-directives)
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

### Components

Components are probably the key concept of the Angular framework. An Angular component contains data and logic associated with an HTML template, and it is used to build the User Interface (UI). It allows to structure your UI and your code. A **component** is represented by a class decorated with the `@Component` decorator.

This `@Component` decorator needs some configuration in order for Angular to understand how to use it. Here are some common `@Component` properties:

- `selector`: HTML tag which will be used to use the component in another HTML template. You can refer the selector by attribute (using `selector: '[app-my-component]'`) or by class (using `selector: '.app-my-component'`)
- `template`: HTML inline code for the component template.
- `templateUrl`: Path to the HTML template of this component (I would recommend to use this instead of `template`).
- `styles`: Array of strings in which you specify CSS code.
- `styleUrls`: Array of CSS files path (I would recommend to use this instead of `styles`).
- `providers`: List of providers this component requires.

> Each component should have its own directory, named with the component name, and the TypeScript file should respect the naming convention `<your_component_name>.component.ts`.

#### Template

Each component has an **HTML template** used to structure and display content, at the end, the HTML code is the only thing the user will see. This template looks like regular HTML but it also contain some Angular template syntax which can modify the HTML.

```html
<button (click)="onSave($event)">Save</button>
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button>
<form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>
```

#### Databinding

**Databinding** is another key concept in Angular. It basically allows communication between your component TypeScript code and your component HTML template. There a few way of communicating between these two entities.

For example, if we want to output data from our TypeScript code in our HTML code, we can use **[string interpolation](#string-interpolation)**. If we want to set a value to a HTML tag, we will use **[property binding](#property-binding)**.

But we might want to communicate in the other way, from the template ot the TypeScript code. For example, if we have a button on the template, we would want the event triggered by this button to refer to a function implemented in the TypeScript code. To do that we will use **[event binding](#event-binding)**.

Finally, we also might want to communicate in both ways at the same time. To do that, we will use the **[two-way-binding](#two-way-databinding)**.

##### String interpolation

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

##### Property binding

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

##### Event binding

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

### Directives

**Directives** are instructions in the DOM. We could say that components are a kind of directives, directives with a template. There are also directives without templates. Angular provides some built-in directives but you can build your own directives too.

#### Built-in directives

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

##### ngFor

`*ngFor` is a structural directive too. It allows to loop over a collection and stamp out one element (the element in which the directive lays) per item in the collection.

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

##### ngClass

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

#### Custom directives

## Components & databinding

### Input

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
    // Then, in the parent component => <app-child [customName]="A name"></app-child>
}
```

### Output

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

## Links and references

### Video course

- https://www.udemy.com/course/the-complete-guide-to-angular-2 (is often on offer)

### Documentation

- [Official Angular documentation](https://angular.io/docs)