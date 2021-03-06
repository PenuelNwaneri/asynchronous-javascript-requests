# Asynchronous JavaScript Requests
By Richard Kalehoff and Co.
Started 9-30-2017 Finished 10-02-2017

# Lesson 1: Ajax with XHR ✔
## Course Intro
- With asynchronous calls, you make a request for data and then do something else
- You deal with the data when it comes back
- Asynchronous javascript and XML refers to many formats now, not just those 2 as in the past
- Today, it's a misnomer but AJAX now refers to any asynchronous calls made to an API
- We will look at XHR object, jQuery, and fetch API

## Client Server Demonstration
- The internet is a world of communication between clients, the internet, and servers
- Clients makes a "GET" request and wait for a "RESPONSE" from the server
- Synchronously, we have to wait for data to arrive IE to render a website
- With async, you can "get" request data and the process will run in the background
- When the response returns, you can use a "callback" to apply special instructions to the response
- Async allows us to make other requests or do other things without having to wait for the response to return
- So with the website, we just wait until the response returns and render *only* that element
- There is no reloading of the whole page which makes the user experience much better

## Ajax Definition & Examples
- AJAX stands for asynchronous JavaScript and XML
- XML use to be the dominant format, now most use JSON format
- So more correctly, AJAJ is a better name but doesn't sound as nice
- So the AJAX response can return as XML, JSON, or HTML
- You can see how it works by playing with it in dev tools under the network tab
- The return formats as follows:
1. XML `<entry></entry>`
2. JSON `{property: data}`
3. HTML `<div></div>`
  
### History
- AJAX was introduced in the 1990's
- At this time, pages loaded synchronously and had to wait for all data to come in before rendering the page
- This made it slow and clunky
- The AJAX method was asynchronous and only renders those elements that need update
- This method is much quickly and seemed almost instantaneous
  
### Problem
- A big problem was many browsers didn't have it implemented so complex code was required
- That was until jQuery and YUI standardized it

## APIs
- We use APIs (Application Programming Interface) to interact with various data sources
- There are millions out there from Google, Youtube, etc.
    - [Google APIs](https://developers.google.com/apis-explorer/)
    - [Giant Database of APIs](http://www.programmableweb.com/apis/directory)
    - [Udacity API](https://www.udacity.com/public-api/v1/catalog)

## Create An Async Request with XHR
- We have to do a lot of the initial setup of a "GET" request
- XHR is provided by the javascript environment and used to make AJAX requests

## The XHR Object
- XHR is short for XMLHttpReqequest
- Provided by javascript just like the document object
- Run this command on Unsplash: `const asyncRequestObject = new XMLHttpRequest()`
- Even though it has XML in it, it is not limited to only XML documents
- XML was just the dominant format in the past
  
### More reading
- MDN's Docs - https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/open
- WHATWG Spec - https://xhr.spec.whatwg.org/
- W3C Spec - https://www.w3.org/TR/XMLHttpRequest/

## XHR's.open() method
- One popular method from setting up an XHR opject is:
- It sets up the stage and gives the object the info it needs when the request is sent
```js
asyncRequestObject.open();
// Takes (method, url, async, user, password)
```
- Methods are GET (to retrieve data) and POST(to send data)
- Remember about [same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) and [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)
- CORS is Cross-Origin Resource Sharing which is where servers install this technology so they can share resources with developers
- Usually with same-origin, you have to be on the same domain to access information, but with CORS, you don't

- Now we try on 
```js
const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');

// Nothing happens? Why?
// It's not enough just to use .open() but we also need to send out the request with .send();
```

## XHR's.send() method
- Now we add a .send() method
```js
const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.send();
```
  
### Handling Success
- Notice that we can see that the request is sent using the network tab in dev tools
- Even though the request was sent, we have to determine what to do with the response
- We need to use the .onload to run the handleSuccess() function
```js
function handleSuccess () {
    // in the function, the `this` value is the XHR object
    // this.responseText holds the response from the server

    console.log( this.responseText ); // the HTML of https://unsplash.com/
}

const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.send();
asyncRequestObject.onload = handleSuccess;
```
  
### Handling Errors
- We need a way to handle if we get any errors also
```js
function handleError () {
    // in the function, the `this` value is the XHR object
    console.log( 'An error occurred 😞' );
}

const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.send();
asyncRequestObject.onload = handleSuccess;
asyncRequestObject.onerror = handleError;
```

## A Full Request
- The full code we have:
```js
function handleSuccess () { 
  console.log( this.responseText ); // the HTML of https://unsplash.com/
}

function handleError () { 
  console.log( 'An error occurred \uD83D\uDE1E' );
}

const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.onload = handleSuccess;
asyncRequestObject.onerror = handleError;
asyncRequestObject.send();
```
  
### APIs and JSONs
- What do we do with a JSON went we get back the response?
- We can use `JSON.parse()` to output it into a javascript object
- So now we tweak out .onload() to accomodate this
```js
function handleSuccess () {
    const data = JSON.parse( this.responseText ); // convert data from JSON to a JavaScript object
    console.log( data );
}

function handleError () { 
    console.log( 'An error occurred \uD83D\uDE1E' );
}
const asyncRequestObject = new XMLHttpRequest();
asyncRequestObject.open('GET', 'https://unsplash.com');
asyncRequestObject.onload = handleSuccess;
asyncRequestObject.onerror = handleError;
asyncRequestObject.send();
```

## Project Initial Walkthrough
- We are going to be making a search form to generate some picture and results
### Clone the repo
- I've cloned the repo from the course with git clone https://github.com/udacity/course-ajax.git4  
  
### Unsplash API
- Create a developer account here - https://unsplash.com/developers
- Next, create an application here - https://unsplash.com/oauth/applications
- this will give you an "Application ID" that you'll need to make requests  
  
### The New York Times API
- Create a developer account here - https://developer.nytimes.com/
- They'll email you your api-key (you'll need this to make requests)  
  
### Unsplash Request Needs Something Else?
- Note that Unsplash requires an HTTP header
- We use XMLHttpRequest.setRequestHeader() as the method to set this
  
- *You must call this method after "open" but before "send"*
- Now we have code to send the request:
```js
function addImage(){}
const searchedForText = 'hippos';
const unsplashRequest = new XMLHttpRequest();

unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
unsplashRequest.setRequestHeader('Authorization', 'Client-ID <your-client-id>');
unsplashRequest.onload = addImage;

unsplashRequest.send()
```

## Setting a Request Header
- Now we need to set the request header before we send to unsplash
```js
const searchedForText = 'hippos';
const unsplashRequest = new XMLHttpRequest();

unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
unsplashRequest.onload = addImage;
unsplashRequest.setRequestHeader('Authorization', 'Client-ID <your-client-id>');
unsplashRequest.send();

function addImage(){
}
```  
  
- We also want to send to NY Times for articles 
- It doesn't require a header so we don't have to set one
```js
function addArticles () {}
const articleRequest = new XMLHttpRequest();
articleRequest.onload = addArticles;
articleRequest.open('GET', `http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=<your-API-key-goes-here>`);
articleRequest.send();
```

## Project Final Walkthrough
- Now we have all the code to send to Unsplash and NY Times
- My working code:
```js
<script>
window.onload = function () {        
    const form = document.querySelector('#search-form');
    const searchField = document.querySelector('#search-keyword');
    let searchedForText;
    const responseContainer = document.querySelector('#response-container');

    form.addEventListener('submit', function (e) {
        e.preventDefault();
        responseContainer.innerHTML = '';
        searchedForText = searchField.value;

        var unsplashRequest = new XMLHttpRequest();
        unsplashRequest.onload = addImage;
        unsplashRequest.onerror = function (err) { console.log(err) };
        unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
        unsplashRequest.setRequestHeader('Authorization', 'Client-ID bf0e9ebe48cbd01b383cbbb18b6f3d49d1e44613ec1ff1db48b399144e9a4239');
        unsplashRequest.send();

        function addImage() {
            let htmlContent = '';
            const data = JSON.parse(this.responseText);
            const firstImage = data.results[0];
            
            htmlContent = `<figure>
                <img src="${firstImage.urls.regular}" alt= "${searchedForText}">
                <figcaption>By <a href=${firstImage.user.links.html}>${firstImage.user.name}</a> / <a href="https://unsplash.com/">Unsplash</a></figcaption>
            </figure>`
            responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
        }

        var nytRequest = new XMLHttpRequest();
        nytRequest.onload = addArticles;
        nytRequest.onerror = function (err) { console.log(err) };
        nytRequest.open('GET', `http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=62441442c2604cc69e2a2c6f8b100ac4`)
        nytRequest.send();

        function addArticles() {
            let htmlContent = '';
            const data = JSON.parse(this.responseText);

            if (data.response && data.response.docs && data.response.docs.length > 1) {
                htmlContent = '<ul>' + data.response.docs.map(article => `<li class="article">
                    <h2><a href="${article.web_url}">${article.headline.main}</a></h2>
                    <p>${article.snippet}</p>
                    </li>`
                ).join('') + '</ul>';
            } else {
                htmlContent = '<div class="error-no-articles">No articles available</div>';
            }

            responseContainer.insertAdjacentHTML('beforeend', htmlContent);
        }
    })
};
</script>
```

## XHR Recap
### To Send An Async Request
- create an XHR object with the XMLHttpRequest constructor function
- use the .open() method - set the HTTP method and the URL of the resource to be fetched
- set the .onload property - set this to a function that will run upon a successful fetch
- set the .onerror property - set this to a function that will run when an error occurs
- use the .send() method - send the request
  
### To Use The Response
- use the .responseText property - holds the text of the async request's response

## XHR Outro
- You do have to write all that code if you want to do an XHR request
- But let's check out jQuery to see how they do it
  
- My personal code for XHR request:
```js
(function () {        
    const form = document.querySelector('#search-form');
    const searchField = document.querySelector('#search-keyword');
    let searchedForText;
    const responseContainer = document.querySelector('#response-container');

    form.addEventListener('submit', function (e) {
        e.preventDefault();
        responseContainer.innerHTML = '';
        searchedForText = searchField.value;
        
        // Add image function and xhr request
        function addImage() {
            let htmlContent = '';
            const data = JSON.parse(this.responseText);
            const firstImage = data.results[0];
            
            htmlContent = `<figure>
                <img src="${firstImage.urls.regular}" alt= "${searchedForText}">
                <figcaption>By <a href=${firstImage.user.links.html}>${firstImage.user.name}</a> / <a href="https://unsplash.com/">Unsplash</a></figcaption>
            </figure>`

            responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
        } // end of addImage()

        var unsplashRequest = new XMLHttpRequest();
        unsplashRequest.onload = addImage;
        unsplashRequest.onerror = function (err) { console.log(err) };
        unsplashRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
        unsplashRequest.setRequestHeader('Authorization', 'Client-ID bf0e9ebe48cbd01b383cbbb18b6f3d49d1e44613ec1ff1db48b399144e9a4239');
        unsplashRequest.send();

        // Add articles function and xhr request
        function addArticles() {
            let htmlContent = '';
            const data = JSON.parse(this.responseText);

            if (data.response && data.response.docs && data.response.docs.length > 1) {
                htmlContent = '<ul>' + data.response.docs.map(article => `<li class="article">
                    <h2><a href="${article.web_url}">${article.headline.main}</a></h2>
                    <p>${article.snippet}</p>
                    </li>`
                ).join('') + '</ul>';
            } else {
                htmlContent = '<div class="error-no-articles">No articles available</div>';
            }

            responseContainer.insertAdjacentHTML('beforeend', htmlContent);
        } // end of addArticles()

        var nytRequest = new XMLHttpRequest();
        nytRequest.onload = addArticles;
        nytRequest.onerror = function (err) { console.log(err) };
        nytRequest.open('GET', `http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=62441442c2604cc69e2a2c6f8b100ac4`)
        nytRequest.send();
    }) // end of form.addEventListener
})(); // end of function
```

# Lesson 2: Ajax with jQuery ✔
## The jQuery Library & Ajax
- jQuery is a pre-built system to make async requests behind the scenes
- You can download the current version or just use the CDN in your software
- jQuery was introduced to because browsers were not standardized on functionality yet
- Now that they have, jQuery is not need but does offer the ajax() method which is very powerful

## jQuery's `ajax()` Method
- This is the main method for the jQuery library to make async requests
- You can call by 2 ways:
  - `$.ajax(<url-to-fetch>, <a-configuration-object>);`
  - `$.ajax(<just a configuration object>);`
  
- The most common way is to use the configuration object
- So you can pass in the object via variable or directly into the ajax method
- It is basically a javascript object
```js
var settings = {
   frosting: 'buttercream',
   colors: ['orange', 'blue'],
   layers: 2,
   isRound: true
};
```
  
- So try it with jquery.com's website
```js
$.ajax({
    url: 'http://swapi.co/api/people/1/'
});
```

### Handling The Returned Data
- AJAX handles the response with the .done() function
```js
function handleResponse(data) {
    console.log('the ajax request has finished!');
    console.log(data);
}

$.ajax({
    url: 'https://swapi.co/api/people/1/'
}).done(handleResponse);
```

### The XHR method conversion to jQuery
- Before we had the XHR method:
```js
const imgRequest = new XMLHttpRequest();
imgRequest.onload = addImage;
imgRequest.open('GET', `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`);
imgRequest.setRequestHeader('Authorization', 'Client-ID <your-client-id-here>');
imgRequest.send();
```
  
- But now the jQUery method is much quicker:
```js
$.ajax({
    url: `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`
}).done(addImage);
```
  
- With the jQuery code:
  - we do not need to create an XHR object
  - instead of specifying that the request is a GET request, it defaults to that and we just provide the URL of the resource we're requesting
  - instead of setting onload, we use the .done() method
  
### Adding authorization and API keys
- We can use the built-in .header() method in jQuery
```js
$.ajax({
  url: `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`
  headers: {
    Authorization: 'Client-ID 123abc456def'
  }
}).done(addImage);
```

## Clean up the Success Callback
- jQuery automatically returns JavaScript if the respone is a JSON file
- There for we need to clean up our code to remove some things
- Old code:
```js
function addImage() {
    const data = JSON.parse(this.responseText);
    const firstImage = data.results[0];

    responseContainer.insertAdjacentHTML('afterbegin', `<figure>
            <img src="${firstImage.urls.small}" alt="${searchedForText}">
            <figcaption>${searchedForText} by ${firstImage.user.name}</figcaption>
        </figure>`
    );
}
```
  
- New code:
```js
function addImage(images) {
    const firstImage = images.results[0];

    responseContainer.insertAdjacentHTML('afterbegin', `<figure>
            <img src="${firstImage.urls.small}" alt="${searchedForText}">
            <figcaption>${searchedForText} by ${firstImage.user.name}</figcaption>
        </figure>`
    );
}
```

### The elements that have changed
- the function now has one parameter: images
- this parameter has already been converted from JSON to a JavaScript object, so * the line that had JSON.parse() is no longer needed.
- the firstImage variable is set to the images.results first item

### My conversion of the NY Times code to AJAX
- Old Code:
```js
function addArticles () {}
  const data = JSON.parse(this.responseText);
  const firstArticle = data.results[0];

  responseContainer.insertAdjacentHTML('afterbegin', `<figure>
          <img src="${firstArticle.urls.small}" alt="${searchedForText}">
          <figcaption>${searchedForText} by ${firstArticle.user.name}</figcaption>
      </figure>`
  );
```
  
- New Code:
```js
function addArticles(articles) {
  const firstArticle = articles.results[0];

  responseContainer.insertAdjacentHTML('afterbegin', `<figure>
      <img src="${firstArticle.urls.small}" alt="${searchedForText}">
      <figcaption>${searchedForText} by ${firstArticle.user.name}</figcaption>
  </figure>`
  );
}  
```

## Code Walkthrough
- Looking at the complete code

## Peek inside $.ajax()
- We look into the source code of jQuery's AJAX method to see how it forms the XHR call
- use the debugger in chrome helps also
- See 
  - [Pause Your Code With Breakpoints](https://developers.google.com/web/tools/chrome-devtools/javascript/breakpoints)
  - [JavaScript Debugging Reference](https://developers.google.com/web/tools/chrome-devtools/javascript/reference)

## Review the Call Stack
- You can use the chrome dev tools to go through the call stack
- It checks the calls starting at he bottom of the stack first

## Walkthrough of .ajaxTransport
- Under the hood, jQuery's ajax method does all of these things for us
  - creates a new XHR object each time it's called
  - sets all of the XHR properties and methods
  - sends the XHR request

## jQuery's Other Async Methods
- jQuery also has some convenience methods but not recommended that we use them
- There are:
  - `.get()`
  - `.getJSON()`
  - `.getScript()`
  - `.post()`
  - `.load()`

## Async with jQuery Outro
- Now that we looked at jQuery's AJAX method, there's another one
- There's fetch API which is even more powerful
  
- My personal code for ajax request:
```js
(function () {
    const form = document.querySelector('#search-form');
    const searchField = document.querySelector('#search-keyword');
    let searchedForText;
    const responseContainer = document.querySelector('#response-container');

    form.addEventListener('submit', function (e) {
        e.preventDefault();
        responseContainer.innerHTML = '';
        searchedForText = searchField.value;

        // Add image function and ajax request
        function addImage(data) {
            let htmlContent = '';
            const firstImage = data.results[0];

            responseContainer.insertAdjacentHTML('afterbegin', `<figure>
                <img src="${firstImage.urls.small}" alt= "${searchedForText}">
                <figcaption>By <a href=${firstImage.user.links.html}>${firstImage.user.name}</a> / <a href="https://unsplash.com/">Unsplash</a></figcaption>
            </figure>`
            );
        } // end of addImage()

        $.ajax({
            url: `https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`,
            headers: {
                Authorization: 'Client-ID bf0e9ebe48cbd01b383cbbb18b6f3d49d1e44613ec1ff1db48b399144e9a4239'
            }
        }).done(addImage)
        .fail(function (err) {
            requestError(err, 'image');
        }); // end of add image ajax request

        // Add articles function and ajax request
        function addArticles(data) {
            let htmlContent = '';

            if (data.response && data.response.docs && data.response.docs.length > 1) {
                htmlContent = '<ul>' + data.response.docs.map(article => `<li class="article">
                    <h2><a href="${article.web_url}">${article.headline.main}</a></h2>
                    <p>${article.snippet}</p>
                    </li>`
                ).join('') + '</ul>';
            } else {
                htmlContent = '<div class="error-no-articles">No articles available</div>';
            }

            responseContainer.insertAdjacentHTML('beforeend', htmlContent);
        } // end of addArticles()

        $.ajax({
            url: `http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=62441442c2604cc69e2a2c6f8b100ac4`
        }).done(addArticles)
        .fail(function (err) {
            requestError(err, 'image');
        }); // end of add articles ajax request
    }); // end of form.addEventListener
})(); // end of function()
```

# Lesson 3: Ajax With Fetch ✔
## Ajax call with the Fetch API
- New technology we are looking at in this lesson
- It's provided right in the browser
  
## What is Fetch
- Fetch is promised-based and works on most browsers
- Check [CanIUse](http://caniuse.com/#feat=fetch) to see if you're supported
- Otherwise use this [polyfill](https://github.com/github/fetch)

## Write the Fetch Request
- A simple fetch request that returns a promise
```js
fetch('<URL-to-the-resource-that-is-being-requested>');

fetch('https://api.unsplash.com/search/photos?page=1&query=flowers');
```
- It still obeys Cross-Origin Rules so you need to add authorizations
- Documents to read:
- https://developer.mozilla.org/en-US/docs/Web/API/GlobalFetch/fetch
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
  
- The two ways to add authorization if you read the articles are:
```js
fetch(https://api.unsplash.com/search/photos?page=1&query=${searchedForText}, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
})
```
```js
const requestHeaders = new Headers();
requestHeaders.append('Authorization', 'Client-ID abc123');
fetch(https://api.unsplash.com/search/photos?page=1&query=${searchedForText}, {
    headers: requestHeaders
})
```
  
### Default Method for Fetch
- The default method for Fetch is GET, see:
- https://fetch.spec.whatwg.org/#methods
- https://fetch.spec.whatwg.org/#requests
  
### Changing the HTTP Method
- You can change it easily but remember to keep it CAPITALIZED
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    method: 'POST'
});
```

## Handle the Response
- Fetch returns a promise so then all you have to do is call .then() on that Promise
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    debugger; // work with the returned response
})
```

## The Response Object
- The Response object is new with Fetch API
- However, it doesn't actually have any of the data, you need to go in teh body
- The Unsplash API returns a JSON to us, so we call .json() on the response variable:
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    return response.json();
});
```
  
- Problem is that returns another promise so we need to chain another .then()
- This is how to actually get and start using the returned data
- We are calling addImage on the return data
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(function(response) {
    return response.json();
}).then(addImage);

function addImage(data) {
    debugger;
}
```
  
- Like how .json() method converts the response to JSON, we use .blob() to fetch an image

## ES6 Arrow Function
- We can minimize our code further using the Arrow function
```js
// without the arrow function
}).then(function(response) {
    return response.json();
})

// using the arrow function
}).then(response => response.json())
```
  
- So our complete request is:
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(response => response.json())
.then(addImage);

function addImage(data) {
    debugger;
}
```

## Display Content & Handling Errors
- Now we have actual JSON data, so let's add an image and caption
```js
function addImage(data) {
    let htmlContent = '';
    const firstImage = data.results[0];

    if (firstImage) {
        htmlContent = `<figure>
            <img src="${firstImage.urls.small}" alt="${searchedForText}">
            <figcaption>${searchedForText} by ${firstImage.user.name}</figcaption>
        </figure>`;
    } else {
        htmlContent = 'Unfortunately, no image was returned for your search.'
    }

    responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
}
```
  
- This code will:
    - get the first image that's returned from Unsplash
    - create a <figure> tag with the small image
    - creates a <figcaption> that displays the text that was searched for along with the first name of the person that took the image
    - if no images were returned, it displays an error message to the user

### Handling Errors
- There might be possible errors with our request
- The .catch() is method available from the Promise API
1. Unsplash not having an image for the searched term
    - We handled this in the addImage function
2. Issues with the network
    - Use the .catch() method
3. Issues with teh fetch request
    - Use the .catch() method
  
- So let's add a .catch() method to handle errors:
```js
fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
    headers: {
        Authorization: 'Client-ID abc123'
    }
}).then(response => response.json())
.then(addImage)
.catch(e => requestError(e, 'image'));

function addImage(data) {
    debugger;
}

function requestError(e, part) {
    console.log(e);
    responseContainer.insertAdjacentHTML('beforeend', `<p class="network-warning">Oh no! There was an error making a request for the ${part}.</p>`);
}
```
- This code adds a .catch() on the end of the promise chain
- Adds requestError function
- Receives error object and stores it in e variable
- It will input e into the requestError function if something fails
- This will log the error and display as a warning message to the user!

## Project Wrap-up
- We now finished our requests using the fetch API

## Fetch Outro
- Fetch is a great way to get asynchronous requests
- Using custom headers and caching is possible also
- It's promise based
  
- My personal code for the fetch request:
```js
(function () {
    const form = document.querySelector('#search-form');
    const searchField = document.querySelector('#search-keyword');
    let searchedForText;
    const responseContainer = document.querySelector('#response-container');

    form.addEventListener('submit', function (e) {
        e.preventDefault();
        responseContainer.innerHTML = '';
        searchedForText = searchField.value;

        // Error handling
        function requestError(e, part) {
            console.log(e);
            responseContainer.insertAdjacentHTML('beforeend', `<p class="network-warning">Oh no! There was an error making a request for the ${part}.</p>`);
        }

        // Add image function and fetch request
        function addImage(data) {
            let htmlContent = '';
            const firstImage = data.results[0];
            
            if (firstImage) {
                htmlContent = `<figure>
                    <img src="${firstImage.urls.small}" alt= "${searchedForText}">
                    <figcaption>By <a href=${firstImage.user.links.html}>${firstImage.user.name}</a> / <a href="https://unsplash.com/">Unsplash</a></figcaption>
                </figure>`;
            } else {
                htmlContent = 'Unfortunately, no image was returned for your search.'
            }
            
            responseContainer.insertAdjacentHTML('afterbegin', htmlContent);
        } // end of addImage()

        fetch(`https://api.unsplash.com/search/photos?page=1&query=${searchedForText}`, {
            headers: {
                Authorization: 'Client-ID bf0e9ebe48cbd01b383cbbb18b6f3d49d1e44613ec1ff1db48b399144e9a4239'
            }
        })
        .then(response => response.json())
        .then(addImage)
        .catch(err => requestError(err, 'image')); // end of add image fetch 

        // Add articles function and fetch request
        function addArticles(data) {
            let htmlContent = '';

            if (data.response && data.response.docs && data.response.docs.length > 1) {
                htmlContent = '<ul>' + data.response.docs.map(article => `<li class="article">
                    <h2><a href="${article.web_url}">${article.headline.main}</a></h2>
                    <p>${article.snippet}</p>
                    </li>`
                ).join('') + '</ul>';
            } else {
                htmlContent = '<div class="error-no-articles">No articles available</div>';
            }

            responseContainer.insertAdjacentHTML('beforeend', htmlContent);
        } // end of addArticles()

        fetch(`http://api.nytimes.com/svc/search/v2/articlesearch.json?q=${searchedForText}&api-key=62441442c2604cc69e2a2c6f8b100ac4`)
        .then(response => response.json())
        .then(addArticles)
        .catch(err => requestError(err, 'image')); // end of add articles fetch
    });
})();
```
## Course Outro
- Now you have XHR, jQuery, and fetch in your toolbox