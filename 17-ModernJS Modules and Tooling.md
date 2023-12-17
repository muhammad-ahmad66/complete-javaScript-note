# !MODERN JAVASCRIPT DEVELOPMENT\_\_MODULES, TOOLING AND FUNCTIONAL

## Table of Contents

1. [Overview_of_Modern_Javascript](#Overview_of_Modern_Javascript)
2. [OVERVIEW_OF_MODULES](#OVERVIEW_OF_MODULES)
3. [EXPORTING_AND_IMPORTING_ES6_MODULES](#EXPORTING_AND_IMPORTING_ES6_MODULES)

---

## Overview_of_Modern_Javascript

**We divide our projects into multiple modules, and these modules can share between them and make our code more organized and maintainable.**

We can also include 3rd-party modules into our own codes. There are thousands of open source modules, which we also called packages that developer share on the NPM repository. And we can then use these packages for free in our code. For example, the popular React framework or jQuery or even the Leaflet library that we used in mapty project.
All these packages are available through NPM. NPM stands for **Node Package Manager**, because it was originally developed together with Node.js and 4Node.js. However NPM has established itself as the go to repository for all kinds of packages in modern javascript development.

In order to download, use, and share packages, we use the NPM software installed in our computer. And this is just a simple command line interface that allows us to go all that.
So basically NPM is both the repository in which packages live and a program that we use on our computers to install and manage these packages.

Now after development step is complete, our project needs to go through a build process, where one big final javascript bundle is built. And that's the final file, which we will deploy to our web server for production, So basically it's the javascript file that will be sent to browsers in production. **Production** means that the application is being used by real users in the real world, Now a build process can be something really complex, but we will include here only two steps.

1. We'll bundle all our modules together into one big file. This is a pretty complex process, which can eliminate unused code and compress our code as well. these step is super important for two big reasons.
   1. All the browsers don't support at all. so code that's in a module could not be executed by any older browser.
   2. It's also better for performance.
2. We do something called transpiling and polyfilling, which is to convert all modern javascript syntax and features back to old ES5 syntax, so old browser understand our code without breaking. And this is usually done using a tool called **Babel**.

We use a tool for this build process steps. Most common build tools available are **webpack** and **parcel** and these are called javascript bundlers
_webpack is most papular but very hard to configure and use. but parcel is easy._ **recommend parcel**

---

## OVERVIEW_OF_MODULES

Modules are super important part of software development.
**A module is a reusable piece of code that encapsulates implementations details of a certain part of our project. that sounds a bit like a functions or a class BUT the difference is that a module is usually a standalone file.** Normally we think of a module we think of a separate file. a module always contains some code but it can also have imports and exports. Exports means we can export values out of a module. **And whatever we export from a module, is called the public API.** This public API is actually consumed by importing values into a module. just like we can export values we can also import values form other modules, these other modules from which we import are called **dependencies** of the importing module. values may be simple value, any function etc...

_REVIEW PDF FILE_

#### Modules in JAVASCRIPT

**Javascript ES6 modules are modules that are actually stored in files, and each file is one module, so there is exactly one module per file.**

**Scripts are usually also files, what's difference in modules and script files?**<br>

- In modules all top level variables are scoped to the module so, basically variables are private to the module by default, and the only way an outside module can access a value that's inside of a module is by exporting that value. On the other hand in script all top level variables are always global.
- Modules are always executed in strict mode but scripts are executed in sloppy mode by default. so in module no more need to declare strict mode manually.
- The _this_ keyword is always undefined at the top level while in script it points to window object.
- In module we can export and import values between modules using import/export syntax in ES6, In regular script importing and exporting values is just completely impossible.

#### There is some to note about import and export:

- **Import and export only happen at the top level.** Remember that top level means outside of any function or any if block etc.
- **Also all imports are hoisted.**
- To link a module any module in our html page we need to **use script tag with the type attribute set to module.** instead of just a plain script tag.
- And finally the about the downloading the module files themselves, always automatically happens in an asynchronous way. and this is true for module loaded from html as well as for modules loaded by importing one module into another using import syntax. While a regular scripts are downloaded by default in a blocking synchronous way, unless we use the async or differ attributes on the script tag.

How modules imports other modules behind the scene.???? where???

---

## EXPORTING_AND_IMPORTING_ES6_MODULES

// ?Simply import a module, without importing any value:
// -first we have made a file with name of shoppingCard.
// !Importing Module
// import './shoppingCard.js';
// console.log('Importing Module');

// !All the imported statements will parse at the top of the file. so in console Importing Module is printing after Exporting Module. although we change the order. like this
// console.log('Importing Module');
// import './shoppingCard.js';

// ? Importing with value:
// import { addToCart } from './shoppingCard.js';

// addToCart('bread', '5');
// !now we used the function that is defined in shoppingCard module.

// !Importing multiple things.
/_
import { addToCart, totalPrice as price, tq } from './shoppingCard.js';
addToCart('bread', 5);
console.log(price, tq);
_/
// !Also we can change the name of the inputs. like totalPrice to price..Also we can change name in export.
// ?Code ⬆

// !We can also import all the exports of a module at the same time.
// import \* as ShoppingCard from './shoppingCard.js';

// ?Here we imported all the exported values form module. now it's just like a namespace if you wants any thing from that file we just use our name(ShoppingCard) with dot and then that thing(variable, function etc.). like this.
// ShoppingCard.addToCart('bread', 12); // working.

// ! ------------------------- !

// \*Default Exports:
// ?Usually, we use default exports when we only want to export one thing per module. there is No name involved at all in exporting module. we are simply exporting any value. then when we import it we can give it any name that we want. here we gave name 'add', we can give any name.
// import add from './shoppingCard.js';
// add('pizza', 2);

// !We could even mix all of them(exports) in the same import statement. import may be both named and default at a same time.
/_
import add, { addToCart, totalPrice as price, tq } from './shoppingCard.js';
add('pizza', 9);
addToCart('pasta', 2);
console.log(price, tq);
_/
// !However in practice we never mix named and default export in the same module.

// import add, { cart } from './shoppingCard.js';
// add('pizza', 3);
// add('apples', 6);
// add('bread', 97);

// console.log(cart);
// !Remember, Imports are not a copies of the exports, They are instead like a live connection, in any instance we change in export it will automatically change in import file as well. in above code we see this, if we look at export file the cart is basically an empty array in exporting statement, from here we added thing to that array. now if we console the cart in import file all things are added....

// ! ------------------------------ !

// Lecture 006
// *Top level await (ES22)
// *Top level await means await outside of any async function.
// ?So starting from this new ES2022 version, we can now use the await keyword outside of async function, at least in the modules.
// !we can use toplevel await in modules only... to use top level await we should write type attribute set to module in script tag.

// ?simple example
/_
console.log('start fetching ');
const res = await fetch('https://jsonplaceholder.typicode.com/posts');
const data = await res.json();
console.log(data);
console.log('Something');
_/
// !Really really important to understand here is to, this actually blocks the execution of the entire module.

// ?Many times, we have the situations where do have an async function that we want to return some data.
// const getLastPost = async function () {
// const res = await fetch('https://jsonplaceholder.typicode.com/posts');
// const data = await res.json();
// console.log(data);
// return { title: data.at(-1).title, text: data.at(-1).body };
// };

// const lastPost = getLastPost();
// console.log(lastPost); //its an object
// !Remember calling an async function will always return a promise. it'll not return an actual data itself. So
// Not very clear.
// lastPost.then(last => console.log(last));

// So, Here for that we can use top-level await for this.
// const lastPost2 = await getLastPost();
// console.log(lastPost2); // working.

// ?One more important application of top level await. and that is the, that one module imports a module which has a top-level await, then the importing module will wait for the imported module to finish the blocking code.

// ! ------------------------------ !

// Lecture 006
// _The Module Pattern:
// The main goal of the module pattern is encapsulate functionality to have private data, and to expose a public API. The base way to achieving that is by simply using a function, because functions give us private data by default and allow us to return values. which can become our public API.
// ?Let's see how the module pattern is implemented. Usually we write an IIFE function actually.
/_
const ShoppingCart2 = (function () {
const cart = [];
const shippingCost = 10;
const totalPrice = 237;
const totalQuantity = 23;

const addToCart = function (product, quantity) {
cart.push({ product, quantity });
console.log(
`${quantity} ${product} added to cart (shipping cost is ${shippingCost})`
);
};

const orderStock = function (product, quantity) {
console.log(`${quantity} ${product} ordered from supplier`);
};
// !Right now all of these data are private because these are inside of the scope of function. Now all we have to do is to return some of this stuff in order to basically return a public API.
// to do data we simply return an object which contains the stuff that we want to make public.
return {
addToCart,
cart,
totalPrice,
totalQuantity,
}; // !However right now we are not storing these returned object anywhere. To fix this we can simply assign this IIFE to any variable (here to 'ShoppingCart2')
})();
\*/
// !This function is only created once because the goal of this function not to reuse code by running it multiple times. The Only purpose of this function is to create a new scope and return data just once.

// ShoppingCart2.addToCart('apple', 4);
// ShoppingCart2.addToCart('pizza', 4);
// console.log(ShoppingCart2.cart); // !it's public, returning.
// console.log(ShoppingCart2.shoppingCost); // !undefined, b/c its private.
// !Why, all of these?
// ?Because this function(IIFE) will execute once in the beginning and it will returning that object, and assigned it to the variable('ShoppingCart2'), then we are able to use all of these and also manipulate the data that is inside of that function.
// !In simple words the answer of HOW ALL OF THESE WORKS?? is due to closures, remember allow a function to have access to all the variables that were present at its birthplace.

// !------------------------!

// Lecture 008
// \*CommonJS Modules:
// Besides native ES modules and modules pattern there are also module systems that have been used by javascript in the past. They relied on some external implementations.
// !Two examples are AMD Modules and CommonJs Modules.
// ?CommonJs modules are important for us, because they have been used in Node.js. (ES modules is very recently implemented in node.js)
// Remember a Node.js is a way of running javascript on a web server outside of a browser.
// !Almost all the modules in the NPM repository still use the commonJS module system. reason for that npm was originally only intended for node.

// _Export
/_
export.addToCart = function (product, quantity) {
cart.push({ product, quantity });
console.log(
`${quantity} ${product} added to cart (shipping cost is ${shippingCost})`
);
};
\*/
// !This is not going to work in the browser but it would work in node.js. this export keyword here is basically an object, that is not defined here in our code also not in the browser. But in node.js it's an important object that is used.

// \*Import
// const {addToCart} = require('./shoppingCard.js');
// !here also require is not defined in browser but it's in node.js. because this is past of the commonJS specification.

// ! -------------------------- !
// Lecture 009
// \*A Brief Introduction to the command line

// ! -------------------------- !
// Lecture 010
// \*Introduction to NPM
// Node Package Manager
// !It's both a software on our computer and a package repository.

// ?Why we need NPM?
// ?Why we need managing packages and dependencies in our project?
// Back in a day before we had NPM we use to include an external libraries we write into html, using the script tag. this may lead some problems.

// ! npm -v command
// to check nodejs is installed or not(it will display version of nodeJS)

// ! In each project where we want to use npm we need to initializing it with nmp init command, then they will ask some question.
// ? After answering all questions we end up with a new file called package.json

// Now we install leaflet library using npm.
// !npm install leaflet
// Also we can write !nmp i leaflet, instead of install just i.
// !After installing we see in package.json file, there is a new field was created for the dependencies. and dependency that we there now is leaflet, with it's version.
// !Now second thing here is, we have new folder called node_modules, and this folder contains the leaflet folder,

// ?If we wanted to use this library that wouldn't be easy without a module bundler, That's because this library actually uses the commonJS module system. there we can't directly import it into our code.
// !For now leave leaflet library.

// ?Instead let's see How we install and import one of the most popular javascript libraries which is Lodash.
// !Lodash is a collection of ton of useful functions for arrays, objects, functions, dates and more.
// if we install like npm install lodash, once again that use commonJS. we can't use commonJS module without module bundler. So here is a special version which is called lodash-es, es because fo es module.
// !npm i lodash-es
// Here in lodash folder we have one file for each of methods that are available in Lodash.

// ?we will include file for cloning objects. (cloneDeep.js) So, we just import that file.
// !in file we see there is exporting default, so could give any name.
// import cloneDeep from './node_modules/lodash-es/cloneDeep.js';

// ?whey i cloneDeep?
// \*Because it very complex to coping a nested object.

// const state = {
// cart: [
// { product: 'bread', quantity: 5 },
// { product: 'pizza', quantity: 8 },
// ],

// user: { loggedIn: true },
// }; // !this is a deeply nested object.

// \*Code to copy nested object by normal javascript.
// const stateClone = Object.assign({}, state);
// console.log(stateClone); // ?It looks exact same as in state object, However see what happen if we change one of the nested objects.
// state.user.loggedIn = false; // !Now we see in copy it's also false.

// !Due to this using of CloneDeep is good idea for deep copy instead of using object.assign.

// const stateDeepClone = cloneDeep(state);

// console.log(stateDeepClone); // now we will change
// state.user.loggedIn = false; // !Now it's not changed in DeepClone...

// \* Now will talk little bit about package.json file:
// ?Lets say that we want to move our project to another computer OR also share it with another developer OR even check it into version control like git. In all these scenarios we should never include the node modules folder.
// ?There is no reason to include this huge node modules folder, because in the real project it will actually be really, really huge.
// ! To add a folder one by one this package.json comes to play.

// !Lets delete node_modules folder.

// ?There is very easy way to get it back all we have to do is NPM and then install or i, but just without any package name. then npm will reach into our package.json file look all the dependencies and then install back.

// ! ----------------------- ! //
// ? ----------------------- ? //

// Lecture 011
// \*Building with Parcel and NPM Script:

// ! Module bundler we will use in this course is called parcel. It's super fast and easy to use and it works without any configurations.
// ?Webpack is also a most papular bundler and especially in react world, However it's so complex.

// Parcel is just another build tool which is also on NPM, we will use npm to install it.
// !npm i parcel --save-dev
// -dev means it's a dev dependency, dev dependency is a tool we use to develop. not like any other modules.

// ! 2 lectures skipped. should watch!

// !------------------------!
// ?------------------------?

// Lecture 013
// \*Review_Writing Clean and Modern JS

// !READABLE CODE
// We should write readable code. we should write code that others can understand it.
// Avoid too clever and overcomplicated solutions.
// Use descriptive variables and functions names.

// !GENERAL
// Use DRY(don't repeat yourself) principle (refactor code.)
// Don't pollute global namespace. it mans don't declare variables in global namespace. Instead encapsulate them into functions, classes or modules.
// Don't use var. always use const if possible
// Use strong type checks (=== and !==)

// !Functions
// Generally, Functions should do only one thing.
// Don't use more than 3 function parameter.
// Use default parameters whenever possible.
// Generally, return same datatype as received.
// Use arrow functions only when they make code more readable like in callback functions in arrays methods.

// !OOP
// Use ES6 classes
// Encapsulate data and don't mutate it from outside the class
// Implement method chaining.
// Do not use arrow functions as methods in regular objects.

// !Avoid Nested Code
// Use early return (guard clauses)
// Use ternary or logical operator instead of if
// Use multiple if instead if/else-if
// Avoid for loops, Instead use Array methods.
// Avoid callback-based asynchronous APIs.

// !Asynchronous Code
// Consume promise with async/await for best readability.
// Whenever possible, run promises in parallel(promise.all)
// Handle errors and promise rejections.

// !--------------------------! //

// Lecture 015
// !Declarative and Functional Javascript Principles.
// Two different paradigms of writing javascript code.
// _1- Imperative code
// We explain the computer every single step it has to follow to achieve a result.
const arr = [2, 3, 4, 5];
const doubled = [];
for (let i = 0; i < arr.length; i++) doubled[i] = arr[i] _ 2;

// _2- Declarative code [modern]
// programmers tells the computer only What to do
// we simply describe the way the computer should achieve the result
const arr1 = [2, 3, 4, 5];
const doubled1 = arr.map(n => n _ 2);

//\* Functional Programming -sub paradigm of declarative
// Writing program simply by combining multiple pure functions, while avoiding side effects and mutating data.
// Very modern and papular way writing code in javascript world.
// ?Side Effects:
// Modification (mutation) of any data outside of the function (mutating external variables, logging to console, writing to DOM, etc)
// ?Pure Functions:
// Functions without any sie effects. Does not depend on external variables. Given the same input always return the same outputs.
// ?Immutability:
// States(data) is never modified, instead data(state) is copied and copy is mutated and returned.

// !-----------------------!
// lecture 016

// \*lETS FIX SOME BAD COE

// \*will make functions immutable.
const spendingLimits = Object.freeze({
jonas: 1500,
muhammad: 100,
});
// !now we can't put any new property on it. in this object.
// we can also use Object.freeze to make array immutable.
// !NOTE: Object.freeze will freeze only first level of the object. It's not a deep freeze.

// !------ROUGH------! //
// \*Fix some bad codes
// const flatted = [
// [0, 1],
// [2, 3],
// [4, 5],
// ].reduce((acc, value) => {
// debugger;
// return acc.concat(value);
// }, []);

// console.log(flatted);

const str = 'Muhammad Ahmad';
console.log(str.length);
const array = [];

for (let i = str.length - 1; i >= 0; i--) {
array.push(str[i]);
}

const reversed = array.join('');

const numbers = [4, 6, 7, 3, 20];

// ?Reverse String 01
const sum = function ([...numbers]) {
return numbers.reduce((acc, num) => {
return acc + num;
}, 0);
};
console.log(sum(numbers));

// ?Reverse String 02
const str1 = 'Hello, World!';
const reversed1 = str.split('').reverse().join('');
reversed; // "!dlroW ,olleH"

// ?Reverse String 03
const str2 = 'Hello, World!';
const reversed2 = [...str].reverse().join('');
console.log(reversed); // "!dlroW ,olleH"

// Destructor with spread operator.
const [first, second, third, ...others] = numbers;
console.log('first: ', first);
console.log('second: ', second);
console.log('third: ', third);
console.log('others: ', others);
