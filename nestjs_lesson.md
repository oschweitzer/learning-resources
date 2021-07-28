# NestJS

- [NestJS](#nestjs)
  - [Introduction](#introduction)
  - [Modules](#modules)
  - [Controllers](#controllers)
  - [Providers](#providers)
    - [Dependency Injection (DI)](#dependency-injection-di)
    - [Custom providers](#custom-providers)
      - [Value providers](#value-providers)
      - [Class providers](#class-providers)
      - [Factory providers](#factory-providers)
  - [Links & references](#links--references)
    - [Video courses](#video-courses)

## Introduction

[NestJS](https://nestjs.com/) is a **Node.js framework for building server-side applications**. It widely uses TypeScript through decorators, classes, interfaces, etc.

By default, under the hood, NestJS uses the [Express.js](https://expressjs.com/) framework and its middlewares. But you can switch to [Fastify](https://www.fastify.io/) if you want to.

Nest.js is heavily inspired by Angular, so it allows developers to learn common concepts and understand the whole frontend/backend chain. NestJS tries to do what frameworks like Angular, React or Vue did for the frontend, provide a consistent way of building an application with a consistent architecture.

## Modules

**Modules** in NestJS are similar to modules in Angular, that means they are classes with a decorator (`@Module` for NestJS and `@NgModule` for Angular).

NestJS modules differ from TypeScript module (ES6 modules for instance), you could consider NestJS modules like bundles. These modules are [singletons](https://en.wikipedia.org/wiki/Singleton_pattern), therefore they can be imported by multiple other modules. They are a good way to organize and segment your code and your application logic.

Each applications has at least one module, the root module (called `AppModule`). It is the starting point of the application.

The `@Module` decorator has several properties.

- `providers`: Array of providers to be available within the module via dependency injection.
- `controllers`: Array of controllers to be instantiated within the module.
- `exports`: Array of providers to export to any other modules.
- `imports`: List of modules required by this module. Any exported provider by these modules will be available in our module via dependency injection.

```typescript
import { Module, HttpModule } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
  imports: [HTTPModule],
})
export class CatsModule {}
```

## Controllers

A **controller** is a class decorated with the `@Controller` decorator and responsible for handling incoming requests and returning responses to the client. A controller is bind to a specific path (`/cat` for example) and contains handlers functions which handle endpoints and HTTP request methods (GET, POST, DELETE...).Controllers can take advantage of the dependency injection to consume providers within the same module.

Handlers are methods of the controller class and are decorated with decorators such as `@Get`, `@Post`, `@Delete`, etc. to define the HTTP methods and the route.

A basic controller workflow would be:

1. An HTTP request is send by the client.
2. The request is routed to the controller and the handler is called with arguments.
3. The handler handles the request and performs operations (retrieving data from a database for example).
4. The handler returns a response value of any type (even an exception) to the client.

```typescript
import { Controller, Get } from '@nestjs/common';
import { CatService } from '../services/cats.service';
import { Cats } from '../interfaces/cats.interface';

@Controller('cats')
export class CatsController {
  constructor(private service: CatsService) {} // we are using dependency injection to use the CatService provider.

  @Get() // Handle on HTTP Get requests and on the '/' route (the full path, will be 'http://<your_hostname>:<your_port>/cats/')
  findAll(): Cats[] {
    return this.service.find();
  }
}
```

## Providers

A **provider** is decorated with the `@Injectable` decorator and can be a plain value, a class, a synchronous/asynchronous factory, etc. It can be injected into constructors via the dependency injection. A provider must be provided to a module to be usable (defined in the `providers` property of a module). It can be exported from a module in order to be available to other modules (that will importing it).

A special type of provider is the service (:warning: not all providers are services). A service, when wrapped with the `@Injectable` decorator and provided to a module, acts as a singleton. That means, the same instance of this service will be shared across the application (single source of truth).

Services are the main source of business logic. A service can, for example, be called from a controller to validate data, create an item in the database and return a response.

Any NestJS component can inject a provider that is decorated with the `@Injectable` decorator. When providing a provider in the controller's constructor, NestJS will take care of the injection, and it will then be available as a class property. This is called **constructor-based injection**.

```typescript
import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

### Dependency Injection (DI)

With the constructor-based injection, we saw one example of dependency injection. Dependency injection is an [inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control) (IoC) technique. The dependencies instantiation is delegated to the IoC container (in our case, the NestJS runtime system) instead doing it in your own code.

When the NestJS IoC container instantiates a controller for example, it first looks for any dependencies. This step is happening during the application bootstrapping, will analyse the dependencies and create a **dependency graph**. This analysis is transitive, if one dependency has dependencies too, those will also be resolved. This dependency graph make sure that the dependencies are resolved in the right order, think of it as a tree where leaves are resolved first.

When finding a dependency, the NestJS IoC container will lookup this dependency token (its runtime representation, a service name for example) and return the associated value (the class if the dependency is a service). Then, Nest will create an instance of this dependency (or return the existing one because it's a singleton).

### Custom providers

In NestJS, you might need custom providers (aside from services for example). There are several ways to create such providers.

#### Value providers

Value providers are created using the `useValue` syntax. It is often used in unit testing, for mocking a service for example.

```typescript
import { CatsService } from './cats.service';

const mockCatsService = {
  /* mock implementation
  ...
  */
};

@Module({
  imports: [CatsModule],
  providers: [
    {
      provide: CatsService,
      useValue: mockCatsService,
    },
  ],
})
export class AppModule {}
```

> You can use a string or a symbol as a injection token if you don't have a class-based token.

```typescript
import { connection } from './connection';

@Module({
  providers: [
    {
      provide: 'CONNECTION',
      useValue: connection,
    },
  ],
})
export class AppModule {}
```

#### Class providers

Class providers is done by using the `useClass` syntax. It allows to dynamically determine a class that a token should resolve to.

```typescript
const configServiceProvider = {
  provide: ConfigService,
  useClass:
    process.env.NODE_ENV === 'development'
      ? DevelopmentConfigService
      : ProductionConfigService,
};

@Module({
  providers: [configServiceProvider],
})
export class AppModule {}
```

#### Factory providers

A factory provider is created using the `useFactory` syntax. It allows to dynamically create providers. It's a very powerful way of tool because you can inject other providers in the factory. In order to do that, the factory provider comes with two mechanisms.

- the factory function can accept arguments.
- the `inject` property accepts an array of providers which will be resolved and passed as arguments to the factory (hence the first point).

```typescript
const connectionFactory = {
  provide: 'CONNECTION',
  useFactory: (optionsProvider: OptionsProvider) => {
    const options = optionsProvider.get();
    return new DatabaseConnection(options);
  },
  inject: [OptionsProvider], // is resolved by Nest, and passed as argument in the above function.
};

@Module({
  providers: [connectionFactory],
})
export class AppModule {}
```

## Links & references

### Video courses

- https://www.udemy.com/course/the-complete-nestjs-developer-enterprise-nodejs-framework/
- https://www.udemy.com/course/nestjs-zero-to-hero/
