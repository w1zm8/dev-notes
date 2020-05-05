# Configuring Jest In JS/TS Project

## Imports in a babel's project

> Full example of configuration you can see in the project [Lotto Game](https://github.com/kowalevski/lotto-game)

Install Jest as dev dependency:

```
npm i --save-dev jest
```

> for correct work of autocomplete in VSCode you should install types for Jest even if you work with project w/o TypeScript

```
npm i --save-dev @types/jest
```

If you are using **babel** in your project, you should pay attention with a configuration of Babel, especially with option for [Tree Shaking](https://webpack.js.org/guides/tree-shaking/) which disables imports in a code:

```js
// ...
    ['@babel/preset-env', { modules: false }],
// ...
```

When imports are disabled in a code, Jest can't parse them in tests - so you can get errors like:

```
    SyntaxError: Cannot use import statement outside a module
```

Therefore, you should check env mode "test" in **babelrc** file:

```js
const isTestMode = process.env.NODE_ENV === 'test';

module.exports = {
  presets: [
    ['@babel/preset-env', { modules: isTestMode ? 'commonjs' : false }],
    '@babel/preset-react'
  ],
// ...
```

## Using module's aliases (absolute paths) in Jest tests

> Full example of configuration you can see in the project [Lotto Game](https://github.com/kowalevski/lotto-game)

If you are using webpack in your project so you can use absolute paths. In webpack's configuration file it will be something like that:

```js
//...
module.exports = {
  resolve: {
    modules: ['node_modules', path.join(__dirname, 'src'), 'components'],
// ...
```

It means that you can write absolute paths

```js
import Gamefield from "Gamefield";
```

instead of

```js
import Gamefield from "../../../components/Gamefield";
```

But by default Jest doesn't know about this absolute paths. So, if you are using absolute paths in your Jest tests you will get error:

```
Cannot find module 'Gamefield' from 'Gamefield.test.jsx'
...
You might want to include a file extension in your import, or update your 'moduleFileExtensions', which is currently ['js', 'json', 'jsx', 'ts', 'tsx', 'node'].
```

You should add option "moduleDirectories" in your Jest's configuration file:

```js
module.exports = {
  moduleDirectories: [
    "node_modules",
    path.join(__dirname, "src"),
    "components",
  ],
};
```
