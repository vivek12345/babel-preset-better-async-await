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

## Motivation and Idea

This babel plugin is inspired from the idea of this post https://blog.grossman.io/how-to-write-async-await-without-try-catch-blocks-in-javascript/ written by - [Dima Grossman](https://twitter.com/dimagrossman)

> In async/await functions we often use try/catch blocks to catch errors.

For example:-

```javascript
async function completeApplicationFlow() {
  // wait for get session status api to check the status
  let response;
  try {
    response = await getSessionStatusApi();
  } catch (err) {
    // if error show a generic error message
    return handleError(err);
  }

  // wait for getting next set of questions api
  try {
    response = await getNextQuestionsApi();
  } catch (err) {
    // if error show a generic error message
    return handleError(err);
  }

  // finally submit application
  try {
    response = await submitApplication();
  } catch (err) {
    // if error show a generic error message
    return handleError(err);
  }
}
```

> Approach inspired from the blog and a different way of doing this could be:-

```javascript
async function completeApplicationFlow() {
  // wait for get session status api to check the status
  let err, response;
  // wait for get session status api to check the status
  [err, response] = await getSessionStatusApi();
  // if error show a generic error message
  if (err) return handleError(err);
  // call getNextQuestion Api
  [err, response] = await getNextQuestionsApi();
  // if error show a generic error message
  if (err) return handleError(err);
  // finally submit application
  [err, response] = this.submitApplication();
  if (err) return handleError(err);
}
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
