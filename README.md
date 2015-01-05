#throttled-request
Node.js module to easily throttle HTTP requests.

##How it works
This tool was made to work with the popular [request](https://github.com/request/request) module, which simplifies the HTTP requests in Node.js. Therefore, this must be consireded a wrapper around **request**.

First, you instantiate a **throttledRequest** instance by passing a **request** function, which is going to act as the requester - you still need to `$npm install request` independently. - After this you can configure the throttle rate *(number of requests / time)*, then you're able to use **throttled-request** to perform your HTTP requests.

##Installation
Install it using [npm](https://www.npmjs.com/)
```
$ npm install throttled-request
```

##Usage
First, you must set it up:
```javascript
var request = require('request')
,   throttledRequest = require('throttled-request')(request);

throttledRequest.configure({
  requests: 5,
  milliseconds: 1000
});//This will throttle the requests so no more than 5 are made every second
```

Then you can use `throttledRequest` just as you use [request](https://github.com/request/request): passing a callback, or as a stream.

###Passing a callback
```javascript
throttledRequest(options, function (error, response, body) {
    if (error) {
        //Handle request error
    }
    //Do what you need with `response` and `body`
});
```

###As a stream
```javascript
var request = throttledRequest(options);
request.on('error', function (error) {
    //Handle request error
});
request.pipe(someWriteStream);
```

##The `request` event
Every request object returned by `throttledRequest` emits a `request` event just after the actual request is made.

##Full example
```javascript
var request = require('request')
,   throttledRequest = require('throttled-request')(request)
,   startedAt = Date.now();

throttledRequest.configure({
  requests: 2,
  milliseconds: 1000
});

//Make 10 requests in parallel 
for (var i = 0; i < 10; i++) {
  throttledRequest('https://www.google.com/')
    .on('request', function () {
      console.log('Making a request. Elapsed time: %d ms', Date.now() - startedAt);
    })
    .on('response', function () {
      console.log('Got response. Elapsed time: %d ms', Date.now() - startedAt);
    }); 
}

/*Output:

*/
```

##Can I use everything that comes with **request**?
No, there's some things you can't use. For example, the shortcut functions `.get`, `.post`, `.put`, etc. are not available. If you'd like to have them, this is a great opportunity to contribute!

##Running tests
Run the tests with npm
```
$ npm test
```

##License (MIT)
