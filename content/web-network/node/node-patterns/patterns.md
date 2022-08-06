# Factories design pattern


---

## Content

In order to avoid custom object creation with different arguments, **factories** can be used instead. Their usage is obvious when working with complex constructors or you want to avoid *copypasta*.

Factories will create objects for you so *you don't have to*.

Basic factories pattern:

```javascript
//enki.js
function Enki (args) {
  this.args = args;
}

function create (args) {
  //args should be modified here
  return new Enki(args);
}

module.exports.create = create;
```


# Middleware/pipeline design pattern


---

## Content

The **middleware** or **pipeline** concept is used everywhere in Node.js. They represent a series of processing units connected subsequently: **the output of one unit is the input for the next one**.

```javascript
function(/*input/output */, next) {
  next(/* err and/or output */)
};
```

**Koa** framework does it like this:

```javascript
app.use = function(fn) {
  this.middleware.push(fn);
  return this;
};
```

This concept is usually implemented through `async.waterfall` or `async.auto`:

```javascript
async.waterfall([
  function(callback) {
    callback(...);
  },
  function(args, callback){
    callback(...);
  }
]};
```

Famous Node.js streams also use the concept of pipelining:

```javascript
fs.createReadStream("file.gz")
  .pipe(zlib.createGunzip())
  .pipe(through(function write(data) {
      //handle data
      this.queue(data);
   })
  //write to your file
  .pipe(fs.createWritableStream("out.txt"));
```
