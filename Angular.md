# Angular 

## Misc

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
