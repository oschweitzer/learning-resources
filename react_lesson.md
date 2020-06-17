# React

- [React](#react)
  - [Introduction](#introduction)
  - [React basics](#react-basics)
    - [JSX](#jsx)
      - [Dynamic content](#dynamic-content)
  - [Components](#components)
    - [Props](#props)
    - [State](#state)
      - [useState() hook](#usestate-hook)
    - [Events handling](#events-handling)
    - [Conditional rendering](#conditional-rendering)
    - [Data lists](#data-lists)
  - [Styling](#styling)
  - [Advanced components](#advanced-components)
    - [Class-based components](#class-based-components)
    - [Functional components](#functional-components)
    - [Component lifecycle](#component-lifecycle)
    - [Component creation lifecycle](#component-creation-lifecycle)
    - [Component update lifecycle](#component-update-lifecycle)
  - [HTTP requests](#http-requests)
  - [Routing](#routing)
  - [Forms & validation](#forms--validation)
  - [Redux](#redux)
  - [Authentication](#authentication)
  - [Testing](#testing)
  - [Deployment](#deployment)
  - [Bonus](#bonus)
  - [Links and references](#links-and-references)
    - [Videos](#videos)
    - [Websites](#websites)

## Introduction

[React](https://reactjs.org/) is a JavaScript library (developed by Facebook) allowing to build User Interfaces (UI). Since it's written in JavaScript, React runs in the browser not in servers.

A React user interface is split into components, for example, we could have component for the web page header, one ofr a sidebar and another one for the web page content (where articles are displayed). Components are building blocks that makes your application more manageable and reusable.

## React basics

### JSX

JSX is a syntax extension to JavaScript. It's a kind of a template language where you can use JavaScript code. This syntax has been created for and by React. It basically allows developers to write **code similar to HTML** in JavaScript code.

**Basic React component written in JSX**

```javascript
import React, {Component} from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
        <div className="App">
          <h1>Waouh! nice app</h1>
        </div>
    );

    // Without JSX, we would have to write the following code in order to have the same result:

    // return React.createElement(
    //     'div',
    //     { className: 'App' },
    //     React.createElement('h1', null, 'Waouh! nice app')
    // );

  }
}

export default App;
```

> :warning: In JSX, we are not writing real HTML, React will parse the JSX code to generate the real HTML code. This is the reason why in JSX we can't use the `class` keyword for CSS class in an HTML element, because `class` is a JavaScript reserved word! So; in JSX we have to use `className` for CSS classes.

Most of the HTML syntax will work in JSX, but be careful of the little differences.

#### Dynamic content

With JSX, and if you want dynamic content, you can use the single curly braces to tell React to interpret a variable or an JavaScript expression.

```javascript
import React from 'react';

const person = () => {
  const age = 28;
  return <p>I'm a person and I'm { age } years old</p>
}

export default person;
```



## Components

Components are the core building blocks of React applications. A React application could be depicted as a component tree, with a root component ("App") and a certain amount of nested child components.

Components are very useful, because they allow to write more manageable, reusable code. Your components can be static or you can configure them by passing arguments (which is called `props` in React).

A component needs to return JSX code to tell React which HTML code it should render to the real [DOM (Document Object Model)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).

There are two different ways of creating components.

- `Functional components`: also called `presentational`, `dumb` or `stateless` components.
- `Class-based components`: also called `containers`, `smart` or `stateful` components.

**Functional component**

```javascript
import React from 'react';

const person = () => {
  return <p>I'm a person</p>
}

export default person;
```

**Class-based component**

```javascript
import React, {Component} from 'react';

class Person extends Component {
  render() {
    return <p>I'm a person</p>
  }
}

export default Person;
```

The functional component is the recommended way of creating component, so use it as often as possible.

### Props

In React, the arguments we want to pass to a component are called `props`.

**Person.js**

```javascript
import React from 'react';

// it is a good practice to called this argument "props"
const person = (props) => {
  return <p>I'm a person and I'm { props.age } years old</p>
}

export default person;
```

**App.js**

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (
        <div className="App">
          <Person age={28}/> // passing argument in props
        </div>
    );
  }
}

export default App;
```

If you want to pass more content to your component, especially content between your component tags, React provides a special property on the `props` object which is called `children`.

**Person.js**

```javascript
import React from 'react';

const person = (props) => {
  return (
      <div>
        <p>I'm a person and I'm { props.age } years old</p>
        <p>{props.children}</p>
      </div>
      )
}

export default person;
```

**App.js**

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (
        <div className="App">
          <Person age={29}>More content...</Person>
        </div>
    );
  }
}

export default App;
```

> You can also pass JSX code (structured HTML content) to your component, not only text.

> Changes on the `props` will make React re-render (update) the DOM.

### State

In class-based component (and only in this kind of component), React provides you a particular property called `state`. This property allows you to store some variables for a specific component. The thing to know about this property is that if the state changes, React will re-render the DOM (update it).

**App.js**

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {

  // the name `state` is not optional, you must define it with that name.
  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28
      },
      {
        name: 'Marc',
        age: 32
      },

    ]
  };

  render() {
    return (
        <div className="App">
          <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
          <Person name={this.state.persons[1].name} age={this.state.persons[1].age} />
        </div>
    );
  }
}

export default App;
```

If you change (mutate) the state directly by directly updating the `state` property, React will issue a warning in your web browser console, and your update will not work. To mutate a state, React provides a function called `setState()` that will update the state and tell React that it was updated, so it can re-render the DOM.

`setState` takes an object as an argument and will merge this object with the current `state` object value.

#### useState() hook

Since React version 16.8, there is a way to manage states in non class-based component by using **React Hooks**. The hook we can use to manage states, is the `useState()` hook. This function takes the initial state asa value and returns two values. The first one is the current state represented by an object and the second one is a function that allows us update the state.

```javascript
import React, {Component, useState} from 'react';
import './App.css';
import Person from './Person/Person';

const app = props => {
  const [personsState, setPersonsState] = useState({
    persons: [
      { name: 'Olivier', age: 28 },
      { name: 'Marc', age: 32 },
    ]
  });

  const switchNameHandler = () => {
    // DON'T DO THIS: this.state.persons[0].name = 'Olivier';
    setPersonsState({
      persons: [
        { name: 'John', age: 28 },
        { name: 'Steve', age: 32 },
      ]
    });
  };

  return (
      <div className="App">
        <button onClick={switchNameHandler}>Switch Name</button>
        <Person
            name={personsState.persons[0].name}
            age={personsState.persons[0].age}
        />
        <Person
            name={personsState.persons[1].name}
            age={personsState.persons[1].age}
        />
      </div>
  );
};

export default app;
```

> The state setter function returned by the `useState` hook, will not merge states, but it will replace the old state with the new one. You can have multiple `useState` calls and so have multiple state objects.

### Events handling

In React and JSX, you can handle [events](https://reactjs.org/docs/events.html#supported-events) pretty easily by just defining a handler function and associate it to the wanted event on the wanted element.

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {

  clickHandler = () => {
    console.log('Clicked');
  }

  render() {
    return (
        <div className="App">
          <button onClick={this.clickHandler}>Switch name</button>
        </div>
    );
  }
}

export default App;
```

> Most of the React developers use the 'Handler` suffix on event handling functions.

> :warning: Do not put parenthesis when associating your event handler function to the HTML element. If you do, your function will be called immediately and not when the event is triggered.

### Conditional rendering

Like in other frontend frameworks (Angular and VueJS for example), it is possible to display display content under some conditions. In React, we are not going to use directives (like *ngIf in Angular) but since in JSX everything is JavaScript, we can simply use single curly brackets and ternary expression.

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {

  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28
      },
      {
        name: 'Marc',
        age: 32
      },

    ],
    showPersons: false
  };

  togglePersonHandler = () => {
    this.setState({showPersons: !this.state.showPersons});
  }

  render() {
    return (
        <div className="App">
          <button onClick={this.togglePersonHandler}>Toggle persons</button>
          {
            // here you can't use an "if" block
              this.state.showPersons ?
                  <div>
                    <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
                    <Person name={this.state.persons[1].name} age={this.state.persons[1].age} />
                  </div>
               : null
          }
        </div>
    );
  }
}

export default App;
```

This syntax could be a little confusing, so another way to do that, a more JavaScript way we could say, would be to define a variable containing the HTML code and to use an "if" statement to check the value of the boolean.

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {

  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28
      },
      {
        name: 'Marc',
        age: 32
      },

    ],
    showPersons: false
  };

  togglePersonHandler = () => {
    this.setState({showPersons: !this.state.showPersons});
  }

  render() {
    let persons = null;
    if(this.state.showPersons) {
      persons = (<div>
        <Person name={this.state.persons[0].name} age={this.state.persons[0].age} />
        <Person name={this.state.persons[1].name} age={this.state.persons[1].age} />
      </div>);
    }


    return (
        <div className="App">
          <button onClick={this.togglePersonHandler}>Toggle persons</button>
          {persons} // nothing will be displayed if persons is "null"
        </div>
    );
  }
}

export default App;
```

### Data lists

In the same way as for the conditional rendering, other frameworks propose directives to render lists/arrays (*ngFor in Angular for example). In React, again, everything is JavaScript, so what we need to do is to loop over our list and return valid JSX code.

```javascript
import React, {Component} from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {

  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28
      },
      {
        name: 'Marc',
        age: 32
      },

    ],
    showPersons: false
  };

  togglePersonHandler = () => {
    this.setState({showPersons: !this.state.showPersons});
  }

  render() {
    let persons = null;
    if(this.state.showPersons) {
      persons = (<div>
        {this.state.persons.map((person) => {
          return <Person name={person.name} age={person.age} />
        })}
      </div>);
    }
    return (
        <div className="App">
          <button onClick={this.togglePersonHandler}>Toggle persons</button>
          {persons}
        </div>
    );
  }
}

export default App;
```

> You may see in the web console that there is a warning mentioning the "key"property. This key will allow React to keep track of the individual elements of the list. Therefore, React will know which element changed and which didn't, so it will not have to re-render the whole list (which can lead to bad performances with long lists). The value of key should be unique among siblings not globally. Typically, the key will be the id of your data. For the example, we are going to use the person's name as the key.
> ```javascript
> <Person name={person.name} age={person.age} key={person.name}/>
> ```

## Styling

If you want your components to have CSS styling, there are two ways to do that.

First solution is to create CSS files for your components and then, in the JSX file, import the wanted CSS file. Keep in mind that CSS rules defined in a CSS file are global and not restricted to the component which is imported the file.

**Person.css**

```css
.Person {
    color: blue;
}
```

**Person.js**

```javascript
import React from 'react';
import './Person.css';

const person = (props) => {
  return (
      <div>
        <p className="Person">Hi! I'm a person, my name is {props.name} and I'm { props.age } years old</p>
      </div>
   );
}

export default person;
```

The second solution is to use inline styles. Inline style consists in declaring CSS style in a JavaScript object in the component.

```javascript
import React from 'react';
import './Person.css';

const person = (props) => {

  const style = {
    color: 'red'
  }
  return (
      <div>
        <p style={style}>Hi! I'm a person, my name is {props.name} and I'm { props.age } years old</p>
      </div>
      )
}

export default person;
```

> If you want to use pseudo-selectors in inline styling, you can install third-party libraries like the [styled-components package](https://styled-components.com/).

> If you want to use separate CSS files but don't want your styles to be global, you can use [CSS modules](https://css-tricks.com/css-modules-part-1-need/).

## Advanced components

### Class-based components

Class-based components are Javascript classes that extends the `Component` class. They can access state and they can update it with the `setState` function. They can also use **lifecycle hooks**. Because we are in a class, properties (such as `state` and `props`) will be accessed with the `this` keyword.

### Functional components

Functional components are represented by JavaScript function (mostly using ES6 arrow function notation) assigned to constant (that will be exported in order to use them as component). In the past (before React 16.8 and React Hooks), they couldn't manage state. But, since React hooks were introduced, they can with the `useState` hook. However, lifecycle hooks are not available with function components. Unlike a class-based component, in a functional component, you will get all your properties by the `props` variable.

> Keep in mind, that you could be working on older React project where functional components can't handle state.

### Component lifecycle

The component lifecycle is only available in class-based components. This lifecycle is represented by lifecycle hooks (lifecycle hooks has nothing to do with React hooks!). There are two different lifecycle we could say, one for the component creation and one when a component is updated (because props or state changed).

### Component creation lifecycle

- `constructor(props)` => this is an ES6 feature and by default it is done for you. If you want to define the constructor, remember to always call `super(props)` as the first instruction in the constructor function.
- `getDerivedStateFromProps(props, state)` => this hook exists for the are case where the state depends on props changes during time. It is a static method that should return the updated state. Keep in mind that is very rarely used.
- `render()` => this hook will prepare and structure your JSX code. Don't execute code that can blocked the rendering process in this function. Every child component you included in your rendered component here will then be rendered as well.
- `componentDidMount()` => this hook will be called once all child component of this component have finished their lifecycle. Here you can make your HTTP requests to get data from servers. Don't update the state synchronously (you can do it in a promise for example) in this hook because it will trigger a re-render process that is bad for performances.

> They are still historic lifecycle hooks that you can use, for example `componentWillMount()`, but since they are very rarely used and could lead to side effects, they might be removed in the future.

### Component update lifecycle



## HTTP requests

## Routing

## Forms & validation

## Redux

## Authentication

## Testing

## Deployment

## Bonus

## Links and references

### Videos

- https://www.udemy.com/course/react-the-complete-guide-incl-redux/

### Websites

- https://reactjs.org/