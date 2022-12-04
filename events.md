## Page
1. `DOMContentLoaded event`: 
- when: DOM is ready, but not other resource
- act: in fact, it wait script until done before fire event. So if script wait to use style -> this evt fire after style loaded.

2. `load event`:
- when:  external resources are loaded, so styles are applied, image sizes are known etc.

3. `beforeunload event` 
– when: the user is leaving: we can check if the user saved the changes and ask them whether they really want to leave.
- act: not guaranteed all code in handler excute, some docs say that sync code can work okay, and async code is maybe or not.
       But in practice, the bellow code not excute all sync code (storage.setitem). and the code schedule to excute never run. And if user click close tab again, tab will be close immediately
- folow up: can using navigator.sendBacon to analyst. It run in other timer - background.
  
```javascript
  <script>
       window.addEventListener("beforeunload", (event) => {
        let i = 0;
        setTimeout(()=> {
            localStorage.setItem('i', i++);
        }, 0);
        for (let index = 0; index <= 1000000; index++) {
            localStorage.setItem('k', index);
            // lastest value recored is nearly 730737
        }
        });
   </script>
```

5. `unload`:
– when: the user almost left, but we still can initiate some operations, such as sending out statistics.

## Resource
1. `onload`: when resource is fuly loaded.
2. `onerror`: when error occur during loading resource, we can't track detail error
3. `window.onerror`: to get more detail about error

```javascript
  <script>
    window.onerror = function(message, url, line, col, errorObj) {
      alert(`${message}\n${url}, ${line}:${col}`);
    };
  </script>
  <script crossorigin="anonymous" src="https://cors.javascript.info/article/onload-onerror/crossorigin/error.js"></script>
```
note that: 
- iframe only have `onload` when finish. indicae success or failure.
- load external resource may cause cross origin: we need set
+ `anonymous`: Request uses CORS headers and credentials flag is set to 'same-origin'. There is no exchange of user credentials via cookies, client-side SSL certificates or HTTP authentication, unless destination is the same origin.
It mean, access allowed if the server responds with the header Access-Control-Allow-Origin with * or our origin.
+ `use-credentials`: access allowed if the server sends back the header Access-Control-Allow-Origin with our origin and Access-Control-Allow-Credentials: true. Browser sends authorization information and cookies to remote server.

By default (that is, when the attribute is not specified), CORS is not used at all. The user agent will not ask for permission for full access to the resource
