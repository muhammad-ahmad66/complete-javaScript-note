# JS Behind the Scenes

## Table of Contents

1. [THIS_KEYWORD](#this_keyword)
2. [ARGUMENTS_KEYWORD](#arguments_keyword)
3. [PRIMITIVE_AND_REFERENCE_TYPE](#primitive_and_reference_type)

```js
'use strict';
function calcAge(birthYear) {
  const age = 2037 - birthYear;
  console.log(firstName);
  function printAge() {
    const output = `${firstName}, you are ${age}, born in ${birthYear}`;
    console.log(output);
  }
  printAge(); //should work, b/c this function is in the scope
  return age;
  //printAge(); //should not work, b/c this function will terminate after return, before calling.
}

const firstName = 'Muhammad';
//should work
calcAge();

calcAge();
const firstName = 'Muhammad';
//should NOT work, b/c we call a function before declaring variable, so in function it will not know firstName.

const firstName = 'Muhammad';
calcAge();
printAge();
//Should get error, b/c this function in not in this scope.
```

---

## THIS_KEYWORD

- **Special variable that created for every execution context( every function).**
- **Used to point the values of the owner of the function in which the this keyword is used.**
- **The value of this keyword will not same**, it means it's not a static but dynamic, **it depends on how the functions is calling and its value is only assigned when function is called.**

#### Let's see FOUR WAYS, IN which function can be call

1. **When function is a method**. it's a function with in a object. In this situation **this keyword points to the object that is calling the method.** NOTE method means functions in object.
   **Example:**

   ```js
    const me = {
    firstName: 'Muhammad',
    lastName: 'Ahmad',
    year: 1999,
    calcAge: function() {
    return 2022 = this.year; // inside the method.
    }
    };
    me.calcAge(); //22
   ```

2. **Simple call as normal function,** in this case the **this keyword be undefined, in strict mode**. **if we don't use strict mode then this keyword will pont to window object.**

3. **Arrow function:** If we use **this keyword** in arrow function **then it will point to surrounding function, not in arrow function.** NOTE **In arrow function this keyword will point to parent function**. very important REMEMBER...

4. **A function is called as an event listener. In this case the this keyword will always point to handler function attached to.**

_NOTE_ **_this keyword does not point to the function itself, and also not points to the variable environment of the function._**

---

### EXAMPLES

#### Outside of any Function

**'This' keyword** at outside of function, i-e in global scope. **In global scope it display window object.**

```js
console.log(this); // will display window object.
```

#### Inside any Regular Function.

In any regular function, **this keyword will print undefined in strict mode**, **with strict mode it will print window object.**

```js
const calcAge = function (birthYear) {
  console.log(2022 - birthYear);
  console.log(this); // will print 'undefined',
};
calcAge(2000);
```

#### Inside any Arrow Function.

**Will point to the parent function.**

```js
const calcAgeArrow = (birthYear) => {
  console.log(2030 - birthYear);
  console.log(this);
  // it will point to the parent function. here there is no parent function, so i'll points to the window object.
};
calcAgeArrow(2000);
```

#### Inside a Method (inside a function within object)

**It will point to the object where that function is located.**

```js
const me = {
  birthYear: 2000,
  calcAge: function () {
    console.log(this); // it will print all key value pair from 'me object'. (me is owner of the calcAge function)
    console.log(2022 - this.birthYear); // it'll point to birthYear which is in 'me object'.
  },
};
me.calcAge();

const you = {
  birthYear: 2002,
};

you.calcAge = me.calcAge;
you.calcAge();

const f = me.calcAge;
console.log(f);

f(); // this = undefined, b/c it's now just like a normal function.
```

**_NOTE This keyword always point to the object that's calling the method._**

_you.calcAge = me.calcAge;_ Here we are assigning calcAge function from 'me' object to 'you object'.<br>
_you.calcAge();_ // 20, In this example calcAge is calling the calcAge method which is in 'you object', so it will points to the **you object**, not **me object**.

_const f = me.calcAge;_ // **assigning method from me object to a variable f**. it's possible b/c in abject the methods is just like a function expression. <br>
_console.log(f);_ //will print calcAge method from me object.

_f();_ // when we call the f function. then it will print undefined for this keyword. b/c now this f function is just like any regular function. it's not attached to any object.

---

### Arrow function VS Regular function

#### compression using this keyword.

```js
const me = {
  firstName: 'Muhammad',
  birthYear: 2000,
  calcAge: function () {
    console.log(this); // it will print all key value pair from 'me object'. (me is owner of the calcAge function)
    console.log(2022 - this.birthYear); // it'll point to birthYear which is in 'me object'.
  },
  greet: () => console.log(`Hi, ${this.firstName}`),
};

me.greet(); //hi, undefined. b/c of arrow function
```

hi, undefined. Because greet is an arrow function. In arrow function **this keyword always point to a parent function**, here no parent function so it will point to global scope (window object). it's like a:

_console.log(this.firstName);_ undefined. when we try to access certain property that's does't exist in that object then it'll print undefined. here it's point to window object and there is no firstName property.

**For the best practice WE should never ever use arrow function as a method. and also not use a var to define any variable.**

**If we declare a variable using var keyword then it will be the part(property) of window object.**

```js
var firstName = 'Muhammad';
console.log(`hi, ${this.firstName}`); //hi, Muhammad
```

```js
const me = {
  firstName: 'Muhammad',
  birthYear: 2000,
  calcAge: function () {
    console.log(2022 - this.birthYear);

    // Solution I (old version)
    const self = this; // to avoid error.
    const isTeenAger = function () {
      // console.log(this); //undefined
      console.log(self); // self is storing this keyword and this keyword is pointing to me object. so it will print me object.
      console.log(self.birthYear >= 2004); //error
    };
    isTeenAger(); // here this function calling is just like a regular function as in regular function calling the this keyword will be undefined so, here it's. . .

    // Solution II (ES-6) [using arrow function]
    // Because arrow function will look at parent function. in a parent scope this keyword is me. so it should work.

    const isTeenAger = () => {
      console.log(this);
      console.log(this.birthYear >= 2004);
    };
    isTeenAger();
  },
  greet: function () {
    console.log(`Hi, ${this.firstName}`); // Now it should work with regular function.
  },
};

me.calcAge();
me.greet(); //Hi, Muhammad
```

---

## ARGUMENTS_KEYWORD

**Argument keyword** is only avaIlable in Regular Function. it's just like a this keyword.

```js
const addExp = function (x, y) {
  console.log(arguments); //It will print arguments of the function
  return x + y;
};
addExp(7, 5, 4, 5);

const addArrow = (x, y) => {
  console.log(arguments); // In arrow function the arguments keyword not work.
  return x + y;
};
addArrow(3, 4);
```

## PRIMITIVE_AND_REFERENCE_TYPE

**Difference In primitive types and objects.** (primitive types (primitive) and reference(object-non primitive) type (prospective of memory))

```js
let age = 20;
let oldAge = age;
age = 21;
console.log(age);
console.log(oldAge);

const me = {
  name: 'Muhammad',
  age: 20,
};

const friend = me;

friend.age = 27;

console.log('Friend:', friend);
console.log('Me:', me);
```

Here we see in console that **age of both me and friend changed to 27**, but we only changed friend's age.<br>
**_Why it's coming???_** _Let's Try find out where it's coming...._<br>
**Primitive types are stored in call stack while reference types are stored in heap.**

**When we create an object it will stored in heap, but it not directly points to the heap address, first it will create a memory space in stack and then that address(in stack) will store to the heap address. so it's called reference type b/c it is referencing the memory in stack from heap.** it's due an object may require many memory space so all thing will store in heap and stack just keep a reference where the object's data is located.

So here in example we defined an object named as friend which is pointing at same memory address in heap where me object is located (const friend = me). so whenever we change in friend it will always be reflected in me object. (b/c both are pointing to exact same value.)

**lets take examples:**

_Primitive Types_

```js
let lastName = 'Shamim';
let oldLastName = lastName;
lastName = 'Ahmad';
console.log(oldLastName, lastName);
```

_Reference Types_

```js
const jessica = {
  firstName: 'Jessica',
  lastName: 'Williams',
  age: 20,
};
const marriedJessica = jessica;
marriedJessica.lastName = 'Davis';

console.log(`Before marriage: ${jessica.lastName}`); //Davis
console.log(`After marriage: ${marriedJessica.lastName}`); //Davis
```

We already know that's because both are pointing at same memory at heap. (if we copy one object to another by assigning it to another variable it just make a memory address at stake not on heap. and both memory address holds the address of memory space at heap, that's same address for both (old and copied one. in this case jessica and marriedJessica))<br>
That's a reason we can change a property of objects while using const keyword when defining (**b/c using const keyword we can't change in stack, but we can change in heap.** here all the objects values are stored in heap, except referencing address(which is referencing stack to heap)).

### Way in which we copy one object to other and the value does't change while changing in other:

```js
const jessica2 = {
  firstName: 'Jessica2',
  lastName: 'Williams',
  age: 20,
  family: ['Tom', 'Bob', 'Alice'],
};
```

To copy we use a function **Object.assign()**<br>
This function will merge both object and create a new object.

```js
const jessicaCopy = Object.assign({}, jessica2); //it will merge jessica with empty new object an store in jessicaCopy variable as an object
jessicaCopy.lastName = 'Davis';
console.log(`Before marriage: ${jessica2.lastName}`); //Williams
console.log(`After marriage: ${jessicaCopy.lastName}`); //Davis

jessicaCopy.family.push('Mary');
jessicaCopy.family.push('John');

console.log(`Before marriage: ${jessica2.family}`);
console.log(`After marriage: ${jessicaCopy.family}`);
```

But here is still a problem. **Object.assign() will create a shallow copy not a deep copy**. if we want to change object with in object it will again change in both.
We can see in both objects two family members are added while we have pushed in jessicaCopy only.

Deep clone is not a scope in this section. we will do later......................
