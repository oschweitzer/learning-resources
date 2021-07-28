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
    - [useEffect()](#useeffect)
    - [Optimization](#optimization)
    - [DOM updates](#dom-updates)
    - [Higher Order Components](#higher-order-components)
    - [PropTypes](#proptypes)
    - [Refs](#refs)
    - [Context](#context)
      - [contextType](#contexttype)
      - [useContext()](#usecontext)
  - [HTTP requests](#http-requests)
  - [Routing](#routing)
    - [Router props](#router-props)
    - [Absolute vs Relative paths](#absolute-vs-relative-paths)
    - [Route parameters](#route-parameters)
    - [Navigating programmatically](#navigating-programmatically)
    - [Redirection](#redirection)
    - [Guards](#guards)
    - [Lazy loading](#lazy-loading)
  - [Redux](#redux)
    - [Definition](#definition)
    - [Redux & React](#redux--react)
      - [Connecting components](#connecting-components)
      - [Dispatching actions](#dispatching-actions)
      - [Passing and retrieving data with Action](#passing-and-retrieving-data-with-action)
      - [Combining reducers](#combining-reducers)
    - [State types](#state-types)
    - [Advanced concepts](#advanced-concepts)
      - [Middlewares](#middlewares)
      - [Action creators](#action-creators)
      - [Async code](#async-code)
  - [Links and references](#links-and-references)
    - [Videos](#videos)
    - [Websites](#websites)

## Introduction

[React](https://reactjs.org/) is a JavaScript library (developed by Facebook) allowing to build User Interfaces (UI). Since it's written in JavaScript, React runs in the browser and not in servers.

A React user interface is split into components, for example, we could have component for the web page header, one for a sidebar and another one for the web page content (where articles are displayed). Components are building blocks that makes your application more manageable and reusable.

## React basics

### JSX

JSX is a syntax extension to JavaScript. It's a kind of a template language where you can use JavaScript code. This syntax has been created for and by React. It basically allows developers to write **code similar to HTML** in JavaScript code.

**Basic React component written in JSX**

```javascript
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className='App'>
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

With JSX, and if you want dynamic content, you can use the single curly braces to tell React to interpret a variable or a JavaScript expression.

```javascript
import React from 'react';

const person = () => {
  const age = 28;
  return <p>I'm a person and I'm {age} years old</p>;
};

export default person;
```

## Components

Components are the core building blocks of React applications. A React application could be depicted as a component tree, with a root component ("App") and a certain amount of nested child components.

Components are very useful, because they allow to write more manageable, reusable code. Your components can be static or you can configure them by passing arguments (which are called `props` in React).

A component needs to return JSX code to tell React which HTML code it should render to the real [DOM (Document Object Model)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model).

There are two different ways of creating components.

- `Functional components`: also called `presentational`, `dumb` or `stateless` components.
- `Class-based components`: also called `containers`, `smart` or `stateful` components.

**Functional component**

```javascript
import React from 'react';

const person = () => {
  return <p>I'm a person</p>;
};

export default person;
```

**Class-based component**

```javascript
import React, { Component } from 'react';

class Person extends Component {
  render() {
    return <p>I'm a person</p>;
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
  return <p>I'm a person and I'm {props.age} years old</p>;
};

export default person;
```

**App.js**

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (
      <div className='App'>
        <Person age={28} /> // passing argument in props
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
      <p>I'm a person and I'm {props.age} years old</p>
      <p>{props.children}</p>
    </div>
  );
};

export default person;
```

**App.js**

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  render() {
    return (
      <div className='App'>
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
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  // the name `state` is not optional, you must define it with that name.
  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28,
      },
      {
        name: 'Marc',
        age: 32,
      },
    ],
  };

  render() {
    return (
      <div className='App'>
        <Person
          name={this.state.persons[0].name}
          age={this.state.persons[0].age}
        />
        <Person
          name={this.state.persons[1].name}
          age={this.state.persons[1].age}
        />
      </div>
    );
  }
}

export default App;
```

If you change (mutate) the state by directly updating the `state` property, React will issue a warning in your web browser console, and your update will not work. To mutate a state, React provides a function called `setState()` that will update the state and tell React that it was updated, so it can re-render the DOM.

`setState` takes an object as an argument and will merge this object with the current `state` object value.

#### useState() hook

Since React version 16.8, there is a way to manage states in non class-based
component by using **React Hooks**. The hook we can use to manage states, is the
`useState()` hook. This function takes the initial state as a value and returns
two values. The first one is the current state represented by an object and the
second one is a function that allows us update the state.

```javascript
import React, { Component, useState } from 'react';
import './App.css';
import Person from './Person/Person';

const app = (props) => {
  const [personsState, setPersonsState] = useState({
    persons: [
      { name: 'Olivier', age: 28 },
      { name: 'Marc', age: 32 },
    ],
  });

  const switchNameHandler = () => {
    // DON'T DO THIS: this.state.persons[0].name = 'Olivier';
    setPersonsState({
      persons: [
        { name: 'John', age: 28 },
        { name: 'Steve', age: 32 },
      ],
    });
  };

  return (
    <div className='App'>
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

> If you need to update the state but you depend on the previous state value, it's a best practice to pass a function instead of passing an object as the first argument of the `useState` method. This function takes two parameters, the previous state and the current props and you will have to return the new state.

### Events handling

In React and JSX, you can handle [events](https://reactjs.org/docs/events.html#supported-events) pretty easily by just defining an handler function and associate it to the wanted event on the wanted element.

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  clickHandler = () => {
    console.log('Clicked');
  };

  render() {
    return (
      <div className='App'>
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

Like in other frontend frameworks (Angular and VueJS for example), it is possible to display display content under some conditions. In React, we are not going to use directives (like \*ngIf in Angular) but since in JSX everything is JavaScript, we can simply use single curly brackets and ternary expression.

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28,
      },
      {
        name: 'Marc',
        age: 32,
      },
    ],
    showPersons: false,
  };

  togglePersonHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    return (
      <div className='App'>
        <button onClick={this.togglePersonHandler}>Toggle persons</button>
        {
          // here you can't use an "if" block
          this.state.showPersons ? (
            <div>
              <Person
                name={this.state.persons[0].name}
                age={this.state.persons[0].age}
              />
              <Person
                name={this.state.persons[1].name}
                age={this.state.persons[1].age}
              />
            </div>
          ) : null
        }
      </div>
    );
  }
}

export default App;
```

This syntax could be a little confusing, so another way to do that, a more JavaScript way we could say, would be to define a variable containing the HTML code and to use an "if" statement to check the value of the boolean.

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28,
      },
      {
        name: 'Marc',
        age: 32,
      },
    ],
    showPersons: false,
  };

  togglePersonHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          <Person
            name={this.state.persons[0].name}
            age={this.state.persons[0].age}
          />
          <Person
            name={this.state.persons[1].name}
            age={this.state.persons[1].age}
          />
        </div>
      );
    }

    return (
      <div className='App'>
        <button onClick={this.togglePersonHandler}>Toggle persons</button>
        {persons} // nothing will be displayed if persons is "null"
      </div>
    );
  }
}

export default App;
```

### Data lists

In the same way as for the conditional rendering, other frameworks propose directives to render lists/arrays (\*ngFor in Angular for example). In React, again, everything is JavaScript, so what we need to do is to loop over our list and return valid JSX code.

```javascript
import React, { Component } from 'react';
import './App.css';
import Person from './Person/Person';

class App extends Component {
  state = {
    persons: [
      {
        name: 'Olivier',
        age: 28,
      },
      {
        name: 'Marc',
        age: 32,
      },
    ],
    showPersons: false,
  };

  togglePersonHandler = () => {
    this.setState({ showPersons: !this.state.showPersons });
  };

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        <div>
          {this.state.persons.map((person) => {
            return <Person name={person.name} age={person.age} />;
          })}
        </div>
      );
    }
    return (
      <div className='App'>
        <button onClick={this.togglePersonHandler}>Toggle persons</button>
        {persons}
      </div>
    );
  }
}

export default App;
```

> You may see in the web console that there is a warning mentioning the "key"property. This key will allow React to keep track of the individual elements of the list. Therefore, React will know which element changed and which didn't, so it will not have to re-render the whole list (which can lead to bad performances with long lists). The value of key should be unique among siblings not globally. Typically, the key will be the id of your data. For the example, we are going to use the person's name as the key.
>
> ```javascript
> <Person name={person.name} age={person.age} key={person.name} />
> ```

## Styling

If you want your components to have CSS styling, there are two ways to do that.

First solution is to create CSS files for your components and then, in the JSX
file, import the wanted CSS file. Keep in mind that CSS rules defined in a CSS
file are global and not restricted to the component which is importing the CSS file.

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
      <p className='Person'>
        Hi! I'm a person, my name is {props.name} and I'm {props.age} years old
      </p>
    </div>
  );
};

export default person;
```

The second solution is to use inline styles. Inline style consists in declaring CSS style in a JavaScript object in the component.

```javascript
import React from 'react';
import './Person.css';

const person = (props) => {
  const style = {
    color: 'red',
  };
  return (
    <div>
      <p style={style}>
        Hi! I'm a person, my name is {props.name} and I'm {props.age} years old
      </p>
    </div>
  );
};

export default person;
```

> If you want to use pseudo-selectors in inline styling, you can install third-party libraries like the [styled-components package](https://styled-components.com/).

> If you want to use separate CSS files but don't want your styles to be global, you can use [CSS modules](https://css-tricks.com/css-modules-part-1-need/).

## Advanced components

### Class-based components

Class-based components are Javascript classes that extends the `Component` class. They can access state and they can update it with the `setState` function. They can also use **lifecycle hooks**. Because we are in a class, properties (such as `state` and `props`) will be accessed with the `this` keyword.

### Functional components

Functional components are represented by JavaScript function (mostly using ES6
arrow function notation) assigned to constant (that will be exported in order to
use them as component). In the past (before React 16.8 and React Hooks), they
couldn't manage state. But, since React hooks were introduced, they can with the
`useState` hook. However, lifecycle hooks are not available with functional
components (event though the `useEffect` React Hook allows to do side effects). In a functional component, you will get all your properties by the
`props` variable.

> Keep in mind, that you could be working on projects using older React versions where functional components can't handle state.

### Component lifecycle

The component lifecycle is only available in class-based components. This lifecycle is represented by lifecycle hooks (**lifecycle hooks have nothing to do with React hooks!**). There are two different lifecycles we could say, one for the component creation and one when a component is updated (because `props` or `state` changed).

#### Component creation lifecycle

- `constructor(props)` => this is an ES6 feature and by default it is done for you. If you want to define the constructor, remember to always call `super(props)` as the first instruction in the constructor function.
- `getDerivedStateFromProps(props, state)` => this hook exists for the case
  where the state depends on props changes during time. It is a static method
  that should return the updated state. Keep in mind that it is very rarely
  used.
- `render()` => this hook will prepare and structure your JSX code. Don't execute code that can blocked the rendering process in this function. Every child component you included in your rendered component here will then be rendered as well.
- `componentDidMount()` => this hook will be called once all child components of
  this component have finished their lifecycle. Here you can make your HTTP
  requests to get data from servers for example. Don't update the state synchronously
  (you can do it in a promise for example) in this hook because it will trigger
  a re-render process that is bad for performances.

> They are still historic lifecycle hooks that you can use, for example `componentWillMount()`, but since they are very rarely used and could lead to side effects, they might be removed in the future.

#### Component update lifecycle

- `getDerivedStateFromProps(props, state)` => used to sync state with props but very rarely used.
- `shouldComponentUpdate(nextProps, nextState)` => allows to cancel the updating process. This hook should return a boolean, true if the update should continue, false otherwise. It can be used to do optimization by restricting updating to only changes on specific data.
- `render()` => React goes through the JSX code, evaluates it and constructs its virtual DOM.
- `getSnapshotBeforeUpdate(prevProps, prevState)` => takes the previous props and the previous state as input and returns a snapshot. It is not very used but you can use it for last minute DOM operations (for example, getting the current scrolling position of the user).
- `componentDidUpdate()` => the updating is now done, so here you can do side effects (HTTP requests for example). Avoid synchronous functions otherwise you will have infinite loops.

### useEffect()

Since React Hooks, it is possible to handle state in functional components. The `useEffect()` function basically regroup all the class-based lifecycle methods in one React hook. It is `componentDidUpdate()` and `componentDidMount()` combined in one effect.

The first argument of this function is the effect, the function that will be
executed on changes or creation. The second argument is an array of data. With
this array you can restrict the hook to only trigger on changes on these data.
If this array is empty, the effect will only run one time, on the component
creation (like the `componentDidMount()` function).

You can return a function in the effect function, this allows to do some cleanup
work for example because this function will be called right before the next
lifecycle or when the component is destroyed (be sure to pass an empty array to
`useEffect()` to only run it one time).

You can have more than one `useEffect()` hook in a single component.

### Optimization

To optimize functional component, meaning not re-rendering it when its props don't change, you can wrap it using the `React.memo` function. Of course, if your component will always change with its parent component, don't use this function because you will execute useless code.

If you need to check all the properties of a component in the `shouldComponentUpdate` hook, meaning you know that your component will render the same result given the same props and state, you can extends this component from `PureComponent` instead of `Component`. `PureComponent` will implement the shouldComponentUpdate for you and do a shallow prop and state comparison (be careful if you have complex data structure because it can lead to false positive).

### DOM updates

When calling the `render()` function (for class-based component) or retuning JSX code (for functional component), React starts by using a virtual DOM, which is a DOM representation in Javascript. Actually, React will keep two virtual DOM (faster than touching the real DOM directly), the old virtual DOM and the re-rendered one. The re-rendered one is the one which gets created when the render() method is called. When calling `render()`, React will not immediately update the real DOM, it will first do a comparison between the old virtual DOM and the re-rendered one and checks for differences. If there are differences, it will update the real DOM at places where differences were detected (it will not re-rendered the whole DOM). If there are no differences, the real DOM will not be touched.

### Higher Order Components

Sometimes, you may want to have adjacent JSX code in your component, but this is not possible because JSX is the shorter form of using the `React.createElement()` function and we can't return multiple functions in JS. So to solve that, there are two solutions.

The first one is to use an array, so instead of returning JSX code inside parenthesis, you will put it between brackets and separate your adjacent JSX element with comma like on the example below.

```javascript
import React from 'react';

const cockpit = (props) => {
  return [
    <h1>{props.title}</h1>,
    <p>This is really working!</p>,
    <button className={btnClass} onClick={props.clicked}>
      Toggle
    </button>,
  ];
};

export default cockpit;
```

The second solution, is to use an Higher Order Component (HOC) to wrap your JSX adjacent elements. An HOC is basically a component that will transform a component into a component.

To allow adjacent elements, React provides the `React.Fragment` HOC.

```javascript
import React from 'react';

const cockpit = (props) => {
  return (
    <React.Fragment>
      <h1>{props.title}</h1>
      <p>This is really working!</p>
      <button className={btnClass} onClick={props.clicked}>
        Toggle
      </button>
    </React.Fragment>
  );
};

export default cockpit;
```

HOC can be used to do whatever you want, it's not really a React feature but a design pattern.

### PropTypes

You may wonder how to avoid that a developer uses your components in the wrong way, for example passing unused data or wrong data type to your components props. There is a npm package that will help you do that, it's called [prop-types](https://www.npmjs.com/package/prop-types).

Once installed, in your component, simply import the PropTypes object and use it like in the example below.

```javascript
import React from 'react';
import './Person.module.css';
import PropTypes from 'prop-types';

const person = (props) => {
  const style = {
    color: 'red',
  };
  return (
    <div onClick={props.click}>
      <p style={style}>
        Hi! I'm a person, my name is {props.name} and I'm {props.age} years old
      </p>
    </div>
  );
};

// define the JS type of your props
person.propTypes = {
  click: PropTypes.func,
  age: PropTypes.number,
  name: PropTypes.string,
};

export default person;
```

If the data type is wrong, you will see an error in your web console.

### Refs

React provides a special attribute that you can attach to any component, called `ref` (for reference). Refs are objects of functions created by the `React.createRef()` function. These refs allow to have a direct access to a DOM element or component instance.

**Example on managing the focus on input elements**

```javascript
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // create a ref to store the textInput DOM element
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // Explicitly focus the text input using the raw DOM API
    // Note: we're accessing "current" to get the DOM node
    this.textInput.current.focus();
  }

  render() {
    // tell React that we want to associate the <input> ref
    // with the `textInput` that we created in the constructor
    return (
      <div>
        <input type='text' ref={this.textInput} />
        <input
          type='button'
          value='Focus the text input'
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

In functional component, you can use a React Hook called `useRef()`.

```javascript
import React, { useEffect, useRef } from 'react';

const Cockpit = (props) => {
  const toggleBtnRef = useRef(null);

  useEffect(() => {
    // we need to do that in useEffect, because elsewhere the button is not rendered.
    toggleBtnRef.current.click();
  }, []);

  return (
    <div>
      <button ref={toggleBtnRef} onClick={props.clicked}>
        Toggle persons
      </button>
      {props.persons}
    </div>
  );
};

export default Cockpit;
```

### Context

React provides what is called context feature. This feature allows to pass data through the components tree (from the parent to a grandchild for example) without having to pass these data manually at each layer with the props.

To create a context, simply use `React.createContext()` (you can initialize it with the value you like) and you will get a Javascript object. The key of this feature is that you will decide which component has access to this 'global' object.

To do that, you will have to use the `Provider` component that is provided by React for every Context object. This `Provider` accepts a prop value that will be available to the child component of this `Provider` component.

In the child component, you will have to "consume" the context in order to have your data. To do that, a context has `Consumer` component that need to wrap your component. But be careful, `Consumer` need to wrap a function, not directly the JSX code. This function allows you to have the context as an argument and so you can use it.

For example, take an App component with a child component called Persons which also have children components called Person. If I want to pass data from App to Person without having to pass data through Persons, it is possible with React context.

**Context component**

```javascript
import React from 'react';

const myContext = React.createContext({
  data: 'my data',
});

export default myContext;
```

**Parent component App.js**

```javascript
import React, { Component } from 'react';
import './App.module.css';
import { Persons } from '../components/Persons/Persons';
import Cockpit from '../components/Cockpit/Cockpit';
import MyContext from '../context/MyContext';

class App extends Component {
  /*
   * ...
   */

  render() {
    let persons = null;
    if (this.state.showPersons) {
      persons = (
        // here we are defining our data (default value of the context doesn't really matter then)
        <MyContext.Provider value={{ data: 'my data' }}>
          <Persons
            persons={this.state.persons}
            clicked={this.deletePersonHandler}
          />
        </MyContext.Provider>
      );
    }
    return (
      <div className='App'>
        <Cockpit clicked={this.togglePersonHandler} persons={persons} />
      </div>
    );
  }
}

export default App;
```

**Grandchild component Person.js**

```javascript
import React from 'react';
import './Person.module.css';
import MyContext from '../../../context/MyContext';

const person = (props) => {
  const style = {
    color: 'red',
  };
  return (
    <MyContext.Consumer>
      {
        // here we need a function to get the context
        (context) => (
          <div onClick={props.click}>
            <p style={style}>
              Hi! I'm a person, my name is {props.name} and I'm {props.age}{' '}
              years old. Here is data: {context.data}
            </p>
          </div>
        )
      }
    </MyContext.Consumer>
  );
};

export default person;
```

#### contextType

In class-based component and by using the context like we saw above, you can only access this context in your JSX code. What if you want to access it in a lifecycle hook for example ? Well, React provides another way to use context, and it's a static property named `contextType`. by assigning your Context to this property, your class will have a new property named `context` (and so accessible via `this.context`).

```javascript
import React, { Component } from 'react';
import './Person.module.css';
import MyContext from '../../../context/MyContext';

export class Person extends Component {
  style = {
    color: 'red',
  };

  static contextType = MyContext;

  render() {
    return (
      <div onClick={this.props.click}>
        <p style={this.style}>
          Hi! I'm a person, my name is {this.props.name} and I'm{' '}
          {this.props.age} years old. Here is data: {this.context.data}
        </p>
      </div>
    );
  }
}
```

#### useContext()

For functional components, it is also possible to have this context feature
thanks to React Hooks. The hook for contexts is named `useContext()`. Just pass
your context object as a parameter to this hook. `useContext()` is the
equivalent of the `contextType` property or the `Context.Consumer`. You still
have to use the `Context.Provider` component on your parent component.

```javascript
import React, { useContext } from 'react';
import MyContext from '../../context/MyContext';

const Cockpit = (props) => {
  const myContext = useContext(MyContext);

  return (
    <div>
      <button ref={toggleBtnRef} onClick={props.clicked}>
        Toggle persons
      </button>
      {props.persons}
      <p>{myContext.data}</p>
    </div>
  );
};

export default Cockpit;
```

## HTTP requests

If you want your React application to get/send data from/to a server, you will probably use HTTP requests.

Like mentioned in the component lifecycle, the best place to do HTTP requests (and especially to get data when our component is created) is in the `componentDidMount()` lifecycle hook, because side effects are authorized there.

It is recommended to use the [axios](https://github.com/axios/axios) library
since it's one of the most popular HTTP library and it's available for frontend
code as well as for backend code.

For example, if I have an application that simulates a blog with posts.

**Post.js**

```javascript
import React from 'react';

import './Post.css';

const post = (props) => (
  <article className='Post'>
    <h1>{props.title}</h1>
    <div className='Info'>
      <p>{props.body}</p>
    </div>
  </article>
);

export default post;
```

**Blog.js**

```javascript
import React, { Component } from 'react';
import Post from '../../components/Post/Post';
import './Blog.css';
import axios from 'axios';

class Blog extends Component {
  state = {
    posts: [],
  };

  // Here it is very important to note that an HTTP request is an asynchronous task ! So the Promise should be handled correctly
  async componentDidMount() {
    const response = await axios.get(
      'https://jsonplaceholder.typicode.com/posts'
    );
    this.setState({ posts: response.data });
  }

  render() {
    const posts = this.state.posts.map((post) => (
      <Post key={post.id} title={post.title} body={post.body} />
    ));
    return (
      <div>
        <section className='Posts'>{posts}</section>
      </div>
    );
  }
}

export default Blog;
```

## Routing

If you want to make your single page application more like a multi-pages application, you can use routing. Routing allows to keep an SPA architecture while improving the navigation and the user experience.

React (unlike Angular for example) doesn't provide built in features for routing. Most React developers use the [react-router](https://github.com/ReactTraining/react-router/tree/master/packages/react-router) package to handle routing (which is the core package, but you may also need to install [react-router-dom](https://github.com/ReactTraining/react-router/tree/master/packages/react-router-dom) for DOM bindings).

> Note that technically, only the `react-router-dom` it necessary because it already contains the `react-router` package, but keep in mind that if you search for information, search for `react-router`.

This package will parse the URL (the path), then read our configuration to know which path correspond to which component.

First, you have to wrap your routing component (in the `App.js` file of the `index.js` file for example) between the `BrowserRouter` component from the `react-router-dom` package to activate routing capabilities.

Then in your component, in your JSX code, use the `Route` component to tell the path and the JSX code (or component reference) you want to render when accessing this path.

**App.js**

```javascript
import React, { Component } from 'react';
import Blog from './containers/Blog/Blog';
import { BrowserRouter } from 'react-router-dom';

class App extends Component {
  render() {
    return (
      <div className='App'>
        <BrowserRouter>
          <Blog />
        </BrowserRouter>
      </div>
    );
  }
}

export default App;
```

**Blog.js**

```javascript
import React, { Component } from 'react';
import './Blog.css';
import { Posts } from './Posts/Posts';
import { Route } from 'react-router-dom';
import NewPost from './NewPost/NewPost';

class Blog extends Component {
  render() {
    return (
      <div>
        <header className={'Navigation'}>
          <nav>
            <ul>
              <li>
                <a href={'/'}>Home</a>
              </li>
              <li>
                <a href={'/new-post'}>New Post</a>
              </li>
            </ul>
          </nav>
        </header>
        {/* exact means that the path should exactly correspond, otherwise the router just do a startWith
           comparison */}
        <Route exact path={'/'} component={Posts} />
        <Route exact path={'/new-post'} component={NewPost} />
      </div>
    );
  }
}

export default Blog;
```

This code is working well but there is an issue. Each time you will click on the navigation items, the whole page will reload. This can be an issue because the different states you may have will be reset. What you want is to tell React to only re-render some parts of the page.

To do that, you will have to replace the `<a></a>` elements with `<Link></Link>` components (from `react-router-dom`).

**Blog.js**

```javascript
import React, { Component } from 'react';
import './Blog.css';
import { Posts } from './Posts/Posts';
import { Link, Route } from 'react-router-dom';
import NewPost from './NewPost/NewPost';

class Blog extends Component {
  render() {
    return (
      <div>
        <header className={'Navigation'}>
          <nav>
            <ul>
              <li>
                <Link to={'/'}>Home</Link>
              </li>
              <li>
                <Link to={'/new-post'}>New Post</Link>
              </li>
            </ul>
          </nav>
        </header>
        {/* exact means that the path should exactly correspond, otherwise the router just do a startWith
           comparison */}
        <Route exact path={'/'} component={Posts} />
        <Route exact path={'/new-post'} component={NewPost} />
      </div>
    );
  }
}

export default Blog;
```

> The `to` properties can also take an object allowing to add hashes or query parameters.

> If you want to style your links, use the `<NavLink></NavLink>` component instead of `<Link></Link>`.

### Router props

When using the `Route` component, some additional props are passed to the child component. But these props are not passed down the all component tree, they only are passed to the child component. If you need to get access to these props in the child component children, you can use two different methods.

The first way is to pass the props from the component to its children (with the spread operator `{...props}`).

**Example in Posts.js**

```javascript
<Post
              key={post.id}
              title={post.title}
              body={post.body}
              clicked={() => this.postSelectedHandler(post.id)}
              {...this.props} // passing all the Posts props (including Route props) to the Post component
          />);
```

The second solution is to use an HOC (Higher Order Component) named `withRouter`. To use it, simply wrap the component that needs to have the Route props with the `withRouter()` function.

**Post.js**

```javascript
import React from 'react';
import './Post.css';
import { withRouter } from 'react-router';

const post = (props) => {
  console.log(props);
  return (
    <article className='Post' onClick={props.clicked}>
      <h1>{props.title}</h1>
      <div className='Info'>
        <p>{props.body}</p>
      </div>
    </article>
  );
};

export default withRouter(post); // wrapping component between withRouter HOC
```

### Absolute vs Relative paths

With the `to` property on the Link component, keep in mind that paths are always treated as absolute paths. That means, the provided path will always be appended to your root domain.

If you want relative paths, you can use the `props.match` object and its `url` property. This property is the current path you're on. So simply concatenate this current path with the new path to obtain relative paths.

```javascript
<Link to={props.match.url + '/new'}>
```

### Route parameters

You can use route parameters to have dynamic parameters and created multiple routes. In our example, we might want to access a path like '/<postId>' where `postId` is dynamic because we are not going to write a route for each post id.

To do that, simply add a `Route` and, in this route path, use the ':' operator followed by the name of your dynamic parameter.

**Blog.js**

```javascript
import React, { Component } from 'react';
import './Blog.css';
import { Posts } from './Posts/Posts';
import { NavLink, Route, Switch } from 'react-router-dom';
import NewPost from './NewPost/NewPost';
import FullPost from './FullPost/FullPost';

class Blog extends Component {
  render() {
    return (
      <div>
        <header className={'Navigation'}>
          <nav>
            <ul>
              <li>
                <NavLink exact to={'/'}>
                  Home
                </NavLink>
              </li>
              <li>
                <NavLink to={'/new-post'}>New Post</NavLink>
              </li>
            </ul>
          </nav>
        </header>
        <Switch>
          {/* exact means that the path should exactly correspond, otherwise the router just do a startWith
           comparison */}
          <Route exact path={'/'} component={Posts} />
          <Route exact path={'/new-post'} component={NewPost} />
          <Route exact path={'/:postId'} component={FullPost} />
        </Switch>
      </div>
    );
  }
}

export default Blog;
```

Routes order is important, here `/:postId` should be placed after `/new-post` in order to avoid `new-post` to be interpreted as an `postId`. But this will not resolve it all, because by default the router will render all routes if they match the path. In our case, when going on the `/new-post` path, it will render both `NewPost` and `FullPost` components. To solve that, simply wrap your routes between a `Switch` component (provided by `react-router-dom`). This `Switch` component will only render the first component when the route matches the path.

Then in your child component, you can get this dynamic parameter by using the `props.match` object and its `params` property which contains the parameters list.

> For query/search parameters (delimited by a `?`) and fragments (delimited by `#`) you can access them with the `props.location` object and respectively with the `search` and `hash` properties.

### Navigating programmatically

You may want to navigate between pages using code instead of `<Link></Link>`
component. To do that, simply use the functions provided by the `props.history`
object. For example, to navigate to a specific page, use the `push()` function,
or if you simply want to go back (like you do with the button on your browser),
use the `goBack()` function.

### Redirection

The `react-router-dom` provides a `Redirect` component that allows to redirect users to another path. This component has two properties, `from`, which tells on which path it should trigger and `to`, to tell to which path it should redirect the user.

It is also possible to use this component conditionally by using an if statement in your JSX code. For example when a Post is send to the server, you may want to display the Redirect component meaning redirecting the user to the home page.

> Of course, you can redirect by using the `props.history` object and the `push` or `replace` functions. Basically the `replace` function is doing the same thing as `Redirect` whereas the `push` function allows the user to get back to the previous page after the redirection.

### Guards

Navigation guard are typically used to allow or not a user to access certain routes. In React, with the react router, you can see guard as conditionally displaying components or not.

```javascript
import React, { Component } from 'react';
import './Blog.css';
import { Posts } from './Posts/Posts';
import { NavLink, Redirect, Route, Switch } from 'react-router-dom';
import NewPost from './NewPost/NewPost';

class Blog extends Component {
  state = {
    auth: true,
  };

  render() {
    return (
      <div>
        <header className={'Navigation'}>
          <nav>
            <ul>
              <li>
                <NavLink exact to={'/'}>
                  Home
                </NavLink>
              </li>
              <li>
                <NavLink to={'/new-post'}>New Post</NavLink>
              </li>
            </ul>
          </nav>
        </header>
        {/* exact means that the path should exactly correspond, otherwise the router just do a startWith
           comparison */}
        <Switch>
          {/* Guard */}
          {this.state.auth ? (
            <Route exact path={'/new-post'} component={NewPost} />
          ) : null}
          <Route path={'/posts'} component={Posts} />
          <Redirect from={'/'} to={'/posts'} />
        </Switch>
      </div>
    );
  }
}

export default Blog;
```

### Lazy loading

Lazy loading is a pattern where you only load components when the user really need them. For example if a user never clicks on one of your navigation item, you will still have loaded the associated component for nothing.

To do that, we will have to implement an HOC that will load the provided component.

**HOC AsyncComponent**

```javascript
import React, { Component } from 'react';

const asyncComponent = (importComponent) => {
  return class extends Component {
    state = {
      component: null,
    };

    async componentDidMount() {
      const cmp = await importComponent();
      this.setState({ component: cmp.default });
    }

    render() {
      const C = this.state.component;
      return C ? <C {...this.props} /> : null;
    }
  };
};

export default asyncComponent;
```

**Blog.js**

```javascript
import React, { Component } from 'react';
import './Blog.css';
import { Posts } from './Posts/Posts';
import { NavLink, Redirect, Route, Switch } from 'react-router-dom';
import asyncComponent from '../../hoc/asyncComponent';
// This will import the NewPost component only when the function passed as a parameter is called (when the user will click on the New Post menu)
const AsyncNewPost = asyncComponent(() => import('./NewPost/NewPost'));

class Blog extends Component {
  state = {
    auth: true,
  };

  render() {
    return (
      <div>
        <header className={'Navigation'}>
          <nav>
            <ul>
              <li>
                <NavLink exact to={'/'}>
                  Home
                </NavLink>
              </li>
              <li>
                <NavLink to={'/new-post'}>New Post</NavLink>
              </li>
            </ul>
          </nav>
        </header>
        <Switch>
          {this.state.auth ? (
            <Route exact path={'/new-post'} component={AsyncNewPost} />
          ) : null}
          <Route path={'/posts'} component={Posts} />
          <Redirect from={'/'} to={'/posts'} />
        </Switch>
      </div>
    );
  }
}

export default Blog;
```

> This setup works for React application created with the
> [create-react-app](https://github.com/facebook/create-react-app) package,
> because it uses a [webpack](https://webpack.js.org/) configuration allowing to
> do lazy loading.

**Since React 16.6, there is a new way to use lazy loading.** This new version provides the `lazy()` method where you pass a function that will import the wanted component only when needed. Then use the `Suspense` component that will asynchronously load your component and allow you to specify a `fallback`, a message or a loading spinner to be displayed when waiting for the component to be loaded.

This lazy loading is not only to be used for routing, you can use it on button event for example.

**Example with App.js that just loads a Post on button onClick event**

```javascript
import React, { Component, Suspense } from 'react';
import User from './containers/User';
const Posts = React.lazy(() => import('./containers/Posts'));

class App extends Component {
  state = { showPosts: false };

  modeHandler = () => {
    this.setState((prevState) => {
      return { showPosts: !prevState.showPosts };
    });
  };

  render() {
    return (
      <React.Fragment>
        <button onClick={this.modeHandler}>Toggle Mode</button>
        {this.state.showPosts ? (
          <Suspense fallback={<div>Loading...</div>}>
            <Posts />
          </Suspense>
        ) : (
          <User />
        )}
      </React.Fragment>
    );
  }
}

export default App;
```

## Redux

### Definition

Redux is a JavaScript library used to manage states in web application (it's totally independent from React). Basically, states determine what should rendered on the screen. There are application-wide states or local states, for example, user authentication or just is a modal window open or not. Managing states is a complex task especially as our application grows. React offers ways to manage states but it can be very complex to pass states from components to components and that's why Redux has been developed to make states management easier.

Redux provides a **central store**, a giant JavaScript object that stores the entire application state. When a React component for example, wants to manipulate this application state, it will not do it directly because it will make this state pretty unpredictable. Redux is all about having a clearly defined process of how your state may change.

Besides the central store, Redux provides a building block named **actions**. Actions are dispatched from your JavaScript code (from within your React component for example). These actions are pre-defined information package, descriptions we could say, like "addUser" or "removeUser" for example. These actions can also hold payloads, like which User should be added. Actions don't directly reach the store, they are not containing any logic, they don't know how to update the store, it's just a messenger. The building block that is changing the store is a **reducer**.

Reducers can be combined but in the end you will have one root reducer that will reach the store. The reducer will receive actions, check the type of this action and run some code. A reducer is a [pure function](https://www.freecodecamp.org/news/what-is-a-pure-function-in-javascript-acb887375dfe/) which will receive an action and the old state as inputs and return an updated state. The updated state is then replacing the old state in the store. This has to be done in an immutable way, the updated is based on the old one but it's a new JavaScript object. A reducer has to execute synchronous code only.

Components can subscribe to the store, so when the store receives a new state, it will trigger all subscriptions and components will get notified that the state has been updated.

**Basic Redux example, not linked to React**

```javascript
const redux = require('redux');

const initialState = {
  counter: 0,
};

// Reducer
const rootReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INC_COUNTER':
      return {
        ...state,
        counter: state.counter + 1,
      };
    case 'ADD_COUNTER':
      return {
        ...state,
        counter: state.counter + action.value,
      };
  }
  return state;
};

// Store
const store = redux.createStore(rootReducer);
console.log(store.getState()); // { counter: 0 }

// Subscription
store.subscribe(() => {
  console.log('Subscription', store.getState());
});

// Action
store.dispatch({
  // convention is to name action type in uppercase
  type: 'INC_COUNTER',
});
console.log(store.getState()); // { counter: 1 }

store.dispatch({
  type: 'ADD_COUNTER', // only type property is mandatory, the other properties can be named like you want
  value: 10,
});

console.log(store.getState()); // { counter: 11 }
```

> When returning a new state in a reducer, keep in mind that this is not like `setState` in React, the state will not be merged with the old one, it will replace the old state.

### Redux & React

To use Redux in a React application, first install the `react-redux` package. Then to provide a store to your application, wrap your `App` component between the `Provider` component from `react-redux`. Then, pass your store to the `store` property of this `Provider` component.

**index.js**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import { createStore } from 'redux';
import reducer from './store/reducer';
import { Provider } from 'react-redux';

const store = createStore(reducer);
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
registerServiceWorker();
```

**reducer.js**

```javascript
const initialState = {
  counter: 0,
};

const reducer = (state=initialState, action) => {
  return state;
};

export default reducer;
```

#### Connecting components

In order to connect Redux to your React application, the first thing to do is to use the `connect` HOC from `react-redux` package. This HOC has to wrap the component you want to connect to the store.

The first argument of this `connect` function is `mapStateToProps` which is a function too. Like its name suggests it, `mapStateToProps` is used to select the data from the store that the connected component needs. This function is called every time the store state changes and it will receive the entire state from the store. The goal of this function is to return an object, a piece of the state the component needs.

#### Dispatching actions

By default, if you don't specify another argument to `connect`, your component will receive the `dispatch` function through its props. This function may be used to dispatch actions to the store.

But you can also use the second argument of the `connect` HOC. This argument is the `mapDispatchToProps` function. It is also used for dispatching actions to the store. This method lets you provide action dispatching functions as props. The benefits from this function is that it makes the dispatch logic implementation more declarative. For example, instead of calling `props.dispatch(() => increment())`, you will call `props.increment()`. The second benefit is that this method allows you to pass down action dispatching logic to child components that are not directly connected to the store (see an example above).

**Pass Down Action Dispatching Logic to ( Unconnected ) Child Components**

```javascript
// pass down bookClickHandler to child component
// making Book able to dispatch the bookClickHandler action
const Library = ({ books, bookClickHandler }) => (
  <div>
    {books.map((book) => (
      <Book book={book} onClick={bookClickHandler} />
    ))}
  </div>
);
```

#### Passing and retrieving data with Action

Of course, just like in the Redux example, it is possible to pass data in an action. To do that, simply add a property (the name is up to you) in the object passed in the `dispatch` function.

**Full example of connecting a component and dispatching actions**

**reducer.js**

```javascript
const initialState = {
  counter: 0,
};

const reducer = (state = initialState, action) => {
  let newState = null;
  switch (action.type) {
    case 'INCREMENT':
      newState = {
        counter: state.counter + 1,
      };
      break;
    case 'DECREMENT':
      newState = {
        counter: state.counter - 1,
      };
      break;
    case 'ADD':
      newState = {
        counter: state.counter + action.value,
      };
      break;
    case 'SUBTRACT':
      newState = {
        counter: state.counter - action.value,
      };
      break;
    default:
      newState = {
        ...state,
      };
  }

  return newState;
};

export default reducer;
```

**Counter.js**

```javascript
import React, { Component } from 'react';

import CounterControl from '../../components/CounterControl/CounterControl';
import CounterOutput from '../../components/CounterOutput/CounterOutput';
import { connect } from 'react-redux';

class Counter extends Component {
  state = {
    counter: 0,
  };

  render() {
    return (
      <div>
        <CounterOutput value={this.props.ctr} />
        <CounterControl
          label='Increment'
          clicked={this.props.onIncrementCounter}
        />
        <CounterControl
          label='Decrement'
          clicked={this.props.onDecrementCounter}
        />
        <CounterControl label='Add' clicked={this.props.onAddCounter} />
        <CounterControl
          label='Subtract'
          clicked={this.props.onSubtractCounter}
        />
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    ctr: state.counter,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    onIncrementCounter: () => dispatch({ type: 'INCREMENT' }),
    onDecrementCounter: () => dispatch({ type: 'DECREMENT' }),
    onAddCounter: () =>
      dispatch({
        type: 'ADD',
        value: 5,
      }),
    onSubtractCounter: () => dispatch({ type: 'SUBTRACT', value: 5 }),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

> For eliminating the danger of mistyping action types, it is always a good practice to outsource these action types into constants.

#### Combining reducers

Redux allows to combine reducers into one. To do that in React, simply use the `combineReducers` function. This function will take a JS object as an argument, and each property of this object is one your reducers. The state produced by `combineReducers()` namespaces the states of each reducer under their keys as passed to combineReducers().

**Counter reducer**

```javascript
import * as actionTypes from '../actions';

const initialState = {
  counter: 0,
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case actionTypes.INCREMENT:
      return {
        ...state,
        counter: state.counter + 1,
      };
    case actionTypes.DECREMENT:
      return {
        ...state,
        counter: state.counter - 1,
      };
    case actionTypes.ADD:
      return {
        ...state,
        counter: state.counter + action.value,
      };
    case actionTypes.SUBTRACT:
      return {
        ...state,
        counter: state.counter - action.value,
      };
  }
  return state;
};

export default reducer;
```

**Result reducer**

```javascript
import * as actionTypes from '../actions';

const initialState = {
  results: [],
};

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case actionTypes.STORE_RESULT:
      console.log(state);
      return {
        ...state,
        results: state.results.concat({
          id: new Date(), // concat returns a new Array
          value: action.counter,
        }), // here we need to get the value through an action payload because the state
        // doesn't contain the counter anymore
      };
    case actionTypes.DELETE_RESULT:
      return {
        ...state,
        results: state.results.filter(
          (result) => result.id !== action.resultElId
        ),
      };
  }
  return state;
};

export default reducer;
```

**index.js**

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import { combineReducers, createStore } from 'redux';
import counterReducer from './store/reducers/counter';
import resultReducer from './store/reducers/result';
import { Provider } from 'react-redux';

const rootReducer = combineReducers({
  ctr: counterReducer,
  res: resultReducer,
});
const store = createStore(rootReducer);
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
registerServiceWorker();
```

**Counter.js**

```javascript
import React, { Component } from 'react';
import CounterControl from '../../components/CounterControl/CounterControl';
import CounterOutput from '../../components/CounterOutput/CounterOutput';
import { connect } from 'react-redux';
import * as actionTypes from '../../store/actions';

class Counter extends Component {
  state = {
    counter: 0,
  };

  render() {
    return (
      <div>
        <CounterOutput value={this.props.ctr} />
        <CounterControl
          label='Increment'
          clicked={this.props.onIncrementCounter}
        />
        <CounterControl
          label='Decrement'
          clicked={this.props.onDecrementCounter}
        />
        <CounterControl label='Add' clicked={this.props.onAddCounter} />
        <CounterControl
          label='Subtract'
          clicked={this.props.onSubtractCounter}
        />
        <hr />
        <button onClick={() => this.props.onStoreResult(this.props.ctr)}>
          Store result
        </button>
        <ul>
          {this.props.storedResults.map((storedResult) => (
            <li
              key={storedResult.id}
              onClick={() => this.props.onDeleteResult(storedResult.id)}
            >
              {storedResult.value}
            </li>
          ))}
        </ul>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    // state object will have two properties ctr and res corresponding to the properties you defined in the combineReducers argument
    ctr: state.ctr.counter,
    storedResults: state.res.results,
  };
};

const mapDispatchToProps = (dispatch) => {
  return {
    onIncrementCounter: () => dispatch({ type: actionTypes.INCREMENT }),
    onDecrementCounter: () => dispatch({ type: actionTypes.DECREMENT }),
    onAddCounter: () =>
      dispatch({
        type: actionTypes.ADD,
        value: 5,
      }),
    onSubtractCounter: () => dispatch({ type: actionTypes.SUBTRACT, value: 5 }),
    onStoreResult: (val) =>
      dispatch({ type: actionTypes.STORE_RESULT, counter: val }),
    onDeleteResult: (id) =>
      dispatch({ type: actionTypes.DELETE_RESULT, resultElId: id }),
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

### State types

There are different types of states, and knowing them will allow you to decide whether or not it is worth using Redux.

- **Local UI State**: state that is local to a component and doesn't concern the entire application, for example, the selected tab in a navigation bar. For these cases, it is mostly handled within components and Redux is not really necessary.
- **Persistent state**: things also stored in your server-side database. You can use Redux to only get and manage relevant slice of data. Remember that states are removed if you refresh your page, so, do not consider states as databases.
- **Client state**: things like "is the user authenticated?" or filter settings of a user. These are things you can definitely managed with Redux.

### Advanced concepts

#### Middlewares

A Redux middleware is a piece of code, a function that will be executed before the action arrives in the reducer. It allows to run some logic, for example, logging, reporting crash, talking to an asynchronous API... You can use multiple middlewares by chaining them.

In Redux, middleware have access to a limited subset of the API, you can only call `dispatch` and `getState`. Another important function that will be called by Redux is `next`. This function needs the action as an argument as it will pass it to the next middleware in the chain (or to the reducer if you are in the last middleware). Please note that, `dispatch` is different from `next` because it will send the action back to the first middleware in the chain.

To add a middleware to the store, simply use the `applyMiddleware` function and add the result of this function as a parameter to the `createStore` function.

**Example with a logger middleware**

```javascript
import { applyMiddleware, combineReducers, createStore } from 'redux';

const logger = (store) => {
  return (next) => {
    return (action) => {
      console.log('[Middleware] Dispatching', action);
      const result = next(action); // allow the action to go to the reducer. Very important!
      console.log('[Middleware] newt state', store.getState());
      return result;
    };
  };
};

const store = createStore(rootReducer, applyMiddleware(logger));
```

> To help you debug a Redux application, use the [ReduxDevTools](https://github.com/zalmoxisus/redux-devtools-extension).

#### Action creators

An action can also be created with an **action creator**. An action creator is just a function that creates an action. So instead of creating actions with an object, we will just creating actions through a function.

**Example with STORE_RESULT action**

```javascript
export const storeResult = (value) => {
  return {
    type: 'STORE_RESULT',
    counter: value,
  };
};
```

The perk of action creators is that it will allow us to do asynchronous code.

#### Async code

Remember that a reducer can't run asynchronous code because it is a pure function.

To run asynchronous code, we are going to use actions and to do that, we need a third party library called [`redux-thunk`](https://github.com/reduxjs/redux-thunk). This library provides a thunk middleware. "thunk" means a function that is returned by another function, and the thunk middleware will just look at every action that passes through the system and, if it's a function, it will just call it.

**actions.js**

```javascript
// Synchronous action creator
export const saveResult = (value) => {
  return {
    type: 'STORE_RESULT',
    counter: value,
  };
};

// Asynchronous action creator
export const storeResult = (value) => {
  return (dispatch) => {
    setTimeout(() => {
      dispatch(saveResult(value)); // after timer is out, we execute the synchronous action creator
    }, 2000);
  };
};
```

**index.js**

```javascript
import { applyMiddleware, combineReducers, compose, createStore } from 'redux';
import thunk from 'redux-thunk';

const logger = (store) => {
  return (next) => {
    return (action) => {
      console.log('[Middleware] Dispatching', action);
      const result = next(action); // allow the action to go to the reducer. Very important!
      console.log('[Middleware] newt state', store.getState());
      return result;
    };
  };
};

const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose; // for ReduxDevTools extension
const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(logger, thunk))
); // simply add the thunk middleware
```

> You can now apply data transforming in action creators, but you may ask yourself, where to put that logic? in the action creator or in the reducer? First, if you have asynchronous logic, you can only put it in the action creator. The reducer is the core concept and it's its role to update the state. But there is a difference between returning a new state (reducer) and updating the data that will go to a state (could be done in an action creator). It is still recommended that if you have clean data (after getting it from a server for example) to pass it to the reducer and to limit logic in the action creators.

> `react-thunk` can also pass the getState function as well as the dispatch function. This will allow you to have access to the state within an action creator.

## Links and references

### Videos

- https://www.udemy.com/course/react-the-complete-guide-incl-redux/

### Websites

- https://reactjs.org/
