# Service Worker

(docs)[https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API/Using_Service_Workers]
Service workers are a way to implement **Offline first** applications where even without internet connectivity, you can use cached assets first.

As such they are like a **proxy server** where they can modify your requests and responses and replace them with what you actually want.

## Setting up

You'll need server that supports HTTPS. This is for security reasons. `localhost` also works too.

## Architecture

1. You'll need to first fetch and register your code using `serviceWorkerContainer.register()`.
   1a. The service worker will execute in `ServiceWorkerGlobalScope` which will then be ready to process events. It does not have DOM access.
2. An `install` event will be sent to the service worker which will start caching your assets or populating your `IndexedDB`.
3. After `install` completion, it is considered installed but not active. This is because an older version of the service worker may still be active.
4. When all pages of the old version of the service worker have closed, the new service worker will receive an `activate` event. It can override the previous by calling `skipWaiting()`. but the activate event is mainly used for cleaning up resources from the previous service worker.
5. Once activated, the service worker will now control pages that were opened after the `register()` function was successfully called. You can override this too by calling `clients.claim()`.
