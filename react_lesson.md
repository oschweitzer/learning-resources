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

With JSX, and if you want dynamic content, you can use the single curly braces to tell React to interpret a variable or an JavaScript expression.

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

Components are very useful, because they allow to write more manageable, reusable code. Your components can be static or you can configure them by passing arguments (which is called `props` in React).

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

If you change (mutate) the state directly by directly updating the `state` property, React will issue a warning in your web browser console, and your update will not work. To mutate a state, React provides a function called `setState()` that will update the state and tell React that it was updated, so it can re-render the DOM.

`setState` takes an object as an argument and will merge this object with the current `state` object value.

#### useState() hook

Since React version 16.8, there is a way to manage states in non class-based component by using **React Hooks**. The hook we can use to manage states, is the `useState()` hook. This function takes the initial state asa value and returns two values. The first one is the current state represented by an object and the second one is a function that allows us update the state.

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

> If you need to update the state but you depend on the previous state value, it's a best practice to pass a function instead of passing an object as the first argument of the `userState` method. This function takes two parameters, the previous state and the current props and you will have to return the new state.

### Events handling

In React and JSX, you can handle [events](https://reactjs.org/docs/events.html#supported-events) pretty easily by just defining a handler function and associate it to the wanted event on the wanted element.

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

Functional components are represented by JavaScript function (mostly using ES6 arrow function notation) assigned to constant (that will be exported in order to use them as component). In the past (before React 16.8 and React Hooks), they couldn't manage state. But, since React hooks were introduced, they can with the `useState` hook. However, lifecycle hooks are not available with function components. Unlike a class-based component, in a functional component, you will get all your properties by the `props` variable.

> Keep in mind, that you could be working on older React project where functional components can't handle state.

### Component lifecycle

The component lifecycle is only available in class-based components. This lifecycle is represented by lifecycle hooks (lifecycle hooks has nothing to do with React hooks!). There are two different lifecycle we could say, one for the component creation and one when a component is updated (because props or state changed).

#### Component creation lifecycle

- `constructor(props)` => this is an ES6 feature and by default it is done for you. If you want to define the constructor, remember to always call `super(props)` as the first instruction in the constructor function.
- `getDerivedStateFromProps(props, state)` => this hook exists for the are case where the state depends on props changes during time. It is a static method that should return the updated state. Keep in mind that is very rarely used.
- `render()` => this hook will prepare and structure your JSX code. Don't execute code that can blocked the rendering process in this function. Every child component you included in your rendered component here will then be rendered as well.
- `componentDidMount()` => this hook will be called once all child component of this component have finished their lifecycle. Here you can make your HTTP requests to get data from servers. Don't update the state synchronously (you can do it in a promise for example) in this hook because it will trigger a re-render process that is bad for performances.

> They are still historic lifecycle hooks that you can use, for example `componentWillMount()`, but since they are very rarely used and could lead to side effects, they might be removed in the future.

#### Component update lifecycle

- `getDerivedStateFromProps(props, state)` => used to sync state with props but very rarely used.
- `shouldComponentUpdate(nextProps, nextState)` => allows to cancel the updating process. This hook should return a boolean, true if the update should continue, false otherwise. It can be used to do optimization by restricting updating to only changes on specific data.
- `render()` => React goes through the JSX code, evaluates it and constructs its virtual DOM.
- `getSnapshotBeforeUpdate(prevProps, prevState)` => takes the previous props and the previous state as input and returns a snapshot. It is not very used but you can use it for last minute DOM operations (for example, getting the current scrolling position of the user).
- `componentDidUpdate()` => the updating is now done, so here can do side effects (HTTP requests for example). Avoid synchronous functions otherwise you will have infinite loops.

### useEffect()

Since React Hooks, it is possible to handle state in functional components. The `useEffect()` function basically regroup all the class-based lifecycle methods in one React hook. It is `componentDidUpdate()` and `componentDidMount()` combined in one effect.

The first argument of this function is the effect, the function that will be executed on changes or creation. The second argument is an array of data. With this array you cas restrict the hook to only trigger on changes on these data. If this array is empty, the effect will only run one time, on the component creation (like the `componentDidMount()` function).

You can return a function in the effect function, this allows to do some cleanup work for example because this function will be called right before the next lifecycle or when the component is destroyed (be sure to pass an empty array to `useEffect()` to run it only one time).

You can have more than one `useEffect()` in a single component.

### Optimization

To optimize functional component, meaning not re-rendering it when its props don't change, you can wrap it using the `React.memo` function. Of course, if your component will always change with its parent component, don't use this function because you will execute useless code.

If you need to check all the properties of a component in the `shouldComponentUpdate` hook, meaning you know that your component will renders the same result given the same props ans state, you can extends this component from `PureComponent` instead of `Component`. `PureComponent` will implement the shouldComponentUpdate for you and do a shallow prop and state comparison (be careful if you have complex data structure because it can lead to false positive).

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

Once installed, in your component just import the PropTypes object and use it like in the example below.

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

React provides a special attribute that you can attach to any component, called `ref` (for reference). Refs are are objects of functions created by the `React.createRef()` function. These refs allow to have a direct access to a DOM element or component instance.

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
        // here we are defining our data (default value of the context is doesn't really matter then)
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

For functional components, it is also possible to have this context feature thanks to React Hooks. The hook for contexts is named `useContext()`. Just pass your context object as a parameter to this hook.

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

It is recommended to use the [axios](https://github.com/axios/axios) library since it's one of the most popular HTTP library and it's available for frontend code as well as backend code.

For example, if I have an application that simulate a blog with posts.

**Post.js**

```javascript
import React from 'react';

import './Post.css';

const post = (props) => (
    <article className="Post">
        <h1>{props.title}</h1>
        <div className="Info">
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
    posts: []
  }

  // Here it is very important to note that an HTTP request is an asynchronous task ! So the Promise should be handled correctly
  async componentDidMount() {
    const response = await axios.get('https://jsonplaceholder.typicode.com/posts');
    this.setState({posts: response.data})
  }

  render () {
    const posts = this.state.posts.map(
        (post) => <Post key={post.id} title={post.title} body={post.body}/>
    )
    return (
        <div>
          <section className="Posts">
            {posts}
          </section>
        </div>
    );
  }
}

export default Blog;
```

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
