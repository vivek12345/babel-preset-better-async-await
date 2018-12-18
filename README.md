# babel-preset-better-async-await

> Babel preset for the babel-plugin-better-async-await plugin.
> [babel-preset-better-async-await](https://github.com/vivek12345/babel-plugin-better-async-await).

## Install

```sh
$ npm install --save-dev babel-preset-better-async-await
```

or with `yarn`

```sh
$ yarn add babel-preset-better-async-await --dev
```

## Usage

### Via `.babelrc` (Recommended)

**.babelrc**

```json
{
  "presets": ["better-async-await"]
}
```

### Via CLI

```sh
$ babel script.js --presets better-async-await
```

### Via Node API

```javascript
require("babel-core").transform("code", {
  presets: ["better-async-await"]
});
```
