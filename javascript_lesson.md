# JavaScript

## Concepts

### Syntax parser

The syntax parser reads the code in order to transform it from human readable code to something understandable by the computer.

### Lexical environment

Lexical environments exist in programming language where the location of the code is important (for example, if a variable is inside a function or outside).   

Lexical environment analyzes where the code stands and what is around it.

### Execution context

Execution contexts handle the code which is currently running.

There is a special execution context, the global execution context. It is created by the JavaScript engine.
This context creates a global object (named `window` in the browser and `global` in NodeJS) and the `this` variable.

An execution context is created for each function call. Each execution context have a reference to its outer environment and reserve memory space for functions and variables.

## Asynchronism
## Types and operators
## Objects and functions
## OOP and prototype oriented
## Build objects
## ES6 features
## Miscellaneous