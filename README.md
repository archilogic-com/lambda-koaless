# lambda-koaless

[![Build Status](https://travis-ci.org/archilogic-com/lambda-koaless.svg?branch=master)](https://travis-ci.org/archilogic-com/lambda-koaless) [![npm version](https://badge.fury.io/js/lambda-koaless.svg)](https://www.npmjs.com/package/lambda-koaless) [![codecov](https://codecov.io/gh/archilogic-com/lambda-koaless/branch/master/graph/badge.svg)](https://codecov.io/gh/archilogic-com/lambda-koaless) [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

Wrap AWS Lambda functions with Koa-like functions to simplify your code

Based on [Lambda Expressless](https://github.com/muratcorlu/lambda-expressless) by [Murat Çorlu](https://github.com/muratcorlu)

So instead of writing this:

```js
exports.handler = (event, context, callback) => {
  const requestBody = JSON.parse(event.body);
  const responseBody = {
    success: false,
    data: requestBody.id
  };

  callback(null, {
    statusCode: 201,
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(responseBody)
  });
}
```

you'll have this:

```js
const { ApiGatewayHandler } = require('lambda-koaless')
const bodyParser = require('koa-bodyparser')
const Router = require('koa-router')

const router = Router()

// set response header
router.use(async (ctx, next) => {
  const start = Date.now()
  await next()
  const ms = Date.now() - start
  ctx.set('X-Response-Time', `${ms}ms`)
});

// parse body
router.use(bodyParser())

// handle route
router.get('/hello-world', (ctx, next) => {
  console.log(ctx.request.body) // parsed body
  ctx.status = 200
  ctx.body = { ok: true, message: 'Foo Bar' }
})

exports.handler = ApiGatewayHandler(router)
```

You can use many popular Koa Middlewares.

## Installation

```npm i lambda-koaless```

## Supported Features and Limitations

This project aims to implement functionalities of Koa middlewares as much as possible. `Request` and `Response` objects have properties and methods listed below.

### Context Object

Properties:

| Property    | Notes |
|-------------|-------|
| WIP | WIP |

Methods:

| Method    | Notes |
|-------------|-------|
| WIP | WIP |

### Request Object

Properties:

| Property    | Notes |
|-------------|-------|
| [body](https://expressjs.com/en/4x/api.html#req.body) | You need to use [body-parser](https://github.com/expressjs/body-parser) |
| [hostname](https://expressjs.com/en/4x/api.html#req.hostname) | - |
| [host](https://expressjs.com/en/4x/api.html#req.host) | - |
| [xhr](https://expressjs.com/en/4x/api.html#req.xhr) | - |
| [ip](https://expressjs.com/en/4x/api.html#req.ip) | - |
| [ips](https://expressjs.com/en/4x/api.html#req.ips) | - |
| [path](https://expressjs.com/en/4x/api.html#req.path) | - |
| [protocol](https://expressjs.com/en/4x/api.html#req.protocol) | - |
| [secure](https://expressjs.com/en/4x/api.html#req.secure) | - |
| [method](https://expressjs.com/en/4x/api.html#req.method) | - |
| [query](https://expressjs.com/en/4x/api.html#req.query) | - |
| [params](https://expressjs.com/en/4x/api.html#req.params) | - |
| [headers](https://expressjs.com/en/4x/api.html#req.headers) | - |

Methods:

| Method    | Notes |
|-------------|-------|
| [accepts()](https://expressjs.com/en/4x/api.html#req.accepts) | - |
| [acceptsEncodings()](https://expressjs.com/en/4x/api.html#req.acceptsEncodings) | - |
| [acceptsCharsets()](https://expressjs.com/en/4x/api.html#req.acceptsCharsets) | - |
| [acceptsLanguages()](https://expressjs.com/en/4x/api.html#req.acceptsLanguages) | - |
| [get()](https://expressjs.com/en/4x/api.html#req.get) | - |
| [header()](https://expressjs.com/en/4x/api.html#req.header) | - |
| [is()](https://expressjs.com/en/4x/api.html#req.is) | - |

### Response Object

Methods:

| Method    | Notes |
|-------------|-------|
| [get()](https://expressjs.com/en/4x/api.html#res.get) | - |
| [format()](https://expressjs.com/en/4x/api.html#res.format) | Doesn't support shorthand mime-types |
| [set()](https://expressjs.com/en/4x/api.html#res.set) | Only supports `key, value` parameters |
| [send()](https://expressjs.com/en/4x/api.html#res.send) | Only supports string values |
| [status()](https://expressjs.com/en/4x/api.html#res.status) | - |
| [end()](https://expressjs.com/en/4x/api.html#res.end) | - |
| [json()](https://expressjs.com/en/4x/api.html#res.json) | - |
| [type()](https://expressjs.com/en/4x/api.html#res.type) | - |

## Contribution

Every contribution is very welcome. Keep these in your mind when you want to make a contribution:

1. Because of we use [semantic-release](https://github.com/semantic-release/semantic-release) you need to use [Angular Commit Message Conventions](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines) in your commit messages.
2. Keep code coverage 100% with your updated tests.
3. Check your changes with a Lambda environment. You can use [SAM-CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) to test on your local.
4. Don't forget to update documentation(this readme file) about your changes.
