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

