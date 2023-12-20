# JS Behind the Scenes

## Table of Contents

1. [DEFAULT_PARAMETERS](#default_parameters)
2. [PASSING_ARGUMENTS_INTO_FUNCTION](#passing_arguments_into_function)
3. [FIRST_CLASS_FUNCTIONS](#first_class_functions)
4. [THE_CALL_APPLY_AND_BIND_METHOD](#the_call_apply_and_bind_method)

---

## DEFAULT_PARAMETERS

Some parameters are set by default.  
By using default parameters we can assign any expression to a parameters or even we can assign value from any other variable.

Before ES6, **2 common ways of assigning default values**

1. **Assigning default values using if statement**

   ```js
   if (!numPassengers) numPassengers = 1;
   if (!price) price = 399;
   ```

2. **Assigning default values by OR short circuiting**

   ```js
   numPassengers ||= 1;
   price ||= 199;
   ```

```js
const bookings = [];
const createBooking = function (
  flightNum,
  numPassengers = 1,
  price = 199 * numPassengers
) {
  // NOTE if we wants to assign any other parameter as a default as in this, then that parameter should be before (right side).

  const booking = {
    flightNum,
    numPassengers,
    price,
  };
  console.log(booking);
  bookings.push(booking);
};

createBooking('LH123');
createBooking('LH123', '02');
console.log(bookings);
```

Remember: We can't skip any argument when calling a function if there exist some default assigned parameter, that's way we always write default parm at last. but also we can skip using this trick:

```js
createBooking('Lh223', undefined, 399);
```

It's exact same as undefined, so in function default value will assign to 2nd argument.

---

## PASSING_ARGUMENTS_INTO_FUNCTION

How primitive data-type and reference data-type works, when passing in function.

```js
const flight = 'LH234';
const me = {
  name: 'Muhammad Ahmad',
  passport: 25489789323,
};

const checkIn = function (flightNum, passenger) {
  flightNum = 'PK125';
  passenger.name = 'Mr. ' + passenger.name;
  if (passenger.passport === 25489789323) {
    alert('Checked in');
  } else {
    alert('Wrong passport!');
  }
};

checkIn(flight, me);
console.log(flight); // 'LH234' // Not changed original variable.
console.log(me); // {name: 'Mr. Muhammad Ahmad',passport: 25489789323} // changed original one.
```

_**So Remember:** When we pass **Primitive datatype** in any function, then it will always create a new variable(copy of original) variable. like checkIn(flight); here flight is a primitive, so in parameter flightNum is a copy of flight variable, as a result if we change in flightNum, then it'll not reflect in original one, SO, **when we change parameter's value in any function then it'll not effect value in original one.**_

**BUT** in **Reference data-type**, when we pass in any argument then, only create new reference of the object into heap but the both (original and copy) will point a same address in memory.

```js
const newPassport = function (person) {
  person.passport = Math.trunc(Math.random() * 10000000000);
};

newPassport(me);
checkIn(flight, me); // Here will print wrong passport, b/c passport's value is changed.
```

There are **two type of passing an argument** into any function

1. Pass by reference
2. Pass by value

_**Remember:** Javascript has no passing by reference. but in objet we do it????why??? Basically we passed a reference not by reference, it's a value which contain memory address._

---

## FIRST_CLASS_FUNCTIONS

This enables us to write **Higher Order Functions.**

- Javascript treats a Functions as first class citizen
- It means, the functions are simply values.
- Functions are just another type of object.
- We stored the functions in a variables. -in function expression- and also we store methods in variable
- We can also pass functions as an arguments.
- We can also return a function from another function.
- Like array, object & string there also methods for functions.
- All of these (First class functions) makes it possible to use and write Higher order function.

_**Higher-Order Functions:** Higher order function is either a function that receives another function as an argument or a function returns a new function._

### Examples of a functions that receive another function

```js
addEventListener('click', greet);
```

Here⤴ **addEventListener is a higher-order function** and **greet is a callback function**, We usually say **a function that's passed in any function is a callback function.**

We will discuss about Functions that return any other function (that's more advanced, and more harder to understand)

### Higher Order Functions

Higher Order Function [ **A function that receive any other function or return any function** ]

#### Function Receiving other function

```js
const oneWord = function (str) {
  return str.replaceAll(' ', '').toLowerCase();
};

const upperFirstWord = function (str) {
  const [first, ...others] = str.split(' ');
  return [first.toUpperCase(), ...others].join(' ');
};

// Now using higher order function

const transformer = function (str, fn) {
  console.log(`Original string: ${str}`);
  console.log(`Transformed string: ${fn(str)}`);

  console.log(`Transformed by: ${fn.name}`);
  // it will give the name by function. here is upperFirstWord, that we have passed in argument.
};

transformer('Javascript is the best!', upperFirstWord);
// here transformer is higher-order function and upperFirstWord is a call back function.

// these are just same as addEventLister function
const high5 = function () {
  console.log('💖💖💘');
};
document.body.addEventListener('click', high5);
```

In JS callback function we use so much time.

#### Functions Returning Functions

```js
const greet = function (greeting) {
  return function (name) {
    console.log(`${greeting} ${name}`);
  };
};
```

##### Above functions using Arrow function

```js
const greetArrow = (greeting) => (name) => console.log(`${greeting} ${name}`);

const greeterHey = greet('Hey');
// see in above function, it is returning a function so in this variable the insider function will store and this variable is also a functions, b/c it contains return function.
greeterHey('Muhammad');
```

##### This two steps are in one line

```js
greet('Hey')('Muhammad');
// because the combination greet and first parenthesis is also a function, so we passed another argument to that function.
greetArrow('Hi,')('Saud');
```

---

## THE_CALL_APPLY_AND_BIND_METHOD

// How we can set the 'this' keyword manually, and why we want to do that??

const lufthansa = {
airline: 'lufthanse',
code: 'LH',
bookings: [],
// book: function(){}
book(flightNum, name) { // new syntex to defining methods.
console.log(`${name} booked a seat on ${this.airline} flight ${this.code}${flightNum}`);

        this.bookings.push({ flight: `${this.code} ${flightNum}`, name })
    }

};
lufthansa.book(239, 'Muhammad Ahmad');
lufthansa.book(569, 'Jonas');
console.log(lufthansa.bookings);

const eurowings = {
airline: 'Eurowings',
code: 'EW',
bookings: []
};

const book = lufthansa.book; // now we made a new function of book which is copy of book from lfthansa object.

//book(344, 'Muhammad'); // Now it will give an error b/c this book function is in body (regular function) not in any object (from line 184) so in strict mode the this keyword will not work with regular function. (if not strict mode then will point to window object.)...NOTE : Here we are trying to find solution of this problem. we want to tell explicitly where this keyword should point..

// There are three functions methods to tell this keyword manually/ explicitly where it should point
// 1. call
// 2. apply
// 3. bind

// Subheading
// 1- CALL METHOD
// book(344, 'Muhammad');// NOT worked, so we use call method
book.call(eurowings, 23, 'Muhammad'); // first argument: where point this keyword. and others are that we are passing.
console.log(eurowings);

book.call(lufthansa, 239, 'Mary Cooper');
console.log(lufthansa);

const piaAirline = {
airline: 'PIA Airline',
code: 'PK',
bookings: []
//Remember: A property name should same as original object's property name (lufthansa), where this keyword is written.
}
book.call(piaAirline, 299, 'Hammad Ahmad');
book.call(piaAirline, 344, 'Haris Shehbaz');
book.call(piaAirline, 666, 'Adeel Ahmad');

// Subheading
// 2- APPLY METHOD:
// The apply method does basically exactly same thing that we see in call method, ONLY difference is apply method does't receive a list of arguments after we specifing wehere should this keyword point, BUT instead it's gonna take an array of the arguments.
// first we create an array which contain flightNum & passenger name in this case
const flightData = [555, 'Shoaib Sharif'];
book.apply(piaAirline, flightData); // first argument: this keyword, second should be: array where contain booking data.

// now days apply method is not common b/c we have a better way to doing this same thing. We can use call method and then using spread operator we unpack array's elemets. like this!
book.call(piaAirline, ...flightData);

// Subheading
// 3- BIND METHOD: [ Most Important One ]
//
// it also allow to manually set the this keyword for any function call the difference is that bind does't immediately call the function, instead it returns a new function where this keyword is bound.

// book.bind(piaAirline); // it will create a new function with a this keyword also set to piaAirline. This will not call a function instead it will return a new function where this keyword set to piaAirline. we have to store this return value, that's also a new function.
const bookPIA = book.bind(piaAirline);
bookPIA(999, 'Musab Sharif'); // here it seem as normal function call, that's b/c in this function we already set this keyword.
console.log(piaAirline);

const bookEW = book.bind(eurowings);
const airblue = {
airline: 'Airblue Airline',
code: 'AB',
bookings: []
}
const bookAB = book.bind(airblue);
bookAB(741, 'Abdullah');

// We can actually take this even further.
// like call method we can also do same thing here (passing multiple argumet)

const bookPIA66 = book.bind(piaAirline, 66); // we have made a function which is bind to book and also set first parameter's value. this function is specific for PIA fligntNum 66.
bookPIA66('Muhammad Shamim'); // bookPIA66 function with 66 predefined argument.
bookPIA66('Alina Ahmad'); // this both are PIA PK66 flight. (in one plane)

// OTHER Situations where we can use bind method. (common and v.usefull)

// When we objects together with event listner.
lufthansa.planes = 300; // added new property in lufthansa object.
lufthansa.buyPlane = function () {
console.log(this);
this.planes++;
console.log(this.planes);
}
// here we are going to build when ever we click buy new plane button the planes should increment by one.
document.querySelector('.buy').addEventListener('click', lufthansa.buyPlane.bind(lufthansa)); // It is not working, Remember B/c in event handler the this keyword always point to the element on which the handler is attached. here it's attached to the that button where buy class is attached.
// In event hadler function we still need to set this keyword at lufthansa object. we should manually define this keyword. how??

// Remember Here it's proof that this keyword is dynamic. it always defend on calling function. if we call like this;
// lufthansa.buyPlane(); // it should point to luftansa object.

// Partial Application [another big use case of bind method]
// Partial application means preset paramether. (pre set any parameter's value. )
// In preset, order of parameter is important!.

const addTax = (rate, value) => value + value \* rate;
console.log(addTax(0.1, 200));

const addVAT = addTax.bind(null, 0.23); // we preset the value of rate which is 23% (0.23). and NULL is for this keyword, here is no this keyword.
console.log(addVAT(100));
console.log(addVAT(66));

// We can give value by simply default parameter's value. Why we are using bind method instead of default parameter??
// Answer: by using a bind method we can make a specific function of any general function. here addTax is a general function while addVAT is a specific fucntion b/c in this we are already set rate(rate is fix), if rate is 23% then we use addVAT function. it's a function that binded with addTax function. So, it's not compariable with default value.

// my.......................
// const addTax2 = function (value) {
// return function (rate = 0.23) {
// console.log(value + value \* rate);
// }

// };

// // const result = addTax2(100)
// addTax2(100)();

// his.....................
const addTaxRate = function (rate) {
return function (value) {
return value + value \* rate;
}
}
console.log('From 2...........');
const addVAT2 = addTaxRate(0.23);
console.log(addVAT2(100));
console.log(addVAT2(66));

\*/

////////////////////////////////////////////////////////////////
/\*

// Heading
// ---Challange 0001---
const poll = {
question: "What is your favourite programming language?",
options: ["0: JavaScript", "1: Python", "2: Rust", "3: C++"],
// This generates [0, 0, 0, 0]. More in the next section!
answers: new Array(4).fill(0),
registerNewAnswer() {
// get the answer
const answer = Number(prompt(`${this.question}\n${this.options.join('\n')}\n(Write option number)`));

        // update the answer
        console.log(answer);

        typeof answer === 'number' && answer < this.answers.length && this.answers[answer]++; // using and short circuiting operator.
        // console.log(this.answers);

        // console.log(Number(selectedNum));
        // console.log();
        this.displayResult();
        this.displayResult('string')
    },
    displayResult(type = 'array') {
        if (type === 'array') {
            console.log(this.answers);
        } else if (type === 'string') {
            console.log(`Poll results are ${this.answers.join(', ')}`);
        }
    },

};
document.querySelector('.poll').addEventListener('click', poll.registerNewAnswer.bind(poll));

// const newObject = {
// answers: [5, 2, 3],
// };
// poll.displayResult.call(newObject, 'string');

poll.displayResult.call({ answers: [1, 5, 3, 9, 6, 1] }, 'string');

_/
/_

///////////////////////////////////////////////////////////
// Heading
// - Immediately Invoked Function Expressions-
// Some time in javascript we need a function that is only executed once and then never again. So basicallly a function that disappeares after it's called once.

const runOnce = function () {
console.log('This will never run again');
};
runOnce();

// This not we are gonna built
// (function () {
// console.log('This will never run again');
// }) // without parentheses it'll give an error.
// by using parentheses we get no error but also a function didn't execute yet. we never call it.
(function () {
console.log('This will never run again');
})(); //before last parentheses all is a function so we just called it by using parentheses.
// Now this function is called immediately invoked function expression( IIFE ).
// It also worked for arrow function
(() => console.log('From Arrow Function'))();

// Use for data encalpsulation and data privacy.

// In moderan javascript we can use also block for data encalpsulation (only using const and let, not vor). like this
{
// const isPrivateForConst = 23;
// let isPrivateForLet = 33;
var isPrivateForLet = 44;
}
// console.log(isPrivateForConst); //error
// console.log(isPrivateForLet); //error
console.log(isPrivateForLet); // 44

\*/

////////////////////////////////////////////////////////
// Heading
// ---Closures---
//[ v.important as well as hardest concept ]
// We don't create closures explicetly like an array, functions etc. It simply happened automatically in certain situations we just need to recognize those situations.
// In this example we gonna create one of those situation

const secureBooking = function () {
let passengerCount = 0;

    return function () {
        passengerCount++;
        console.log(`${passengerCount} passengers.`);
    };

};

const booker = secureBooking();
booker(); //1
booker(); //2
booker(); //3
// Remember
// It's changing passenderCount variable, which is private/local scope to that function. so it's possible chnaged local scope from global. HOW??? Answer....

// from return statement the secureBooking function will end their execution and will gone from stack (no longer exist) while returning funcion is a child function of it so child function can manuplate the parent function. As we assigned the returning function to the booker variable (which is acctually that function) that is in global environment/ global scope. The environment where the function is created(inside the secureBookin function) is no longer exist but the variables in that function can be manuplates by booker function that's exactly what the closure does. So we can say that the closures make a function remember all the variables that existed at the function's birthplace. secureBooking is the birthplace of the booker function.

// A function has access to the variable environment of the execution context in which it was created even after that execution context is gone. The closure is then basically this variable environment attached to the function, exactly as is was at the time and place that the function was created.
// Booker function has access to the passengerCount variable b/c it's basically defined in the scope in which the booker function was actually created. in this sense the scope chain is actually preserved through the closure even when a scope has been already destroyed b/c it's execution context is gone. but variable environemnt somehow living somewhere in the engine.
// Now we can say that the booker function closed over its parent scope or over its parent vairable environment. and this includes all function's arguments.
// By closure a function does't lose connection to variables that exist at the function's birthplace.
// Closure has priority over scope chain.

// Some Different Definitions of closure:
// 1- closure is closed over variable environment of execution context in which a function was created, even after that execution context is gone or even a funtion in which the execution context belogns has returned. -this is we saa above.

// 2- A closure gives a function access to all the variables of its parent function. So the function in which it is defined even after that parent function has returned.

// 3- Closure make sure that the function never loose connection to variables that exist at the funtion's birth place. it's like a person who doesn't lose connection to their hometown.

// 4- Closure is like a backpack(bag) that a function carries around where it goes. This backpack has all the variable that were present in the environment where the function was created. like a person is a function, his bag is a closure and his copies,laptop etc that are in bag are variables.

// Summery
// Koi b function apny birthplace environment k sary variables ka access rakhta ha even ki usky bith place wala function execute ho k khatm ho gya ho. khatm hota mtml call-stack sei distroy hva ho, tb b wo function variables to istimal kr skta ha closure ki waja sea.

console.dir(booker);

// NOW Create two more situations in which closure gonna appear.

// First example

let f;

const g = function () {
const a = 23;
f = function () {
console.log(a \* 2);
};
};

const h = function () {
const b = 777;
f = function () {
console.log(b \* 2);
};
}

g();
f();
console.dir(f) // here closure contains a

// Reasigned f-variable
h();
f();

// let's see variable environmet
console.dir(f) // here closure conatins b

// Second Example

const boardPassengers = function (n, wait) {
const perGroup = n / 3;

    setTimeout(function () {
        console.log(`We are now boarding all ${n} passengers. `);
        console.log(`There are 3 groups each with ${perGroup} passengers. `);
    }, wait * 1000)   // this funtion will execute after wati * 1000ms.
    console.log(`We start boarding in ${wait} seconds. `);

};

const perGroup = 10000; // this variable will not use in settimeout function. it's prove that closure have priority over scope chain.
boardPassengers(180, 3);

// In this example the callback function(settimeout) is executed completely after execution of boardpassenger b/c we set a time of 3 sec. Only way to access variables n and wait by the callback function is a closure. the closure stores the variable.

////////////////////////////////////////////

// Heading
// -----Coding Challange #02-----

(function () {
const header = document.querySelector('h1');
header.style.color = 'red';
document.querySelector('body').addEventListener('click', function () {
header.style.color = 'blue';
})
})();
