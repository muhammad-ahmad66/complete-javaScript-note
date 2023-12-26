# Asynchronous Javascript, AJAX and APIs

```js
'use strict';

const btn = document.querySelector('.btn-country');
const countriesContainer = document.querySelector('.countries');
```

1. [Asynchronous_Javascript_AJAX_And_API_Intro](#asynchronous_javascript_ajax_and_api_intro)
2. [First_AJAX_Call-XML_HTTP_Request](#first_ajax_call-xml_http_request)
3. [Request_And_Response_On_Web](#request_and_response_on_web)
4. [Sequence_of_AJAX_Calls](#sequence_of_ajax_calls)
5. [PROMISES_AND_FETCH_API](#promises_and_fetch_api)
6. [CONSUMING_PROMISES](#consuming_promises)
7. [Chain_Promises](#chain_promises)
8. [ERROR_HANDLING](#error_handling)
9. [THROWING_ERRORS_MANUALLY](#throwing_errors_manually)
10. [Asynchronous_Behind_The_Scene_The_Event_Loop](#asynchronous_behind_the_scene_the_event_loop)

---

## Asynchronous_Javascript_AJAX_And_API_Intro

**What synchronous code is?**  
**Synchronous** simply means that the code is executed line by line, in the exact order of execution. which is normal way that we are doing so far.  
This can create problems, when one line of code takes a long time to run. Like if we use alert function, then entire code will block until we click OK button on alert window. next lines will executed after clicking OK.

**Timer is an asynchronous javascript.** **setTimeout()**. In setTimeout() the time counter will run in background, while the execution flow to the next line, without waiting for timer and after completed timer counting the call-back function will execute.

In **timer this callback** function is asynchronous code. **But** understand that all callback functions are not asynchronous code.

```js
.addEventListener('load', function(){...}) ;
```

⤴ This callback function is also asynchronous code.  
**But Remember:** Like callback function alone do not make code asynchronous also eventListener alone do not...

**Why click event synchronous and load event is an asynchronous?**  
If we add a **click event** then it will do nothing in background, it simply wait for a click. while load event will load image in background.  
There are many examples of asynchronous javascript, like **geolocation API** or **Ajax calls** etc.  
**AJAX Calls** are most important use case of asynchronous javascript.

**WHAT ARE AJAX CALLS??**  
**AJAX stand of Asynchronous Javascript And XML.** It allows us to communicate with remote web servers in an asynchronous way. With AJAX calls, we can request data from web server dynamically.  
**We make AJAX calls in our code in order to request some data from a web server dynamically, without reloading the page.**

**HOW AJAX WORKS??**  
**Lets take an example:**  
We have a javascript application running in the browser(client), and we want to get some data from a web server, lets say data about countries. With **AJAX** we can do an **HTTP request** to the server which has this data, and then server will send back a **response** containing that data that we requested. And this back and forth between client(browser) and server all happens asynchronously in the background. And there can even be different types fo requests, **get requests** to receive data and **post request** to send data to a server.  
When we are asking a server to send us some data, this server usually contains an **web API**. and this API is one that has the data that we required.

**WHAT IS AN API (Application Programming Interface)?**  
**Application Programming Interface:** Piece of software that can be used by another piece of software, in order to allow applications to talk each other.  
In web development there are countless types of APIs. DOM, Geolocation ....

**Important types of API**, when we use AJAX.

1. **Online API**
   Online API is essentially an application running on web server, which receives requests for data, then retrieves(get-back) this data from some database, then sends it back to the client.  
   **This 'online API' are called 'web API' or just 'API'.** We will use APIs all the time

[SEE PDF FILE](./theory-lectures-v2.pdf)

**API Data Format:**  
As **AJAX** stands for Asynchronous Javascript and XML. so here **XML** is a data format, which is widely used to transmit data on the web, However basically these days no API uses XML data anymore. [the term AJAX is just and old name]. Instead, most APIs these days use the **JSON** data format.

**JSON Data Format:**  
**JSON** is most popular data format today, because it's basically just a javascript object but converted to a string.

---

## First_AJAX_Call-XML_HTTP_Request

Lets make our first API call  
We built a UI component which will have contains data about certain country, and this data about the countries actually comes from a third party online API.  
In javascript there are multiple ways of doing AJAX calls.

### Most old school one: [XML http request]

**Steps to use XMLHttpRequest() function**:

- Call this function and store it into a variable.
- Need URL to which we'll make the AJAX call. for that we use object.open() here we pass two parameter.
  1. request type in string
  2. url of API in string

Remember for search any public API, <https://github.com/public-apis/public-apis> GO HERE on github. Here we using '**REST Countries API**'  
Send request to that url by simply using object.send() function. (here object means the we made by using new keyword.) here the API will fetched asynchronously.  
Register a callback function on the request object for the load event.

```js
const getCountryData = function (country) {
  const request = new XMLHttpRequest();

  // request.open('GET', 'https://restcountries.com/v3.1/name/pakistan');
  // Instead of hard code:
  request.open('GET', `https://restcountries.com/v3.1/name/${country}`);

  request.send();

  request.addEventListener('load', function () {
    console.log(this.responseText); // Remember here this keyword is a request.
    // This responseText property only gonna be set once the data has actually arrived.
    // Here in console we have bunch of texts about Pakistan in a JSON format.
    // Now we have to convert this into javascript object.

    const data = JSON.parse(this.responseText);
    console.log(data); // CONVERTED!!! // HERE IT'S AN ARRAY CONTAINING ONE OBJECT. so destructure that.
    const [data] = JSON.parse(this.responseText);
    console.log(data); // now its one object

    const html = `
          <article class="country">
            <img class="country__img" src="${data.flags.png}" />
            <div class="country__data">
              <h3 class="country__name">${data.name.common}</h3>
              <h4 class="country__region">${data.region}</h4>
              <p class="country__row"><span>👫</span>${(
                +data.population / 1000000
              ).toFixed(1)}</p>
              <p class="country__row"><span>🗣️</span>${data.languages.urd}</p>
              <p class="country__row"><span>💰</span>${
                data.currencies.PKR?.name
              }</p>
            </div>
          </article>
        `;

    countriesContainer.insertAdjacentHTML('beforeend', html);
    countriesContainer.style.opacity = 1;
    // THAT'S AMAZING
  });
};

getCountryData('pakistan');
getCountryData('usa');
getCountryData('germany');
```

**REMEMBER** Here always display in order of the data arrives first. Fist arrives will display first on page. No matter we call it last or first. That's due to ajax calls.

**If we want to this requests in a specific way what we have to do???**  
Here we need to chain the request, second only after finishing first and so on.....this is a topic that we will discuss after next lecture.

### QUICK RECAP

<https://github.com/public-apis/public-apis> [link of public API]

**XML HTTP Request function:**  
AJAX calls using **XMLHttpRequest()** function: [Old school way of doing AJAX]

#### STEPS IN XMLHttpRequest

0. **Create a request Object using New operator:**

   ```js
   const request = new XMLHttpRequest();
   ```

1. **Open the Request:**

   ```js
   request.open('GET', `https://restcountries.com/v3.1/name/${countryName}`);
   ```

2. **Sent the Request:**

   ```js
   request.send();
   ```

3. **Register a load event on request object:**

   ```js
   request.addEventListener('load', function () {});
   ```

4. **Convert JSON to JavaScript Object**

   ```js
   request.addEventListener('load', function () {});
   const [data] = JSON.parse(this.responseText);
   ```

---

## Request_And_Response_On_Web

**REMEMBER:** Whenever we try to access a web server, the browser(also called client) sends a request to the server, and the server will then send back a response. And that response contains the data or web page that we requested. This whole process is called **'Request-Response Model'** OR **'Client-Server Architecture'**.

### Dive Deeper into this Process

Every URL gets an **HTTP** or **HTTPS**,(protocol use to connection) then domain name, and then after a slash we have so-called **resource**, that we want to access.  
This domain name is not actual real address of the server that we're trying to access, (it's just a nice name).  
Which means we need a way of converting the domain name to the real address of the server. that's the work of **DNS**.

**DNS** is a special kind of server. like a phone books of the internet.

So the first step that happens when we access any web server, the browser makes a request to a **DNS** and it will simply match the web address of the url to the server's real IP address. - All of these happens through **Internet Service Provider(ISP)**.

Then DNS will send back IP address to the browser, we can finally call it.  
Looks like: <https://10.32.232.89.443>  
First part: **https protocol**  
Second part(10...89): **IP address**  
Third part(443): **Port number.** Port number is a specific service that running on a server. like a sub-address.

**Once we have real IP address a TCP/IP socket connection is established between the browser and the server.**

**TCP** is the **T**ransmission **C**ontrol **P**rotocol and **IP** is the **I**nternet **P**rotocol. they are the one who set the rules about how data moves on the internet.

After establishing a connection it's time to make the request. the request means http request, where **HTTP stands for HyperText Transfer Protocol.**  
It is yet another communication protocol. **In the case of HTTP**, it's just a protocol that allows clients and web servers to communicate, that works by sending requests and response messages from client to server and back.

#### Request message will look something like this

GET /rest/v2/PT HTTP/1.1  
Host: <www.google.com>  
user-agent: .....  
language: ....

Beginning of the message is called start line, and this one contains the HTTP method that is used in the request. some HTTP methods are '**get**', '**post**', '**put**', '**patch**'

rest/v2/PT is a target in http request.  
After that there are some information about request called header. language etc.  
There also a request body. that **body** contains the data we are sending.

Now it hit server. Then server will work on it. Once it ready it will send back using http response, http response message is quite similar to the request, with a **start line**, **headers** and **body**.

It will tell about weather the request has been successful or failed. for example in standard **200** means successful and **404** means page not found.

**TCP/IP**  
**Job of TCP is to break the requests and responses down into thousands of small chunks, called packets before they are sent.** Once the final packet arrives at final destination, TCP will reassemble all the packets into the original request or response.

**Job of the IP protocol is to actually send and route these packets through the Internet**, It ensures that they arrives at the destination using IP addresses on each packet.

---

## Sequence_of_AJAX_Calls

```js
const renderCountry = function (data, className = '') {
  const html = `<article class="country ${className}">
    <img class="country__img" src="${data.flags.png}" />
    <div class="country__data">
        <h3 class="country__name">${data.name.common}</h3>
        <h4 class="country__region">${data.region}</h4>
        <p class="country__row"><span>👫</span>${(
          +data.population / 10000000
        ).toFixed(1)} people</p>
        <p class="country__row"><span>🗣️</span>${data.languages.urd}</p>
        <p class="country__row"><span>💰</span>${data.currencies.PKR.name}</p>
    </div>
    </article>`;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;
};

const getCountryAndNeighbor = function (countryName) {
  // !AJAX CALL COUNTRY 1:
  const request = new XMLHttpRequest();
  request.open('GET', `https://restcountries.com/v3.1/name/${countryName}`);
  request.send();

  request.addEventListener('load', function () {
    const [data] = JSON.parse(this.responseText);
    console.log(data);

    // RENDER COUNTRY 1:
    renderCountry(data);

    // NOW GET NEIGHBOR COUNTRY:
    const neighbor = data.borders[0];

    // If any country have no neighbor at all:
    if (!neighbor) return;

    // SECOND AJAX CALL FOR COUNTRY 2
    const request2 = new XMLHttpRequest();
    request2.open('GET', `https://restcountries.com/v3.1/alpha/${neighbor}`);
    request2.send();

    request2.addEventListener('load', function () {
      //   console.log(request2.responseText);
      //   console.log(this.responseText);
      const [data2] = JSON.parse(this.responseText);
      console.log(data2);
      //   renderCountry(data2, 'neighbor');
      const html = `<article class="country neighbour">
    <img class="country__img" src="${data2.flags.png}" />
    <div class="country__data">
        <h3 class="country__name">${data2.name.common}</h3>
        <h4 class="country__region">${data2.region}</h4>
        <p class="country__row"><span>👫</span>${(
          +data2.population / 10000000
        ).toFixed(1)} people</p>
        <p class="country__row"><span>🗣️</span>${data2.languages.pus}</p>
        <p class="country__row"><span>💰</span>${data2.currencies.AFN.name}</p>
    </div>
    </article>`;
      countriesContainer.insertAdjacentHTML('beforeend', html);
      countriesContainer.style.opacity = 1;
    });
  });
};

getCountryAndNeighbor('pakistan');
```

In this API we see there is a property of borders for each country. like pakistan have india, china, afghanistan etc.  
Using this property we can also render the neighboring countries, besides the original country. It means **second AJAX call really depends on the first** one. To implement **AJAX calls** should be in sequence, without completing first one we will never get border property of it. so we need order here.  
Here we have one **AJAX call that depends on another call**. One callback function then inside of that we have yet another one. In other words **We have nested callbacks.**

**Now how we do if we want to do more requests in sequence..?** like neighbor of neighbor and more....  
In these case we will end up callbacks inside of callbacks, inside of callbacks.... for That kind of structure and behavior we have a special name called **'Callback Hell'**  
**Callback Hell** is when we have lot of nested callbacks. in order to execute asynchronous tasks in sequence. And In fact, this happens for all asynchronous tasks not just AJAX calls. for example:⤵

```js
setTimeout(() => {
  console.log('1 Second Passed!');
  // new timer
  setTimeout(() => {
    console.log('2 seconds passed!');

    // another one
    setTimeout(() => {
      console.log('3 seconds passed!');
      setTimeout(() => {
        console.log('4 seconds passed!');
      }, 1000);
    }, 1000);
  }, 1000);
}, 1000);
```

Problem with **callback hell** is that it makes code look very messy, had to maintain and very difficult to understand.  
Solution of **callback hell** is simply escape **callback hell**. Instead we use **Promises**.

```js
const renderCountry = function (data, className = '') {
const html = `<article class="country ${className}">
            <img class="country__img" src="${data.flags.png}" />
            <div class="country__data">
            <h3 class="country__name">${data.name.common}</h3>
            <h4 class="country__region">${data.region}</h4>
            <p class="country__row"><span>👫</span>${(+data.population / 1000000).toFixed(1)}</p>
            <p class="country__row"><span>🗣️</span>${data.languages.eng}</p>
            <p class="country__row"><span>💰</span>${data.currencies.PKR?.name}</p>
            </div>
        </article>`;

    countriesContainer.insertAdjacentHTML('beforeend', html)

    countriesContainer.style.opacity = 1;
}

const getCountryAndNeighbor = function (country) {

    // AJAX call country 1
    const request = new XMLHttpRequest();

    request.open('GET', `https://restcountries.com/v3.1/name/${country}`);

    request.send()

    request.addEventListener('load', function () {

        const [data] = JSON.parse(this.responseText);
        console.log(data);

        // RenderCountry 1
        renderCountry(data);


        // Get Neighbor Country 2:
        const neighbor = data.borders[1];
        // console.log(neighbor); // CHN

        some countries have no border at all like island, so we check
        if (!neighbor) return;

        // AJAX call country 2
        const request2 = new XMLHttpRequest();
        //request.open('GET', `https://restcountries.com/v3.1/name/${country}`); // here we not don't have a country name only we have a country code (CHN  -NOT CHINA)

        // IN THIS API we can also search any country using country code.
        request2.open('GET', `https://restcountries.com/v3.1/alpha/${neighbour}`)

        request2.send();

        // event listener function
       // this event listener is in inside an event listener function, so there is no way to execute this first.
        request2.addEventListener('load', function () {
            // console.log(this.responseText);

            const [data2] = JSON.parse(this.responseText);
            console.log(data2);

            renderCountry(data2, 'neighbor');
        })
    })
}

getCountryAndNeighbor('pakistan');

```

---

## REMEMBER SOME ABBREVIATIONS

**AJAX:** Asynchronous JavaScript And XML  
**API:** Application Programming Interface  
**DNS:** Domain Name System/Server/Service  
**JSON:** JavaScript Object Notation

---

## PROMISES_AND_FETCH_API

[ES6_Feature]

Before starting promises we should know about **Fetch API**  
Replace old **XMLHttpRequest()** function with modern way of making **AJAX calls**. that is by using **Fetch API**.

DONE USING XMLHttpRequest()

```js
const request = new XMLHttpRequest();
request.open('GET', `https://restcountries.com/v3.1/name/${country}`);
request.send();

// Fetch API
const request = fetch('<https://restcountries.com/v3.1/name/pakistan>');
console.log(request); // returned promise here.
```

There are some more arguments we can specify in fetch function -object of options. But to make a **simple GET request** all we need to pass tha URL.

### Now what exactly promise is? And what can we do with it?

**Promise** **is an object that is used as a placeholder for the future result of an asynchronous operation.** we can also say **Promise is a container for an asynchronously delivered value**. OR **promise is a container for a future value.**  
Perfect example of future value is value coming from an AJAX call. When we call ajax call there is no value yet.

**What's the big advantages of promises?**

1. By using promises, **we no longer need to rely on events and callback function to handle asynchronous result**.
2. We can **chain promises to a sequence of asynchronous operations instead callbacks** (escape callback hell)

### The Promise Lifecycle

**Pending promise:** This is before any value resulting from the asynchronous task.  
**Settled promise:** When the task finally finished, we say promise is settled. There are two different types of settled promises '**fulfilled**' and '**rejected**' promises.  
**A promise is only settled once**. **so promise will either fulfilled or rejected**, it's impossible to change their states.

**Consume a Promise:** Get a result of promise. fetch API build a promise and return us to consume.

---

## CONSUMING_PROMISES

```js
// CONSUMING PROMISE
const renderCountry = function (data, className = '') {
  const html = `

  <article class="country ${className}">
    <img class="country__img" src="${data.flag}" />
    <div class="country__data">
      <h3 class="country__name">${data.name}</h3>
      <h4 class="country__region">${data.region}</h4>
      <p class="country__row"><span>👫</span>${(
        +data.population / 1000000
      ).toFixed(1)} people</p>
      <p class="country__row"><span>🗣️</span>${data.languages[0].name}</p>
      <p class="country__row"><span>💰</span>${data.currencies[0].name}</p>
    </div>
  </article>
  `;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;
};

const renderError = function (msg) {
  countriesContainer.insertAdjacentText('beforeend', msg);
  countriesContainer.style.opacity = 1;
};

// CODE //
const getCountryData = function (country) {
  fetch(`https://restcountries.com/v3.1/name/${country}`)
    .then(function (response) {
      return response.json();
    })
    .then(function (data) {
      renderCountry(data[0]);
    });
};

getCountryData('portugal');
```

**LET'S ANALYZE THE CODE** ⤴  
fetch(`https://restcountries.com/v3.1/name/${country}`);  
**Calling a fetch function will return a promises**. So, we will use **then method to that promises**, **then method is available for all promises**. In that **then method** we will pass a callback function that we want to **execute** as soon as the **promise is fulfilled**. And that callback function will receive one argument that will be a **resulting value(response)** of **fulfilled promise**. And then to that response we will call a **json method**, which is available on all the responses.  
Now the problem is that, this **json method itself is also an asynchronous function**, it means it will also **return a new promise**, so we return that json method, and remember! that will return a new promise. and then we'll call another **then method** to previous then method, which is returning **AJAX** b/c of .json() method. In this callback function we'll do our main task.

### How to consuming Promises

We'll consume the promise that was returned by the fetch function.  
**fetch('url') method will return a promise.** In all promise we will call after that **then method**, into **then method** we need to pass a callback function that we want to execute as soon as the promise is actually fulfilled.  
**then function** will receive one argument and that argument is resulting value of fulfilled promise.  
Then inside the callback function on the response we need to call **json method**. json() method is available on each response.  
Here the Problem is that this **json function** is also an asynchronous function. it will also return a new promise. so we will return that **promise**. After that we have to call another **then method** to callback function after returning the json promise. In this callback function we'll do our main task. In this case render Country data dy calling renderCountry() function.

```js
const getCountryData = function (country) {
  fetch(`https://restcountries.com/v3.1/name/${country}`)
    .then(function (response) {
      // console.log(response);

      return response.json();
    })
    .then(function (data) {
      // console.log(data);  // now it's working.
      renderCountry(data[0]);
    });
};

getCountryData('pakistan');
```

---

## Chain_Promises

we will chain promises by rendering the neighbor country of the initial country.

```js
const getCountryData = function (country) {
  // Country 1
  fetch(`https://restcountries.com/v3.1/name/${country}`)
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      // console.log(data);
      renderCountry(data[0]);

      const neighbour = data[0].borders[1];
      // console.log(neighbour);
      if (!neighbour) return;

      // Country 2
      // v common mistake:
      // fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`).then(response => response.json()); //
      // Instead always return. like this ⤵.
      return fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`);
    })
    .then((response) => {
      return response.json();
    })
    .then((data) => renderCountry(data[0], 'neighbour'));
};

getCountryData('pakistan');
```

We can chain many promises without callback hell, Here Instead of callback hell we have a flat chain of promises

---

## ERROR_HANDLING

### Handling Rejected Promises

**Remember:** **A promise in which an error happens is a rejected promise**, so we learn how to handle rejected promise. The only way in which fetch promise rejects is when the user loses his internet connection. We'll handle only this error here.  
There are two ways to handling rejection:

1. **Pass a second callback function into the then method**. Remember we have already passed a first callback function into then method, which will be always for fulfilled promise. second callback function will be for error handling, **which will always be call with argument err itself**.
2. **Handling globally just in one central place. we will add at the end of the chain a catch method.** Here we will pass same callback function. this **catch method** will catch any error that occur in any place in the chain.

Beside **'then'** and **'catch'** methods there is one more methods that are available on all promises that is **'finally'** method. It will also take a callback, this callback always executes no matter if the promise is fulfilled or rejected.  
One good use case of **finally method is show and hide loading spinners like rotating circles**, while loading.  
**REMEMBER:** **then method** only called when promise is **fulfilled** and **catch method** only called when promise is **rejected**, while **finally** method will call in **both** cases.

---

#### 1. CODE FOR PASSING SECOND CALLBACK FUNCTION INTO THEN METHOD

```JS

const getCountryData = function (country) {
// COUNTRY 1
fetch(`https://restcountries.com/v3.1/name/${country}`)
.then(
function (response) {
return response.json();
},
function (err) {
return alert(err);
}
)
.then(function (data) {
renderCountry(data[0]);
const neighbor = data[0].borders[0];

      if (!neighbor) return;

      // COUNTRY 2
      return fetch(`https://restcountries.com/v3.1/alpha/${neighbor}`);
    })
    .then(
      function (response) {
        return response.json();
      },
      function (err) {
        return alert(err);
      }
    )
    .then(function (data) {
      // renderCountry(data, 'neighbour');
    });

};

btn.addEventListener('click', function () {
getCountryData('portugal');
});
```

---

#### 2. CODE WITH ALL OF THESE THINGS

- Consuming Promises
- Chaining Promises
- Handling Rejected Promises
- Throwing Errors Manually
- Implementing All Three Methods [then, catch, finally]

```JS

const renderError = function (msg) {
countriesContainer.insertAdjacentText('beforeend', msg);
// countriesContainer.style.opacity = 1; // Added in finally method
}

const getCountryData = function (country) {
// Country 1
fetch(`https://restcountries.com/v3.1/name/${country}`)
.then(response => {
console.log(response);

            if (!response.ok) {
                throw new Error(`Country not found (${response.status})`)
            } // error may occur in second method. neighbour.
            return response.json() // , err => alert(err)


        })
        .then(data => {
            renderCountry(data[0]);

            const neighbour = data[0].borders[1];
            if (!neighbour) return;

            // Country 2
            return fetch(`https://restcountries.com/v3.1/alpha/${neighbour}`);

        }).then(response => {
            if (!response.ok) {
                throw new Error(`Country not found (${response.status})`)
            }
            return response.json() //, err => alert(err)
        }).then(data => renderCountry(data[0], 'neighbour')).catch(err => {
            console.error(`${err} error🔴🔴🔴`);

            renderError(`Something went wrong 🔴 🔴 ${err.message}. Try again!`)
        }).finally(() => {
            countriesContainer.style.opacity = 1;
        });

}


btn.addEventListener('click', function () {
// getCountryData('pakistan');
});

```

## THROWING_ERRORS_MANUALLY

[CODE ⤴]

HERE ⤴ without using response.ok property in then method. getCountryData('kkkk'); giving a 404 error on console, it means API couldn't find any country by this name, BUT fetch function still not reject in this case. it is going to execute a then method, and from there it is printing an error other then 404 error. So we sill have to do it manually. this is all about next lecture.

Here we see in console the 'ok' property is false since it couldn't found that country name remember that if we find then ok property will true and status property should be 200 else ok: false and status: 404 so we use this property to print exact 404 error on page. throw will immediately terminate function like a return. and promise will reject and this rejection will propagate down to the catch handler. and will display this error.

countriesContainer.style.opacity = 1; // this code is common in both catch and then so we'll put it in finally method.

Remember err is an object in js it has methods like message...  
Also note that catch and then methods always return a promises.

---

FINAL CODE TILL NOW WITH SOME REFACTORING⤵  
we will build a helper functions for all main functionalities(fetch, error handling, conversion to JSON)

```js
const renderCountry = function (data, className = '') {
  const html = `

<article class="country ${className}">
<img class="country__img" src="${data.flags.png}" />
<div class="country__data">
<h3 class="country__name">${data.name.common}</h3>
    <h4 class="country__region">${data.region}</h4>
<p class="country__row"><span>👫</span>${(+data.population / 1000000).toFixed(
    1
  )}</p>
    <p class="country__row"><span>🗣️</span>${data.languages.eng}</p>
<p class="country__row"><span>💰</span>${data.currencies.PKR?.name}</p>
</div>

</article>
`;

  countriesContainer.insertAdjacentHTML('beforeend', html);
};

const renderError = function (errMsg) {
  countriesContainer.insertAdjacentText('beforeend', errMsg);
};

// Converting to JSON
const getJSON = function (url, errorMsg = 'Something went wrong') {
  return fetch(url).then((response) => {
    if (!response.ok) {
      throw new Error(`${errorMsg} (${response.status})`);
    }

    return response.json();
  });
};

const getCountryData = function (country) {
  getJSON(`https://restcountries.com/v3.1/name/${country}`, `Country not found`)
    .then((data) => {
      renderCountry(data[0]);

      const neighbour = data[0].borders[0];
      if (!neighbour) return;
      return getJSON(`https://restcountries.com/v3.1/alpha/${neighbour}`);
    })
    .then((response) => {
      if (!response.ok) {
        throw new Error(`Country not found. (${response.status})`);
      }

      return response.json();
    })
    .then((data) => {
      renderCountry(data[0], 'neighbour');
    })
    .catch((err) => {
      renderError(`Something went wrong 🔴 🔴 ${err.message}. Try again!`);
      console.error(`Something went wrong 🔴 🔴 ${err.message}`);
    })
    .finally(() => {
      countriesContainer.style.opacity = 1;
    });
};

getCountryData('pakistan');
```

---

## Asynchronous_Behind_The_Scene_The_Event_Loop

// !remember: javascript engine is built around the idea of a single threat. It means it can only do one thing at a time.
// ? if there is only one thead of execution in the engine then how can asynchronous code be executed in a non blocking way?
// fist all asynchronous codes will be added in WEB APIs section(js has three main parts call stack, web APIs, Callback & microtask queue). as any asynchronous code completes its execution it will add into callback queue.
// !callback queue is an ordered list of all callback functions that are in line to be executed. (it's just like a todo list which the call stack have to complete)
// ?EVENT LOOP:
// It looks into the call stack it's empty or not, except for the global context. If the stack is empty, which means there is no code running, then it will take the first callback from the callback queue and put it on the stack to be executed. and this is called an event loop tick.
// each time an event loop takes callback form the callback queue to stack we say there was event loop tick.
// So here we see an event loop do coordination between the call stack and the callback queue.
// !remember BUT for callbacks related to promises will not go into the callback queue. Callback which is coming from promise will not moved into the callback queue. Instead callback of promises have a special queue themselves, called microtasks queue.
// ?MICROTASK QUEUE:
// Microtask queue has priority over callback queue. So, at the end of an event loop tick(taking callback form callback queue), the event loop check if there are any callbacks in the microtasks queue. If there are run all of them, before any more callbacks from the regular callback queue.
// !microtask queue(promise's callback queue) has priority over regular callback queue.

// \*CODE PRACTICE:
// console.log('test start');
// setTimeout(() => {
// console.log('0 sec timer');
// }, 0);
// Promise.resolve('Resolved Promise 1').then(response => console.log(response));
// console.log('Test end');
// Promise.resolve will be a promise that will resolved(fulfilled) immediately. So, setTimeout and promise's callbacks both have same time to execute, they will both finished at exact same time and will move to queues(setTime -> callback queue & promise -> microtask queue). So, according to our knowledge which one will be executed first???
// As microtask queue has priority over callback queue there for the callback in microtask queue will execute first.
// ?Order of output will be:
// test start
// Test end
// Resolved Promise 1
// 0 sec timer

// Here we set time of setTimeout to 0 seconds. If any callback from microtask queue takes time greater then 0 seconds then setTimeout's callback function may take long time to run although we set to zero because the microtask callback will execute first.
// ?Lets verify it:
/\*
console.log('test start');
setTimeout(() => {
console.log('0 sec timer');
}, 0);
Promise.resolve('Resolved Promise 1').then(response => console.log(response));

Promise.resolve('Resolved Promise 2').then(response => {
// just for take larger time then 0sec to execute
for (let i = 0; i <= 10000000000; i++) {}
console.log(response); // yeah verified
});
console.log('Test end');

_/
// !-----------------------------------! //
//_-----------------------------------*//
// ?-----------------------------------? //
//*Lecture 016
// \*BUILDING A SIMPLE PROMISE:

// At this point in course we know about consuming promises but we have never built our own promise. LETS DO THAT:

// !Promise constructor will take only one arguments, which is called executor function.
// As soon as the promise constructor runs, it will automatically execute this executor function that we pass in.
// !And as it executes this function here, it will do so by passing in two other arguments. And those arguments are the resolve and reject functions.

// ?Creating Promise:
// const lotteryPromise = new Promise(function (resolve, reject) {
// if (Math.random() >= 0.5) {
// resolve('You WIN 💰'); // !calling a resolve() functions like this, we mark this promise as a fulfilled(resolved). and what ever we pass in resolve function is gonna be the result of the promise that will be available in the then handler.
// } else {
// reject('You Lost your money 💩'); // !into the reject we will pass an error message, that we later want to be able to catch in catch method.
// }
// });

// ?Consuming this promise:
// lotteryPromise
// .then(response => console.log(response))
// .catch(err => console.error(err));
// !It is working HOWEVER right now it's not asynchronous yet.

// _So for that we wil simulate these codes:
/_
const lotteryPromise = new Promise(function (resolve, reject) {
console.log('Lottery draw is happening...');
setTimeout(() => {
if (Math.random() >= 0.5) {
resolve('You WIN 💰');
} else {
// reject('You Lost your money 💩'); //?instead just error we can create an error object.
reject(new Error('You lost your money 💩'));
}
}, 2000);
});

lotteryPromise
.then(response => console.log(response))
.catch(err => console.error(err));
\*/

// !In Practice most of the time we only consume a promise. We usually only build promises to wrap old callback base functions into promises. And this is a process to build and consume promises, it's called Promisify.
// \*So basically promisifying means to convert callback base asynchronous behavior to promise based.

// ?LETS SEE PROMISIFYING IN ACTION:(real world example)
// _Promisifying the set timeout function and create a wait function.
/_
const wait = function (seconds) {
return new Promise(function (resolve) {
setTimeout(resolve, seconds \* 1000);
});
};

wait(2)
.then(() => {
console.log('I waited for 2 seconds.');
return wait(1); //!for chain one more promise with 1 more second.
})
.then(() => {
console.log('I waited for 1 second.');
});
\*/
// ?Explanation:
// !no need to specify a reject function in executor function, because of impossible of error in timer.
// !In setTimeout function the callback function should be a resolve function. because we want a resolve function to execute after a certain time.

// \*Another way to a fulfilled or a rejected immediately. (very easy way)
// Promise.resolve('You WIN.').then(x => console.log(x));
// Promise.reject('Error.').catch(x => console.error(x));

// !-----------------------------------! //
// _-----------------------------------_ //
// ?-----------------------------------? //
// *Lecture 017
//*PROMISIFYING THE GEOLOCATION API
/\*
navigator.geolocation.getCurrentPosition(
position => console.log(position),
err => console.log(err)
);

// ?Lets convert the callback based API into a Promise based API.

const getPosition = function () {
return new Promise(function (resolve, reject) {
navigator.geolocation.getCurrentPosition(
position => resolve(position),
err => reject(err)
);
navigator.geolocation.getCurrentPosition(resolve, reject);
});
};

getPosition().then(pos => console.log(pos));
\*/

// ?In last coding challenge we build a function which received GPS coordinates as inputs and then render the corresponding country. Now here we got these coordinates via geolocation API, so we don't have to pass in any custom coordinates.

// !Here is coding challenge 1
/\*
const whereAmI = function () {
getPosition()
.then(pos => {
const { latitude: lat, longitude: lng } = pos.coords;

      return fetch(`https://geocode.xyz/${lat},${lng}?geoit=json`);
    })
    .then(res => {
      if (!res.ok) throw new Error(`Problem with geocoding ${res.status}`);
      return res.json();
    })
    .then(data => {
      console.log(data);
      console.log(`You are in ${data.city}, ${data.country}`);

      return fetch(`https://restcountries.eu/rest/v2/name/${data.country}`);
    })
    .then(res => {
      if (!res.ok) throw new Error(`Country not found (${res.status})`);

      return res.json();
    })
    .then(data => renderCountry(data[0]))
    .catch(err => console.error(`${err.message} 💥`));

};

btn.addEventListener('click', whereAmI);

\*/

// !-----------------------------------! //
// _-----------------------------------_ //
// ?-----------------------------------? //

// !CHALLENGE #02:

/_
const wait = function (seconds) {
return new Promise(function (resolve) {
setTimeout(resolve, seconds_ 1000);
});
};

const imgContainer = document.querySelector('.images');

const createImage = function (imgPath) {
return new Promise(function (resolve, reject) {
const img = document.createElement('img');
img.src = imgPath;
img.addEventListener('load', function () {
imgContainer.append(img);
resolve(img);
});

    img.addEventListener('error', function () {
      reject(new Error('Image not found'));
    });

});
};

let currentImg;

createImage('img/img-1.jpg')
.then(img => {
currentImg = img;
console.log('Image 1 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
return createImage('img/img-2.jpg');
})
.then(img => {
currentImg = img;
console.log('Image 2 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
return createImage('img/img-3.jpg');
})
.then(img => {
currentImg = img;
console.log('Image 3 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
})
.catch(err => console.log(err));

\*/

// !-----------------------------------! //
// _-----------------------------------_ //
// ?-----------------------------------? //

// !RENDER FUNCTION(THAT WE ARE USING)
const renderCountry = function (data, className = '') {
const html = `

  <article class="country ${className}">
    <img class="country__img" src="${data.flags.png}" />
    <div class="country__data">
      <h3 class="country__name">${data.name.common}</h3>
      <h4 class="country__region">${data.region}</h4>
      <p class="country__row"><span>👫</span>${(
        +data.population / 1000000
      ).toFixed(1)} people</p>
      <p class="country__row"><span>🗣️</span>${data.languages.por}</p>
      <p class="country__row"><span>💰</span>${data.currencies.EUR.name}</p>
    </div>
  </article>
  `;
  countriesContainer.insertAdjacentHTML('beforeend', html);
  countriesContainer.style.opacity = 1;
};

const renderError = function (msg) {
countriesContainer.insertAdjacentText('beforeend', msg);
countriesContainer.style.opacity = 1;
};

// ! ------------------------------- !

// *lecture# 019
//*Consuming Promises with Async Await
// ?Better way to consuming promises in ES17, which is called async await.

// We start with creating a special kind of function, which is an async function. we do that with writing async before function keyword before any function.

// !and that function will become an asynchronous function, so a function that will basically keep running in the background while performing the code that inside of it, then when this function is done it automatically returns a promise.

// !Inside an async function, we can have one or more await statements, after that await we need a promise.

// !await keyword use to wait for the result of this promise. So basically await will stop the code execution at this point of the function until the promise is fulfilled.

// ?We might think, Is stopping the execution of code block the execution??
// \*NO, stopping execution in an async function is actually not a problem because this(async) function is already running in the background. So therefor it's not blocking the main threat of execution, which means it's not blocking the call stack. In fact because of this, async await is so special.

// !As soon as this promise(promise after await) is resolved then the value of this whole await expression is going to be the resolved value of the promise. So we can simply store that into a variable like we did here (stored in variable response )
/_
const whereAmI = async function (country) {
const res = await fetch(`https://restcountries.com/v3.1/name/${country}`);
console.log(res);
};
whereAmI('portugal');
console.log('First');
_/

// ?it is so simple, no callback hell, no then & catch methods etc.
// !BUT We need to understand that async await is in fact simply syntactic sugar over 'then' method in promises. so behind the scenes we are still using promises. this code is exact same as old way like..

/_
const whereAmIOld = function (country) {
fetch(`https://restcountries.com/v3.1/name/${country}`).then(function (
resOld
) {
console.log(resOld); // exact same output!!!
});
};
whereAmIOld('portugal');
_/

// !Async await is only about consuming promises. We can't build any promise using async await.

/\*

const getPosition = function () {
return new Promise(function (resolve, reject) {
navigator.geolocation.getCurrentPosition(
position => resolve(position),
err => reject(err)
);
navigator.geolocation.getCurrentPosition(resolve, reject);
});
};

const whereAmI = async function () {
// \*Geolocation
const { pos } = await getPosition();
console.log(pos);
const { latitude: lat, longitude: lng } = pos.coords;

// \*Reverse Geocoding
const resGeo = await fetch(`https://geocode.xyz/${lat},${lng}?geoit=json`);
const dataGeo = await resGeo.json();
console.log(dataGeo);
!Reverse geo code API is not working...

// _Country Data
const res = await fetch(
`https://restcountries.com/v3.1/name/${dataGeo.country}`
);
/_ res.json(); //!remember this also return a promise. so now we use await again instead of then. and store the result into a variable.
/\*

const data = await res.json();
console.log(data);
renderCountry(data[0]);
};
whereAmI();
console.log('First');

\*/

// !-----------------------------------! //
// _-----------------------------------_ //
// ?-----------------------------------? //
// *Lecture# 20
//*Error Handling with try ... catch

// ?How error handling works with async await??
// With async await we can't use the catch method Instead we use something called a try catch statement. Try catch statement is used regular javascript as well. Try catch is not really attached with async await, but we can use it to catch errors in async functions.

// _One Simple Example of Try Catch (in regular code):
/_
try {
let y = 1;
const x = 2;
x = 3;
} catch (err) {
alert(err.message);
}
_/
// !In try catch we simple write all of our code in try block(try means try to execute), then after that we write a catch block with that will catch errors from the try block. ( Errors are no longer appears in the console. )
/_
const getPosition = function () {
return new Promise(function (resolve, reject) {
navigator.geolocation.getCurrentPosition(
position => resolve(position),
err => reject(err)
);
navigator.geolocation.getCurrentPosition(resolve, reject);
});
};

const whereAmI = async function () {
try {
// \*Geolocation
const { pos } = await getPosition();
console.log(pos);
const { latitude: lat, longitude: lng } = pos.coords;

    // *Reverse Geocoding
    const resGeo = await fetch(`https://geocode.xyz/${lat},${lng}?geoit=json`);

    if (!resGeo.ok) throw new Error('Problem getting location data');

    const dataGeo = await resGeo.json();
    console.log(dataGeo);
    //!Reverse geo code API is not working...

    // *Country Data
    const res = await fetch(
      `https://restcountries.com/v3.1/name/${dataGeo.country}`
    );
    if (!res.ok) throw new Error('Problem getting Country data');

    const data = await res.json();
    // console.log(data);
    renderCountry(data[0]);

    return `You are in ${dataGeo.city}, ${dataGeo.country}`;

} catch (err) {
renderError(`Something went wrong... ${err.message}`);
}

//! Reject Promise returned from async function.
throw err;
};
// whereAmI();

console.log('1: will get location');
\*/
// const city = whereAmI();
// console.log(city);

// whereAmI()
// .then(city => console.log(city))
// .catch(err => console.error(`2: ${err.message} 🔥`))
// .finally(() => console.log('3: finished getting location.'));
// console.log('3: Finished getting location');

// IIFE (Immediately Invoked Function Expression)
/_
(async function () {
try {
const city = await whereAmI();
console.log(`2: ${city}`);
} catch (err) {
console.error(`2: ${err.message} 🔥`);
}
console.log('3: finished getting location.');
})();
_/
// !-----------------------------------! //
// _-----------------------------------_ //
// ?-----------------------------------? //

// *Lecture# 021
//*code modified in above example.
// \*Returning Values from Async Functions

// !Remember async function will always return a promise.
// !If we returned any value from the async function then that value will become the fulfilled value of promise that is returned by the async function.
// ?code above⬆

// \*If there is an error in async function then return value will execute, this is a problem with async function promise, So to fix that we have to rethrow that error
// ?code above⬆

// !-------------------------------! //
// _-------------------------------_ //
// ?-------------------------------? //

// *Lecture# 022
//*Running promises in Parallel
/\*
const getJSON = function (url, errorMsg = 'Something went wrong') {
return fetch(url).then(response => {
if (!response.ok) throw new Error(`${errorMsg} (${response.status})`);

    return response.json();

});
};

const get3Countries = async function (c1, c2, c3) {
// !in async function always write a code in try catch block.
try {

    const [data1] = await getJSON(`https://restcountries.com/v3.1/name/${c1}`);
    const [data2] = await getJSON(`https://restcountries.com/v3.1/name/${c2}`);
    const [data3] = await getJSON(`https://restcountries.com/v3.1/name/${c3}`);

    */

// console.log(data1.capital, data2.capital, data3.capital);
// !All three await statements are executing one another, not in parallel. which will take so much time. to load if there is ton of calls. So we will try to execute all of these in parallel.

// !To do that we use promise.all combinator function,
// It call a combinator function because it allow to combine all promises. there are few other combinator function, we will discuss in next lecture.
// ?This promise.all function will take an array of promises and it will return a new promise, which will run all the promises in the array at the same time.
/\*
const data = await Promise.all([
getJSON(`https://restcountries.com/v3.1/name/${c1}`),
getJSON(`https://restcountries.com/v3.1/name/${c2}`),
getJSON(`https://restcountries.com/v3.1/name/${c3}`),
]);

    // !One thing here is very important: If one of the promise is reject then the whole promise will also reject as well.
    // !This technique is very commonly used in js.

    console.log(data.map(d => d[0].capital));

} catch (err) {
console.error(err);
}
};

get3Countries('portugal', 'canada', 'tanzania');
_/
// !-------------------------------! //
//_-------------------------------\* //
// ?-------------------------------? //

// *Lecture# 023
//*Other Promise Combinators
// \*race, allSettled and any

// _Promise.race: It is just like any other combinator receives an array of promises and it also returns a promise. Now this promise returned by Promise.race is settled as soon as one of the input promise settles.
// !remember, Settles means that a value is available, but it doesn't matter if the promise got rejected or fulfilled.
// ?So basically in promise.race the first settled promise wins the race.
// !Code
/_
(async function () {
const res = await Promise.race([
getJSON(`https://restcountries.com/v3.1/name/italy`),
getJSON(`https://restcountries.com/v3.1/name/egypt`),
getJSON(`https://restcountries.com/v3.1/name/mexico`),
]);

console.log(res[0]);
})();
\*/
// !Remember, In promise.race we get only one result. the result that win the race, a rejected promise can also win the race.
// !So we say, Promise.race short circuits whenever one of the promise gets settled.
// ?In real world Promise.race is actually very useful to prevent against never ending promises or also very long running promises.

// ?If any promise take too log to fetch, then we can create a set time out function that automatically rejects after a certain time has passed.
// !code
/_
const timeout = function (sec) {
return new Promise(function (\_, reject) {
setTimeout(() => {
reject(new Error('Request took too long'));
}, sec_ 1000);
});
};

Promise.race([
getJSON(`https://restcountries.com/v3.1/name/italy`),
timeout(0.1),
])
.then(res => console.log(res[0]))
.catch(err => console.error(err));
\*/
// ? Promise.all and Promise.race are two most important combinators. there are two more combinators...

// _Promise.allSettled (ES2020)
// ?It is also takes an array of promises and it will simply return an array of all settled promises, No matter if the promise got rejected or not.
// !It's similar to Promise.all but Promise.all will short circuit as soon as one promise rejects. It means Promise.all will display error as soon as one of the promise is rejected. but promise.allSettled never short circuits. It will returns all the results of all the promises.
/_
Promise.allSettled([
Promise.resolve('success'),
Promise.reject('Error'),
Promise.resolve('Success'),
]).then(res => console.log(res));
*/
//*Promise.any (ES2021)
// ?Promise.any also takes an array of multiple promises and will return only the first fulfilled promise. It'll simply ignore rejected promises.
// !Promise.any is very similar to Promise.race with the difference that rejected promises are ignored by Promise.any.
/_
Promise.any([
Promise.resolve('success'),
Promise.reject('Error'),
Promise.resolve('Success'),
]).then(res => console.log(res));
_/
// !Important ones are Promise.all and Promise.race.

// !-------------------------------! //
// _-------------------------------_ //
// ?-------------------------------? //
// \*CODING CHALLENGE #03

const wait = function (seconds) {
return new Promise(function (resolve) {
setTimeout(resolve, seconds \* 1000);
});
};

const imgContainer = document.querySelector('.images');

const createImage = function (imgPath) {
return new Promise(function (resolve, reject) {
const img = document.createElement('img');
img.src = imgPath;
img.addEventListener('load', function () {
imgContainer.append(img);
resolve(img);
});

    img.addEventListener('error', function () {
      reject(new Error('Image not found'));
    });

});
};

let currentImg;

/\*
createImage('img/img-1.jpg')
.then(img => {
currentImg = img;
console.log('Image 1 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
return createImage('img/img-2.jpg');
})
.then(img => {
currentImg = img;
console.log('Image 2 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
return createImage('img/img-3.jpg');
})
.then(img => {
currentImg = img;
console.log('Image 3 loaded');
return wait(2);
})
.then(() => {
currentImg.style.display = 'none';
})
.catch(err => console.log(err));

\*/
// !--------------------------!
// Solution
// !Part 1:

const loadNPause = async function () {
try {
// Load Image 1
let img = await createImage('img/img-1.jpg');
console.log('Image 1 loaded');
await wait(2);
img.style.display = 'none';

    // Load Image 2
    img = await createImage('img/img-2.jpg');
    console.log('Image 2 loaded');
    await wait(2);
    img.style.display = 'none';

    // Load Image 2
    img = await createImage('img/img-3.jpg');
    console.log('Image 3 loaded');
    await wait(2);
    img.style.display = 'none';

} catch (err) {
console.error(err);
}
};

// loadNPause();

// !Part 2
const loadAll = async function (imgArr) {
try {
const imgs = imgArr.map(async img => await createImage(img));
console.log(imgs);

    const imgsEl = await Promise.all(imgs);
    console.log(imgsEl);

    imgsEl.forEach(img => img.classList.add('parallel'));

} catch (err) {}
};

loadAll(['img/img-1.jpg', 'img/img-2.jpg', 'img/img-3.jpg ']);

// !26 / 05 / 2023 (6 : 08 PM)
// !Q-Hostel (MUST)

### CHALLENGE #1

```js
const whereAmI = function (lat, lng) {
  fetch(`https://geocode.xyz/${lat},${lng}?geoit=json`)
    .then(function (response) {
      // console.log(response.json());
      return response.json();
    })
    .then(function (data) {
      console.log(data);
    });
};
whereAmI(52.508, 13.381);
```
