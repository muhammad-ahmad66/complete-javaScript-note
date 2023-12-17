# FUNDAMENTALS OF JAVASCRIPT

## Table of Contents

1. [STRICT_MODE](#STRICT_MODE)
2. [FUNCTIONS](#FUNCTIONS)
3. [ARRAYS](#ARRAYS)
4. [OBJECTS](#OBJECTS)
5. [LOOPS](#LOOPS)
   1. [FOR_LOOP](#FOR_LOOP)
   2. [CONTINUE_AND_BREAK_STATEMENT](#CONTINUE_AND_BREAK_STATEMENT)
   3. [WHITE_LOOP](#WHITE_LOOP)

---

## STRICT_MODE

```js
'use strict';
```

Activate STRICT MODE. it provide more secure environment and help us detect and fix bugs.<br>
let's check strict mode

```js
let hasDrivingLicense = false;
const passTest = true;
if (passTest) {
hasDrivingLicense = true; //creating some spell mistakes in variable hadDrivingLicense.
}
if (hasDrivingLicense) {
console.log("I can drive!!!")
}
let if = 43;
```

**ALWAYS ACTIVATE STRICT MODE.** STRICT ACTIVATION SHOULD BE TOP OF SCRIPT (BEFORE WRITING A SINGLE CODE.)

---

## FUNCTIONS

Calling functions are also called '**invoking**' or '**running**'.

```js
function fruitProcessor(apples, oranges) {
  console.log(apples, oranges);
  let juice = `Juice with ${apples} apples and ${orange}oranges.`;
  return juice;
}
fruitProcessor(5, 0);
```

\*\*Storing the return value from the function:

```js
const appleJuice = fruitProcessor(5, 0); //It stores the return value from the function.
console.log(appleJuice); //It should print juice's value

const appleOrangeJuice = fruitProcessor(2, 4);
console.log(appleOrangeJuice);
```

#### Function Declarations VS Function Expressions

1.  **FUNCTION DECLARATIONS:**
    It is a Regular function that we often use. like this:

    ```js
    function calcAge1(birthYear) {
      return 2022 - birthYear;
    }
    const age1 = calcAge1(1991);
    console.log(age1);
    ```

2.  **FUNCTION EXPRESSIONS:**
    _We build a function without a name and stor it in any variable_, this variable is like a function name.

    ```js
    const calcAge2 = function (birthYear) {
      return 2022 - birthYear;
    };
    const age2 = calcAge2(1991);
    console.log(age1, age2); //31 31
    ```

    **_REMEMBER THAT:_** _THIS FUNCTION IS AN EXPRESSION. HERE WHOLE FUNCTION IS A VALUE, SO WE CAn STORE IN VARIABLE._<br>
    NOTE: FUNCTIONS ARE VALUE.<br>
    BIG DIFFERENCE B/W FUNCTION DECLARATION AND FUNCTION EXPRESSION IS THAT: **we can call function declaration before/ above the function definition BUT in expression we can't.**

3.  **ARROW FUNCTION**
    It's a newer syntax for defining functions.
    More compact than regular function, but they do the same thing.
    It allows to write functions without key word 'function'.
    **_syntax look like this:_**
    _const sum = (x, y) => {
    return x + y;
    }_

    CODE EXAMPLE:

    ```JS
         const square = (num) => {
         return num * num;
         }

         //it's just like
         function square(num) {
         return num * num
         }
         console.log(square(4)); //16
    ```

    **IF there is a single parameter then parentheses are optional**. we can write like this.

    ```js
    const square = (num) => {
      return num * num;
    };
    console.log(square(5)); //25
    ```

    **BUT IF there is NO PARAMETER or MORE THAN ONE then it will give error!!** we should've to write parentheses even empty!!.

    ```js
    const rollDie = () => {
      return Math.floor(Math.random() * 6) + 1;
    };
    console.log(rollDie());

    const sum = (x, y) => {
      return x + y;
    };
    console.log(sum(5, 45)); //50
    ```

    **ARROW FUNCTION will implicitly return value.** by using this we can make more compact.

    ```js
     const rollDie = () => {
     Math.floor(Math.random() _ 6) + 1;
     }

      console.log(rollDie()); //undefined//that will not return any value.
    ```

    **To return implicitly return we use parenthesis instead of curly bracket.** like this!!!!!!!

    ```js
    const rollDio = () => Math.floor(Math.random() * 6) + 1;
    console.log(rollDio()); // working!!!! will give some number.
    ```

    **If we have short code in function then implicit return is very useful**. we can write like this

    ```js
    const add = (x, y) => x + y; //function in just one line!!!
    ```

    Again we can compact with get rid of parentheses.
    NOTE: **To remove the parentheses, the function and statement should in one line.**

    **_WE CAN SUMMARIZE_**<BR><BR>
    **REGULAR FUNCTION**

    ```JS
     function add(x, y) {
     return x + y;
     }
    ```

    **ARROW WITH RETURN**

    ```JS
    const add = (x, y) => {
     return x + y;
     }
    ```

    **ARROW WITHOUT RETURN WITH PARENTHESES**

    ```JS
     const add = (x, y) => ( a
    x + y
    );
    ```

    **ARROW WITHOUT RETURN AND WITHOUT PARENTHESES AROUND PARAMETER AND STATEMENT**

    ```JS
     const add = (x, y) => x + y
    ```

    **ARROW WITHOUT RETURN WITH PARENTHESES**

    ```JS
     const isEven = num => num % 2 === 0;
    ```

---

### FUNCTIONS_CALLING_OTHER_FUNCTIONS

```JS
function cutFruitPieces(fruit) {
  return fruit * 4;
}
function fruitProcessor(apples, oranges) {
  const applePieces = cutFruitPieces(apples);
  const orangePieces = cutFruitPieces(oranges);
  const juice = `Juice with ${applePieces} pieces of apple and ${orangePieces} pieces of orange.`;
  return juice;
}
console.log(fruitProcessor(3, 2));
```

---

## ARRAYS

```JS
const friends = ["Shoaib", "Huzaifa", "Rashid", "Zakir"];
console.log(friends);

```

**ANOTHER WAY TO CREATING AN ARRAY**. we use array function.

```JS
const years = new Array(1996, 1997, 1998, 1999, 2000);
console.log(years);
```

```JS
console.log(friends[0]);
console.log(friends.length); //4
console.log(friends[friends.length - 1]); //to access last element.
```

**_Although we use const keyword to declare an array but still we can change elements in array. BUT note that we can't change entire array like this⤵_**

```JS
friends[2] = "Yaseen";
friends = ["Aqib", "Najm"]; // error
console.log(friends[2]);
```

**In javascript an Array can hold elements of different data types. even an array of array or an expression or any variable etc.**

```js
const firstName = 'Muhammad';
const birthYear = 2000;

const myData = [firstName, 'Ahmad', 2022 - birthYear, 'Student', friends];
console.log(myData);
```

Array Exercise:

```js
const calcAge = function (birthYear) {
  return 2037 - birthYear;
};

const years = [1990, 1967, 2002, 2010, 2018];

const age1 = calcAge(years[0]);
const age2 = calcAge(years[1]);
const age3 = calcAge(years[years.length - 1]);
console.log(age1, age2, age3);

const ages = [
  calcAge(years[0]),
  calcAge(years[1]),
  calcAge(years[years.length - 1]),
];
console.log(ages);
```

<hr>

### BASIC ARRAY METHODS (OPERATIONS)

**_PUSH METHOD_**<br>
_USE to add element at end of array_

```js
const friends = ['Shoaib', 'Huzaifa', 'Rashid', 'Zakir'];
friends.push('Younus');
console.log(friends);
```

NOTE: PUSH is a function which **accept arguments as well as return a value.** Argument should be a value we want to add in Array and **return value will length of Array after pushed**.

```js
const newLength = friends.push('Yaseen', 'Aqib');
console.log(friends); // friends array values....
console.log(newLength); // 7 (now array length is 7)
```

<hr>

**_UNSHIFT METHOD_**<br>
_USE to add element at beginning of an array._<br>
**It also get value as a parameter and also return new length of array after adding, just like push() method**

```js
const newArrayLength = friends.unshift('Najm');
console.log(friends);
console.log(newArrayLength); //8
```

<hr>

**_POP METHOD_**<br>
_USE to remove the last element._<br>
**It does not accept any argument but it returns the removed (poped) element.**

```js
const firstRemoved = friends.pop(); //No argument needed.
const secondRemoved = friends.pop();
console.log(friends); //Yaseen & Aqib should remove
console.log(secondRemoved); //Yaseen
```

<hr>

**_SHIFT METHOD_**<br>
_USE to remove form start._ all thing is just like pop

```js
const shiftedEle = friends.shift();
console.log(friends); //Najm should removed
console.log(shiftedEle); //Najm
```

<hr>

**_indexOf METHOD_**<br>
_USE to tell about index of particular element._

```js
console.log(friends.indexOf('Zakir')); //3
```

**if we try to find indexOf any element that's not in array, than it will simply return -1**

```js
console.log(friends.indexOf('Peer')); //-1
```

<hr>

**_INCLUDES METHOD_**<br>
_It will return true if the element is in the array and false if not._<br>
_It will check using strict equality check._

```js
console.log(friends.includes('Rashid')); //true
console.log(friends.includes('Saud')); //false
```

We can use includes in conditions.(UseCase)

```js
if (friends.includes('Huzaifa')) {
  console.log('You have a friend called Huzaifa.');
}
```

---

## OBJECTS

#### RECAP FROM ARRAY

```js
const myDataArray = [
  'Muhammad',
  'Ahmad',
  2022 - 2000,
  'Student',
  ['Huzaifa', 'Shoaib', 'Rashid', 'Zakir'],
];
```

**In array we can't give 'muhammad' to fName, 'ahmad' to lName, BUT HERE CAME OBJECT...**<br>
**OBJECT IS BASICALLY KEY - VALUE PAIRS.**<br>

```js
const myData = {
  firstName: 'Muhammad',
  lastName: 'Ahmad',
  age: 2022 - 2000,
  job: 'student',
  friends: ['Huzaifa', 'Shoaib', 'Rashid', 'Zakir'],
};
console.log(myData);
```

Here fistName, age, job, etc are called KEYS and 'Muhammad', 'Student' etc ara called VALUES. (keys are also called properties)<br>

**BIG DIFFERENCE B/W ARRAYS AND OBJECT IT THAT, arrays are depends on ORDER, but object not!!!!!!**

**_Two ways to getting (Retrieve) a property from an object._**

1. _Using dot (.) operator_
   ```js
   console.log(myData.firstName);
   console.log(myData.age);
   ```

2) _Using square bracket_

   ```

     console.log(myData['firstName']);
     console.log(myData['age']);
   ```

Difference b/w these two is that **in bracket notation we can put any expression in square bracket.**

```js
const nameKey = 'Name';
console.log(myData['first' + nameKey]);
console.log(myData['last' + nameKey]);


// Here name holds value 'Name', and then we concatenate with first and last.
// so we see 'first' + nameKey is a expression

// we can't write same thing with . operator.
console.log(myData.'first' + nameKey) //error
```

**Another example:**

```js
const interestedIn = prompt(
  'What do you want to know about me? Choose between firstName, lastName, age, job, friends.'
);
console.log(myData.interestedIn); //undefined.
// Because myData have no property named interestedIn. Note that!! . operator always works on directly property (key). so we have to use bracket notation to access.

console.log(myData[interestedIn]); //WoW!! working..
```

REMEMBER THAT: IF we want to access any property that's not exist in object. it will print undefined...USE this to build a logic....let try

```js
if (myData[interestedIn]) {
  console.log(myData[interestedIn]);
} else {
  console.log(
    'Wrong Request! Choose between firstName, lastName, age, job, friends.'
  );
}
```

**ADD NEW PROPERTIES TO THE OBJECT USING DOT AND BRACKET NOTATION**

```js
myData.location = 'Yougo-Baltistan';
myData['mail'] = 'muhammadugv66@gmail.com';
console.log(myData);
```

**QUICK CHALLENGE:**<br>
PRINT USING DOT AND BRACKET: Muhammad has 4 friends and his best friend is Huzaifa.

```js
console.log(
  `${myData.firstName} has ${myData.friends.length} friends, and his best friend is ${myData.friends[0]}`
);
```

---

### OBJECT METHODS

```js
const myData = {
  firstName: 'Muhammad',
  lastName: 'Ahmad',
  birthYear: 2000,
  job: 'student',
  friends: ['Huzaifa', 'Shoaib', 'Rashid', 'Zakir'],
  hasDriversLicense: true,
  calcAge: function (birthYear) {
    //it's just regular function expression.
    // Remember that: here we cant't use function declaration. in object we need an expression
    return `${2022 - birthYear} years`;
  },
};
```

To use any property within this object we use **THIS** keyword

```js
calcAge: function () {
console.log(this); //prints all key-value from this object where this function is pointing.
return 2022 - this.birthYear;
//one step more further!!!
this.age = 2022 - this.birthYear;
return this.age;//although we did't need of return statement.
},
```

If we have to access multiple time this function(calcAge)⤴ then every time we need to calculate an age. that will not take a bad effect in this case, but if we have large calculation then this may take large time so avoid of that we made a property that store the resultant value from this function. so age is a new property added in object myData.

**QUICK CHALLENGE:**
create a new method which summarize the person's detail like this.
_'Muhammad' is a 22 years old, and he has a driver's license._

```js

getSummery: function () {
console.log(`${this.firstName} is a ${this.age}-years old, and he has ${this.hasDriversLicense ? 'a ' : 'no '} driving license`);
 }

```

```js
console.log(myData.calcAge(2000));
console.log(myData['calcAge'](2000));

console.log(myData.calcAge());
console.log(myData.age);
console.log(myData.getSummery());
```

---

## LOOPS

### FOR_LOOP

```js
const types = [];
const myData = [
  'Muhammad',
  'Ahmad',
  2022 - 2000,
  'student',
  ['Huzaifa', 'Shoaib', 'Rashid', 'Zakir', 'Umair'],
];

for (let i = 0; i < myData.length; i++) {
  //reading form myData array
  console.log(myData[i], typeof myData[i]);

  //filling an array types
  types[i] = typeof myData[i];

  //another way to fill data using loop
  types.push(typeof myData[i]);
}
console.log(types);
```

In above example we made an array types by giving a value of same data-type as myData's elements have, using loop.

```js
const years = [1991, 2007, 1969, 2020];
const ages = [];
for (let i = 0; i < years.length; i++) {
  // ages[i] = 2022 - years[i];
  // Another way
  ages.push(2022 - years[i]);
}
console.log(ages);
```

---

### CONTINUE_AND_BREAK_STATEMENT

**continue** AND **break** statement in loops<br>

**continue:** Use to exit the current iteration of the loop and start the next one.
**Break:** Use to completely terminates the whole loop.

```js
console.log('------Only Strings.');
for (let i = 0; i < myData.length; i++) {
  if (typeof myData[i] !== 'string') continue; //to print only elements which have strings data type
  // If there is any 'number' type then, leave it and go to the next iteration.
  console.log(myData[i], typeof myData[i]);
}
```

```js
console.log('------Break with numbers.');
for (let i = 0; i < myData.length; i++) {
  if (typeof myData[i] === 'number') break;
  // If there is any 'number' type then, terminate the loop
  console.log(myData[i], typeof myData[i]);
}
```

**Access Array from backward (reverse)**

```js
const types = [];
const myData = [
  'Muhammad',
  'Ahmad',
  2022 - 2000,
  'student',
  ['Huzaifa', 'Shoaib', 'Rashid', 'Zakir', 'Umair'],
];

for (let i = myData.length - 1; i >= 0; i--) {
  console.log(i, myData[i]);
}
```

---

### WHITE_LOOP

```JS
let dice = Math.trunc(Math.random() * 6) + 1;
while (dice !== 6) {
console.log(`You rolled a ${dice}`);
dice = Math.trunc(Math.random() * 6) + 1;
if (dice === 6) {
console.log("Loop is about to end...");
}
}
```

---
