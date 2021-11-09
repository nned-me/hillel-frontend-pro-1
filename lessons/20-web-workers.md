

## JS problems with one thread
- freeze main thread
- no heavy working
- difficult workarounds


## Worker usage
- heavy calc
- tabs common communicate
- advanced caching & offline working


## Worker types
- **Dedicated Workers**: utilized by a single script. 
Context is a [DedicatedWorkerGlobalScope](https://developer.mozilla.org/en-US/docs/Web/API/DedicatedWorkerGlobalScope) object.

- **Shared Workers**: can be utilized by multiple scripts in different windows, IFrames, etc., as long as they are in the same domain as the worker. 
More complex than dedicated workers — scripts must communicate via an active port. 
Context is a [SharedWorkerGlobalScope](https://developer.mozilla.org/en-US/docs/Web/API/SharedWorkerGlobalScope)

- **Service Workers**: proxy servers that sit between web applications, the browser, and the network (when available). 
Offline experiences, intercept network requests and take appropriate action based on whether the network is available, and update assets residing on the server. 
They will also allow access to push notifications and background sync APIs.
Context is [ServiceWorkerGlobalScope](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerGlobalScope)


## Worker Example

Web Workers run in an isolated thread. The code needs to be in a separate file. 
If file request gets error, then worker fails silently.

```
if (window.Worker) {
  const worker = new Worker('task.js');
  worker.postMessage(); // Start the worker.
}
```

## Worker 

- worker is running in another context. 
- there is no window object
- you can't directly manipulate the DOM from inside a worker
- use `self` for a global context
- `Worker.close()` or `self.close()` inside worker to terminate

**Workers have access to:**
- The navigator object
- The location object (read-only)
- XMLHttpRequest / fetch
- setTimeout()/clearTimeout() and setInterval()/clearInterval()
- The Application Cache
- Importing external scripts using the importScripts() method
- Spawning other web workers

**Workers do NOT have access to:**
- The DOM (it's not thread-safe)
- The window object
- The document object
- The parent object

**Full list of worker API:**
- Broadcast Channel API, 
- Cache API,
- Channel Messaging API,
- Console API, 
- Crypto, 
- CustomEvent, 
- Data Store (Firefox only), 
- DOMRequest and DOMCursor, 
- Fetch, 
- FileReader, 
- FileReaderSync (only works in workers!), 
- FormData, 
- ImageData, 
- IndexedDB, 
- Network Information API, 
- Notifications, 
- Performance, PerformanceEntry, PerformanceMeasure, PerformanceMark, PerformanceObserver, PerformanceResourceTiming, 
- Promise, 
- Server-sent events,
-  ServiceWorkerRegistration, 
-  TextEncoder and TextDecoder, 
-  URL, 
-  WebGL with OffscreenCanvas (enabled behind a feature preference setting gfx.offscreencanvas.enabled), 
-  WebSocket, 
-  XMLHttpRequest




## Worker Messaging
- use messages to communicate betwwen workers and main thread
- message payload is in `Event.data`
- messages are copies not references


Listen for the worker messages:
```
worker.addEventListener('message', function(e) {
  console.log('Worker said: ', e.data);
}, false);
```
Or:
```
worker.onmessage = (e) => {
  console.log('Worker said: ', e.data);
};
```


Listen for messages inside worker:

```
worker.addEventListener('message', (e) => {
  console.log('Message received:', e);
  e.data.answer = 42;
  self.postMessage(e.data);
}
```
Or:
```
self.onmessage = (e) => {
  console.log('Message received:', e);
  e.data.answer = 42;
  self.postMessage(e.data);
}, false);
```


The following functions are only available to workers:

- WorkerGlobalScope.importScripts() (all workers),
- DedicatedWorkerGlobalScope.postMessage (dedicated workers only).


# Transferable objects

We can transfer objects like `ArrayBuffer`, `File`, `Blob`,   

```
worker.postMessage({data: int8View, moreData: anotherBuffer},
                   [int8View.buffer, anotherBuffer]);
```

Read more: https://developer.mozilla.org/en-US/docs/Glossary/Transferable_objects


## Importing scripts and libraries
Worker threads have access to a global function, importScripts(), which lets them import scripts. 
It accepts zero or more URIs as parameters to resources to import; all of the following examples are valid:

```
  importScripts();                         /* imports nothing */
  importScripts('foo.js');                 /* imports just "foo.js" */
  importScripts('foo.js', 'bar.js');       /* imports two scripts */
  importScripts('//example.com/hello.js'); /* You can import scripts from other origins */
```

## Shared Workers
One big difference is that with a shared worker you have to communicate via a **port object** — an explicit port is opened 
that the scripts can use to communicate with the worker (this is done implicitly in the case of dedicated workers).

The port connection needs to be started either implicitly by use of the onmessage event handler or explicitly with the start() method before any messages can be posted. 
Calling start() is only needed if the message event is wired up via the addEventListener() method.




## Service Workers

- Service worker is a programmable network proxy, allowing you to control how network requests from your page are handled.
- It's terminated when not in use, and restarted when it's next needed, so you cannot rely on global state within a service worker's onfetch and onmessage handlers. 
If there is information that you need to persist and reuse across restarts, service workers do have access to the IndexedDB API.
- Service workers use promises
- You need HTTPS
- can be found on `chrome://inspect/#service-workers`

**Lifecycle:**
- The service worker URL is fetched and registered via serviceWorkerContainer.register().
- If successful, the service worker is executed in a ServiceWorkerGlobalScope; this is basically a special kind of worker context, running off the main script execution thread, with no DOM access.
- The service worker is now ready to process events.
- Installation of the worker is attempted when service worker-controlled pages are accessed subsequently. An Install event is always the first one sent to a service worker (this can be used to start the process of populating an IndexedDB, and caching site assets). This is really the same kind of procedure as installing a native or Firefox OS app — making everything available for use offline.
- When the oninstall handler completes, the service worker is considered installed.
- Next is activation. When the service worker is installed, it then receives an activate event. The primary use of onactivate is for cleanup of resources used in previous versions of a Service worker script.
- The Service worker will now control pages, but only those opened after the register() is successful. i.e. a document starts life with or without a Service worker and maintains that for its lifetime. So documents will have to be reloaded to actually be controlled.

![image](https://user-images.githubusercontent.com/22635061/140978235-06ea44f3-ba2b-4aa3-8896-c119239d4dba.png)
![image](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers/sw-lifecycle.png)


```
if('serviceWorker' in navigator) {
  navigator.serviceWorker.register('./pwa-examples/js13kpwa/sw.js');
};
```

More real world example:
```
if ('serviceWorker' in navigator) {
  window.addEventListener('load', function() {
    navigator.serviceWorker.register('/sw.js').then(function(registration) {
      // Registration was successful
      console.log('ServiceWorker registration successful with scope: ', registration.scope);
    }, function(err) {
      // registration failed :(
      console.log('ServiceWorker registration failed: ', err);
    });
  });
}
```

**Installing and adding the cache:**
```
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('v1').then((cache) => {
      return cache.addAll([
        './sw-test/',
        './sw-test/index.html',
        './sw-test/style.css',
        './sw-test/app.js',
        './sw-test/image-list.js',
        './sw-test/star-wars-logo.jpg',
        './sw-test/gallery/',
        './sw-test/gallery/bountyHunters.jpg',
        './sw-test/gallery/myLittleVader.jpg',
        './sw-test/gallery/snowTroopers.jpg'
      ]);
    })
  );
});
```

**Hijacking the request**

- The service worker will only catch requests from clients under the service worker's scope.
- The max scope for a service worker is the location of the worker.
- If your service worker is active on a client being served with the Service-Worker-Allowed header, you can specify a list of max scopes for that worker.
- In Firefox, Service Worker APIs are hidden and cannot be used when the user is in private browsing mode.


```
self.addEventListener('fetch', (event) => {
  event.respondWith(
    // magic goes here
  );
});
```

```
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request)
  );
});
```

```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});
```

```
self.addEventListener('fetch', function(event) {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }

        return fetch(event.request).then(
          function(response) {
            // Check if we received a valid response
            if(!response || response.status !== 200 || response.type !== 'basic') {
              return response;
            }

            // IMPORTANT: Clone the response. A response is a stream
            // and because we want the browser to consume the response
            // as well as the cache consuming the response, we need
            // to clone it so we have two streams.
            var responseToCache = response.clone();

            caches.open(CACHE_NAME)
              .then(function(cache) {
                cache.put(event.request, responseToCache);
              });

            return response;
          }
        );
      })
    );
});
```

Example app: https://github.com/mdn/sw-test




## Workbox from Google

Library from Google to easily deal with the service workers usually tasks

https://developers.google.com/web/tools/workbox



