# MDN | Web APIs | Fetch API
# Using the Fetch API

The [Fetch API](/en-US/docs/Web/API/Fetch_API) provides a JavaScript interface for accessing and manipulating parts of the HTTP pipeline, such as requests and responses. It also provides a global [`fetch()`](/en-US/docs/Web/API/fetch) method that provides an easy, logical way to fetch resources asynchronously across the network.

This kind of functionality was previously achieved using [`XMLHttpRequest`](/en-US/docs/Web/API/XMLHttpRequest). Fetch provides a better alternative that can be easily used by other technologies such as [`Service Workers`](/en-US/docs/Web/API/Service_Worker_API "Service Workers"). Fetch also provides a single logical place to define other HTTP-related concepts such as [CORS](/en-US/docs/Web/HTTP/CORS) and extensions to HTTP.

The `fetch` specification differs from `jQuery.ajax()` in the following significant ways:

-   The Promise returned from `fetch()` **won't reject on HTTP error status** even if the response is an HTTP 404 or 500. Instead, as soon as the server responds with headers, the Promise will resolve normally (with the [`ok`](/en-US/docs/Web/API/Response/ok "ok") property of the response set to false if the response isn't in the range 200–299), and it will only reject on network failure or if anything prevented the request from completing.
-   Unless `fetch()` is called with the [`credentials`](/en-US/docs/Web/API/fetch#credentials) option set to `include`, `fetch()`:
    -   won't send cookies in cross-origin requests
    -   won’t set any cookies sent back in cross-origin responses

A basic fetch request is really simple to set up. Have a look at the following code:

    fetch('http://example.com/movies.json')
      .then(response => response.json())
      .then(data => console.log(data)); 

Copy to Clipboard

Here we are fetching a JSON file across the network and printing it to the console. The simplest use of `fetch()` takes one argument — the path to the resource you want to fetch — and does not directly return the JSON response body but instead returns a promise that resolves with a [`Response`](/en-US/docs/Web/API/Response) object.

The [`Response`](/en-US/docs/Web/API/Response) object, in turn, does not directly contain the actual JSON response body but is instead a representation of the entire HTTP response. So, to extract the JSON body content from the [`Response`](/en-US/docs/Web/API/Response) object, we use the [`json()`](/en-US/docs/Web/API/Response/json "json()") method, which returns a second promise that resolves with the result of parsing the response body text as JSON.

**Note:** See the [Body](#body) section for similar methods to extract other types of body content.

Fetch requests are controlled by the `connect-src` directive of [Content Security Policy](/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) rather than the directive of the resources it's retrieving.

## [Supplying request options](#supplying_request_options "Permalink to Supplying request options")

The `fetch()` method can optionally accept a second parameter, an `init` object that allows you to control a number of different settings:

See [`fetch()`](/en-US/docs/Web/API/fetch) for the full options available, and more details.

    // Example POST method implementation:
    async function postData(url = '', data = {}) {
      // Default options are marked with *
      const response = await fetch(url, {
        method: 'POST', // *GET, POST, PUT, DELETE, etc.
        mode: 'cors', // no-cors, *cors, same-origin
        cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
        credentials: 'same-origin', // include, *same-origin, omit
        headers: {
          'Content-Type': 'application/json'
          // 'Content-Type': 'application/x-www-form-urlencoded',
        },
        redirect: 'follow', // manual, *follow, error
        referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        body: JSON.stringify(data) // body data type must match "Content-Type" header
      });
      return response.json(); // parses JSON response into native JavaScript objects
    }

    postData('https://example.com/answer', { answer: 42 })
      .then(data => {
        console.log(data); // JSON data parsed by `data.json()` call
      }); 

Copy to Clipboard

Note that `mode: "no-cors"` only allows a limited set of headers in the request:

-   `Accept`
-   `Accept-Language`
-   `Content-Language`
-   `Content-Type` with a value of `application/x-www-form-urlencoded`, `multipart/form-data`, or `text/plain`

## [Sending a request with credentials included](#sending_a_request_with_credentials_included "Permalink to Sending a request with credentials included")

To cause browsers to send a request with credentials included on both same-origin and cross-origin calls, add `credentials: 'include'` to the `init` object you pass to the `fetch()` method.

    fetch('https://example.com', {
      credentials: 'include'
    }); 

Copy to Clipboard

**Note:** `Access-Control-Allow-Origin` is prohibited from using a wildcard for requests with `credentials: 'include'`. In such cases, the exact origin must be provided; even if you are using a CORS unblocker extension, the requests will still fail.

**Note:** Browsers should not send credentials in _preflight requests_ irrespective of this setting. For more information see: [CORS > Requests with credentials](/en-US/docs/Web/HTTP/CORS#requests_with_credentials).

If you only want to send credentials if the request URL is on the same origin as the calling script, add `credentials: 'same-origin'`.

    // The calling script is on the origin 'https://example.com'

    fetch('https://example.com', {
      credentials: 'same-origin'
    }); 

Copy to Clipboard

To instead ensure browsers don't include credentials in the request, use `credentials: 'omit'`.

    fetch('https://example.com', {
      credentials: 'omit'
    }) 

Copy to Clipboard

## [Uploading JSON data](#uploading_json_data "Permalink to Uploading JSON data")

Use [`fetch()`](/en-US/docs/Web/API/fetch) to POST JSON-encoded data.

    const data = { username: 'example' };

    fetch('https://example.com/profile', {
      method: 'POST', // or 'PUT'
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(data),
    })
    .then(response => response.json())
    .then(data => {
      console.log('Success:', data);
    })
    .catch((error) => {
      console.error('Error:', error);
    }); 

Copy to Clipboard

## [Uploading a file](#uploading_a_file "Permalink to Uploading a file")

Files can be uploaded using an HTML `<input type="file" />` input element, [`FormData()`](/en-US/docs/Web/API/FormData/FormData "FormData()") and [`fetch()`](/en-US/docs/Web/API/fetch).

    const formData = new FormData();
    const fileField = document.querySelector('input[type="file"]');

    formData.append('username', 'abc123');
    formData.append('avatar', fileField.files[0]);

    fetch('https://example.com/profile/avatar', {
      method: 'PUT',
      body: formData
    })
    .then(response => response.json())
    .then(result => {
      console.log('Success:', result);
    })
    .catch(error => {
      console.error('Error:', error);
    }); 

Copy to Clipboard

## [Uploading multiple files](#uploading_multiple_files "Permalink to Uploading multiple files")

Files can be uploaded using an HTML `<input type="file" multiple />` input element, [`FormData()`](/en-US/docs/Web/API/FormData/FormData "FormData()") and [`fetch()`](/en-US/docs/Web/API/fetch).

    const formData = new FormData();
    const photos = document.querySelector('input[type="file"][multiple]');

    formData.append('title', 'My Vegas Vacation');
    for (let i = 0; i < photos.files.length; i++) {
      formData.append(`photos_${i}`, photos.files[i]);
    }

    fetch('https://example.com/posts', {
      method: 'POST',
      body: formData,
    })
    .then(response => response.json())
    .then(result => {
      console.log('Success:', result);
    })
    .catch(error => {
      console.error('Error:', error);
    }); 

Copy to Clipboard

## [Processing a text file line by line](#processing_a_text_file_line_by_line "Permalink to Processing a text file line by line")

The chunks that are read from a response are not broken neatly at line boundaries and are Uint8Arrays, not strings. If you want to fetch a text file and process it line by line, it is up to you to handle these complications. The following example shows one way to do this by creating a line iterator (for simplicity, it assumes the text is UTF-8, and doesn't handle fetch errors).

    async function* makeTextFileLineIterator(fileURL) {
      const utf8Decoder = new TextDecoder('utf-8');
      const response = await fetch(fileURL);
      const reader = response.body.getReader();
      let { value: chunk, done: readerDone } = await reader.read();
      chunk = chunk ? utf8Decoder.decode(chunk) : '';

      const re = /\n|\r|\r\n/gm;
      let startIndex = 0;
      let result;

      for (;;) {
        let result = re.exec(chunk);
        if (!result) {
          if (readerDone) {
            break;
          }
          let remainder = chunk.substr(startIndex);
          ({ value: chunk, done: readerDone } = await reader.read());
          chunk = remainder + (chunk ? utf8Decoder.decode(chunk) : '');
          startIndex = re.lastIndex = 0;
          continue;
        }
        yield chunk.substring(startIndex, result.index);
        startIndex = re.lastIndex;
      }
      if (startIndex < chunk.length) {
        // last line didn't end in a newline char
        yield chunk.substr(startIndex);
      }
    }

    async function run() {
      for await (let line of makeTextFileLineIterator(urlOfFile)) {
        processLine(line);
      }
    }

    run(); 

Copy to Clipboard

## [Checking that the fetch was successful](#checking_that_the_fetch_was_successful "Permalink to Checking that the fetch was successful")

A [`fetch()`](/en-US/docs/Web/API/fetch) promise will reject with a [`TypeError`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypeError) when a network error is encountered or CORS is misconfigured on the server-side, although this usually means permission issues or similar — a 404 does not constitute a network error, for example. An accurate check for a successful `fetch()` would include checking that the promise resolved, then checking that the [`Response.ok`](/en-US/docs/Web/API/Response/ok) property has a value of true. The code would look something like this:

    fetch('flowers.jpg')
      .then(response => {
        if (!response.ok) {
          throw new Error('Network response was not OK');
        }
        return response.blob();
      })
      .then(myBlob => {
        myImage.src = URL.createObjectURL(myBlob);
      })
      .catch(error => {
        console.error('There has been a problem with your fetch operation:', error);
      }); 

Copy to Clipboard

## [Supplying your own request object](#supplying_your_own_request_object "Permalink to Supplying your own request object")

Instead of passing a path to the resource you want to request into the `fetch()` call, you can create a request object using the [`Request()`](/en-US/docs/Web/API/Request/Request "Request()") constructor, and pass that in as a `fetch()` method argument:

    const myHeaders = new Headers();

    const myRequest = new Request('flowers.jpg', {
      method: 'GET',
      headers: myHeaders,
      mode: 'cors',
      cache: 'default',
    });

    fetch(myRequest)
      .then(response => response.blob())
      .then(myBlob => {
        myImage.src = URL.createObjectURL(myBlob);
      }); 

Copy to Clipboard

`Request()` accepts exactly the same parameters as the `fetch()` method. You can even pass in an existing request object to create a copy of it:

    const anotherRequest = new Request(myRequest, myInit); 

Copy to Clipboard

This is pretty useful, as request and response bodies can only be used once. Making a copy like this allows you to effectively use the request/response again while varying the `init` options if desired. The copy must be made before the body is read.

**Note:** There is also a [`clone()`](/en-US/docs/Web/API/Request/clone "clone()") method that creates a copy. Both methods of creating a copy will fail if the body of the original request or response has already been read, but reading the body of a cloned response or request will not cause it to be marked as read in the original.

## [Headers](#headers "Permalink to Headers")

The [`Headers`](/en-US/docs/Web/API/Headers) interface allows you to create your own headers object via the [`Headers()`](/en-US/docs/Web/API/Headers/Headers "Headers()") constructor. A headers object is a simple multi-map of names to values:

    const content = 'Hello World';
    const myHeaders = new Headers();
    myHeaders.append('Content-Type', 'text/plain');
    myHeaders.append('Content-Length', content.length.toString());
    myHeaders.append('X-Custom-Header', 'ProcessThisImmediately'); 

Copy to Clipboard

The same can be achieved by passing an array of arrays or an object literal to the constructor:

    const myHeaders = new Headers({
      'Content-Type': 'text/plain',
      'Content-Length': content.length.toString(),
      'X-Custom-Header': 'ProcessThisImmediately'
    }); 

Copy to Clipboard

The contents can be queried and retrieved:

    console.log(myHeaders.has('Content-Type')); // true
    console.log(myHeaders.has('Set-Cookie')); // false
    myHeaders.set('Content-Type', 'text/html');
    myHeaders.append('X-Custom-Header', 'AnotherValue');

    console.log(myHeaders.get('Content-Length')); // 11
    console.log(myHeaders.get('X-Custom-Header')); // ['ProcessThisImmediately', 'AnotherValue']

    myHeaders.delete('X-Custom-Header');
    console.log(myHeaders.get('X-Custom-Header')); // null 

Copy to Clipboard

Some of these operations are only useful in [`ServiceWorkers`](/en-US/docs/Web/API/Service_Worker_API "ServiceWorkers"), but they provide a much nicer API for manipulating headers.

All of the Headers methods throw a `TypeError` if a header name is used that is not a valid HTTP Header name. The mutation operations will throw a `TypeError` if there is an immutable guard ([see below](#guard)). Otherwise, they fail silently. For example:

    const myResponse = Response.error();
    try {
      myResponse.headers.set('Origin', 'http://mybank.com');
    } catch (e) {
      console.log('Cannot pretend to be a bank!');
    } 

Copy to Clipboard

A good use case for headers is checking whether the content type is correct before you process it further. For example:

    fetch(myRequest)
      .then(response => {
         const contentType = response.headers.get('content-type');
         if (!contentType || !contentType.includes('application/json')) {
           throw new TypeError("Oops, we haven't got JSON!");
         }
         return response.json();
      })
      .then(data => {
          /* process your data further */
      })
      .catch(error => console.error(error)); 

Copy to Clipboard

### [Guard](#guard "Permalink to Guard")

Since headers can be sent in requests and received in responses, and have various limitations about what information can and should be mutable, headers' objects have a _guard_ property. This is not exposed to the Web, but it affects which mutation operations are allowed on the headers object.

Possible guard values are:

-   `none`: default.
-   `request`: guard for a headers object obtained from a request ([`Request.headers`](/en-US/docs/Web/API/Request/headers)).
-   `request-no-cors`: guard for a headers object obtained from a request created with [`Request.mode`](/en-US/docs/Web/API/Request/mode) `no-cors`.
-   `response`: guard for a headers object obtained from a response ([`Response.headers`](/en-US/docs/Web/API/Response/headers)).
-   `immutable`: guard that renders a headers object read-only; mostly used for ServiceWorkers.

**Note:** You may not append or set the `Content-Length` header on a guarded headers object for a `response`. Similarly, inserting `Set-Cookie` into a response header is not allowed: ServiceWorkers are not allowed to set cookies via synthesized responses.

## [Response objects](#response_objects "Permalink to Response objects")

As you have seen above, [`Response`](/en-US/docs/Web/API/Response) instances are returned when `fetch()` promises are resolved.

The most common response properties you'll use are:

-   [`Response.status`](/en-US/docs/Web/API/Response/status) — An integer (default value 200) containing the response status code.
-   [`Response.statusText`](/en-US/docs/Web/API/Response/statusText) — A string (default value ""), which corresponds to the HTTP status code message. Note that HTTP/2 [does not support](https://fetch.spec.whatwg.org/#concept-response-status-message) status messages.
-   [`Response.ok`](/en-US/docs/Web/API/Response/ok) — seen in use above, this is a shorthand for checking that status is in the range 200-299 inclusive. This returns a boolean value.

They can also be created programmatically via JavaScript, but this is only really useful in [`ServiceWorkers`](/en-US/docs/Web/API/Service_Worker_API "ServiceWorkers"), when you are providing a custom response to a received request using a [`respondWith()`](/en-US/docs/Web/API/FetchEvent/respondWith "respondWith()") method:

    const myBody = new Blob();

    addEventListener('fetch', function(event) {
      // ServiceWorker intercepting a fetch
      event.respondWith(
        new Response(myBody, {
          headers: { 'Content-Type': 'text/plain' }
        })
      );
    }); 

Copy to Clipboard

The [`Response()`](/en-US/docs/Web/API/Response/Response "Response()") constructor takes two optional arguments — a body for the response, and an init object (similar to the one that [`Request()`](/en-US/docs/Web/API/Request/Request "Request()") accepts.)

**Note:** The static method [`error()`](/en-US/docs/Web/API/Response/error "error()") returns an error response. Similarly, [`redirect()`](/en-US/docs/Web/API/Response/redirect "redirect()") returns a response resulting in a redirect to a specified URL. These are also only relevant to Service Workers.

## [Body](#body "Permalink to Body")

Both requests and responses may contain body data. A body is an instance of any of the following types:

-   [`ArrayBuffer`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
-   [`ArrayBufferView`](/en-US/docs/Web/API/ArrayBufferView) (Uint8Array and friends)
-   [`Blob`](/en-US/docs/Web/API/Blob)/[`File`](/en-US/docs/Web/API/File)
-   string
-   [`URLSearchParams`](/en-US/docs/Web/API/URLSearchParams)
-   [`FormData`](/en-US/docs/Web/API/FormData)

The [`Request`](/en-US/docs/Web/API/Request) and [`Response`](/en-US/docs/Web/API/Response) interfaces share the following methods to extract a body. These all return a promise that is eventually resolved with the actual content.

-   [`Request.arrayBuffer()`](/en-US/docs/Web/API/Request/arrayBuffer) / [`Response.arrayBuffer()`](/en-US/docs/Web/API/Response/arrayBuffer)
-   [`Request.blob()`](/en-US/docs/Web/API/Request/blob) / [`Response.blob()`](/en-US/docs/Web/API/Response/blob)
-   [`Request.formData()`](/en-US/docs/Web/API/Request/formData) / [`Response.formData()`](/en-US/docs/Web/API/Response/formData)
-   [`Request.json()`](/en-US/docs/Web/API/Request/json) / [`Response.json()`](/en-US/docs/Web/API/Response/json)
-   [`Request.text()`](/en-US/docs/Web/API/Request/text) / [`Response.text()`](/en-US/docs/Web/API/Response/text)

This makes usage of non-textual data much easier than it was with XHR.

Request bodies can be set by passing body parameters:

    const form = new FormData(document.getElementById('login-form'));
    fetch('/login', {
      method: 'POST',
      body: form
    }); 

Copy to Clipboard

Both request and response (and by extension the `fetch()` function), will try to intelligently determine the content type. A request will also automatically set a `Content-Type` header if none is set in the dictionary.

## [Feature detection](#feature_detection "Permalink to Feature detection")

Fetch API support can be detected by checking for the existence of [`Headers`](/en-US/docs/Web/API/Headers), [`Request`](/en-US/docs/Web/API/Request), [`Response`](/en-US/docs/Web/API/Response) or [`fetch()`](/en-US/docs/Web/API/fetch) on the [`Window`](/en-US/docs/Web/API/Window) or [`Worker`](/en-US/docs/Web/API/Worker) scope. For example:

    if (window.fetch) {
      // run my fetch request here
    } else {
      // do something with XMLHttpRequest?
    } 

Copy to Clipboard

## [Specifications](#specifications "Permalink to Specifications")

| Specification |
| ------------- |

\| [Fetch Standard  
# fetch-method](https://fetch.spec.whatwg.org/#fetch-method) \|

## [Browser compatibility](#browser_compatibility "Permalink to Browser compatibility")

[Report problems with this compatibility data on GitHub](https://github.com/mdn/browser-compat-data/issues/new?body=%3C%21--+Tips%3A+where+applicable%2C+specify+browser+name%2C+browser+version%2C+and+mobile+operating+system+version+--%3E%0A%0A%23%23%23%23+What+information+was+incorrect%2C+unhelpful%2C+or+incomplete%3F%0A%0A%23%23%23%23+What+did+you+expect+to+see%3F%0A%0A%23%23%23%23+Did+you+test+this%3F+If+so%2C+how%3F%0A%0A%0A%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EMDN+page+report+details%3C%2Fsummary%3E%0A%0A*+Query%3A+%60api.fetch%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FFetch_API%2FUsing_Fetch%0A*+Report+started%3A+2022-04-14T04%3A19%3A14.832Z%0A%0A%3C%2Fdetails%3E&title=api.fetch+-+%3CPUT+TITLE+HERE%3E "Report an issue with this compatibility data")

|     | desktop | mobile | server |
| --- | ------- | ------ | ------ |
|     |         |        |        |

Chrome

 \| 

Edge

 \| 

Firefox

 \| 

Internet Explorer

 \| 

Opera

 \| 

Safari

 \| 

WebView Android

 \| 

Chrome Android

 \| 

Firefox for Android

 \| 

Opera Android

 \| 

Safari on iOS

 \| 

Samsung Internet

 \| 

Deno

|  |
|  |

\| 

`fetch`

 | Full supportChrome42

Toggle history | Full supportEdge14

Toggle history | Full supportFirefox39

Toggle history | No supportInternet ExplorerNo

Toggle history | Full supportOpera29

Toggle history | Full supportSafari10.1

Toggle history | Full supportWebView Android42

Toggle history | Full supportChrome Android42

Toggle history | Full supportFirefox for Android39

Toggle history | Full supportOpera Android29

Toggle history | Full supportSafari on iOS10.3

Toggle history | Full supportSamsung Internet4.0

Toggle history | Full supportDeno1.0

footnote

Toggle history |
\| 

Support for blob: and data:

 | Full supportChrome48

Toggle history | Full supportEdge79

Toggle history | Full supportFirefox39

Toggle history | No supportInternet ExplorerNo

Toggle history | Full supportOpera35

Toggle history | Full supportSafari10.1

Toggle history | Full supportWebView Android43

Toggle history | Full supportChrome Android48

Toggle history | Full supportFirefox for Android39

Toggle history | Full supportOpera Android35

Toggle history | Full supportSafari on iOS10.3

Toggle history | Full supportSamsung Internet5.0

Toggle history | Full supportDeno1.9

Toggle history |
\| 

referrerPolicy

 | Full supportChrome52

Toggle history | Full supportEdge79

Toggle history | Full supportFirefox52

Toggle history | No supportInternet ExplorerNo

Toggle history | Full supportOpera39

Toggle history | Full supportSafari11.1

Toggle history | Full supportWebView Android52

Toggle history | Full supportChrome Android52

Toggle history | Full supportFirefox for Android52

Toggle history | Full supportOpera Android41

Toggle history | No supportSafari on iOSNo

Toggle history | Full supportSamsung Internet6.0

Toggle history | No supportDenoNo

Toggle history |
\| 

signal

 | Full supportChrome66

Toggle history | Full supportEdge16

Toggle history | Full supportFirefox57

Toggle history | No supportInternet ExplorerNo

Toggle history | Full supportOpera53

Toggle history | Full supportSafari11.1

Toggle history | Full supportWebView Android66

Toggle history | Full supportChrome Android66

Toggle history | Full supportFirefox for Android57

Toggle history | Full supportOpera Android47

Toggle history | Full supportSafari on iOS11.3

Toggle history | Full supportSamsung Internet9.0

Toggle history | Full supportDeno1.11

Toggle history |
\| 

Available in workers

 | Full supportChrome42

Toggle history | Full supportEdge14

Toggle history | Full supportFirefox39

Toggle history | No supportInternet ExplorerNo

Toggle history | Full supportOpera29

Toggle history | Full supportSafari10.1

Toggle history | Full supportWebView Android42

Toggle history | Full supportChrome Android42

Toggle history | Full supportFirefox for Android39

Toggle history | Full supportOpera Android29

Toggle history | Full supportSafari on iOS10.3

Toggle history | Full supportSamsung Internet4.0

Toggle history | Full supportDenoYes

Toggle history |

### Legend

Full support

Full support

No support

No support

See implementation notes.

The compatibility table on this page is generated from structured data. If you'd like to contribute to the data, please check out [https://github.com/mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) and send us a pull request.

## [See also](#see_also "Permalink to See also")

-   [ServiceWorker API](/en-US/docs/Web/API/Service_Worker_API)
-   [HTTP access control (CORS)](/en-US/docs/Web/HTTP/CORS)
-   [HTTP](/en-US/docs/Web/HTTP)
-   [Fetch polyfill](https://github.com/github/fetch)
-   [Fetch examples on GitHub](https://github.com/mdn/fetch-examples/)

#### Found a problem with this page?

-   [Edit on **GitHub**](https://github.com/mdn/content/edit/main/files/en-us/web/api/fetch_api/using_fetch/index.md "You're going to need to sign in to GitHub first (Opens in a new tab)")
-   [Source on **GitHub**](https://github.com/mdn/content/blob/main/files/en-us/web/api/fetch_api/using_fetch/index.md?plain=1 "Folder: en-us/web/api/fetch_api/using_fetch (Opens in a new tab)")
-   [Report a problem with this content on **GitHub**](https://github.com/mdn/content/issues/new?template=page-report.yml&mdn-url=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FFetch_API%2FUsing_Fetch&metadata=%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EPage+report+details%3C%2Fsummary%3E%0A%0A*+Folder%3A+%60en-us%2Fweb%2Fapi%2Ffetch_api%2Fusing_fetch%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FAPI%2FFetch_API%2FUsing_Fetch%0A*+GitHub+URL%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fblob%2Fmain%2Ffiles%2Fen-us%2Fweb%2Fapi%2Ffetch_api%2Fusing_fetch%2Findex.md%0A*+Last+commit%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fcommit%2F0440b0fa1c116be244d5047c37a0f1197af13282%0A*+Document+last+modified%3A+2022-04-11T18%3A17%3A24.000Z%0A%0A%3C%2Fdetails%3E "This will take you to GitHub to file a new issue")
-   Want to fix the problem yourself? See [our Contribution guide](https://github.com/mdn/content/blob/main/README.md).

**Last modified:** Apr 11, 2022, [by MDN contributors](/en-US/docs/Web/API/Fetch_API/Using_Fetch/contributors.txt) 
 [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 
 [https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
