# Numbers Dates Intl and Timers

## Table of Contents

1. [Converting_And_Checking_Numbers](#converting_and_checking_numbers)
2. [SOME_MATHEMATICAL_OPERATIONS_AND_ROUNDING_NUMBERS](#some_mathematical_operations_and_rounding_numbers)
3. [REMINDER_OPERATOR](#reminder_operator)
4. [NUMERIC_SEPARATOR](#numeric_separator)
5. [BIG_INT](#big_int)
6. [DATES_AND_TIMES](#dates_and_times)
7. [INTERNATIONALIZATION_DATES](#internationalization_dates)
8. [TIMERS](#timers)
9. [BANKIST_APP](#bankist_app)

---

## Converting_And_Checking_Numbers

**How numbers works in Javascript?**  
**How to convert values to numbers?**  
**How to check is certain value is number or not?**

In javascript all numbers presented internally as floating point numbers **PROOF...**

```JS
console.log(23 === 23.0); // true.
```

**Also numbers are represented internally 64 base 2 format.** which means, that numbers are always stores in a binary format, not in base 10 ( 0 to 9 );  
As numbers are represents in binary(0, 1). There are certain numbers that are very difficult to represent in base 2. like 0.1

```js
console.log(0.1 + 0.2); // It will give very weird results b/c of base 2.
// here printing 0.300000000000004, output should 0.3

console.log(0.1 + 0.2 === 0.3); //false.. what!!! it's printing false.
// This is very common error in javascript due to base 2. -not in js but also seems in php, ruby etc.
```

---

### CONVERT_STRING_TO_NUMBER

```js
console.log(Number('33'));
```

Easier Way: -trick to convert

```js
console.log(+'33');
```

**PARSING:** By parsing javascript will automatically find a number from a string. even it contains letters and then convert it to number by removing letters. BUT the string should not start with letter, always with number.

#### ParseInt() Function

```js
console.log(Number.parseInt('44fs')); //44
console.log(Number.parseInt('Fe44')); // not work //NaN
console.log(Number.parseInt('22Fe44')); // only 22
```

This will v.helpful like this situation, if we have to access some units of css and then get rid of that unit. like..

```js
console.log(Number.parseInt('48px')); //48
```

Actually paseInt function accept second argument, which is so called regex. **Regex** is a base of numeral system that we are using.

```js
console.log(Number.parseInt('48px'));
// here and in all above examples we are using base 10 number system, so regex is 10.

console.log(Number.parseInt('36px', 10));
console.log(Number.parseInt('101px', 2)); // it'll convert base to (101) to base 10 number (5)
console.log(Number.parseInt('0011', 2)); // 3
```

#### parseFloat() Function

```js
console.log(Number.parseFloat(' 32.43rem', 10)); // 32.43
console.log(Number.parseInt(' 32.43rem', 10)); // 32 only
```

we can also do this, BUT not good practice:

```js
console.log(parseInt('312em', 10)); // 312 . working. -without namespace Number.
```

**Remember:** Before parseInt, a 'Number' method/function is called **namespace**. **Number** is namespace for all different function like parseInt, parseFloat, etc.

ANOTHER FUNCTION OF THE NUMBER NAMESPACE:

#### IsNAN (isNaN()): to check is any value Not a Number?

```js
console.log(Number.isNaN(+'sas')); // true
console.log(Number.isNaN(44)); // false
console.log(Number.isNaN(22 / 0)); // false // why?
console.log(Number.isNaN('ddd')); // false b/c is not gives NaN.
```

**Remember:** isNaN is not a perfect way to checking numbers.

Another WAY to check numbers, WHICH IS LOT BETTER:

#### Number.isFinite()

```js
console.log(Number.isFinite(10 / 0)); // false
console.log(Number.isFinite(3 / 2)); // true
console.log(Number.isFinite('dd')); // false
console.log(Number.isFinite('ffdd33f')); // false
```

**Remember** isFinite is best way to check is number.

When we working only with integer (not floating) then best way of check a number or not is isInteger function.

#### isInteger Function

```js
console.log(Number.isInteger('44')); // false b/c its string
console.log(Number.isInteger(44)); // true
console.log(Number.isInteger(33.2)); // false b/c floating
console.log(Number.isInteger(44 / 0)); // false b/c infinity
console.log(Number.isInteger(33.0)); // true all int is float
```

### CONCLUSION

USE **parseFloat()** to parse any string to number  
USE **isFinite** to check is a Number  
USE **isInteger** to check is an integer.

---

## SOME_MATHEMATICAL_OPERATIONS_AND_ROUNDING_NUMBERS

### SQUARE ROOT

**Math.sqrt()** Function

```JS
console.log(Math.sqrt(25)); //5
```

**Another way**:

```js
console.log(25 ** (1 / 2)); //5 (here 2 for square root)
console.log(25 ** (1 / 3)); // (here 3 for cubic root)
console.log(27 ** (1 / 3)); //3 (here 3 for cubic root)

console.log(16 ** (1 / 2)); //4 (here 3 for square root)
console.log(16 ** (1 / 4)); //2 (here 3 for square root)
```

---

### MAXIMUM VALUE

```js
console.log(Math.max(3, 2, 4, 1, 4, 5, 9, 6, 7)); //9
```

It does type coercion

```js
console.log(Math.max(23, 43, 32, '44', 12, '11', 1)); //44
```

It's not parsing

```js
console.log(Math.max(23, 43, 32, '44ff', 12, '11', 1)); //NaN
```

### MIN VALUE

```js
console.log(Math.min(32, 23, 12, 5, 32, 45)); // 5
```

---

### There's also COUPLE OF CONSTANTS on Math object/namespace

```js
console.log(Math.PI); // 3.141592653589793
```

calculate Area of Circle with 10px radius. (PI(r^2))

```js
console.log(Math.PI * Number.parseFloat('10px') ** 2);
```

**Remember** to find square we use \*\*

---

### RANDOM FUNCTION

```js
console.log(Math.trunc(Math.random() * 6 + 1));
```

Generalize this formula by making a function which receive two numbers, min and max and based on that numbers it generates a random integers.

```js
const randomInt = (min, max) =>
  Math.floor(Math.random() * (max - min) + 1) + min;
// this is a formula =can check
console.log(randomInt(2, 4));
```

⤴General Function for **Generating Random Numbers** between two Numbers

---

### ROUNDING

#### Rounding Integers

**Math.trunc()** [simply remove decimal part]

```js
console.log(Math.trunc(34.43)); // 34
console.log(Math.trunc(34.8)); // 34
```

**Math.round()** [will round to nearest integer]

```js
console.log(Math.round(12.4)); // 12
console.log(Math.round(12.5)); // 13
console.log(Math.round(12.7)); // 13
```

**Math.ceil()** [Always round to up integer]

```js
console.log(Math.ceil(11.1)); // 12
console.log(Math.ceil(11.4)); // 12
console.log(Math.ceil(11.8)); // 12
```

**Math.floor()** [Always round down]

```js
console.log(Math.floor(11.1)); // 11
console.log(Math.floor(11.4)); // 11
console.log(Math.floor(11.9)); // 11
```

**Remember:** All of these also do type coercion. It means it'll automatically change string to number.

```js
console.log(Math.floor('33.2')); //33
console.log(Math.trunc('33.2')); //33
console.log(Math.ceil('33.2')); //34
console.log(Math.round('33.2')); //33
```

**floor** and **trunc** do exact same when we dealing with positive numbers However for negative number both doesn't work same.

```js
console.log(Math.trunc(-33.54)); // -33
console.log(Math.floor(-33.54)); // -34
```

So, **floor** is little bit better than **trunc**. it'll work accurate, no matter we are dealing with +ve or -ve numbers.

---

#### Rounding Decimals

Here little bit different

console.log((2.3).toFixed(0)); // 2 // syntax will be same as this one. first we write a number in parentheses and then call toFixed method where we pass number of numbers after decimal point. in this case we passed 0 so no number after decimal point. lets take some more example

```js
console.log((2.45).toFixed(4)); // 2.4500
console.log((2.34535).toFixed(2)); // 2.35
```

**Remember** toFixed method always return a string, not a number.  
If we have convert into number then we simply add + sign

```js
console.log(+(2.45).toFixed(2)); // now it's a number
```

---

## REMINDER_OPERATOR

It simply returns a reminder of any division.

```js
console.log(5 % 2); // 1
console.log(23 % 4); // 3
```

Most commonly use to check even and odd numbers

Checking an printing **even** and **odd** numbers

```js
const checkEvenOdd = (num) =>
  `${num} is ${num % 2 === 0 ? 'even' : 'odd'} number.`;
console.log(checkEvenOdd(31));
```

Checking for only even numbers.

```js
const isEven = (num) => num % 2 === 0;
console.log(isEven(23));
console.log(isEven(230));
console.log(isEven(232321));
console.log(isEven(22));
```

Receiving two numbers and check first is divisible by second or not

```js
const isDivisible = (x, y) => x % y === 0;
console.log(isDivisible(3, 2));
console.log(isDivisible(22, 2));
console.log(isDivisible(31, 3));
console.log(isDivisible(33, 3));
```

### Practice Exercise

lets selects all of rows of movements from html

```js
document.querySelectorAll('.movements__row');
```

Using spread operator covert this node to array

```js
labelBalance.addEventListener('click', function (e) {
  [...document.querySelectorAll('.movements__row')].forEach(function (row, i) {
    // 0, 2, 4, 6
    if (i % 2 === 0) {
      row.style.backgroundColor = 'orangered';
    }

    // 0, 3, 6, 9
    if (i % 3 === 0) {
      row.style.backgroundColor = 'blue';
    }
  });
});
```

To perform any operation every nth time the reminder is very good option. ( v useful operator )

---

## NUMERIC_SEPARATOR

[ES-2021]

Use to format numbers in away that is easier for us, or for other developer to read and to understand.

lets say we wanted to represent a really large number. for example the diameter of our solar system.

const diameterBad = 287460000000;  
we would write this number like this: 287,460,000,000. here numeric separator comes to play

Numeric Separators are simply underscores that we can place anywhere that we want in the number, which will make it really easy to understand.

```js
const diameterBad = 287460000000;
const diameterGood = 287_460_000_000;

console.log(diameterBad); //287460000000

console.log(diameterGood); // 287460000000 -same // JS engine basically ignores these underscores. (it's just for readable in program)
```

```js
const price = 345_99;

// const pi = 3._1415; // not allowed -error.
// const pi = \_3.1415; // not allowed -error.
// const pi = 31415_; // not allowed -error.
// const pi = 31\_\_\_415; // not allowed -error.
```

When we convert a string that contains underscores, to a number that will not work as expected.

```js
console.log(Number('340_000')); // NaN // not working.
```

---

## BIG_INT

[ ONE OF PRIMITIVE DATA TYPE ]

We learnt that **the numbers are represented internally 64 bits**, which means there are 64 ones or zeros to represent any number. From these 64 bits, only 53 are used to actually store the digits themselves, the rest are for storing the position of decimal point and sign.

If there are only 53 bits to store a number than there is a limit of how big number can be stored

we can calculate that number

```js
console.log(2 ** 53 - 1); // 9007199254740991
// this is the biggest number that js safely represent

console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991 SAME
```

It may create problem when interacting with way bigger than this one. like database ids, or when interacting with real 60 bit number. or from any API get a number that is larger than this.

BUT NOW these problems are solved in **ES-2020**, by introducing new primitive data type which is **BigInt**.**BigInteger:** we can store a number as log, no matter how big. In bigInt we write n at the end of the number.

```js
console.log(23234242342454345346456534345345345354389845); // If we use 'n' at the end, then it will bigInt like this
console.log(23234242342454345346456534345345345354389845n);
```

This n transform a regular number into a BigInt.

We can also create BigInt by using bigInt function. like this:

```js
console.log(BigInt(3434234472893479287376583465873848945799));
```

### SOME OPERATIONS ON BIG-INT

```js
console.log(10000n + 100000n);
console.log(
  76248364273498792387467863547890248792739487n *
    78937598374957348957938475938475893758389457398n
);
```

We can't mix **BigInt** with regular int. i.e can't do operations bigInt with regular numbers. like....

```js
console.log(100000 + 20000n); // error, can't mix BigInt and others.
const huge = 443245n;
const num = 442;
console.log(huge - num); // error, cant mix BigInt with others
```

Here we can convert this regular num with BigInt

```js
console.log(huge - BigInt(num));
```

**HOWEVER THERE ARE TWO EXCEPTIONS TO THIS**. which are the **compression operators** and **plus operator** when working with strings.

**First Exception** [compression operators]

```js
console.log(20n > 23); // false // can do this
console.log(20n === 20); // false // b/c triple equal will does't type coercion. this two values have different primitive type. lets check that

console.log(typeof 20, typeof 20n); // number bigint

// BUT
console.log(20n == 20); // now true. lose equality.
```

**Second Exception** [concatenation with string (+)]

```js
console.log(huge + ' is REALLY big!!!'); // ok. working
```

**Remember:** Also math operations will not work on BigInt

```js
console.log(Math.sqrt(16n)); // error
```

#### BIG-INT WITH DIVISION

```js
console.log(10n / 3n); // 3n cutting decimal part
console.log(10 / 3); // 3.3333333335
```

---

## DATES_AND_TIMES

[ Creating Dates and Times ]

### Creating Dates

- There are exact four ways of creating dates in Javascript
- The all use the new date constructor function, but they can accept different parameters.

1. First way is to **simple use the new date constructor** like this: new Date();

   ```js
   const now = new Date();
   console.log(now);
   ```

2. **Pass the date from a data string**

   ```js
   console.log(new Date('Sat Feb 04 2023'));

   console.log(new Date('14 august 2022'));
   ```

   Generally this is not a good idea to do this b/c it's quite unreliable, However is the string was actually created by javascript itself then of course it is pretty safe. lets take an example from our accounts we have movements date lets try to parse that dates.

   ```js
   console.log(new Date(account1.movementsDates[0]));
   ```

3. We can also pass years, months, days, hours, minutes, or sec.

   ```js
   console.log(new Date(2022, 10, 29, 15, 23, 59));
   ```

   On console here printing november so, it means in javascript months are 0 based.  
   **JavaScript also auto correct dates**

   ```js
   console.log(new Date(200, 10, 33)); // 33 -> 03 dec
   ```

4. We can also pass into the date constructor function, the amount of milliseconds passed since the beginning of the UNIX time, which is january 1, 1970.

   ```js
   console.log(new Date(0)); // yes, jan 01 1970
   ```

   Now create a date three days after this in milliseconds.

   ```js
   console.log(new Date(3 * 24 * 60 * 60 * 1000)); // now we get jan 4 1970
   ```

   This output is called timestamp.

**Remember:** These dates that we just created here are in fact just another special type of object. therefor they have their own methods. we can use these methods to get or set components of a date,

### Working With DATES. ( SOME PRACTICE EXERCISE )

```js
const future = new Date(2037, 10, 30, 15, 21);
console.log(future);
```

Here is future is an object so, we can apply some methods.

```js
console.log(future.getFullYear()); // 2037 / Always use getFullYear() to get a year. Their is another one getYear, don't use that
console.log(future.getMonth()); // 10 / remember this one is 0 based, so it's November
console.log(future.getDate()); // 30 / this is use to get day. it will get day of month.
console.log(future.getDay()); // 1 (monday) / use to get Day in week - starting from sunday.
console.log(future.getHours()); // 15
console.log(future.getMinutes()); // 21
console.log(future.getSeconds()); // 0
console.log(future.toISOString()); // it will convert a particular data object to string.

console.log(future.getTime()); // 2143189260000 / using this we can get timestamp for the date.
```

**Remember:** The timestamp is the milliseconds, which has been passed since january 1, 1970.

We can use this timestamp to get actual date.(reversed to date)

```js
console.log(new Date(2143189260000)); // will give the date that we've entered.
```

**Timestamp is soo important**, we've a special method that we can use to get the **timestamp** for right now.

```js
console.log(Date.now()); //1675491859637 / timestamp
```

There is also a **set version** of all of these methods.

```js
future.setFullYear(2040);
console.log(future); // changed to 2040;
// also exist for each methods. to set them.
```

---

### Performing_Some_Operations_On_Dates

**We can do calculation with dates.** For example we can subtract two dates to find how many days have passed between two dates. This work b/c whenever we convert date to a number, then the result is going to be the timestamp in milliseconds. And these milliseconds we can perform calculations.

```js
const future = new Date(2037, 10, 19, 15, 23);
console.log(Number(future)); // now it'll display in timestamp. Same thing'll happen when we add + operator on it. like this...
console.log(+future); // same result
```

**It means whenever we performs a mathematical operations it'll converts the date into timestamp.** Then again we've to convert that timestamp into a number.

NOW CREATE A FUNCTION that takes two dates and return the number of days b/w them.

```js
const calcDaysPassed = (date1, date2) =>
  Math.abs(date2 - date1) / (1000 * 60 * 60 * 24); // 1000 => convert millisecond to second, 60 => convert second to minute.... (All of these convert timestamp to days. )

const days1 = calcDaysPassed(new Date(2037, 3, 14), new Date(2037, 3, 24));
console.log(days1); // 10

const days2 = calcDaysPassed(new Date(2037, 3, 14), new Date(2037, 3, 4));
console.log(days2); // 10 // Math.abs(). used to remove -ve.

const days3 = calcDaysPassed(
  new Date(2037, 3, 4),
  new Date(2037, 3, 14, 10, 8)
);
console.log(days3); // 10.422222222222222
```

Using these we can display dates in movements in more decent way. like this:  
If any movement happened today, there should display today instead of date, also for yesterday, two days age, three days age  
These are really nice use-case of these type of functions. Implemented in Bankist Section ⬇⤵⏬

---

## INTERNATIONALIZATION_DATES

Javascript has a new **Internationalization API**, it allow us to easily format numbers and strings according to different languages. So, with this new API we can make our applications support different languages for users around the world, which is pretty important. for example currency and dates are represented in a completely different way in US or in Asia. Now there is a lot of language specific things that we can do with the internationalization API. But in this section we will talk about formatting dates and numbers.

### FORMATTING DATES USING INTERNATIONALIZATION API

#### EXPERIMENTING API

```js
const now = new Date();
const options = {
  hour: 'numeric',
  minute: 'numeric',
  day: 'numeric',
  // month: 'numeric', // 2 for february
  // month: '2-digit', // 02 for february
  month: 'long', // february
  year: 'numeric', // 2023
  // year: '2-digit' // 23

  weekday: 'long', // Saturday
  // weekday: 'narrow', // S
  // weekday: 'short', // Sat
};
labelDate.textContent = new Intl.DateTimeFormat('en-US', options).format(now);
```

Here **Intl** is a namespace for **internationalization**. In **DateTimeFormat()** function we need to pass local string, **local string** is usually the language-country, here as a second operator we can also put values itself or first store values in any object an put that object here like done here. All of these creates a new formatter, On that formatter we can call dot format(), and here we pass in the date that we have to format.

```js
labelDate.textContent = new Intl.DateTimeFormat('ar-SA').format(now); // wow. it's changed to sudiaArbia format.
labelDate.textContent = new Intl.DateTimeFormat('ur-PK').format(now);
```

In many situations, it actually make more sense to not define locale manually, but **instead to simply get it from the user's browser.** that's pretty easy to do as well. like this.

```js
const locale = navigator.language; // it will take from users browser.
// Now put this local variable into DateTimeFormat's parameter like this.
labelDate.textContent = new Intl.DateTimeFormat(locale, options).format(now);
```

---

### FORMATTING NUMBERS USING INTERNATIONALIZATION API

```js
const num = 3884764.23;

console.log('US: ', new Intl.NumberFormat('en-US').format(num));

console.log('Germany: ', new Intl.NumberFormat('de-DE').format(num));

console.log('Saudi: ', new Intl.NumberFormat('ar-SA').format(num));

console.log('Pakistan: ', new Intl.NumberFormat('ur-PK').format(num));

console.log('Browser: ', new Intl.NumberFormat(navigator.language).format(num));

console.log(
  navigator.language,
  new Intl.NumberFormat(navigator.language).format(num)
);
```

We can also use objects to formatting

```js
const num = 3883433.43;
const options = {
  style: 'unit',
  unit: 'mile-per-hour', // always check MDN documentation.
};
const options = {
  style: 'unit', // 'currency', 'percent' are also avaliable.
  // style: 'percent', // unit will ignored
  // style: 'currency', // should define currency after this
  // currency: 'usd',
  // currency: 'PKR',
  currency: 'EUR',
  unit: 'celsius', // always check MDN documantion

  // useGrouping: false, // it'll remove separator in numbers.

  // For any properties in that we set here in option, just check out MDN documantation.
};

console.log('US: ', new Intl.NumberFormat('en-US', options).format(num));

console.log('Germany: ', new Intl.NumberFormat('de-DE', options).format(num));

console.log('Saudi: ', new Intl.NumberFormat('ar-SA', options).format(num));

console.log('Pakistan: ', new Intl.NumberFormat('ur-PK', options).format(num));

console.log(
  'Browser: ',
  new Intl.NumberFormat(navigator.language, options).format(num)
);

console.log(
  navigator.language,
  new Intl.NumberFormat(navigator.language, options).format(num)
);
```

---

## TIMERS

TWO TYPES OF TIMERS **[setTimeout & setInterval]**  
**setTimeout** timer runs just once after a defined time while, **setInterval** timer keeps running forever, until we stop it.

### SET TIMEOUT FUNCTION

Basically we can use setTimeout to execute some code at some point in the future.

Let use this to simulate ordering a pizza. So when we order a pizza, it doesn't arrive right away, so instead it gonna take some time and we can simulate that.

**setTimeout** function receives two arguments **1. callback function** **2. specify time in milliseconds**

```js
setTimeout(() => console.log('Here is your Pizza 🍕'), 5000);
console.log('I am from after the setTimer function.'); // it will display first.
```

But Remember that the code execution does not stop at this point. execution will continues by registering that callback function will be call after the elapsed of that time. This mechanism is called Asynchronous Javascript. (Detail Later)

**Passing arguments to callback function** (that is not an easy b/c we are not calling by itself.) But the setTimeout function has solution of that, basically all the arguments that we passed after the time delay will consider as argument to the callback function.

```js
setTimeout(
  (ing1, ing2) => console.log(`Here is your pizza with ${ing1} and ${ing2}`),
  5000,
  'olives',
  'spinash' // last two are argument to the function.
);
```

**We can cancel the timer**, at least until the delay has passed. So before 5 seconds we can cancel timeout. HOW? like this.

```js
const ingredients = ['olvives', 'spinash'];
const pizzaTimer = setTimeout(
  (ing1, ing2) => console.log(`Here is your pizza with ${ing1} and ${ing2}`),
  5000,
  ...ingredients
);
console.log('waiting....');

if (ingredients.includes('spinash')) clearTimeout(pizzaTimer); // here we have clear/delete the timer. here we'll not see this callback function in console.
```

lets go back to our application and simulate the loan, by taking five seconds ofter click the button. ⤵⏬

---

### SET INTERVAL FUNCTION

clock (as a challenge)

```js
setInterval(function () {
  const now = new Date();

  console.log(`${now.getHours()}:${now.getMinutes()} ${now.getSeconds()}`);
}, 1000);
```

_06-Feb-2023_

---

---

## BANKIST_APP

### DIFFERENT DATA! Contains movement dates, currency and locale

```js
const account1 = {
  owner: 'Muhammad Ahmad',
  movements: [200, 455.23, -306.5, 25000, -642.21, -133.9, 79.97, 1300],
  interestRate: 1.2, // %
  pin: 1111,

  movementsDates: [
    '2019-11-18T21:31:17.178Z',
    '2019-12-23T07:42:02.383Z',
    '2020-01-28T09:15:04.904Z',
    '2020-04-01T10:17:24.185Z',
    '2020-05-08T14:11:59.604Z',
    '2023-01-28T17:01:17.194Z',
    '2023-02-01T23:36:17.929Z',
    '2023-02-03T10:51:36.790Z',
  ],
  currency: 'EUR',
  locale: 'pt-PT', // de-DE
};

const account2 = {
  owner: 'Jessica Davis',
  movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
  interestRate: 1.5,
  pin: 2222,

  movementsDates: [
    '2019-11-01T13:15:33.035Z',
    '2019-11-30T09:48:16.867Z',
    '2019-12-25T06:04:23.907Z',
    '2020-01-25T14:18:46.235Z',
    '2020-02-05T16:33:06.386Z',
    '2020-04-10T14:43:26.374Z',
    '2023-01-01T18:49:59.371Z',
    '2023-01-03T12:01:20.894Z',
  ],
  currency: 'USD',
  locale: 'en-US',
};

const accounts = [account1, account2];
```

### Elements

```js
const labelWelcome = document.querySelector('.welcome');
const labelDate = document.querySelector('.date');
const labelBalance = document.querySelector('.balance**value');
const labelSumIn = document.querySelector('.summary**value--in');
const labelSumOut = document.querySelector('.summary**value--out');
const labelSumInterest = document.querySelector('.summary**value--interest');
const labelTimer = document.querySelector('.timer');

const containerApp = document.querySelector('.app');
const containerMovements = document.querySelector('.movements');

const btnLogin = document.querySelector('.login__btn');
const btnTransfer = document.querySelector('.form__btn--transfer');
const btnLoan = document.querySelector('.form__btn--loan');
const btnClose = document.querySelector('.form__btn--close');
const btnSort = document.querySelector('.btn--sort');

const inputLoginUsername = document.querySelector('.login**input--user');
const inputLoginPin = document.querySelector('.login__input--pin');
const inputTransferTo = document.querySelector('.form__input--to');
const inputTransferAmount = document.querySelector('.form__input--amount');
const inputLoanAmount = document.querySelector('.form__input--loan-amount');
const inputCloseUsername = document.querySelector('.form__input--user');
const inputClosePin = document.querySelector('.form__input--pin');
```

### Functions

```js
const formatMovementDate = function (date, locale) {
const calcDaysPassed = (date1, date2) => Math.round(Math.abs(date2 - date1) / (1000 _60_ 60 \* 24));

const daysPassed = calcDaysPassed(new Date(), date);
console.log(daysPassed)

if (daysPassed === 0) return 'Today';
if (daysPassed === 1) return 'Yestarday';
if (daysPassed <= 7) return `${daysPassed} days age`;
else {

    // const day = `${date.getDate()}`.padStart(2, 0);
    // const month = `${date.getMonth()}`.padStart(2, 0);
    // const year = date.getFullYear(); // b/c this's zero based.
    // return `${day}/${month}/${year}`;


    return new Intl.DateTimeFormat(locale).format(date);

}
};
```

General function that can take any value, any locale & any currency, then will return it nicely formatted according to that locale.

```js
const formatCur = function (value, locale, currency) {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency: currency,
  }).format(value);
};
```

Now we are also displaying each movement's date.

```js
const displayMovements = function (acc, sort = false) {
  containerMovements.innerHTML = '';
  const movs = sort
    ? acc.movements.slice().sort((a, b) => a - b)
    : acc.movements;

  movs.forEach(function (mov, i) {
    const type = mov > 0 ? 'deposit' : 'withdrawal';

    const date = new Date(acc.movementsDates[i]);

    const displayDates = formatMovementDate(date, acc.locale);

    const formattedMov = formatCur(mov, acc.locale, acc.currency);
    // Refactored by using function (upper)
    // new Intl.NumberFormat(acc.locale, {
    //   style: 'currency',
    //   currency: acc.currency,
    // }).format(mov)

    const html = `
      <div class="movements__row">
        <div class="movements__type movements__type--${type}">${
      i + 1
    } ${type}</div>
        <div class="movements__date">${displayDates}</div>
        <div class="movements__value">${formattedMov}</div>
      </div>
    `;

    containerMovements.insertAdjacentHTML('afterbegin', html);
  });
};
```

```js
const calcDisplayBalance = function (acc) {
acc.balance = acc.movements.reduce((acc, mov) => acc + mov, 0);

labelBalance.textContent = formatCur(acc.balance, acc.locale, acc.currency)
};

const calcDisplaySummary = function (acc) {
const incomes = acc.movements
.filter(mov => mov > 0)
.reduce((acc, mov) => acc + mov, 0);
labelSumIn.textContent = formatCur(incomes, acc.locale, acc.currency);

const out = acc.movements
.filter(mov => mov < 0)
.reduce((acc, mov) => acc + mov, 0);
labelSumOut.textContent = formatCur(out, acc.locale, acc.currency);

const interest = acc.movements
.filter(mov => mov > 0)
.map(deposit => (deposit \* acc.interestRate) / 100)
.filter((int, i, arr) => {
// console.log(arr);
return int >= 1;
})
.reduce((acc, int) => acc + int, 0);
labelSumInterest.textContent = formatCur(interest, acc.locale, acc.currency);
};

const createUsernames = function (accs) {
accs.forEach(function (acc) {
acc.username = acc.owner
.toLowerCase()
.split(' ')
.map(name => name[0])
.join('');
});
};
createUsernames(accounts);

const updateUI = function (acc) {
// Display movements
displayMovements(acc);

// Display balance
calcDisplayBalance(acc);

// Display summary
calcDisplaySummary(acc);
};
```

#### Function to handle logout timer

```js
const startLogoutTimer = function () {
  const tick = function () {
    // converting seconds to min. b/c we wants minutes also.
    const min = String(Math.trunc(time / 60)).padStart(2, 0);
    const sec = String(time % 60).padStart(2, 0); // reminder of  time/60 should be seconds. (think it's )

    //3. In each call print the remaining tiem to UI
    labelTimer.textContent = `${min}: ${sec}`;

    // 4. when reached 0 sec, stop timer and logout user
    if (time === 0) {
      clearInterval(timer);
      labelWelcome.textContent = 'Log in to get started';
      containerApp.style.opacity = 0;
    }

    // Decrease one second in each call
    time--;
  };
  // 1. set time to 5 minutes
  let time = 30;

  tick();
  // 2. call the timer every seconds.
  const timer = setInterval(tick, 1000);

  return timer;
};
```

Here tick is very common name of this function, if we directly insert this function in timeInterval as a callback function then it will take some time to start may be one or half seconds. so will will build this function and then will call this explicitly once logged in.⤴

// some pseud code to what exactly we are doing here..
// 1-set time to 5 minutes.
// 2-call the timer every second.
// 3-in each call print the remaining time to the UI.
// 4-when reached 0 second, stop timer and logout user.

// 1. set time to 5 minutes
let time = 30;

tick();
// 2. call the timer every seconds.
const timer = setInterval(tick, 1000)

⤴⬆ setting time interval like this is very common in development.

---

### Event handlers

```js
let currentAccount, timer;

// FAKE ALWAYS LOGGED IN [always logged in]
// currentAccount = account1;
// updateUI(currentAccount);
// containerApp.style.opacity = 100;

// Remember : this date & time is static. to change we've to use timer. (will do later)

btnLogin.addEventListener('click', function (e) {
  // Prevent form from submitting
  e.preventDefault();

  currentAccount = accounts.find(
    (acc) => acc.username === inputLoginUsername.value
  );
  console.log(currentAccount);

  if (currentAccount?.pin === Number(inputLoginPin.value)) {
    // Display UI and message
    labelWelcome.textContent = `Welcome back, ${
      currentAccount.owner.split(' ')[0]
    }`;
    containerApp.style.opacity = 100;

    // display current date and time

    const now = new Date();
    const options = {
      hour: 'numeric',
      minute: 'numeric',
      day: 'numeric',
      month: 'numeric', // 2 for february
      // month: '2-digit', // 02 for february
      // month: 'long',  // february
      year: 'numeric', // 2023
      // year: '2-digit' // 23

      // weekday: 'long', // Saturday
      // weekday: 'narrow', // S
      // weekday: 'short', // Sat
    };
    // const locale = navigator.language;
    labelDate.textContent = new Intl.DateTimeFormat(
      currentAccount.locale,
      options
    ).format(now);

    // const now = new Date();
    // // labelDate.textContent = now; // it's printing like this: As of Sat Feb 04 2023 11:46:51 GMT+0500 (Pakistan Standard Time) BUT we want to display like this: day/month/year only So,
    // const day = `${now.getDate()}`.padStart(2, 0);
    // const month = `${now.getMonth()}`.padStart(2, 0);
    // const year = now.getFullYear() + 1; // b/c this's zero based.
    // const hour = `${now.getHours()}`.padStart(2, 0);
    // const min = `${now.getMinutes()}`.padStart(2, 0);
    // labelDate.textContent = `${day}/${month}/${year}, ${hour}:${min}`

    // Clear input fields
    inputLoginUsername.value = inputLoginPin.value = '';
    inputLoginPin.blur();

    if (timer) {
      clearInterval(timer);
    }
    // calling logout timer function
    timer = startLogoutTimer();

    // Update UI
    updateUI(currentAccount);
  }
});
```

```js
btnTransfer.addEventListener('click', function (e) {
  e.preventDefault();
  const amount = Number(inputTransferAmount.value);
  const receiverAcc = accounts.find(
    (acc) => acc.username === inputTransferTo.value
  );
  inputTransferAmount.value = inputTransferTo.value = '';

  if (
    amount > 0 &&
    receiverAcc &&
    currentAccount.balance >= amount &&
    receiverAcc?.username !== currentAccount.username
  ) {
    // Doing the transfer
    currentAccount.movements.push(-amount);
    receiverAcc.movements.push(amount);

    // Add transfer Date
    currentAccount.movementsDates.push(new Date().toISOString());
    receiverAcc.movementsDates.push(new Date().toISOString());

    // Update UI
    updateUI(currentAccount);

    // Reset the Timer
    clearInterval(timer);
    timer = startLogoutTimer();
  }
});
```

```js
btnLoan.addEventListener('click', function (e) {
e.preventDefault();

const amount = Math.floor(inputLoanAmount.value);

if (amount > 0 && currentAccount.movements.some(mov => mov >= amount \* 0.1)) {
// Add movement
setTimeout(function () {
currentAccount.movements.push(amount);

      // Add loan Date
      currentAccount.movementsDates.push(new Date().toISOString());

      // Update UI
      updateUI(currentAccount);

      // Reset the Timer
      clearInterval(timer);
      timer = startLogoutTimer();

    }, 5000)

}
inputLoanAmount.value = '';
});
```

```js
btnClose.addEventListener('click', function (e) {
  e.preventDefault();

  if (
    inputCloseUsername.value === currentAccount.username &&
    +inputClosePin.value === currentAccount.pin
  ) {
    const index = accounts.findIndex(
      (acc) => acc.username === currentAccount.username
    );
    console.log(index);
    // .indexOf(23)

    // Delete account
    accounts.splice(index, 1);

    // Hide UI
    containerApp.style.opacity = 0;
  }

  inputCloseUsername.value = inputClosePin.value = '';
});
```

```js
let sorted = false;
btnSort.addEventListener('click', function (e) {
  e.preventDefault();
  displayMovements(currentAccount.movements, !sorted);
  sorted = !sorted;
});
```
