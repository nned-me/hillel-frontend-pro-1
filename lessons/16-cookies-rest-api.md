## Cookies
It is just a bit of information. Can be set by server or client. Can be accesses by js via `document.cookie`


To get cookies, just use `document.cookie`
To set one parameter of cookie, use:
```
document.cookie = `${key}=${val}`;
```

To set cookie expiration date use `expires` or `max-age`. 
```
date = date.toUTCString();
document.cookie = "user=John; expires=" + date;
// or use max-age
document.cookie = "user=John; max-age=" + 60*60*24;
```

- Strict
- Domain
- path
- SameSite : `Strict`, `Lax`, and `None`
- httpOnly


.
.
.
.

## Local Storage & Session Storage

### Local Storage

Объекты хранилища localStorage и sessionStorage предоставляют одинаковые методы и свойства:

```
setItem(key, value) – save key/val.
getItem(key) – get vavlue by key.
removeItem(key) – remove key.
clear() – remove all.
key(index) – get key by index
length – storage size
```


### Session Storage
- sessionStorage существует только в рамках текущей вкладки браузера.
- Другая вкладка с той же страницей будет иметь другое хранилище.
- Но оно разделяется между ифреймами на той же вкладке (при условии, что они из одного и того же источника).
- Данные продолжают существовать после перезагрузки страницы, но не после закрытия/открытия вкладки.


### Event onstorage 
This event goes

```
window.onstorage = (event) => {
  console.log(event);
  // key – updated key name (null, if .clear()).
  // oldValue – old value  (null, if new key).
  // newValue – new value   (null, if key was deleted).
  // url – url of document where update was made
  // storageArea – localStorage/sessionStorage object
}
```



## REST / RESTfull API
representational state transfer
https://restfulapi.net


### Identification of resources 
The interface must uniquely identify each resource involved in the interaction between the client and the server.

### Manipulation of resources through representations
The resources should have uniform representations in the server response. API consumers should use these representations to modify the resources state in the server.

### Self-descriptive message
Each resource representation should carry enough information to describe how to process the message. 
It should also provide information of the additional actions that the client can perform on the resource.

### Hypermedia as the engine of application state
The client should have only the initial URI of the application. The client application should dynamically drive all other resources and interactions with the use of hyperlinks.






