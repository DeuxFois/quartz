# Consuming events


Node applications are event driven applications, an event occurs upon a change of state in an application, for example, a button being clicked, or data being inputted.

Node events are consumed when an in-application event occurs, modules subscribe to events by listening to the event on a given object. For example, in a file system, an event could be that a file has been edited:

```javascript
let system = require('.filesystem.js');
system.file.on('edit', function() {

  // do something...

});
```

In this example, the `system.file` object is an event emitter, a module can listen to this object by both using the `.on` method and passing in a function which is called whenever an event with that given name occurs.

Events can also produce relevant data, for example if you wanted to know who edited a file in the system:

```javascript
system.file.on('edit', function
                      (fileID, initials) {

  console.log('File number %d was edited'
          + 'by %s', fileID, initials);

});
```

To unsubscribe to events, use the `.removeListener`  method and specify the event type and the event listener function.

```javascript
function onEdit(fileID, initials) {
  // on edit, do something...
}

system.file.removeListener('edit', onEdit);
```

# Event Delivery


Node is asynchronous, however as no I/O is involved in emitting events, the delivery of events is treated synchronously. Therefore:

```javascript
const EventEmitter = require('events')
  .EventEmitter;

const emitter = new EventEmitter();

emitter.on('hi', function() {
  console.log('hi!');
});

emitter.on('hi', function() {
  console.log('hi again!');
});

console.log('pre hi');
emitter.emit('hi');
console.log('post hi');
```

Gives the following output:

```plain-text
pre hi
hi!
hi again!
post hi
```

Remember that when emitting events, listeners will be called before `emitter.emit` returns.

# Handling event errors


All events are treated equally as all event types are defined by an arbitrary string. When an event emitter[1] emits an event with no attached listeners the event is ignored.

If the event is called *error* however, the error is thrown into the event loop, then generating an uncaught exception. To stop this from breaking the application, uncaught exceptions can be caught by listening to the `uncaughtException` which the global event emitter object emits. Take `test` as a sample event emitter:

```javascript
test.on(‘uncaughtException’, function(err)
{
  console.error(‘uncaught exception: ‘,
                    err.stack || err);


  closeApp(function(err) {
    if (err)
      // error closing down


    test.exit(1);
  });

});
```

