- `XMLHttpRequest` usage
- URL
- Url encoding
- Form Uploading
- File Uploading
- data urls
- CORS
- Cookies




## XMLHttpRequest Basic Usage
```
// 1. Create request
const xhr = new XMLHttpRequest();

// 2. init it
xhr.open(method, URL, [async, user, password])

// 3. Subscribe on events
xhr.onload = (evt) => {
  // check xhr.status and xhr.response
}

xhr.onerror = (err) => {
  // error handling
}

// 4. send request, add body if we need
xhr.send([body])
```




### XMLHttpRequest Example 
```
// 1. Create a new XMLHttpRequest object
let xhr = new XMLHttpRequest();

// 2. Configure it: GET-request for the URL /article/.../load
xhr.open('GET', '/article/xmlhttprequest/example/load');

// 3. Send the request over the network
xhr.send(requestBody);

// 4. This will be called after the response is received
xhr.onload = function() {
  if (xhr.status != 200) { // analyze HTTP status of the response
    alert(`Error ${xhr.status}: ${xhr.statusText}`); // e.g. 404: Not Found
  } else { // show the result
    alert(`Done, got ${xhr.response.length} bytes`); // response is the server response
  }
};

xhr.onprogress = function(event) {
  if (event.lengthComputable) {
    alert(`Received ${event.loaded} of ${event.total} bytes`);
  } else {
    alert(`Received ${event.loaded} bytes`); // no Content-Length
  }

};

xhr.onerror = function() {
  alert("Request failed");
};
```




### XMLHttpRequest progress states

XHR.readyState property has current state of the request. There are several values possible:

```
const UNSENT = 0; // initial state
const OPENED = 1; // open called
const HEADERS_RECEIVED = 2; // response headers received
const LOADING = 3; // response is loading (a data packet is received)
const DONE = 4; // request complete
// UNSENT 0 -> OPENED 1 -> HEADERS_RECEIVED 2 -> LOADING 3-> DONE 4

let xhr = new XMLHttpRequest();
xhr.open('GET', '/article/xmlhttprequest/example/load');

xhr.onreadystatechange = function() {
  if (xhr.readyState == 3) {
    // loading
  }
  if (xhr.readyState == 4) {
    // request finished
    if (xhr.status != 200) { // analyze HTTP status of the response
      alert(`Error ${xhr.status}: ${xhr.statusText}`); // e.g. 404: Not Found
    } else { // show the result
      alert(`Done, got ${xhr.response.length} bytes`); // response is the server response
    }    
  }
};

xhr.send();
```





### URL objects
We can use URL to build and parse urls. Also, we can use them instead of strings with `Fetch` & `XMLHttpRequest`

```
let url = new URL('https://google.com/search');
url.searchParams.set('q', 'test me!');

// the parameter 'q' is encoded
xhr.open('GET', url); // https://google.com/search?q=test+me%21

for (const [key, value] of url.searchParams.entries()) {
  console.log(key, value);
}
```



### URL encoding

Url with special symbols must be encoded.
There are several ways to encode & decode URL:
- `encodeURI`
- `encodeURIComponent`
- use `URL` object
- use some web services
- copy & paste & copy again browser address 



### Forms, encdoding and FormData

Forms are send using `multipart/form-data`, method is usually POST.
 
They can be sent in 3 ways
1) Automatically, on subscribe button
2) Grab all data, format body as `multipart/form-data`, send AJAX
3) Use formData to grab data, send AJAX


```
<form name="person">
  <input name="name" value="John">
  <input name="surname" value="Smith">
</form>

<script>
  // pre-fill FormData from the form
  let formData = new FormData(document.forms.person);

  // add one more field
  formData.append("middle", "Lee");

  // send it out
  let xhr = new XMLHttpRequest();
  xhr.open("POST", "/article/xmlhttprequest/post/user");
  xhr.send(formData);

  xhr.onload = () => alert(xhr.response);
</script>
```


There are 2 ways to encode data in forms:
- `multipart/form-data`
- `application/x-www-form-urlencoded`

```
<form action="http://localhost:8000" method="post" enctype="multipart/form-data">
</form>

<formaction="http://localhost:8000" method="post" enctype="application/x-www-form-urlencoded">
</form>
```

`multipart/form-data`: adds a few bytes of boundary overhead to the message, and must spend some time calculating it, but sends each byte in one byte.

`application/x-www-form-urlencoded`: has a single byte boundary per field (&), but adds a linear overhead factor of 3x for every non-printable character.

Therefore, even if we could send files with application/x-www-form-urlencoded, we wouldn't want to, because it is so inefficient.


### File Uploading
Files usually are uploaded via forms with `<input type="file">`
But you can also construct request by yourself.

```
<form name="person">
  <input name="name" value="John">
  <input name="surname" value="Smith">
  <input name="photo" type="file" accept="image/png, image/jpeg">
  <input name="cv" type="file" accept="application/pdf, application/vnd.openxmlformats-officedocument.wordprocessingml.document">
</form>

<script>
  // pre-fill FormData from the form
  let formData = new FormData(document.forms.person);

  // add one more field
  formData.append("middle", "Lee");

  // send it out
  let xhr = new XMLHttpRequest();
  xhr.open("POST", "/article/xmlhttprequest/post/user");
  xhr.send(formData);

  xhr.onload = () => alert(xhr.response);
</script>
```
```

### 

