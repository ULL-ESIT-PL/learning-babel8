## Goal

- Learn Babel 8. New features. Internals. TypeScript.
- Migrate plugins left-side and default-vector from Babel 7 to Babel 8
- 
## Resources

- [Blog: Announcing Babel 8 Beta](https://babel.dev/blog/2025/05/30/babel-8-beta)
- [Upgrade to Babel 8](https://babeljs.io/docs/v8-migration/)
- [Upgrade to Babel 8 (API)](https://babeljs.io/docs/v8-migration-api) for plugin writers  (because, for example, you maintain a custom Babel plugin)

## Hello Babel 8!

```bash
mkdir hello
cd hello
npm init -f
npm i @babel/core@v8.0.0-beta.0 @babel/cli@v8.0.0-beta.0
npm i @babel/preset-env@^8.0.0-beta.0
```

Let us check the installed versions:

```bash
➜  hello git:(main) ✗ npm ls | grep babel
hello@1.0.0 /Users/casianorodriguezleon/campus-virtual/2122/learning/compiler-learning/babel8/hello
├── @babel/cli@8.0.0-beta.0
├── @babel/core@8.0.0-beta.0
└── @babel/preset-env@8.0.0-beta.0
```

Then in the `package.json` file, add the following script:

```json
{
 "scripts": {
    "build": "babel src -d lib"
  },
  "dependencies": {
    "@babel/cli": "^8.0.0-beta.0",
    "@babel/core": "^8.0.0-beta.0",
    "@babel/preset-env": "^8.0.0-beta.0"
  }
}
```

Inside the folder `hello`

```
➜  hello git:(main) ✗ mkdir src
➜  hello git:(main) ✗ mkdir lib
```

Then create a file `src/index.js` with the following content:

```javascript
const hello = () => {
  console.log('Hello, Babel 8!');
};
hello();
```
Also created a `babel.config.js` file with the following content:

```javascript 
module.exports = {
  presets: [
    '@babel/preset-env'
  ]
};
```

Now you can run the build command:

```bash
➜  hello git:(main) ✗ npm run build

> hello@1.0.0 build
> babel src -d lib

Successfully compiled 1 file with Babel (249ms).
```
The output will be in the `lib` folder. You can check the content of the compiled file:

```js
➜  hello git:(main) ✗ cat lib/index.js 
"use strict";

const hello = () => {
  console.log('Hello, Babel 8!');
};
hello();%   
```
## Example with an included plugin

Now we add an example using an included plugin, such as `transform-optional-chaining`:

```js 
➜  hello git:(main) ✗ cat src/deeply-nested-properties.js 
// Example Accessing deeply nested properties 
// https://babeljs.io/docs/babel-plugin-transform-optional-chaining#accessing-deeply-nested-properties
// Run it with: npx babel --plugins @babel/plugin-transform-optional-chaining script.js
const obj = {
  foo: {
    bar: {
      baz: 42,
    },
  },
};

const baz = obj?.foo?.bar?.baz; // 42

const safe = obj?.qux?.baz; // undefined

// Optional chaining and normal chaining can be intermixed
obj?.foo.bar?.baz; // Only access `foo` if `obj` exists, and `baz` if
// `bar` exists

// Example usage with bracket notation:
obj?.["foo"]?.bar?.baz; // 42
```
Now we need to update the `babel.config.js` file to include the `transform-optional-chaining` plugin:

```js 
➜  hello git:(main) cat babel.config.js 
export default {
  presets: [["@babel/preset-env", 
    {
      include: ["transform-optional-chaining"],
    }
  ]],
}%    
```

And when we run the build command again:

```bash
➜  hello git:(main) ✗ npm run build
```
The output will be:

```js
➜  hello git:(main) ✗ cat lib/deeply-nested-properties.js 
"use strict";

var _obj$foo, _obj$qux, _obj$foo$bar, _obj$foo2;
// Example Accessing deeply nested properties 
// https://babeljs.io/docs/babel-plugin-transform-optional-chaining#accessing-deeply-nested-properties
// Run it with: npx babel --plugins @babel/plugin-transform-optional-chaining script.js
const obj = {
  foo: {
    bar: {
      baz: 42
    }
  }
};
const baz = obj === null || obj === void 0 || (_obj$foo = obj.foo) === null || _obj$foo === void 0 || (_obj$foo = _obj$foo.bar) === null || _obj$foo === void 0 ? void 0 : _obj$foo.baz; // 42

const safe = obj === null || obj === void 0 || (_obj$qux = obj.qux) === null || _obj$qux === void 0 ? void 0 : _obj$qux.baz; // undefined

// Optional chaining and normal chaining can be intermixed
obj === null || obj === void 0 || (_obj$foo$bar = obj.foo.bar) === null || _obj$foo$bar === void 0 || _obj$foo$bar.baz; // Only access `foo` if `obj` exists, and `baz` if
// `bar` exists

// Example usage with bracket notation:
obj === null || obj === void 0 || (_obj$foo2 = obj["foo"]) === null || _obj$foo2 === void 0 || (_obj$foo2 = _obj$foo2.bar) === null || _obj$foo2 === void 0 ? void 0 : _obj$foo2.baz; // 42%  


## hello/src/hello-babel.js

Example taken from https://github.com/ULL-ESIT-PL/babel-learning?tab=readme-ov-file#babeltransformsync.
Worked without changes:

```console
➜  hello git:(main) ✗ npx babel src/hello-babel.js | node | node
Hello, Babel!
```