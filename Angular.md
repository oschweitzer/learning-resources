# Angular 

## Misc/News

- In addition to `ngOnDestroy()`, you can inject Angular's `DestroyRef` and register callback functions to be called when the enclosing context is destroyed.

```typescript
    const destroyRef = inject(DestroyRef);
    destroyRef.onDestroy(() => { /* do some cleaning stuff */ });
```

- to inject a dependency:
    - use classic constructor injection `constructor(private myService: MyService) {}`
    - use new `inject()` function `myService = inject(MyService);`


- provide `HttpClient` service:
    - use `provideHttpClient()` in providers array `providers: [provideHttpClient()]` (in @Component configuration, @NgModule configuration or in bootstrapApplication call (`bootstrapApplication(AppComponent, { providers: [provideHttpClient()] })`) )

- Dependency injection type: https://angular.dev/guide/di/hierarchical-dependency-injection

- Forms
    - template-driven: controls a form in the template code. Suitable for small and simple forms not so much for complex forms.
        ```html
        <form #form="ngform" (ngSubmit)="onSubmit(form)">
            <input id="email" name"email" type="email" ngModel required email />
            <input id="password" name"password" type="password" ngModel required minlength="6" />
        </form>
        ```

        ```typescript
        // ...
        onFormSubmit(formData: NgForm) {
            if (formData.form.invalid) return;
            const enteredEmail = formData.form.value.email;
            // ...
        }
        // ...
        ```
    - reactive: Done in Typescript part. More suitable for complex forms.
    ```typescript
    @Component({
        selector: 'app-login',
        standalone: true,
        imports: [ReactiveFormsModule]
        templateUrl: './login.component.html',
        styleUrl: './login.component.css',
    })
    export class LoginComponent {
        form = new FormGroup({
            email: new FormControl('', {
                validators: [Validators.email, Validators.required]
            }),
            password: new FormControl('', {
                validators: [Validators.required, Validators.minLength(6)]
            }),
        })
    }
    ```

          ```html
        <form [formGroup]="form">
            <input id="email" name"email" type="email" [formControl]="form.controls.email" />
            <input id="password" name"password" type="password" formControName="password" /> <!-- we don't want to prepopulate a value for the password input, so we use formControName directive>
        </form>
        ```

- Angular routing:

```typescript
// main.ts
bootstrapApplication(AppComponent, {
    providers: [provideRouter([
        {
            path: 'tasks', // <your-domain>/tasks
            component: TasksComponent
        }
    ])]
})
```

> it's recommended to externalize this routes configuration in another file (typically `app.routes.ts`).

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'tasks', // <your-domain>/tasks
        component: TasksComponent
    }
]
```

> You can do the same with the global application configuration

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
    providers: [
        provideRouter(routes)
    ]
}
```

- router outlet

```html
<app-header></app-header>
<div>
    <router-outler /> <!-- RouterOutlet should be imported in your component or module >
</div>
```


- link

```html
<a routerLink="/tasks">my link</a> <!-- RouterLink should be imported>
```

- dynamic route

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'tasks', // <your-domain>/tasks
        component: TasksComponent
    },
    {
        path: 'users/:userId', // <your-domain>/users/<uid>
        component: UserTasksComponent
    },
]
```

```html
<a [routerLink]="['/users', userId]">my link</a> <!-- RouterLink should be imported>
```

- extract dynamic route parameters with Signals

```typescript
// user-tasks.component.ts
@Component({
    selector: 'app-user-tasks',
    standalone: true,
    templateUrl: './user-tasks.component.html',
    styleUrl: './user-tasks.component.css'
})
export class UserTasksComponent {
    userId = input.required<string>(); // using Signal
}
```


```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
    providers: [
        provideRouter(routes, withComponentInputBinding() /* this will allow to get route parameter as an Input in our component */)
    ]
}
```

- extract dynamic route parameters with Observable

```typescript
// user-tasks.component.ts
@Component({
    selector: 'app-user-tasks',
    standalone: true,
    templateUrl: './user-tasks.component.html',
    styleUrl: './user-tasks.component.css'
})
export class UserTasksComponent implements OnInit {
    userId = ''
    private activatedRoute = inject(ActivatedRoute);
    private destroyRef = inject(DestroyRef);

    ngOnInit(): void {
        const subscription = this.activatedRoute.paramMap.subscribe({
            next: paramMap => this.userId = paramMap.get()'userId')
        });
        this.destroyRef.onDestroy(() => subscription.unsubscribe());
    }
}
```

- nested routes

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'tasks', // <your-domain>/tasks
        component: TasksComponent,
    },
    {
        path: 'users/:userId', // <your-domain>/users/<uid>
        component: UserTasksComponent,
        children: [
            {
                path: 'tasks', // <your-domain>/users/<uid>/tasks
                component: TasksComponent
            }
        ]
    },
]
```


```html
<!-- user-tasks.component.html >
<div>
    <router-outler /> <!-- RouterOutlet should be imported in your component or module >
</div>
```

- get parent router data from inside nested routes

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { routes } from './app.routes';
import { provideRouter, withComponentInputBinding, withRouterConfig } from '@angular/router';

export const appConfig: ApplicationConfig = {
    providers: [
        provideRouter(routes, withComponentInputBinding(),
            withRouterConfig({
                paramsInheritanceStrategy: 'always', // this allows to get params as input in nested routes
            })
        )
    ]
}
```

- activatedRoute vs Activated Route Snapshot 
    - ActivatedRoute is using Observable, so it makes it reactive
    - Activated Route Snapshot is not using Observable, you will have directly the values. To be used if you don't need or count on route values to change.

- query params using Signals

```html
<a routerLink="./" [queryParams]="{ order: 'asc' }">my link</a> <!-- RouterLink should be imported>
```


```typescript
// tasks.component.ts
@Component({
    selector: 'app-tasks',
    standalone: true,
    templateUrl: './tasks.component.html',
    styleUrl: './tasks.component.css'
})
export class TasksComponent {
    order: input();
}
```

- query params using Observable

```html
<a routerLink="./" [queryParams]="{ order: 'asc' }">my link</a> <!-- RouterLink should be imported>
```


```typescript
// tasks.component.ts
@Component({
    selector: 'app-tasks',
    standalone: true,
    templateUrl: './tasks.component.html',
    styleUrl: './tasks.component.css'
})
export class TasksComponent implements OnInit {
    order: input();
    private activatedRoute = inject(ActivatedRoute);
    private destroyRef = inject(DestroyRef);

    ngOnInit(): void {
        const subscription = this.activatedRoute.queryParams.subscribe({
            next: queryParams => this.order = queryParams['order']
        });
        this.destroyRef.onDestroy(() => subscription.unsubscribe());
    }
}
```

- resolve route related dynamic data


```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'users/:userId', // <your-domain>/users/<uid>
        component: UserTasksComponent,
        children: [
            {
                path: 'tasks', // <your-domain>/users/<uid>/tasks
                component: TasksComponent
            }
        ],
        resolve: {
            username: resolveUserName // use the resolver function here
        }
    },
]
```


```typescript
// user-tasks.component.ts
@Component({
    selector: 'app-user-tasks',
    standalone: true,
    templateUrl: './user-tasks.component.html',
    styleUrl: './user-tasks.component.css'
})
export class UserTasksComponent  {
    userName = input.required<string>(); // here we et the value coming from the resolver function `resolveUserName`
}

// resolver function for Router
export const resolveUserName: ResolveFn<string> /* ResolveFn is comiong from @angular/router */ = (activatedRoute: ActivatedRouteSnapshot, routerState: RouterStateSnapshot) => {
    // this function will be executed every time the route is active, so we don't need the activated route with Observable.
    
    // it is possible to inject services in a ResolveFn, using the `inject()` method. There is no constructor in a function so it's not possible to inject services in the old way.
    const userService = inject(UsersService);
    const userName = usersService.find(u => u.id === activatedRoute.paramMap.get('userId'))?.name || '';
    return userName;
}
```

> You can also use class to define resolver, but it's deprecated and Angular recommends to it with a function.

> You can get static and dynamic data from routes by using the ActivatedRoute through the `activatedRoute.data` property, which is an Observable that merges both static and dynamic data.

> Resolver functions will be trigger when route parameters change but not when query parameters change. To enable them for query params, you need to add the `runGuardsAndResolvers: 'paramsOrQueryParamsChange'` in your route configuration.

- Route title

```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'users/:userId', // <your-domain>/users/<uid>
        component: UserTasksComponent,
        // title: 'My static route title',
        title: resolveTitle // you can use aresolver function for dynamic title
        children: [
            {
                path: 'tasks', // <your-domain>/users/<uid>/tasks
                component: TasksComponent
            }
        ],
        resolve: {
            username: resolveUserName // use the resolver function here
        }
    },
]
```

- Router guard

```typescript
const taskCanMatch: CanMatchFn = (router, segments) => {
    const router = inject(router);
    const shouldGetAccess = /* some logic to determine access or not */ Math.random();
    if (shouldGetAccess < 0.5) {
        return true;
    }
    return new RedirectCommand(router.parseUrl('/unauthorized'));
}

export const routes: Routes = [
    {
        path: 'users/:userId', // <your-domain>/users/<uid>
        component: UserTasksComponent,
        canMatch: [taskCanMatch],
        children: [
            {
                path: 'tasks', // <your-domain>/users/<uid>/tasks
                component: TasksComponent
            }
        ],
    },
]
```

> `canMatch` (check if a route can match, can be used to avoid lazy chunk to be loaded unlike with canActivate), `canActivate` (check if a route can be activated, it means it has already been matched), `canDeactivate` (check if the user can leave a route) are some of the possible guard logic you can use.

## Lazy loading

Def: only load elements when you need them to reduce bundle at start.

2 approaches for lazy loading: routing and deferrable views

### Route-base lazy loading


```typescript
// app.routes.ts
import { Routes } from '@angular/router';

export const routes: Routes = [
    {
        path: 'tasks', // <your-domain>/users/<uid>/tasks
        loadComponent: () => import('../tasks.tasks.component').then(module => module.TaskComponent)
    }
]
```

> `loadChildren` can be used to load lazily child components.

You can also load lazily services by removing the configuration in the `@Injectable()` decorator and add this service in the `providers` property of your route.

### Deferrable views

With recent versions of Angular, you can use deferrable views to load lazily your components.

```html
@defer {
    <app-my-component-to-load>
}
```

You can define a trigger for the defer function to decide when to load your component.

```html
@defer(on viewport) {
    <app-my-component-to-load>
} @placeholder {
    <p>We are waiting...</p>
}
```

> only standalone components will be deferred, if you use a NgModule-based component it will be eagerly loaded.


## Detection strategy

### OnPush strategy

### With Zone.js

```typescript
@Component({
  selector: 'app-movie-card',
  standalone: true,
  imports: [
    NgOptimizedImage
  ],
  templateUrl: './movie-card.component.html',
  styleUrl: './movie-card.component.scss',
  changeDetection: ChangeDetectionStrategy.OnPush // <-- change strategy here
})
```

- only detect changes:
    - if some events happen in this component or in its child components
    - if some Input values changed in this component
    - if you manually detect changes
        ```typescript
            private cdRef = inject(ChangeDetectorRef);
            /**/
            this.cdRef.markForCheck();
        ```
    - if Signals are used
    - if `async` pipe is used

### Get rid of Zone.js (Angular v18)

- using Signals
- in angular.json => remove polyfils zone.js
- use provider `provideExperimentalZonelessChangeDetection()`


## RxJS vs Signals

Signals:
- represent information that changes over time. Signals allows to follow those changes and react to them.
- provided in Angular directly
- better perf since change detection is more fine-grained
- can allow to remove Zone.js and therefore make Angular lighter
- computed values allow to recalculate a value when the dependent values change. It avoids to recalculate them at each change detection

References:
- https://angular.fr/signals/

Differences:

- Observables:
    - values over time
    - great for managing events and streamed data
- Signals:
    - values in a container (no subscription, it is done for you)
    - great for managing application state

Angular provides tools to convert from one to another

- `toObservable(mySignal)` => to convert a signal to an Observable
- `toSignal(myObservable$)` => to convert an Observable to a signal


## Deploying our app

### SPA (Single Page Application)

Single Page Application:

- build a client-side only web application
- all code executes in the browser
- no dynamic web server needed - a static host suffices (ex: Firebase)
- potential disadvantages: initially missing content, bad SEO

> You can use `ng add` and `ng deploy` to easily deploy your app on static hosts.

### SSR (Server Side Rendered App)

- Angular app routes are rendered on-demand on a dynamic web server
- Browser receives finished, rendered page.
- Web app is hydrated ("activated") and becomes a SPA after initial rendering
- Dynamic web server is required
- potential disadvantages: long-taking tasks may cause empty pages, complexity

To add SSR capabilities to your Angular app

- `ng add @angular/ssr`
- use `afterNextRender(() => {})` function to run code that only works in browser (usage of local storage for example)

### SSG (Static Site Generation)

- Angular app routes are pre-rendered at build time
- Browser receives finished, rendered pages
- Web app is hydrated ("activated") and becomes a SPA
- Dynamic web server is required - static host suffices if ALL pages are pre-rendered
- potential disadvantages: No dynamic server-side data fetching
