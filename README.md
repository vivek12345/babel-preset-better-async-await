# babel-preset-better-async-await

> Babel preset for the babel-plugin-better-async-await plugin.
> [babel-plugin-better-async-await](https://github.com/vivek12345/babel-plugin-better-async-await).

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

## 📒 Examples

**In**

```javascript
async function test() {
  const [err, resp] = await api.getData(5);
}
```

**Out**

```javascript
async function test() {
  const [err, resp] = await api
    .getData(5)
    .then(resp => {
      return [null, resp];
    })
    .catch(error => {
      return [error];
    });
}
```

======================================

**In**

```javascript
async function test() {
  const [err, resp] = await getData(5);
}
```

**Out**

```javascript
async function test() {
  const [err, resp] = await getData(5)
    .then(resp => {
      return [null, resp];
    })
    .catch(error => {
      return [error];
    });
}
```

======================================

**In**

```javascript
async function test() {
  const [err, resp] = await getData;
}

function getData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(true);
    }, 1000);
  });
}
```

**Out**

```javascript
async function test() {
  const [err, resp] = await getData()
    .then(resp => {
      return [null, resp];
    })
    .catch(error => {
      return [error];
    });
}

function getData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(true);
    }, 1000);
  });
}
```

======================================

**In**

```javascript
async function test() {
  const [err, resp] = await new Promise((resolve, reject) => {
    resolve(true);
  });
}
```

**Out**

```javascript
async function test() {
  const [err, resp] = await new Promise((resolve, reject) => {
    resolve(true);
  })
    .then(resp => {
      return [null, resp];
    })
    .catch(error => {
      return [error];
    });
}
```

======================================

**In**

```javascript
async function test() {
  const [err, resp] = await Promise.resolve(true);
}
```

**Out**

```javascript
async function test() {
  const [err, resp] = await Promise.resolve(true)
    .then(resp => {
      return [null, resp];
    })
    .catch(error => {
      return [error];
    });
}
```

======================================
