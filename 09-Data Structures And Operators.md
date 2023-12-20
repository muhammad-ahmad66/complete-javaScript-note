# DATA STRUCTURES AND OPERATORS

## Table of Contents

1. [DESTRUCTURING](#destructuring)
   1. [ARRAY-DESTRUCTURING](#array-destructuring)
   2. [OBJECT-DESTRUCTURING](#object-destructuring)
2. [SPREAD_OPERATOR](#spread_operator)
   1. [SPREAD_OPERATOR_IN_ARRAY](#spread_operator_in_array)
   2. [SPREAD_OPERATOR_IN_OBJECT](#spread_operator_in_object)
3. [REST_PATTERN_AND_REST_PARAMETERS](#rest_pattern_and_rest_parameters)
   1. [REST_PATTERN_IN_ARRAY](#rest_pattern_in_array)
   2. [REST_PATTERN_IN_OBJECTS](#rest_pattern_in_objects)
   3. [REST_PARAMETERS](#rest_parameters)
4. [SHORT_CIRCUITING--AND_OR](#short_circuiting--and_or)
   1. [BY_OR_OPERATOR](#by_or_operator)
   2. [BY_AND_OPERATOR](#by_and_operator)
5. [NULLISH_COALESCING_OPERATOR](#nullish_coalescing_operator)
6. [LOGICAL_ASSIGNMENT_OPERATORS](#logical_assignment_operators)
7. [FOR_OF_LOOP](#for_of_loop)
8. [ENHANCED_OBJECT_LITERALS](#enhanced_object_literals)
9. [OPTIONAL_CHAINING](#optional_chaining)
10. [LOOPING_OVER_OBJECTS](#looping_over_objects)
11. [DATA_STRUCTURE](#data_structure)
    1. [SET_DATA_STRUCTURE](#set_data_structure)
    2. [MAP_DATA_STRUCTURE](#map_data_structure)
    3. [WHEN_WHERE_WHICH](#when_where_which)
12. [STRINGS](#strings)
    1. [length-Property_indexOf-Method_slice-Method](#length-property_indexof-method_slice-method)
    2. [toUpperCase_toLoweCase](#touppercase_tolowecase)
    3. [trim-trimStart-trimEnd_Methods](#trim-trimstart-trimend_methods)
    4. [replace_Method](#replace_method)
    5. [includes_startWith_endWith_Methods](#includes_startwith_endwith_methods)
    6. [split_And_join_Methods](#split_and_join_methods)
    7. [padStart_And_padEnd_Methods](#padstart_and_padend_methods)
    8. [repeat_Method](#repeat_method)
    9. [repeat_Method](#repeat_method)

---

## DESTRUCTURING

**_DESTRUCTURING_** _means unpacking/ splitting values of an array or objects into a sub pieces. We can destructure array and object by separate variables._<br>
_In other words destructuring means break a complex data structures into a simpler data structures like a variable._

### ARRAY-DESTRUCTURING

```js
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],
};
```

If we want this: (manually)

```js
const arr = [4, 5, 2];
const a = arr[0];
const b = arr[1];
const c = arr[2];
console.log(a, b, c);
```

Using destructuring array method

```js
const [x, y, z] = arr; //x, y, z are a just a regular variable.
console.log(x, y, z);
```

```js
const [first, second] = restaurant.categories;
// will store 1st and 2nd array element from catagories array
console.log(first, second);
```

**If we want elements not in ordering like 1st and 3rd, by skipping 2nd element:**

```js
let [first, , third] = restaurant.categories;
// We just leave a hole in destructuring operator
console.log(first, third);
```

**_Swapping values of variables using regular method:_**<br>
_If we want change values(swap) of first and third: in regular we write like this_

```js
let temp = first;
first = third;
third = temp;
console.log(first, third);
```

**_Swapping values of variables using destructuring method:_**<br>
_We can do this using destructuring, like this:_

```js
[first, third] = [third, first];
console.log(first, third);
```

**_Destructuring two return values into a variables._**

```js
console.log(restaurant.order(2, 0)); //print like an array
const [starter, main] = restaurant.order(2, 0); //destructuring into a variable
console.log(starter, main);
```

**_Destructuring nested array_**

```js
const nested = [2, 3, [4, 5]]; // nested array.
const [first, , third] = nested;
console.log(first, third); //will print 2 and array which contain 4&5
```

**What should do if we want to destructure inside array???**<br>
We'll have to do destructuring inside destructuring.

```js
const [first, , [third, forth]] = nested;
console.log(first, third, forth); //will print 2, 4, 5
```

**_Giving destructuring variable a default value._**<br>
_We can also set the default value for variables when destructuring_

```js
const array = [8, 9];
const [i, j, k] = array;
console.log(i, j, k); //8, 9, undefined: to avoid we set default value.
const [i = 1, j = 1, k = 1] = array;
console.log(i, j, k); //8, 9, 1
```

---

### OBJECT-DESTRUCTURING

**We use curly braces in object and in array use square bracket**<BR>
**In object we should write variable name exact same as property name in object.**

```JS
const restaurant = {
name: 'Classico Italiano',
location: 'Via Angelo Tavanti 23, Firenze, Italy',
categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
mainMenu: ['Pizza', 'Pasta', 'Risotto'],

openingHours: {
thu: {
open: 12,
close: 22,
},
fri: {
open: 11,
close: 23,
},
sat: {
open: 0, // Open 24 hours
close: 24,
},
},
order: function (starterIndex, mainIndex) {
return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
},
orderDelivery: function ({ starterIndex = 1, mainIndex = 0, address, time = '12:00' }) { // Destructuring object into variables in parameter
console.log(`Order Received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}.`);
}
};

//calling method
restaurant.orderDelivery({
time: '22:30',
address: 'yougo',
mainIndex: 2,
starterIndex: 2
}); //Passing object to a function as an argument.

//calling method not using some properties that are in argument (to see default value)
restaurant.orderDelivery({
address: 'Skardu',
mainIndex: 1
})


const { name, openingHours, categories } = restaurant;
console.log(name, openingHours, categories);

```

**_Naming variables other than property name:_**<br>
if we want set variables name different from the property name we write like this **_propertyName: newName_**

```js
const {
  name: restaurantName,
  openingHours: hours,
  categories: tags,
} = restaurant;

console.log(restaurantName, hours, tags);
```

**_Give default value:_**<br>

```js
const { menu = [], starterMenu: starter = [] } = restaurant; //gave empty array as a default value
console.log(menu, starter);
```

**Mutating(swapping) variables:**

```js
let a = 111;
let b = 444;
const obj = {
  a: 23,
  b: 43,
  c: 53,
};


{ a, b } = obj; // ERROR
```

**⤴ It will gives error b/c when we start a statement with curly bracket javascript expected that a code block. so for destructuring to variables that are already declared, always enclose in ⤵**

```js
({ a, b } = obj);
console.log(a, b); // 23, 43
```

**_Nested Objects:_**

```js
const { fri } = openingHours; //friday also an object, contains open and close property. to destructure this..:
const {
  fri: { open: openingTime, close: closingTime },
} = openingHours;
console.log(openingTime, closingTime); //11, 23
```

---

## SPREAD_OPERATOR

**Spread Operator** is used to expend an array into all its elements.<br>
**Unpacking all its elements at once.**

### SPREAD_OPERATOR_IN_ARRAY

```js
const arr = [7, 8, 9];
```

To put two new array in beginning we might do like this

```js
const badNewArray = [1, 2, arr[0], arr[1], arr[2]];
console.log(badNewArray);
```

**Doing this thing using spread operator**(which is newer in javascript)

```js
const goodNewArray = [1, 2, ...arr];
// It will expend all the elements from arr and then put in goodNewArray
console.log(goodNewArray);

console.log(...goodNewArray); // It will print all array elements individually. like 1, 2, 7, 8, 9
```

_Data we will use for First Section⤵_

```js
const restaurant = {
  name: 'Classico Italiano',
  location: 'Via Angelo Tavanti 23, Firenze, Italy',
  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  openingHours: {
    thu: {
      open: 12,
      close: 22,
    },
    fri: {
      open: 11,
      close: 23,
    },
    sat: {
      open: 0, // Open 24 hours
      close: 24,
    },
  },
  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },
  orderDelivery: function ({
    starterIndex = 1,
    mainIndex = 0,
    address,
    time = '12:00',
  }) {
    // Destructuring object into variables in parameter
    console.log(
      `Order Received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}.`
    );
  },
  orderPasta: function (ing1, ing2, ing3) {
    console.log(
      `Here is your delicious pasta with ${ing1}, ${ing2} and ${ing3}`
    );
  },
};

restaurant.orderDelivery({
  //calling method
  time: '22:30',
  address: 'yougo',
  mainIndex: 2,
  starterIndex: 2,
});
```

```js
const newMenu = [...restaurant.mainMenu, 'Gnocchi'];
//keep in mind we are creating a new array, not manipulation original one that's in restaurant object.

console.log(newMenu);
```

**_Spread Operator is bit similar to destructuring but main difference is that the spread operator takes out all the elements of the array and does't create any variable to hold them one by one._**

#### Two Important use cases of spread operator

1. **Copy Array**

   ```js
   const copyMainMenu = [...restaurant.mainMenu];
   ```

2. **Join Two Arrays**

   ```js
   const menu = [...restaurant.starterMenu, ...restaurant.mainMenu];
   ```

**_Spread operator not works on only arrays but it works on all iterables._**
_Iterables in javascript are like Arrays, Strings, Maps, Sets BUT not objects._

#### Spread Operator in String

```js
const str = 'Muhammad';
const letters = [...str, ' ', 'S.'];
// Array which contains all letters from muhammad and space and S.

console.log(letters);
```

**_We can use spread operator only when building an array or we pass values into functions._**

```js
console.log(...str); //passing values to log function // M u h a m m a d
```

```js
console.log(`${...str}, 'Ahmad'`);
// Here it will give error, b/c here unexpected multiple values separated by comma,
// Usually multiple values separated by comma expected when we passing arguments into a function or when we building a new array.
// So usually we use spread operator in array and passing arguments to function.
```

**Real world Example of spread in function as a argument**

```js
const ingredients = [
  prompt("Let's make pasta! Ingredient 1?"),
  prompt("Let's make pasta! Ingredient 2?"),
  prompt("Let's make pasta! Ingredient 3?"),
];
console.log(ingredients);

restaurant.orderPasta(ingredients[0], ingredients[1], ingredients[2]); // without using spread operator.
restaurant.orderPasta(...ingredients); // Using spread operator.
```

---

### SPREAD_OPERATOR_IN_OBJECT

**_Spread Operator also works on Objects even though objects are not iterables._**<br>
_Creating new object with restaurant objects_

```js
const newRestaurant = { foundedIn: 1998, ...restaurant, founder: 'Guiseppe' };
console.log(newRestaurant);
```

**We can make also copy of any object as we did in array** (Remember Shallow copy not deep)

```js
const copyRestaurant = { ...restaurant };
copyRestaurant.name = 'Ristoranta Roma';

console.log(copyRestaurant.name, restaurant.name); // Both are different.
```

---

## REST_PATTERN_AND_REST_PARAMETERS

### REST_PATTERN_IN_ARRAY

**_Rest pattern looks exactly same as the spread operator, it has same syntax but it does opposite of the spread operator._**

Remember that we used spread **operator** to build new arrays or to pass multiple values to a function. In these both cases we used an array into expends its individual elements. Now the rest pattern uses the exact same syntax however to collect multiple elements and put them into an array. so that's really the opposite of the spread operator.

**_spread used to unpack an array and rest use to pack into an array._**

```js
const arr = [1, 2, 3, ...[4, 5, 6]]; //here we using spread operator
```

**_spread operator use right side of assignment operator while rest pattern use left side of assignment operator._**

```js
const [a, b, ...others] = [1, 2, 3, 4, 5, 6];
console.log(a, b, others);
// Here a and b are variables (we destructured) and others is an array(using rest pattern)
```

_*Data used from above in spread section*_

```js
const [pizza, , risotto, ...otherFoods] = [
  ...restaurant.mainMenu,
  ...restaurant.starterMenu,
];

console.log(pizza, risotto, otherFoods);
// Pizza, Risotto, and an array which contain all others.
```

_*It collect all the elements after the last variable. in this example after risotto, It does't include any skipped elements. So that's reason **rest pattern always must be last in the destructuring assignment.** Otherwise how will javascript know until when it should collect the rest of an array. And also **should be one rest pattern** in any destructuring assignment*_

```js
const [x, y, ...otherNumbers, z] = [1, 2, 3, 4, 5, 6, 7];
// It will give error 'Rest element must be last element'

const [x, y, ...lessThenFive, ...lessThenTen] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
// Also Error. should one rest.
```

So, **Rest operator should be last one and also should one rest operator.**

### REST_PATTERN_IN_OBJECTS

```js
const { sat, ...weekDays } = restaurant.openingHours;
// console.log(weekDays);
```

Remember: In this example first we **destructuring** "sat" object form openingHours and store into sat variable [ _Reminder destructuring variable name should be same as property in object._ ] and then storing rest objects into weekDays using rest pattern.

---

### REST_PARAMETERS

#### Rest Operator in Functions

**_We can make a function that accept any arbitrary amount of arguments._**

```js
const add = function (...numbers) {
  console.log(numbers); //It will show all the parameters in array.
  let sum = 0;
  for (let i = 0; i < numbers.length; i++) {
    sum += numbers[i];
  }
  console.log(sum);
};
add(2, 3);
add(4, 6, 2, 8);
add(3, 9, 8, 2, 3, 5);

const x = [23, 5, 7];
add(...x);
```

In this example we use rest operator in parameter to combine all the argument's value into an array an then we add by looping through array.

At last statement when we calling add function, we make an array and then pass these array value into add function by using spread operator in argument. **_Here we see spread use in argument and rest use in parameter._**

PRACTICE (MAKING A FUNCTION THAT RETURN A MAX VALUE FROM AN ARRAY OR FROM MULTIPLE ARGUMENTS)

```js
const max = function (...numbers) {
  let max = numbers[0];
  for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] > max) {
      max = numbers[i];
    }
  }
};
return max;

console.log(max(2, 3, 5, 9, 1, 6, 4, 8)); //9 wow working!!!!!!

// now we give an array to return max element from array
let array = [1, 2, 43, 54, 23, 65, 7, 54, 6, 43];
console.log(max(...array)); // 65
```

---

## SHORT_CIRCUITING--AND_OR

Up until this point we used logical operators only to combine Boolean values. But we can do lot more with logical operators.

- logical operator use any datatype not only boolean.
- They also can return any datatype.
- And they do short circuiting also called short circuit evaluation.

```js
console.log('3' || 'Jonas'); // '3'
// here we used two values that are not boolean and returns a value that is also not a boolean.
```

### BY_OR_OPERATOR

**SHORT CIRCUITING by OR OPERATOR ||**: In case of the OR operator short circuiting means **if the first value is truthy value it immediately returns a first value. It not evaluate second value.** what exactly that we see in above example.

```js
console.log('' || 'Muhammad'); // Muhammad. b/c first is falsy value
console.log(true || 0); // true. b/c first was truthy value therefor it not evaluate second
console.log(undefined || null); // null. b/c first was falsy.
console.log(0 || 'n'); // n. b/c 0 is falsy.
console.log(null || 0 || undefined || '' || 'Hello' || 23 || NaN); //Hello. b/c hello was first truthy value
```

#### Practical Example

```js
restaurant.numGuest = 23;
const guests1 = restaurant.numGuest ? restaurant.numGuest : 10;
// In this example if restaurant.numGuest is exist it will assign that to guests1 and if it not exist we assign guests1 to 10.
console.log(guests1);
```

We can do this by using OR short circuiting.

```js
const guest2 = restaurant.numGuest || 10;
console.log(restaurant.numGuest); //undefined
console.log(guest2); //10 b/c numGuest not exist so it is falsy value Undefined.if exist it will print that number
```

---

### BY_AND_OPERATOR

**And** operator short circuiting is **exact opposite** way of **OR** operator.

**If first value is falsy value it immediately return first value, without evaluating other values.**

```js
console.log(0 && 'Jonas'); //0
console.log(true && ''); // ''
console.log(undefined && null); //'undefined'
console.log(4 && 'Jonas'); // jonas// both are true so it will print last one.
console.log(23 && 'Hello' && undefined && null && 0 && ''); //undefined. first falsy value.
```

#### Practical Example

```js
//if orderPizza exist
if (restaurant.orderPizza) {
  restaurant.orderPizza('mushroom', 'spinach');
}
```

By using and operator we can do simple way

```js
restaurant.orderPizza && restaurant.orderPizza('mushroom', 'spinach');
```

**_In practical applications we can use OR operator to set the default value and we can use the AND operator to execute code in the second operand if the first one is true._**

---

## NULLISH_COALESCING_OPERATOR

THE NULLISH COALESCING OPERATOR (??)

```js
restaurant.numGuests = 0;
const guest2 = restaurant.numGuests || 10;
console.log(guest2); //10 But we assigned 0!!!!!.
```

Here if we set with 0 then the short circuiting OR will not give correct result, it will always give 10, **as 0 is a falsy value so it will always go to the second operand**. . There is a solution of this.

The Solution is: Nullish coalescing operator (very funny name!!!!!!!)

**It is almost the same as OR operator**, but it'll fix the error

```js
const guestCorrect = restaurant.numGuests ?? 10;
console.log(guestCorrect); // Wow!!! Now it's 0.
```

**Nullish Coalescing operator** works with the ides or with the concept of **nullish values** instead of **falsy values.**  
**Nullish Values** are: **null** and **undefined** only, (not **0** and **''**).

---

## LOGICAL_ASSIGNMENT_OPERATORS

LOGICAL ASSIGNMENT OPERATORS (??)

```JS
rest1.numGuest = rest1.numGuest || 10;
rest2.numGuest = rest2.numGuest || 10;
```

### LOGICAL OR ASSIGNMENT OPERATOR

These are that we learnt in last lecture (OR short circuit)  
Here we can minimize the code by using logical OR assignment operator.

```js
rest1.numGuest ||= 10;
rest2.numGuest ||= 10;
```

**_Assigns value to variable only if that variable is currently falsy._** _here second one is falsy so it will hold 10. it will also assign value to variable when we already assigned 0. To solve this here come:_

### LOGICAL NULLISH ASSIGNMENT OPERATOR

```js
rest1.numGuest ??= 10;
rest2.numGuest ??= 10;
```

### LOGICAL AND ASSIGNMENT OPERATOR

```js
rest2.owner = rest2.owner && '<ANONYMOUS>';
```

REPLACED the owner to Anonymous if exist  
**_And operator always first check for falsy value if there exist any falsy value it will return that falsy value without evaluating other value_**

```js
rest2.owner &&= rest2.owner;

console.log(rest1);
console.log(rest2);
```

---

## FOR_OF_LOOP

FOR-OF LOOP [ NEWER WAY TO LOOPING OVER ARRAYS ]

```js
const menu = [...restaurant.starterMenu, ...restaurant.mainMenu];
```

```js
for (const item of menu) {
  console.log(item);
}
```

**_One problem with for off loop:_** _what we do when need value as well as current index. finding index is pit pain_  
**Currently this problem is solved:** For that we use **entries method**. let's see

```js
for (const item of menu.entries()) {
  console.log(item);
}
```

**Whenever we use entries method in each iteration it will make an array which contain the index of current element of an array and element itself**. (so each iteration item is an array in this example).  
**So Here item itself is an array which have two elements**, at index 0 it hold the index of current element in original array and at index 1 it hold the element itself at that position.

```js
for (const item of menu.entries()) {
  console.log(`${item[0] + 1}: ${item[1]}`); // +1 to start from 1.
}
```

### One step further

As an item is an array in each iteration so, we can destructure it and put it in two variables; one will hold the index number and another value itself

```js
for (const [i, el] of menu.entries()) {
  console.log(`${i + 1}: ${el}`); // i+1 to start from 1, not from 0
}
```

---

## ENHANCED_OBJECT_LITERALS

ENHANCED OBJECT LITERALS [ NEWER IN ES6 ]

Regularly we use object literals to define any object. Here we will discuss more enhanced object literals

### ES6 introduced three ways, which make easier to write object literals

1. Lets take restaurant example

   Let us consider we have an object outside of any object like this, (openingHours is outside of restaurant object) and we wants to that outsider object inside the main object.  
   Consider we have to take openingHours object inside the restaurant as well. To do that we simply add that object by exact name in it

2. Writing methods into an objects.  
   We can write methods without writing function and colon like this add(){};  
   In restaurant object implemented in first two methods.

3. We can compute/calculate properties
   example, use weekdays array and compute in object

   ```js
   const weekDays = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];
   const openingHours = {
     // thu: {  // instead of thu we can compute above array and take thu from there like this:
     [weekDays[3]]: {
       open: 12,
       close: 22,
     },
     [weekDays[4]]: {
       open: 11,
       close: 23,
     },
     sat: {
       open: 0, // Open 24 hours
       close: 24,
     },
   };
   const restaurant = {
     name: 'Classico Italiano',
     location: 'Via Angelo Tavanti 23, Firenze, Italy',
     categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],
     starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],
     mainMenu: ['Pizza', 'Pasta', 'Risotto'],
     // 1.Object literal
     openingHours, // just wrote outsider object name.

     order(starterIndex, mainIndex) {
       return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
     },
     orderDelivery({
       starterIndex = 1,
       mainIndex = 0,
       address,
       time = '12:00',
     }) {
       console.log(
         `Order Received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${address} at ${time}.`
       );
     },
     orderPasta: function (ing1, ing2, ing3) {
       console.log(
         `Here is your delicious pasta with ${ing1}, ${ing2} and ${ing3}`
       );
     },
     orderPizza: function (mainIngredient, ...otherIngredients) {
       console.log(mainIngredient);
       console.log(otherIngredients);
     },
   };
   ```

---

## OPTIONAL_CHAINING

OPTIONAL CHAINING (?) [ NEWER FEATURE OF OBJECT & ARRAY IN ES6 ]  
_Used restaurant data to test_

```js
console.log(restaurant.openingHours.mon); //undefined! b/c mon is not defined in object
console.log(restaurant.openingHours.mon.open); //error b/c undefined.something will give error

// To avoid this kind of error first we check if it exists

if (restaurant.openingHours.mon) {
  console.log(restaurant.openingHours.mon.open);
}
```

**We can write this king of things very easily with optional chaining**  
I**f any certain property does't exist then optional chaining will return undefined immediately.**  
_Above code with optional chaining:_

```js
console.log(restaurant.openingHours.mon.open); //error!!!!!!!!
console.log(restaurant.openingHours.mon?.open); //undefined. b/c mon is not defined in object..
// Only if mon exist then print open. otherwise immediately returns undefined.
```

**Exist means nullish property, should not null and undefined**  
We can have multiple optional chaining.

```js
if (restaurant.openingHours && restaurant.openingHours.mon) {
}
console.log(restaurant.openingHours?.mon?.open); //undefined.
```

Example:

```js
const days = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];

for (const day of days) {
  console.log(day);
  const open = restaurant.openingHours[day]?.open ?? 'close';
  // Reminder: we use bracket notation in object when we want to use a variable name as a property name.
  // This will be the same as restaurant.openingHours.mon phr tue phr wed..here the day name will come dynamically from the days array.
  console.log(`On day ${day}, we open at ${open}`);
}
```

### On methods

**_Optional chaining on methods:_** _It also works calling methods. We can check a method is exist or not before calling._

```javascript
console.log(restaurant.order?.(0, 1) ?? "method does't exist"); //exist a method named by order
console.log(restaurant.orderRisotto?.(0, 1) ?? "method does't exist"); //Not exist an method named by orderRisotto
```

### On Arrays

**_Optional chaining also works on arrays._** _we can use it to check if an array is empty?_

```js
const users = [{ name: 'Muhammad', email: 'Hello@me.io' }];
console.log(users[0]?.name ?? 'User array empty!'); //Muhammad
// It will check user[0] exist or not if exist .name will print and if not exist, print 'user array empty!'.
```

Without optional chaining we may write like this

```js
if (users.length > 0) console.log(users[0].name);
else console.log('Users Array is Empty!');
```

**_Optional chaining operator and nullish coalescing operator are always use with each other. They both we use together to do some thing In case we don't get result from the object._**

---

## LOOPING_OVER_OBJECTS

_We can loop over the property name or values or both of them_  
**_Looping over Property names [keys]_**

WE use **Object.keys(objectName)**  
we still use for of loop but indirect way. **we will not loop directly over object instead we loop over array.** lets check...

```js
const property = Object.keys(restaurant.openingHours);
console.log(property);
```

**By writing this it will print all the properties in openingHours within Array.** So actually it will get all the keys from object and store in array and then we can iterate them by looping.

```js
const property = Object.keys(restaurant.openingHours);
console.log(property);
console.log(`We are open on ${property.length} days.`);

let openStr = `We are open on ${property.length} days.`;
for (const day of property) {
  openStr += `${day},`;
}
console.log(openStr);

for (const day of Object.keys(restaurant.openingHours)) {
  console.log(day);
}
```

**_Looping over property Values_**  
We use **Object.values(ObjectName)**

```js
const values = Object.values(restaurant.openingHours);
console.log(values);
```

**_Subheading: Looping over Both [entire object] (keys and values)_**  
We use **Object.entries(ObjectName)**  
**It will convert entire object into an array. In key value pair, Each key value pair will also store in array at index 0 key and at index 1 value.**

```js
const me = {
  name: 'Muhammad',
  mail: 'me@gmail.com',
  job: 'student',
  friends: ['huzaifa', 'saud', 'shoaib'],
};

const meEntries = Object.entries(me);
console.log(meEntries);

// OUTPUT
[
  ['name', 'Muhammad'],
  ['role', 'admin'],
  ['mail', 'me@gmail.com'],
  ['job', 'student'],
  ['roNo', 20],
  ['friends', ['huzaifa', 'saud', 'shoaib']],
];
```

```JS
const entries = Object.entries(restaurant.openingHours);
console.log(entries);
for (const [key, { open, close }] of entries) {
// console.log(`key is: ${x[0]} and value is: ${x[1]}`);
console.log(`On ${key} we open at ${open} and close at ${close}`);
}
```

---

## DATA_STRUCTURE

In past there are only two build-in data structure of javascript (**Array** and **Objects**. ) . But in ES6 two more data structure were intrOduced, and that are **Sets** and **Maps**.

### SET_DATA_STRUCTURE

_**Set** is just a collections of unique values. sets can never have any duplicate value._

```js
const orderSet = new Set([
  'Pizza',
  'Pasta',
  'Pizza',
  'Risotto',
  'Pasta',
  'Pizza',
]);

console.log(orderSet); //Only three are printed. all duplicate values are count as one.
```

we define set by using new keyword with set and pass any iterable to the set. and most common iterable is an array. All duplicates are counted one.

And we see in console that a set is just like an array.

```js
console.log(new Set('Muhammad'));
```

**_size:_** _we use size in set instead of length in array._

```js
console.log(orderSet.size); // NOTE In set .size use instead of .length
```

**_has method:_** _Just like a **includes** method in array_

```js
console.log(orderSet.has('Pizza')); // true
// By using .has method we can check pizza is element of orderSet or not?

console.log(orderSet.has('Pasta')); // true
// has method is similar to includes method in array
```

**_add method:_** _Just like **push** method in array_

```js
orderSet.add('Burger');
orderSet.add('Chips');
console.log(orderSet);
```

**_delete method:_** _Just like pop in array._

```js
orderSet.delete('Burger');
orderSet.delete('Chips');
console.log(orderSet);
```

**_clear method:_** _To delete all of elements from the set._

```js
orderSet.clear();
console.log(orderSet); // DELETED
```

_**How to retrieve an element from a sets???**_

**_Like array it does't works on index number or order of element._**  
_There is no way to get elements from the sets._

**Sets are iterables So, we can loop over it.**

```js
for (const order of orderSet) {
  console.log(order);
}
```

_**Example OR Application of Sets**_  
**Sets are use to remove duplicates from the Arrays.**

```js
const staff = ['waiter', 'chef', 'waiter', 'manager', 'chef', 'waiter'];

// we want to remove all the duplicates so,
const staffUnique = new Set(staff);

console.log(staffUnique);
```

Now we wants to put this set into an array so, here spread operator will work b/c set is also an iterable.

```js
const staffUniqueArray = [...new Set(staff)];
console.log(staffUniqueArray);
console.log(staffUnique.size);

console.log(new Set('Muhammad Ahmad').size); // 8 // not included repeated one.
```

**_Sets are a data structure that holds unique values._**

---

### MAP_DATA_STRUCTURE

**Just like an Objects, In Maps data is also store in key value pair. big difference is that in objects we can have only string type of keys, but in maps we can have different data types of keys, it can even be objects, arrays or other maps.**

```js
const rest = new Map();
//easiest way to create a map is to first we create an empty map and then fill it with set method.
```

**set Method():** Calling set method not only fill up(add) elements to Maps but also return a Map after adding this number.

```js
rest.set('name', 'Classico Italiano');
// First one is key and second is value. Here set method is same as add method in sets.

rest.set(1, 'Firenze Italy');
// Calling set method not only fill up(add) elements to Maps but also return a Map after adding this number.
console.log(rest.set(2, 'Lisbon Portugal'));
```

We can insert multiple values by chaining set method.

```js
rest
  .set('categories', ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'])
  .set('open', 11)
  .set('close', 23)
  .set(true, 'We are open!')
  .set(false, 'We are closed!');
```

**get Method():** To retrieve data from the map we use get method.

```js
console.log(rest.get('name'));
console.log(rest.get(true));
```

Lets take some advantages of having boolean type as a key (example)

```js
const time = 21;
console.log(rest.get(time > rest.get('open') && time < rest.get('close')));
```

**has method():** Check has a key or not, based upon key

```js
console.log(rest.has('categories')); //true
```

**delete method():** Delete element from the **Map**, again based upon key.

```js
rest.delete(2);
console.log(rest);
```

**size property**: As **Set**, **Map** also have a size property.

```js
console.log(rest.size);
```

**clear method(): Also can clear all the elements from the**Map\*\*.

```js
rest.clear();
```

**_We can also set array and objects as a map key_**

```js
rest.set([1, 2], 'Test');
```

Get value using **keys as an array**

```js
console.log(rest.get([1, 2])); //undefined it will not work
```

To solve that **problem** we first define an array and store the value in that array and then set that array variable into the Map key like this

```js
const arr = [1, 2];
rest.set(arr, 'Test');
console.log(rest.get(arr));

rest.set(document.querySelector('h1'), 'heading');
console.log(rest);
```

#### Making Quiz App

```js
const question = new Map([
  ['question', 'What is the best programming language in the world? '],
  [1, 'C'],
  [2, 'Java'],
  [3, 'Javascript'],
  ['correct', 3],
  [true, 'correct🎉'],
  [false, 'Try again!'],
  //Here we passed an array which contain also number of arrays with two elements for each.
]);
console.log(question);

// Convert objects to Maps
console.log(Object.entries(restaurant.openingHours)); // see console; maps is bit similar to this.

const hoursMap = new Map(Object.entries(restaurant.openingHours));
console.log(hoursMap);

// Iteration in Maps: it is also iterable.
console.log(question.get('question'));
// we will use get method to retrieve data from map.
// we simple put key in get then it will give value.
// Remember key will consider always first element and value is second element from insider array.
for (const [key, value] of question) {
  if (typeof key === 'number') {
    console.log(`Answer ${key}: ${value}`);
  }
}

const answer = 3;
const answer = Number(prompt('Your Answer?'));
// we are converting into a number because we have to compare with key values of answers(C, java, javascript);
console.log(answer);

console.log(question.get(question.get('correct') === answer)); // here we first compare and if it true then, it will give us a value from true key value and if it is false it will give value from false key;

// Convert Maps to Array
console.log([...question]);

console.log(question.entries());
console.log(question.keys());
console.log(question.values());

// we can also put all keys, and value in an array separately
console.log([...question.keys()]);
console.log([...question.values()]);
```

---

### WHEN_WHERE_WHICH

**When and where we use data structure?**

There are 4 build in data structures in **javascript** So,
Which data structure we should use.  
Basically **if we need just simple list of data then we use Array or Sets**, On the other hand **if we need key value pairs then we use Objects or Maps.**  
In key value pair, all those values that are describing, e-g, all those values that have some description, (key tells about description of value).

**_Source of Data_** _From where we get data and store in the data structures_
**Usually there may be 3 sources of data**

1. From the **program itself**. (Data written directly in the source code. ex: status messages )
2. Data **input from the user** or **data written in DOM**. (e.g tasks in todo app)
3. From **external source:** Data fetched from Web API. (data coming from other web pages. )
   Usually the most common source of data is getting data from web API (Application Programming Interface). Data from web APIs usually comes in special data formats called **JSON**. **JSON** can be converted easily into objects and arrays.

#### Array VS Sets (When use & where use)

**_Arrays:_** _Need ordered list of values -might contain duplicates-, and when need manipulate data b/c arrays have ton of methods to manipulate them._  
**_Sets:_** _When need unique values fo data, and when high performance is really important, also use to remove duplicates from array._

#### Objects Vs Maps

**Maps is better then objects:** b/c it has any data types, easy to iterate and easy to compute the size of the map.  
**Only advantage of object** is that easy to write and get data by simply using dot or bracket operator.  
And if we need functions as values then we should use object. In object these functions then called methods.

---

## STRINGS

WORKING WITH STRINGS

### length-Property_indexOf-Method_slice-Method

```JS
const airline = 'PIA Airline Pakistan';
const plane = 'A320';

console.log(plane[0]); // A
console.log(plane[1]); // 3
console.log(plane[2]); // 2

console.log('B71F'[0]); // B
```

#### length property

```js
console.log(airline.length); //20
console.log('B71F'.length); //4
```

As **Array**, **Strings** have also methods, some of them are quit similar to the array's methods.

#### indexOf() method

```js
console.log(airline.indexOf('a')); //13
```

It will give first occurrence of that element but we can get last one by using:  
**lastIndexOf()** method.

```js
console.log(airline.lastIndexOf('a')); // 18
```

For **entire word**

```js
console.log(airline.indexOf('Pakistan'));
```

**If a value not found it will return -1**, which may very useful

```js
console.log(airline.indexOf('India')); //-1
```

#### slice() method

```js
const slicedStr = airline.slice(4, 11);

console.log(slicedStr.length); // 7 (11 - 4)
```

Slice start from 0 and **initial index will count**(in this example 4) and **end index will not**(11). **The length of the extracted string will always end minus begin** (11 - 4) =7

**_By using any method the resultant string will be a new one,_** _It'll not change it original one it will remain always same._

Some soft coding using these methods

```js
console.log(airline.slice(0, airline.indexOf(' '))); // PIA // to extract first word from any string

console.log(airline.slice(airline.lastIndexOf(' ') + 1)); // to print last word.
// If we not specify +1 then it will also print space

console.log(airline.slice(-2)); // will start from end
console.log(airline.slice(1, -1)); // all characters except first and last one
```

```js
const checkMiddleSeat = function (seat) {
  // usually in plane, last digit denoted the column, B and E are middle seats
  const s = seat.slice(-1);
  if (s === 'B' || s === 'E') {
    console.log('You got the middle seat!');
  } else {
    console.log('You got lucky');
  }
};

checkMiddleSeat('23B');
checkMiddleSeat('12A');
checkMiddleSeat('14C');
checkMiddleSeat('12E');
```

---

### toUpperCase_toLoweCase

Change cases. toUpperCase / toLowerCase

```js
console.log(airline.toUpperCase());
console.log(airline); // no changes in original one
console.log(airline.toLowerCase());
console.log('muhammad'.toUpperCase());
```

_no changes in original one_  
Fix Capitalization in name // Intersection....

```js
const passenger = 'mUhammAD'; // we wants to convert into Muhammad
const passengerLower = passenger.toLowerCase();
const passengerCorrect =
  passengerLower[0].toUpperCase() + passengerLower.slice(1);
// slice(1) will print all element except first.
console.log(passengerCorrect); // Muhammad
```

A function for fix capitalization of any name.

```js
const correctName = function (name) {
  const passengerLower = name.toLowerCase();
  const passengerCorrect =
    passengerLower[0].toUpperCase() + passengerLower.slice(1);
  return passengerCorrect;
};

console.log(correctName('muhAmmAd'));
console.log(correctName('HArIs'));
console.log(correctName('haMmAd')); // Wow, all working!!!!!!
```

### trim-trimStart-trimEnd_Methods

#### Comparing Email and Trim Method

trim() will remove white spaces.also we have trimStar() and trimEnd().

```js
const email = '<hello@ugv.io>'; // Valid email
const loginEmail = ' <Hello@ugv.io> \n'; // Remember Backslash n is a replacement of enter key.
const lowerEmail = loginEmail.toLowerCase();
const trimmedEmail = lowerEmail.trim(); // trim() will remove white spaces.also we have trimstar() and trimend().
console.log(trimmedEmail);
```

Doing this 3 steps in one statement

```js
const normalizeEmail = loginEmail.toLowerCase().trim();
console.log(normalizeEmail); // just a same
```

### replace_Method

```js
const priceGB = '288,97PKR';
// In Europe comma (,) will written as a decimal point while in US the dot(.) will written.
// SO, hare we wants to compute this price to US, (using dot and dollar sign)
const priceUS = priceGB.replace('PKR', '$').replace(',', '.');
console.log(priceUS);
```

```js
const announcement =
  'All passengers come to boarding door 23. Boarding door 23!';
console.log(announcement.replace('door', 'gate')); // It will replace only first occurrence of specific word.

// To replace all instances we use replaceALL()
console.log(announcement.replaceAll('door', 'gate')); // replaced all
```

We have one more solution to replace all the occurrence of word or letter. here we will use **Regular Expression** (automata wala)  
**In regular expression we write strings in b/w slash**

```js
console.log(announcement.replace(/door/g, 'gate')); // changed!!
```

---

### includes_startWith_endWith_Methods

**There are three methods that returns boolean value** 1. **includes** 2. **startsWith**, 3. **endsWith**

```js
const planeNew = 'A320neo';
console.log(planeNew.includes('A320')); // true
console.log(planeNew.startsWith('A34')); //false
console.log(planeNew.startsWith('A32')); //true
console.log(planeNew.endsWith('eo')); //true
```

```js
const planeNew = 'Airbus A320neo';
if (planeNew.startsWith('Airbus') && planeNew.endsWith('neo')) {
  console.log('Part of New Airbus family!');
}
```

#### Practice Exercise

```js
const checkBaggage = function (items) {
  const baggage = items.toLowerCase(); // For any input from user we usually it converts to lower case. it make easy to compression.

  if (baggage.includes('knife') || baggage.includes('gun')) {
    console.log('You are not allowed on board!');
  } else {
    console.log('Welcome aboard!');
  }
};
checkBaggage('I have a Laptop, some food and a pocket knife.');
checkBaggage('Socks and Camera');
checkBaggage('Got some snacks and a gun for protection.');
```

---

### split_And_join_Methods

#### split() Method

**split() method** use to split strings into multiple parts based on divider string.

```js
console.log('a+vary+nice+string'.split('+'));
// It will split this strings based on + and the result will store into an array.

console.log('Muhammad Ahmad Shamim Ahmad'.split(' ')); // Array with 4 elements.
```

_Split method is very useful and powerful._  
**_Split method with destructuring._**

```js
const [firstName, lastName] = 'Muhammad Ahmad'.split(' ');
console.log(firstName, lastName);
```

#### join() Method

```js
const newName = ['Mr.', firstName, lastName.toUpperCase()].join(' ');
console.log(newName); // it will join all of these with space. of course we can put any letters instead

const newName2 = ['Mr.', firstName, lastName.toUpperCase()].join('-----');
console.log(newName);
console.log(newName2);
```

_**join method** and **split method** is **opposite** of each other, in **join method** we join using any string and in **split method** we split using any string._  
_join method join elements of array to form a string._  
_split method split string to an array._

```js
console.log(['a', 'b', 'c', 'd'].join('-')); //convert that array to string
console.log('a b c d'.split()); // converts this string into array.
```

##### Exercise

```js
const capitalizeName = function (name) {
  const names = name.split(' ');
  const namesUpper = [];
  for (const n of names) {
    // namesUpper.push(n[0].toUpperCase() + n.slice(1));
    // another way
    namesUpper.push(n.replace(n[0], n[0].toUpperCase()));
  }
  console.log(namesUpper.join(' '));
};

capitalizeName('muhammad ahmad shamim Ahmad');
capitalizeName('yougo baltistan');
```

---

### padStart_And_padEnd_Methods

**padding a string means add a number of characters to the string until the string has a certain desired length.**  
In these methods first we will put number of characters after adding this characters and then character itself, that we want to add.

```js
const message = 'Go to gate 23!';
console.log(message.padStart(25, '#').length);
// After adding #, the length of this entire string should be 25.

console.log(message.padStart(25, '-').padEnd(30, '-')); // At the end there should add only 5 -, b/c length is already 25. so 25+5=30
```

#### Practice Example

using cods of credit card look like this: **\*\***\***\*\***4323, In this function we will convert numbers to this type.

```js
const mastCreditCard = function (number) {
  // console.log(typeof number); //number
  // const str = String(number);
  // console.log(typeof str); //string

  // Another way (easier) to convert number to string:
  const str = number + '';
  // b/c by using + sign, when one of the operand is string it will convert others to string.
  const lastFour = str.slice(-4);
  console.log(`A code before masking: ${str}`);
  return lastFour.padStart(str.length, '*');
};

console.log(`A code after masking: ${mastCreditCard(1343089849384232)}`);
console.log(`A code after masking:${mastCreditCard('8943893449384232')}`);
console.log(`A code after masking:${mastCreditCard(4356389849438921)}`);
```

---

### repeat_Method

**Allows to repeat the same string multiple times**

```js
const message2 = 'Bad weather... All Departures Delays... ';
console.log(message2.repeat(5)); // it will print a long string, repeating this one message 5 times.
```

#### Practice Exercise:

```js
const planeInLine = function (n) {
  console.log(`There are ${n} planes in line ${'✈'.repeat(n)}`);
};

planeInLine(6); //n = no. of plane in line;
planeInLine(3);
planeInLine(12);
```

---

### String_Practice

```js
const flights =
  '_Delayed_Departure;fao93766109;txl2133758440;11:25+_Arrival;bru0943384722;fao93766109;11:45+_Delayed_Arrival;hel7439299980;fao93766109;12:05+_Departure;fao93766109;lis2323639855;12:30';

const getCode = (str) => str.slice(0, 3).toUpperCase();

for (const flight of flights.split('+')) {
  const [type, from, to, time] = flight.split(';');

  const output = `${type.startsWith('_Delayed') ? '🔴' : ''}${type.replaceAll(
    '_',
    ' '
  )}, ${getCode(from)} ${getCode(to)}, (${time.replace(':', 'h')})`.padStart(
    45
  );
  console.log(output);
}

// practice Exercise
// Find sum of any length of numbers.
const add = function (...nums) {
  let sum = 0;
  for (const num of nums) {
    sum += num;
  }
  return sum;
};
console.log(add(4, 2, 4, 11, 1, 3, 7));
```