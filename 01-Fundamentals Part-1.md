# VERY BASIC AND FUNDAMENTALS OF JAVASCRIPT

## Table of Contents

1. [STARTING](#STARTING)
2. [TYPEOF](#TYPEOF)
3. [VARIABLES](#VARIABLES)
4. [JAVASCRIPT_OPERATORS](#JAVASCRIPT_OPERATORS)
5. [STING_AND_TEMPLATE_LATERALS](#STING_AND_TEMPLATE_LATERALS)
6. [DECISION_MAKING](#DECISION_MAKING)
7. [TYPE_CONVERSION_AND_COERCION](#TYPE_CONVERSION_AND_COERCION)
8. [TRUTHY_AND_FALSY_VALUES](#TRUTHY_AND_FALSY_VALUES)
9. [EQUALITY_OPERATORS](#EQUALITY_OPERATORS)
10. [LOGICAL_OPERATORS](#LOGICAL_OPERATORS)
11. [SWITCH_STATEMENT](#SWITCH_STATEMENT)
12. [STATEMENT_VS_EXPRESSION](#STATEMENT_VS_EXPRESSION)
13. [CONDITIONAL-TERNARY_OPERATOR](#CONDITIONAL-TERNARY_OPERATOR)

## STARTING

```js
console.log('Hi!!!!');
let PI = 3.1415;
console.log(javascriptIsFun);
```

---

## TYPEOF

```js
let javascriptIsFun = true;
console.log(typeof true); //boolean
console.log(typeof javascriptIsFun); //boolean
console.log(typeof 222); //number
console.log(typeof 'Random Text'); //string
```

**_IN JAVA SCRIPT WE CAN CHANGE TYPE OF ANY VARIABLE BY INITIaLIZING IT WITH DIFFERENT DATA TYPE._**<br>
_ALSO A JAVASCRIPT DATA TYPE DEPENDS ON VALUES WE SHOULD NOT SPECIFY ANY DATA TYPE._

```js
javascriptIsFun = 'Yes!'; //changed data type of variable
console.log(typeof javascriptIsFun); //Now string
```

**_undefined datatype_**

```js
let year;
console.log(year); //undefined
console.log(typeof year); //undefined
```

**typeof always gives us the type in string**, like 'object', 'string', 'number' etc.

```js
year = 2000;
console.log(typeof year); //number

console.log(typeof null); //object!!! this is a bug in javascript.
```

---

## VARIABLES

**THREE DIFFERENT WAYS TO DECLARING VARIABLE IN JS.**

1. VAR
2. CONST
3. LET

```JS
let age = 27; //changEable  // mutable
const dateOfBirth = 2000;   // unchangeable
```

**RECOMMENDED: ALWAYS TRY TO USE CONST AS MUCH AS POSSIBLE INSTEAD OF LET. (GOOD PRACTICE)**

---

## JAVASCRIPT_OPERATORS

**Mathematical operator**

```JS
const now = 2022;
const myAge = now - 1996;
const jonasAge = now - 1987;
console.log(myAge, jonasAge); //26 35
console.log(myAge * 2, jonasAge / 2, 2 ** 3); //52 17.5 8
```

**We can use + operator to concatenates/join strings.**

```JS
const firstName = "Muhammad";
const lastName = "Ahmad";
console.log(firstName + " " + lastName);
```

**ASSIGNMENT OPERATORs**

```JS
let x = 3 + 2;
console.log(x); //5
x += 10;
console.log(x); //15
x *= 10;
console.log(x); //15 _ 10 = 150
x++; // x= x+1;
console.log(x); //151
```

**Compression Operators**<BR>
_COMPRESSION OPERATORS ALWAYS PRODUCE BOOLEAN VALUES. TRUE/FALSE._

```JS
console.log(myAge > jonasAge); //false
console.log(myAge >= 18); //true
const isFullAge = myAge >= 18;
console.log(isFullAge); //true
console.log(typeof isFullAge); //boolean
```

**PRECEDENCE OF OPERATORS.** _check out MDN_
_GENERALLY : MATH > COMPRESSION_

---

## STING_AND_TEMPLATE_LATERALS

```js
const firstName = 'Muhammad';
const currentJob = 'Student';
const birthYear = 2000;
const year = 2022;

const muhammad =
  "I'm " +
  firstName +
  ', a ' +
  (year - birthYear) +
  ' years old ' +
  currentJob +
  '!';
console.log(muhammad);
```

**Alternative to this one⤴.**

```js
const muhammadNew = `I'm ${firstName}, a ${
  year - birthYear
} years old ${currentJob}!`;
console.log(muhammadNew);
```

```js
console.log(
  'Strings\n\
with\n\
multi-lines'
);
```

**Alternative of this⤴**

```js
console.log(`strings
with
multi-lines.`);
```

---

## DECISION_MAKING

**IF STATEMENT**

```js
const age = 15;
const isOldEnough = age >= 18;
if (isOldEnough) {
  console.log('Can start driving license 🚗');
}
```

```js
if (age >= 18) {
console.log("You can start driving license 🚗") [click WINDOW KEY + . to insert emojis]
}
else {
const yearsLeft = 18 - age;
console.log(`You are too young. Wait another ${yearsLeft} years.`);
}
```

---

## TYPE_CONVERSION_AND_COERCION

**Type Conversion:** manually conversion of type

```js
const inputYear = '1991'; //it's a string.
console.log(inputYear + 18); //199118. b/c it treated as a string.
```

### TYPE CONVERSION

**TO CONVERT IN NUMBER:**

```js
console.log(Number(inputYear), inputYear); //1991 '1991' one is converter to number and last one is still string. if we convert using Number function, original data will not change. inputYear is still string.
console.log(inputYear + 18); //199119 result is still string
console.log(Number(inputYear) + 18); //now 2009
```

**If we try to convert into a number that is not possible, javascript will return NaN.**(not a number)

```js
console.log(Number('Muhammad')); //NaN
```

**To convert number to string:**

```js
console.log(String(18), 18); //'18', 18
```

### TYPE COERCION

**Type Coercion:** Automatically conversion by Javascript

```js
console.log("I'm " + 23 + ' Years Old.'); // convert number to string.
console.log('12' - '10' - 1); //converts string to number.
```

**_SO IF WE USE PLUS OPERATOR B/W STRINGS AND NUMBERS THAN, NUMBERS WILL CONVERT TO STRING. AND IF USE MINUS/MULTIPLY/DIVISION OPERATOR THAN, STRINGS WILL CONVERT TO NUMBER._** _LET CHECK IT!!!!!!!!_

```js
let n = 1 + '1'; //here n=11
n = n - 1; //11-1=10
console.log(n); //10 A/C to rule.
```

---

## TRUTHY_AND_FALSY_VALUES:

**FALSY:** Falsy values are not exactly false but will **become false when we try to convert them into a BOOLEAN**.

#### IN JS THERE ARE ONLY 5 FALSY VALUES, THEY ARE:

**0**, **""**, **undefined**, **null**, **NaN**.<br>
**AND EVERY ELSE ARE TRUTHY VALUES.**

```js
console.log(Boolean(0)); //false
console.log(Boolean(undefined)); //false
console.log(Boolean('')); //false
console.log(Boolean(NaN)); //false
console.log(Boolean(null)); //false
console.log(Boolean('Mohammad.')); //true: any string which not a empty is truthy
console.log(Boolean(12)); //true: any number is truthy except 0.
console.log(Boolean({})); //true: empty object is also truthy.
```

```js
const money = 0;
if (money) {
  //remember that!! conditional statements only work on boolean. so it will convert to boolean. here 0 is falsy value so i'ts not true.
  console.log("Don't Spent at all.");
} else {
  console.log('You should get a job.'); //this will print.
}
```

**These(Truthy & False) are used to check, variables are defined or not**.

```js
let height;
if (height) {
  console.log('YAY!! Height is defined.');
} else {
  console.log('UFF!! Height is Undefined.'); //it will execute b/c height is undefined.
}
```

**As undefined is falsy (we can't convert undefined to boolean) that's why the condition will false.**

```js
let height = 9; //if block should execute
let height = 0; //else block should execute
let height = ''; //else block should execute
let height = null; //else block should execute
let height = 'a'; //if block should execute
if (height) {
  console.log('YAY!! Height is defined.');
} else {
  console.log('UFF!! Height is Undefined.');
  // For any value (except 0, "", null, NaN) the height is truthy so condition should true. therefor if block will execute.
}
```

---

## EQUALITY_OPERATORS

**EQUALITY OPERATORS** (== VS ===) (LOOSE== & STRICT===)

```js
const age = 18;
if (age == 18) {
  console.log('You just become an adult! (loose) ');
}
if (age === 18) {
  console.log('You just become an adult! (strict) ');
}
const age = '18';
if (age == 18) {
  console.log('You just become an adult! (loose) ');
}
if (age === 18) {
  console.log('You just become an adult! (strict) ');
}
```

```js
const age = prompt('Enter Your Age!');
if (age == 18) {
  console.log(age);
  console.log('You just become an adult! (loose)');
}
if (age === 18) {
  console.log(age);
  console.log('You just become an adult! (strict)');
}
```

IN THESE TWO BLOCKS WE SEE THAT ALWAYS LOOSE EQUALITY IS GOING TRUE. BECAUSE WHEN WE PROMPT ANY THING. BY DEFAULT IT WILL BE A STRING. SO STRICT EQUALITY WILL NEVER BE TRUE. As ('18' !== 18)<BR>
TO AVOID THIS WE HAVE TO CONVERT IT INTO NUMBER MANUALLY.

```JS
const age = Number(prompt("Enter Your Age!"));
if (age == 18) {
console.log(age);
console.log("You just become an adult! (loose)");
}
if (age === 18) {
console.log(age);
console.log("You just become an adult! (strict)");
}
//NOW WE SEE BOTH ARE PRINTING.
```

---

## LOGICAL_OPERATORS

```JS
const hasDrivingLicense = true;
const hasGoodVision = true;
const isTired = false;

//should dive if first two are true.
let shouldDrive = hasDrivingLicense && hasGoodVision;

if (shouldDrive) {
  console.log("Able to drive!!!!!!");
}

//should dive if first two are true and third is false.
if (hasDrivingLicense && hasGoodVision && !isTired) {
console.log("Able to drive!!!!!!");
} else {
console.log("Don't drive!!!!!!!");
}
```

---

## SWITCH_STATEMENT

**Will check strict equality**

```JS
const day = 'sunday';
switch (day) {
  case 'monday': //day === 'monday' //strict equality check
console.log("Plan Course Structure");
console.log("Go to Coding Meetup");
break;
case 'tuesday':
console.log("Prepare Theory");
break;
case 'wednesday':
case 'thursday':
console.log("Coding Practice");
break;
case 'friday':
console.log("Holy day!");
break;
case 'saturday':
case 'sunday':
console.log("Enjoy Weekend\_\_Only Sleep! ");
break;
default:
console.log("Not a valid day");
}
```

---

## STATEMENT_VS_EXPRESSION

**EXPRESSION:** A piece of code which produce a value; for-example: 2 + 3, also 1996,
**STATEMENT:** Usually a bigger piece of code. which is not concern with producing value. it is like a sentences which perform some action; for example:

```js
if (23 < 54) {
  console.log('23 is smaller than 54');
}
```

---

## CONDITIONAL-TERNARY_OPERATOR

It is also like a _IF-ELSE_ and _SWITCH_ statements to check conditions, BUT it is an **_operator(expression)_** not a statement.<br>
In if block only one line line of code is allowed and also we have to write mandatory else block after that. which we write separated by colon (:)

```js
const age = 21;
age >= 18
  ? console.log('I like to drink wine 🍷')
  : console.log('I like to drink water 🥤');
```

**let rewrite this code:**

```js
let drink = age >= 18 ? 'wine 🍷' : 'water 💧';
console.log(drink);
```

**Again ReWriting this code using if-else.**

```js
let drink2;
if (age >= 18) {
  drink2 = 'wine 🍷';
} else {
  drink2 = 'water 💧';
}
console.log(drink2);
```

More further use of ternary/conditional operator:

```js
console.log(`I like to drink ${age >= 18 ? 'wine 🍷' : 'water 💧'}`);
```

**_NOTE: WE CAN PASS EXPRESSION [LIKE TERNARY/ OPERATOR] INTO A TEMPLATE LITERAL BUT CAN'T PASS STATEMENT INTO IT [LIKE IF-ELSE/ SWITCH]._**<BR>
A TERNARY OPERATOR is not thought as a replacement of if else statement. When we have a bigger blocks of code, ternary operator fails there. we should use if/else statement. **_Ternary operator is perfect when we have to take quick decision._**
