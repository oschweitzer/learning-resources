# TypeScript

- [TypeScript](#typescript)
  - [Introduction](#introduction)
  - [Basics & basic types](#basics--basic-types)
    - [Union types](#union-types)
    - [Type guards](#type-guards)
    - [Literal types](#literal-types)
    - [Type alias & custom types](#type-alias--custom-types)
    - [Function types](#function-types)
  - [Classes](#classes)
    - [Classes basics](#classes-basics)
    - [Inheritance](#inheritance)
    - [Override](#override)
    - [Protected scope](#protected-scope)
    - [Getter/setter](#gettersetter)
    - [Static methods/properties](#static-methodsproperties)
    - [Abstract classes](#abstract-classes)
    - [Singleton and private constructor](#singleton-and-private-constructor)
  - [Interfaces](#interfaces)
    - [Definition](#definition)
    - [Interface inheritance](#interface-inheritance)
    - [Function type](#function-type)
  - [Advanced types](#advanced-types)
    - [Intersection types](#intersection-types)
    - [Advanced type guards](#advanced-type-guards)
    - [Discriminated union](#discriminated-union)
    - [Type casting](#type-casting)
    - [Index properties](#index-properties)
    - [Function overloads](#function-overloads)
    - [Optional chaining](#optional-chaining)
    - [Nullish coalescing](#nullish-coalescing)
  - [Generics](#generics)
    - [Definition](#definition-1)
    - [Generic utility types](#generic-utility-types)
    - [Decorators](#decorators)
    - [Definition](#definition-2)
    - [Usage](#usage)
  - [Modules and namespaces](#modules-and-namespaces)
    - [Namespaces](#namespaces)
    - [Modules](#modules)

## Introduction

TypeScript is a programming language which adds new features and typing to JavaScript. It is basically a superset of JavaScript, it is build upon JavaScript.

TypeScript can't be directly run in a web browser because the latter needs JavaScript. So, TypeScript code needs to be compiled into JavaScript.

TypeScript allows to easily write better code.

## Basics & basic types

<table class="tg">
    <thead>
        <tr>
            <th class="tg-0pky">Type</th>
            <th class="tg-0pky">Description</th>
            <th class="tg-0pky">Example</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td class="tg-0pky">number</td>
            <td class="tg-0pky">There is only one type for all numbers be it integers or floats. </td>
            <td class="tg-0pky">
                <pre><code>
                    const a = 0;
                    const b = 1.2;
                    const c = -86;
                </code></pre>
            </td>
        </tr>
        <tr>
            <td class="tg-0pky">string</td>
            <td class="tg-0pky">Simple sequence of characters. There are no character type.</td>
            <td class="tg-0pky">
                <pre><code>
                    const a = 'Hello';
                    const b = "Hello";
                    const c = `Hello`;
</pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">boolean</td>
            <td class="tg-0pky">Binary value, true or false.</td>
            <td class="tg-0pky">
                <pre><code>
                    const a = true;
                </pre></code>
</td>
        </tr>
        <tr>
            <td class="tg-0pky">object (or {})</td>
            <td class="tg-0pky">Generic type for objects.</td>
            <td class="tg-0pky">
                <pre><code>
                    const a = {
                        &nbsp;&nbsp;age: 30
                    };
</pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">Array (or [])</td>
            <td class="tg-0pky">Array of a single type or mixed type.</td>
            <td class="tg-0pky">
                <pre><code>
                    const a = [ 1, 5, 10];
                    const b = [ 1, 'hey', true];
                </pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">Tuple</td>
            <td class="tg-0pky">Array with fixed length and type.</td>
            <td class="tg-0pky">
                <pre><code>
                    // tuple with exactly 2 elements, first one is a number and second one is a string
                    const a: [ number, string ] = [ 1, 'hey'];
                </pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">enum</td>
            <td class="tg-0pky">Allows to define a set of constants.</td>
            <td class="tg-0pky">
                <pre><code>
                    enum Test { TEST1 ,TEST2, TEST3 };
</pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">any</td>
            <td class="tg-0pky">Store any kind of type. Avoid using it though because it breaks the power of TypeScript.
            </td>
            <td class="tg-0pky"></td>
        </tr>
        <tr>
            <td class="tg-0pky">void</td>
            <td class="tg-0pky">Mostly used as the return type of function that doesn't return anything.</td>
            <td class="tg-0pky">
                <pre><code>
                    function test(): void {
                        &nbsp;&nbsp;console.log('Test');
                    }
</pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">undefined</td>
            <td class="tg-0pky">Type for absence of value.</td>
            <td class="tg-0pky"></td>
        </tr>
        <tr>
            <td class="tg-0pky">unknown</td>
            <td class="tg-0pky">Allows to assign any value to a variable having this type (like any). But unlike any you
                must check the type first.</td>
            <td class="tg-0pky">
                <pre><code>
                    let a: unknown;
                    let b: string;
                    a = 5;
                    a = 'Hello'; 
                    b = a; // Error, will work with ‘any’ type.
                    if (typeof a === 'string') {
                          b = a; // Works!
                    }
</pre></code></td>
        </tr>
        <tr>
            <td class="tg-0pky">never</td>
            <td class="tg-0pky">Can be used as a function return type to functions that will never return anything (like
                functions with a single throw statement or with an infinite loop).</td>
            <td class="tg-0pky">
                <pre><code>
                    function generateError(message: string, code: number): never {
                        &nbsp;&nbsp;throw { message, code};
                    }
                    generateError('Oups', 500);
</pre></code></td>
        </tr>
    </tbody>
</table>

When assigning a value to a variable, TypeScript will determine the variable type. This is called **type inference**.

### Union types

**Union type** is a TypeScript feature allowing to define a type as an union of multiple types. To do that, separate the types with the `|` operator.

```typescript
const a: number | string;
```

This new type is an union which means that TypeScript **makes only available the common methods of the types that are part of this union**.

### Type guards

In order to tell TypeScript which type of an union type we want to use, we can use a **type guard**. A type guard is a simple test on the type of a variable.

```typescript
const myFunction = (x: number | string): void => {
  if (typeof x === 'number') {
    console.log('The variable X it\'s like a number')
  }

  if (typeof x === 'string') {
    console.log('The variable X it\'s like a string')
  }
}
```

This allows to narrow the variable type down to one type of the union.

### Literal types

Literal types are types derived from primitive types (string, number...). We can say that they are specific versions of these types. They are mainly used in union types.

```typescript
const animal: 'cat' | 'dog'; // animal is a string, but we limit this string to two specific values
```

### Type alias & custom types

A type alias means creating a type by using the `type` keyword. It can be used with union types or to create a custom type.

```typescript
type Pet = 'cat' | 'dog';
type User = { name: string; age: number; };
```

### Function types

You can give a variable the type of a specific function (default type is `Function`).

```typescript
function add(a: number, b: number) {
	return a + b;
}

let f: (a: number, b: number) => number;
f = add;
```

:warning: A callback function can return something even if its return type is `void`.

```typescript
function sendRequest(data: string, cb: (response: any) => void) {
 // ... sending a request with "data"
 return cb({data: 'Hi there!'});
}
 
sendRequest('Send this!', (response) => {
 console.log(response);
 return true;
 });

// This code will compile.
```

## Classes

### Classes basics

Classes in TypeScript are templates allowing to create objects. A class is defined by the `class` keyword and is instantiated with the `new` keyword (example: `const car = new Car();`).

Classes have methods and properties which can be public or private. We can set the scope with the `public` and `private` keywords. By default, all the methods and properties are public. Private means that the method or property is not accessible outside the class.

Every class has a constructor function (simply called `constructor`). This constructor can accept arguments or not. It is possible to create and assign properties directly in the constructor arguments.

```typescript
class Car {
  constructor(private brand: string) {}
}

// The above code is equivalent to the code below

class Car {
private brand: string;
  constructor(brand: string) {
	this.brand = b;
  }
}
```

In order to avoid that a property could be modified after its assignation in the constructor, you can specify this property as `readonly`.

```typescript
class Car {
  constructor(private readonly brand: string) {}
}
```

:warning: The notion of private scope doesn't exist in JavaScript yet. Keep in mind that after the compilation, this private scope (and the readonly notion) won't exist anymore.

### Inheritance

Class inheritance is possible through the `extends` keyword. The child class will inherit of the methods and properties from the parent class (as well as the constructor function). If you want to call the parent constructor in the child class, use the `super` keyword in the child class constructor.

```typescript
class Vehicle {
   constructor(private  brand: string) {}
}

class Car extends Vehicle {
   constructor(brand: string, private color: string) {
       super(brand);
   }
}
```

### Override

It is possible to override a method from a parent class in the child class.

```typescript
class Vehicle {
   constructor(private brand: string) { }

   test() {
       console.log('test');
   }
}

class Car extends Vehicle {
   constructor(brand: string, private color: string) {
       super(brand);
   }

   test() {
       console.log('Car test');
   }
}

const vehicle = new Vehicle('VW');
const car = new Car('Audi', 'black');
vehicle.test(); // test
car.test(); // Car test
```

### Protected scope

With class inheritance comes a new scope in addition to `public` and `private`, the `protected` scope. Protected means that methods and properties are accessible in child classes.

### Getter/setter

In order to read and modify private properties, TypeScript comes with accessors.

- A **getter** is a function defined with the `get` keyword. It allows to get the value of a property and adding some extra logic (validation for example)
- a **setter** is a function defined with the `set` keyword. It allows to modify the property of a class.

```typescript
const fullNameMaxLength = 10;

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (newName && newName.length > fullNameMaxLength) {
            throw new Error("fullName has a max length of " + fullNameMaxLength);
        }
        this._fullName = newName;
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

You can notice that we access to `fullName` like it is a public property. It is transparent to the user of the class.

### Static methods/properties

By using the `static` keyword, it is possible to define static methods and properties. Static means that we can access these methods and properties without having to instantiate the class. You can use this concept for helper classes or constants (see the `Math` class for example).

### Abstract classes

An abstract class is a class that can't be instantiated. Its goal is to be used as a model that can be later inherit. It is possible to define default function implementation or abstract methods. Abstract methods are just function signature without implementation, the implementation will have to be done in child classes (it is mandatory).

A class with at least one abstract method will be an abstract class.

```typescript
abstract class Animal {
    abstract makeSound(): void; // must be implemented in child class
    move(): void {
        console.log("roaming the earth...");
    }
}
```

### Singleton and private constructor

A singleton is a design pattern which consist to have only one instance of a specific class. To do that, it is possible to make the constructor private by adding the `private` keyword in front of the constructor function. by doing that, it is not possible to create an instance of the class with the `new` keyword anymore.

To create an instance of this class, we must add a static function in this class as well as a private and static property. This property will keep a reference of this unique instance of the class. The static method will test if the instance of the class exists and will return it if it's the case.

```typescript
class Vehicle {
   private static instance: Vehicle;

   private constructor(private brand: string) { }

   static getInstance() {
       if (this.instance) {
           return this.instance;
       }
       this.instance = new Vehicle('Audi');
       return this.instance;
   }
}


const vehicle = Vehicle.getInstance();
const vehicle2 = Vehicle.getInstance(); // same instance as vehicule
```

## Interfaces

### Definition

An interface (created with the `interface` keyword) allows to describe the structure of an object. You could think of it as a custom type. An interface can be used as a type but also being implemented by a class (using the `implements` keyword). Implementing an interface means having a contract between the class and the interface, the class must respect the interface structure.

```typescript
interface Greetable {
   name: string;
   greet(): void;
}

class Person implements Greetable {
   // Person must have a name property and a greet function
   constructor(public name: string) {}

   greet() {
       console.log(this.name);
   }
}
```

In an interface, it is not possible to use the `public` and `private` keywords. on the other way, the `readonly` keyword is available.

An interface is not compiled into JavaScript, it won't appear in the JavaScript code.

### Interface inheritance

An interface can inherit from another interface.

```typescript
interface Named {
   name: string;
}

interface Greetable extends Named {
   greet(): void;
}

class Person implements Greetable {

   constructor(public name: string) { }

   greet() {
       console.log(this.name);
   }
}
```

### Function type

An interface can define a function type.

```typescript
interface AddFn {
   (a: number, b: number): number;
}

let add: AddFn; add = (a: number, b: number) => {
   return a + b;
}
```

## Advanced types

### Intersection types

It is possible to combine different types into one. To do that, you can use whether the `&` operator on a `type` declaration, or use the interfaces inheritance.

`&` operator and `type`

```typescript
type Admin = {
   name: string;
   privileges: string[];
}

type Employee = {
   name: string;
   startDate: Date;
}

type ElevatedEmployee = Admin & Employee;
const e: ElevatedEmployee = {
   name: 'John',
   privileges: ['create-server'],
   startDate: new Date()
}
```

Interfaces inheritance

```typescript
interface Admin {
   name: string;
   privileges: string[];
}

interface Employee {
   name: string;
   startDate: Date;
}

interface ElevatedEmployee extends Admin, Employee {}
const e: ElevatedEmployee = {
   name: 'John',
   privileges: ['create-server'],
   startDate: new Date()
}
```

### Advanced type guards

Type guards on primitive types is done by using the `typeof` operator.

```typescript
function add(a: number | string, b: number | string) {
   if (typeof a === 'string' || typeof b === 'string') {
       return a.toString() + b.toString();
   }
   return a + b;
}
```

For custom types, one technique is to check if a property exists on an object by using the `in` keyword.

```typescript
type Admin = {
   name: string;
   privileges: string[];
}

type Employee = {
   name: string;
   startDate: Date;
}

type ElevatedEmployee = Admin & Employee;
const e: ElevatedEmployee = {
   name: 'John',
   privileges: ['create-server'],
   startDate: new Date()
}

function printEmployeeInfo(emp: ElevatedEmployee) {
   if ('privileges' in emp) {
       console.log(`Privileges : ${emp.privileges}`);
   }
   console.log(`Name: ${emp.name}`)
}

printEmployeeInfo(e);
```

For classes, we will use the `instanceof` keyword because `typeof` will return 'object'.

```typescript
class Car {
  drive() {
    console.log('Driving...');
  }
}

class Truck {
  drive() {
    console.log('Driving a truck...');
  }

  loadCargo(amount: number) {
    console.log('Loading cargo ...' + amount);
  }
}

type Vehicle = Car | Truck;

const v1 = new Car();
const v2 = new Truck();

function useVehicle(vehicle: Vehicle) {
  vehicle.drive();
  if (vehicle instanceof Truck) {
    vehicle.loadCargo(1000);
  }
}

useVehicle(v1);
useVehicle(v2);
```

### Discriminated union

**Discriminated union** is a pattern used with union types to simplify the type guards. To do that, we will add a property containing the type name in each type of the union type. Then, in the type guard, you simply have to do a switch/case on this type property.

```typescript
interface Bird {
  type: 'bird';
  flyingSpeed: number;
}

interface Horse {
  type: 'horse';
  runningSpeed: number;
}

type Animal = Bird | Horse;

function moveAnimal(animal: Animal) {
  let speed;
  switch (animal.type) {
    case 'bird':
      speed = animal.flyingSpeed;
      break;
    case 'horse':
      speed = animal.runningSpeed;
  }
  console.log('Moving at speed: ' + speed);
}
```

### Type casting

Type casting is used to force the type of a value. There are a two ways to do that.

- By using `<>` before the value to cast.
- By using the `as` key word (more used in the community in order to avoid errors of the first method in React).

```typescript
const userInputElement = <HTMLInputElement>document.getElementById('user-input')!;

const userInputElement = document.getElementById('user-input') as HTMLInputElement;
```

> Notice the `!` in the first line, it is used to tell TypeScript that this value will never be `null`.

### Index properties

It is possible to describe index types with interfaces. An index type is a type accessible with an index. TypeScript allows only two types for these indexes, `number` and `string` (because these are the two types used to define key in a object).

```typescript
interface ErrorContainer { // { email: 'Not a valid email', username: 'Must start with a character!' }
  [prop: string]: string;
}

const errorBag: ErrorContainer = {
  email: 'Not a valid email!',
  username: 'Must start with a capital character!'
};
```

### Function overloads

In TypeScript, you can define several signature for the same function. TypeScript will do its job and merge the signature and the implementation.

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: string, b: number): string;
function add(a: number, b: string): string;
function add(a: Combinable, b: Combinable) {
  if (typeof a === 'string' || typeof b === 'string') {
    return a.toString() + b.toString();
  }
  return a + b;
}

const result = add('Hello', ' World');
result.split(' ');
```

### Optional chaining

Since TypeScript version 3.7, the `?` operator allows to check if an element exists or not. It is very useful with nested objects.

```typescript
const fetchedUserData = {
  id: 'u1',
  name: 'Max',
  job: { title: 'CEO', description: 'My own company' }
};

console.log(fetchedUserData?.job?.title);
// if one the property doesn't exist, the code will end and not throw an error like it used to.
```

### Nullish coalescing

This feature is linked ot the optional chaining and allows to test if a value is `null` or `undefined`. Before this feature, thanks to the `||` operator, it was possible to check is a value was falsy (`null`, `undefined` **but** also `""` or `0`). Now, if you want to check if a value is only `null` or `undefined`, you can use the `??` operator.

```typescript
const userInput = undefined;
const storedData = userInput ?? 'DEFAULT'; // storedData will be equal to 'DEFAULT'
const storedData1 = userInput || 'DEFAULT'; // storedData will be equal to 'DEFAULT'

const userInput2 = 0;
const storedData2 = userInput2 ?? 'DEFAULT'; // storedData will be equal to 0
const storedData3 = userInput2 || 'DEFAULT'; // storedData will be equal to 'DEFAULT'
```

## Generics

### Definition

A generic type is a type that will be replaced by a another one allowing to write code that will work for a wide variety of types. We use the `<>` syntax to specify the generic type.

```typescript
function merge<T extends object, U extends object>(objA: T, objB: U) {
  return Object.assign(objA, objB);
}
// extends is here to allow the Object.assign function to succeed because we need only objects for this function.

const mergedObj = merge({ name: 'Max', hobbies: ['Sports'] }, { age: 30 });
console.log(mergedObj);
```

Constraints can be added to generic types with the `extends` keyword. A nice constraint is the use of the `keyof` operator to created a type representing an object keys.

```typescript
function extractAndConvert<T extends object, U extends keyof T>(
  obj: T,
  key: U
) {
  return 'Value: ' + obj[key];
}

extractAndConvert({ name: 'Max' }, 'name');
```

Generics can be used on classes as well.

```typescript
class DataStorage<T extends string | number | boolean> {
  private data: T[] = [];

  addItem(item: T) {
    this.data.push(item);
  }

  removeItem(item: T) {
    if (this.data.indexOf(item) === -1) {
      return;
    }
    this.data.splice(this.data.indexOf(item), 1); // -1
  }

  getItems() {
    return [...this.data];
  }
}

const textStorage = new DataStorage<string>();
textStorage.addItem('Max');
textStorage.addItem('Manu');
textStorage.removeItem('Max');
console.log(textStorage.getItems());

const numberStorage = new DataStorage<number>();
```

### Generic utility types

TypeScript provides some builtin generics.

The `Partial` type allows to take all the properties of a type et make them optional.

```typescript
interface CourseGoal {
  title: string;
  description: string;
  completeUntil: Date;
}

function createCourseGoal(
  title: string,
  description: string,
  date: Date
): CourseGoal {
  let courseGoal: Partial<CourseGoal> = {};
  courseGoal.title = title;
  courseGoal.description = description;
  courseGoal.completeUntil = date;
  return courseGoal as CourseGoal;
}
```

The `Readonly` type forbids an object or an array to be modified.

```typescript
const names: Readonly<string[]> = ['Max', 'Anna'];
// names.push('Manu'); // Error
// names.pop(); // Error
```

:warning: Don't confuse Generics with union types. Union types allows to chose among several types whereas generics fix a type.

### Decorators

### Definition

A decorator is a function that will run some logic on the arguments it will receive. A decorator can be applied to functions, classes, properties, accessors or parameters. To apply a decorator, use the `@` symbol and then the function name. A decorator is **executed when the function or class on which it is applied is defined**. 

### Usage

Here is a basic example of decorator.

```typescript
function Logger(constructor: Function) {
  console.log('Logging...');
  console.log(constructor);
}

@Logger
class Person {
  name = 'Max';

  constructor() {
    console.log('Creating person object...');
  }
}

const pers = new Person();
console.log(pers); // prints the class whole class code (since it's a constructor function)
```

In order to send any arguments we want to a decorator, it is recommended to use a decorators factory. A factory is a pattern which consists of a function creating functions.

```typescript
function Logger(logString: string) {
  return function(constructor: Function) {
    console.log(logString);
    console.log(constructor);
  };
}

@Logger('LOGGING - PERSON')
class Person {
  name = 'Max';

  constructor() {
    console.log('Creating person object...');
  }
}

const pers = new Person();
console.log(pers);
```

The execution order of decorators is not the same as standard functions. The first decorator that will be executed is the one close to the decorator's target. Basically, the order is bottom to top, starting point being the target.

We are talking about the decorator functions order and not the factories. Factory functions will be executed is the standard order (from top to bottom).

```typescript
function Logger(logString: string) {
  console.log('LOGGER FACTORY');
  return function(constructor: Function) {
    console.log(logString);
  };
}

function WithTemplate(template: string, hookId: string) {
  console.log('TEMPLATE FACTORY');
  return function(constructor: any) {
    console.log('Rendering template');
    const hookEl = document.getElementById(hookId);
    const p = new constructor();
    if (hookEl) {
      hookEl.innerHTML = template;
      hookEl.querySelector('h1')!.textContent = p.name;
    }
  }
}

@Logger('LOGGING')
@WithTemplate('<h1>My Person Object</h1>', 'app')
class Person {
  name = 'Max';

  constructor() {
    console.log('Creating person object...');
  }
}

const pers = new Person();

/* 
  Logs order will be:
    LOGGER FACTORY
    TEMPLATE FACTORY
    Rendering template
    LOGGING
*/
```

We can apply a decorator on many targets (class, function, property, accessor, function argument).

```typescript
function Log(target: any, propertyName: string | Symbol) {
  console.log('Property decorator!');
  console.log(target, propertyName);
}

function Log2(target: any, name: string, descriptor: PropertyDescriptor) {
  console.log('Accessor decorator!');
  console.log(target);
  console.log(name);
  console.log(descriptor);
}

function Log3(
  target: any,
  name: string | Symbol,
  descriptor: PropertyDescriptor
) {
  console.log('Method decorator!');
  console.log(target);
  console.log(name);
  console.log(descriptor);
}

function Log4(target: any, name: string | Symbol, position: number) {
  console.log('Parameter decorator!');
  console.log(target);
  console.log(name);
  console.log(position);
}

class Product {
  @Log
  title: string;
  private _price: number;

  @Log2
  set price(val: number) {
    if (val > 0) {
      this._price = val;
    } else {
      throw new Error('Invalid price - should be positive!');
    }
  }

  constructor(t: string, p: number) {
    this.title = t;
    this._price = p;
  }

  @Log3
  getPriceWithTax(@Log4 tax: number) {
    return this._price * (1 + tax);
  }
}

const p1 = new Product('Book', 19);
const p2 = new Product('Book 2', 29);
```

A decorator can return a value, this value depends on the decorator's target. If you use a decorator on a class for example, the decorator function can return a constructor. In that case, it allows to replace the target class with another one.

Only decorators on classes, methods or accessors will return values that will be interpreted by TypeScript. You can return a value in a property decorator but it's useless.

```typescript
function WithTemplate(template: string, hookId: string) {
  console.log('TEMPLATE FACTORY');
  return function<T extends { new (...args: any[]): {name: string} }>(
    // T extends an object with a new() function aka a class
    originalConstructor: T
  ) {
    return class extends originalConstructor {
      constructor(..._: any[]) {
        super();
        console.log('Rendering template');
        const hookEl = document.getElementById(hookId);
        if (hookEl) {
          hookEl.innerHTML = template;
          hookEl.querySelector('h1')!.textContent = this.name;
        }
      }
    };
  };
}

@WithTemplate('<h1>My Person Object</h1>', 'app')
class Person {
  name = 'Max';

  constructor() {
    console.log('Creating person object...');
  }
}

const pers = new Person();
```

## Modules and namespaces

### Namespaces

Namespaces are TypeScript only feature. They allow to regroup code having a common logic. A namespace can include functions, classes, variables and interfaces.

To create a namespace, simply use the `namespace` keyword. To make the elements of the namespace accessible outside, use the `export` keyword in front of the element export.

```typescript
namespace StringUtility {
  
  export function ToCapital(str: string): string {
    return str.toUpperCase();
  }

  export function SubString(
    str: string,
    from: number,
    length: number = 0
  ): string {
    return str.substr(from, length);
  }
}
```

To import a namespace, you must use the following TypeScript syntax.

```typescript
/// <reference path="string-utility.ts" />

const a = 'test';
console.log(ToCapital(a));
```

If you want JavaScript to access namespaces from other files, you must modify the TypeScript compiler configuration. This configuration will tell TypeScript to compile all the TypeScript files into one JavaScript file.

```typescript
// tsconfig.js

{
/******/
  "outFile": "./dist/bundle.js",
/*******/
}
```

:warning: This option works only with amd and system modules. You should also modify the `module` section of the `tsconfig.json` file.

### Modules

In TypeScript and in Javascript it is possible to use ES6 modules. In TypeScript it is recommended to use ES6 modules instead of namespaces. To do that, simply use the `export` and `import` keywords. Modules have their own scope, meaning that without export, variables, functions, classes, etc. will only be accessible inside the module.

There are a few ways to import and export code.

```typescript
import { Class1, function1 } from './path/to/a-lib.js'; // standard import
 // import everything of this file and you will use it through myLib
import * as myLib from './path/to/my-lib.js';

export const myConstant = 'test'; // named 
export default class MyClass {} // default export of this file
/* ... */
import MyClass from './path/to/my-class.js'; // default import
```