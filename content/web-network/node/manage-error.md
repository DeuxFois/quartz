# First-error callbacks in Node


The `"error-first"` callback (also "errorback" or "err-back") has become the standard protocol for **Node** as to enable a balanced, non-blocking flow of control and processing power across applications and modules.

The parameters of an error-first callback functions should look like:

```javascript
function(err, data)
```

The first argument is an error object. If the response is successful `err` will be equal to `null`. Otherwise it will take the type of error.

Implementing an "error-first" callback:

```javascript
fs.readFile('/text.txt',
 function(err, data) {
   if (err) {
     console.log('error')
     console.log(err)
   } else {
     console.log(data);
   }
});
```

Note that the errors can be more specific:

```javascript
// ...
function(err, data) {
  if (err.fileNotFound) {
    console.log('wrong source')
  }
}

```


# `try-catch` only for **sync** code


All *JavaScript* errors are handled as exceptions that will **instantly** generate and `throw` and error. To handle them, `try-catch` constructor is used.

However, most errors from within **asynchronous** APIs behave differently (mostly with callbacks or `EventEmitters`) so they can't be handled with `try-catch`.

For example, `JSON.parse` method happens **synchronously**. We can handle its errors with a `try-catch` block:

```javascript
function readJSON(filePath, callback) {  
  fs.readFile(filePath, (err, data) => {
    let parsedJson;

    // Handle error
    if (err) {
       return callback(err);
    }

    // Parse JSON with JSON.parse method
    try {
      parsedJson = JSON.parse(data);
    } catch (exception) {
      return callback(exception);
    }

    return callback(null, parsedJson);
  });
}

```

# uncaughtException listener in Node.js


Use an `uncaughtException` listener to prevent a program crashing due to an unhandled exception.

For example:

```javascript
process.on('uncaughtException',
  function(err) {
     console.log('exception: ' + err);
  }
)
```

An unhandled exception means an application is in an **undefined state**. It is not possible to continue the program safely and should restart.

Note that `uncaughtException` is a crude mechanism for exception handling and is therefore not recommended in production.