<frontmatter>
  title: "Javascript: Modules"
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Javascript: Modules

**Author: Gilbert Emerson**<br>
Reviewers: Chelsey Ong, Ong Shu Peng, Amrut Prabhu

<box id="article-toc">

* [What Is a Module?‎](#what-is-a-module)
* [How to Modularize JavaScript Code?‎](#how-to-modularize-javascript-code)
    * [ES6 modules‎](#es6-modules)
    * [CommonJS‎](#commonjs)
    * [Module Pattern‎](#module-pattern)
* [Which to use?‎](#which-to-use)
* [How to start?‎](#how-to-start)
* [Further Reading‎](#further-reading)
</box>

<box type="info">

This article assumes the reader has some basic knowledge of JavaScript.
</box>

## What is a Module?

In programming, the term module (other similar terms: _package, library, dependency, plugin, etc._) is used to refer to _a small part of code that is broken up from a larger code base_.

Modules help programmers in many ways. Here are some of the examples:

**1. Modules make the code more managable** <br>
Using modules will break the code base into smaller parts which if done well can help in managing a code base, especially a large one.

Let's say you have an application with functionalities A and B, where functionality A needs functionality B. Without modules, both of these functionalities are mixed together in the code base without a clear separation. With modules, we can separate those 2 functionalities into separate module each. When A need B, A will "include" B and A will be able to work as if A and B has never been separated.

**2. Modules help minimize name clashes** <br>
Breaking code into modules result in breaking the code's namespace into smaller parts too. This will help in minimizing name clashes and the need for global variables.

**3. Modules promote reuse** <br>
Modules allows developers to reuse their code that is contained in a module. If for example we have an application that rely on a fuctionality such as string comparison function, we can separate that function into a module and let the application to use the function from that module instead of having to always repeating that function in all the places where it is needed.

## How to Modularize JavaScript Code?

There are 3 common ways to use modules in JavaScript: 1. using ES6 modules, 2. using CommonJS, 3. using the module pattern. While ES6 is the most recent and the official implementation, this article covers the other two as well because there is still a large number of existing projects that use them.

### ES6 Modules

Introduced in 2015, ES6 modules is the official implementation of modules in JavaScript. It introduces 2 new syntax `import` and `export` to use modules in JavaScript.

In the example below, `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. So, `anExampleModule.js` module will need to _export_ the function using `export` syntax and `index.js` module will need to _import_ that function using `import` syntax.

```js
// anExampleModule.js

var variableOne = 1;
var variableTwo = 2;

export sumOfVariable() {
    return variableOne + variableTwo;
}
```

```js
// index.js

import * as anExampleModule from './anExampleModule.js';

anExampleModule.sumOfVariable(); // 3
```

ES6 modules supports advanced features such as _[asynchronous loading](http://exploringjs.com/es6/ch_modules.html#sec_modules-in-browsers), [tree shaking](https://medium.com/@netxm/what-is-tree-shaking-de7c6be5cadd), [static code analysis](http://exploringjs.com/es6/ch_modules.html#static-module-structure)_, etc. These features will not be covered in this article.

Due to the very recent adoption of ES6 modules by browsers and there is still some browsers that do not support it, you might not be able to use ES6 modules right away in those unsupported browsers.

There are 2 workarounds for this issue. You can use _[transpiler](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them)_ such as _[Babel](https://babeljs.io/)_ and _[bundler](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da)_ such as _[Webpack](https://webpack.js.org/)_ to serve your application to those unsupported browsers or use one of the other two approaches mentioned below.

A more in-depth explanation of ES6 modules can be found in the [Modules chapter of the Exploring ES6 online book](http://exploringjs.com/es6/ch_modules.html).

### CommonJS

CommonJS is in wide use today because it is used by _NodeJS_ which in turn is used by many JavaScript applications.

The example below will replicate the same example in previous chapter using CommonJS. `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. So, `anExampleModule.js` module will need to _export_ the function using `module.exports` syntax and `index.js` module will need to _import_ that function using `require` syntax.

```js
// anExampleModule.js

var variableOne = 1;
var variableTwo = 2;

sumOfVariable = function() {
    return variableOne + variableTwo;
}

module.exports = {
  sumOfVariable: sumOfVariable,
};
```

```js
// index.js

var anExampleModule = require('./anExampleModule.js');

anExampleModule.sumOfVariable(); // 3
```

If your project does not use NodeJS and does not allow the use of  _[bundler](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da)_ such as _[Webpack](https://webpack.js.org/)_, you can consider the approach given in the next section as it does not need any external tool.

A more in-depth explanation of CommonJS can be found in the [Modules chapter of NodeJS API documentation](https://nodejs.org/docs/latest/api/modules.html).

### Module Pattern

Using a technique in JavaScript called _[IIFE (Immediately Invoked Function Expression)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)_, JavaScript developers can create module by wrapping their code in an IIFE.

<box>

An IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined. <br>
**Syntax:** `(function() { statements })();`

_Source: [MDN Glossary - IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)_
</box>

The example below will replicate the same example in previous chapters using module pattern. `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. To achieve this, we use the `<script>` tag in HTML to _import_ `anExampleModule.js` for `index.js` to use.

```html
<!-- index.html -->

<script src="./anExampleModule.js" />
<script src="./index.js" />
```

```js
// anExampleModule.js

var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    // We return an object with functions or variables
    // that we want to be exposed
    return {
        sumOfVariable: sumOfVariable
    }
})()
```

```js
// index.js

anExampleModule.sumOfVariable(); // 3
```

The result of importing the modules in the HTML will be as follow:
```js
// anExampleModule.js

var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    // We return an object with functions or variables
    // that we want to be exposed
    return {
        sumOfVariable: sumOfVariable
    }
})()

// index.js

anExampleModule.sumOfVariable(); // 3
```

A more in-depth explanation of module pattern can be found in the [this course blog on mastering module pattern](https://ultimatecourses.com/blog/mastering-the-module-pattern).

## Which to Use?

Although ES6 modules is the official way to implement modules, there are situations where you might have to use one of the other options. Here are some examples:
- If your application does not allow you to use transpiler and bundler (e.g. because of the additional overhead they add), you can use the module pattern.
- If your application is NodeJS based, you might want to use CommonJS because NodeJS does not natively support ES6 modules.

## How to Start?

You can start with module pattern right away by refactoring segments of your code into different files and wrapping the code in IIFE.

If you are planning to use CommonJS or ES6 modules, you can start by refactoring segments of your code, but you will also need to be familiar with transpiler and bundler such as Babel and Webpack.

## Further Reading

You can read more on JavaScript modules at following websites:

- [JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) <br>
This article gives a broad explanation of JavaScript modules, explaining different kinds of module system in JavaScript, yet easy to follow.
- [JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/) <br>
This article truly focuses on IIFE, CommonJS, and ES6 Modules, giving a very comprehensive usage example.
- [Learn the basics of the JavaScript module system and build your own library](https://medium.freecodecamp.org/anatomy-of-js-module-systems-and-building-libraries-fadcd8dbd0e) <br>
While the aim of the article may not be the same with the aim of this book article, this article gives a very comprehesive comparison between CommonJS and ES6 Modules.
