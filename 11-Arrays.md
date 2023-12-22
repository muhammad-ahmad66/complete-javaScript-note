# BANKIST APP

## DATA

```js
const account1 = {
  owner: 'Muhammad Ahmad',
  movements: [200, 450, -400, 3000, -650, -130, 70, 1300],
  interestRate: 1.2, // %
  pin: 1111,
};

const account2 = {
  owner: 'Hammad Ahmad',
  movements: [5000, 3400, -150, -790, -3210, -1000, 8500, -30],
  interestRate: 1.5,
  pin: 2222,
};

const account3 = {
  owner: 'Haris Shehbaz',
  movements: [200, -200, 340, -300, -20, 50, 400, -460],
  interestRate: 0.7,
  pin: 3333,
};

const account4 = {
  owner: 'Adeel Ahmad',
  movements: [430, 1000, 700, 50, 90],
  interestRate: 1,
  pin: 4444,
};

const accounts = [account1, account2, account3, account4];
```

## Elements

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

const btnLogin = document.querySelector('.login**btn');
const btnTransfer = document.querySelector('.form**btn--transfer');
const btnLoan = document.querySelector('.form**btn--loan');
const btnClose = document.querySelector('.form**btn--close');
const btnSort = document.querySelector('.btn--sort');

const inputLoginUsername = document.querySelector('.login**input--user');
const inputLoginPin = document.querySelector('.login**input--pin');
const inputTransferTo = document.querySelector('.form**input--to');
const inputTransferAmount = document.querySelector('.form**input--amount');
const inputLoanAmount = document.querySelector('.form**input--loan-amount');
const inputCloseUsername = document.querySelector('.form**input--user');
const inputClosePin = document.querySelector('.form__input--pin');
```

### SOME QUICK TIPS

- For any specific functionality always use functions.
- Always pass variables as an arguments that being use in function instead of directly using.
- Always write a function for any specific task -good practice.

```js
const displayMovement = function (movements, sort = false) {
  // we added second parameter for sorting, by default it should not sort. and when click it will sort and by again clicking it should unsorted....

  containerMovements.innerHTML = '';
  // This is for empty the existing elements from containerMovements. NOTE: it's common.

  // Here we are not sorting actual array elements in array variable but we wants to only display when click.
  // Remember sort method will change entire array in sorted form. so...here we use slice method to copy of array.
  const movs = sort ? movements.slice().sort((a, b) => a - b) : movements;

  movs.forEach(function (mov, i) {
    const type = mov > 0 ? 'deposit' : 'withdrawal';
    const html = `
    <div class="movements__row">
      <div class="movements__type   movements__type--${type}">${
      i + 1
    } ${type}</div>
      <div class="movements__value">${mov}€</div>
    </div>
    `;
    // We can directly paste html data in string literal. string literal is v.v useful here
    containerMovements.insertAdjacentHTML('afterbegin', html);
  });
};

displayMovement(account1.movements);

// Now applying reduce method to calculate balance and print total
const calcDisplayBalance = function (acc) {
  acc.balance = acc.movements.reduce(function (acc, mov) {
    return acc + mov;
  }, 0);
  labelBalance.textContent = `${acc.balance}€`;
};
// calcDisplayBalance(account1.movements);
```

Implementing array chaining methods to the Application to calculate deposits, withdraws and interest.  
Again we'll implement in one function. -best practice-

```js
const calcDisplaySummary = function (acc) {
const incomes = acc.movements.filter(mov => mov > 0).reduce((acc, mov) => acc + mov, 0);
labelSumIn.textContent = `${incomes}€`;

const out = acc.movements.filter(mov => mov < 0).reduce((acc, mov) => acc + mov, 0);
labelSumOut.textContent = `${Math.abs(out)}€`;

const interest = acc.movements.filter(mov => mov > 0).map(deposit => deposit \* acc.interestRate / 100).filter(deposit => deposit >= 1).reduce((acc, interest) => acc + interest, 0);
labelSumInterest.textContent = `${interest}€`;
// second filter is for that: company is paying for only those who's interest is greater that one.
}

// calcDisplaySummary(account1.movements)
```

Now we are creating a user name using map method. user-account will contain first letter of each word of name from accounts objects.

```js
const user = 'Muhammad Ahmad Shamim'; // user name should Muh.
const createUserNames = function (accs) {
  accs.forEach(function (accs) {
    accs.username = accs.owner
      .toLowerCase()
      .split(' ')
      .map(function (name) {
        return name[0]; // to return only first letter from each element.
      })
      .join(''); // join is to convet array to string.
  });
};
createUserNames(accounts);
// console.log(accounts);
```

## EVENT HANDLERS

Building log-in functionality.

```js
let currentAccount;
const updateUI = function (acc) {
  // Display Movements
  displayMovement(acc.movements);

  // Display Balance
  calcDisplayBalance(acc);

  // Display Summary
  calcDisplaySummary(acc);
};

btnLogin.addEventListener('click', function (e) {
  // Remember : see console, when we click on button LOGIN is appearing in console and then disappearing. it's going b/c, in html when we click button it will reload the page. so it's disappearing....
  // To prevent from this we will pass an event in parameter and then..
  e.preventDefault(); // it'll prevent this form from submitting.

  currentAccount = accounts.find(
    (acc) => acc.username === inputLoginUsername.value
  );
  // console.log(currentAccount);

  if (currentAccount?.pin === Number(inputLoginPin.value)) {
    // If password and username are correct then?
    // display UI and Welcome message.

    labelWelcome.textContent = `Welcome Back ${
      currentAccount.owner.split(' ')[0]
    }`;

    containerApp.style.opacity = 100;

    // Clear Input Fields when successfully logged in
    inputLoginUsername.value = inputLoginPin.value = '';
    inputLoginPin.blur(); // to lose it's focus otherwise the cursor will blinking after login.

    // Display Movements
    // displayMovement(currentAccount.movements);

    // Display Balance
    // calcDisplayBalance(currentAccount);

    // Display Summary
    // calcDisplaySummary(currentAccount);

    updateUI(currentAccount); // calling function.
  }
});
```

### Building Money Transformation Functionality

```js
btnTransfer.addEventListener('click', function (e) {
  // Preventing Default hehavior of form's btn
  e.preventDefault(); // v.common, when workin with form

  const amount = Number(inputTransferAmount.value);
  const receiverAcc = accounts.find(
    (acc) => acc.username === inputTransferTo.value
  );

  // clear input fields and focus
  inputTransferAmount.value = inputTransferTo.value = '';
  inputTransferAmount.blur();

  if (
    amount > 0 &&
    receiverAcc &&
    amount <= currentAccount.balance &&
    receiverAcc?.username !== currentAccount.username
  ) {
    // adding in receiver a/c and subtracting from sender a/c
    currentAccount.movements.push(-amount);
    receiverAcc.movements.push(amount);

    // to update UI
    updateUI(currentAccount);
  }
});
```

### Use of some method

use to request a loan to a back.

```js
btnLoan.addEventListener('click', function (e) {
e.preventDefault();

const amount = Number(inputLoanAmount.value);

// conditions to receive load.
// loan only take if it have at least one (some) deposit that is greater or equal to the 10% of the loan.
if (amount > 0 && currentAccount.movements.some(mov => mov >= /_amount_ 10 / 100 OR _/ /\_amount / 10 OR_/ amount\_ 0.1)) {

    // add the movement
    currentAccount.movements.push(amount);

    // update UI
    updateUI(currentAccount)

}

// clearing input field
inputLoanAmount.value = '';
})
```

### Use of findIndex method

Here we want to delete the current account, if the user wants wo close his/her account, by using closeAccount button.

To delete any array element we use splice method, So first we have to find current account and then delete it. so to find account we use findIndex method.

```js
btnClose.addEventListener('click', function (e) {
  e.preventDefault(); // prevent from default behavior of form submission btn.

  // checking username and pin
  if (
    inputCloseUsername.value === currentAccount.username &&
    Number(inputClosePin.value) === currentAccount.pin
  ) {
    const index = accounts.findIndex(
      (acc) => acc.username === currentAccount.username
    );

    // Delete Account
    accounts.splice(index, 1); // will mutates original one.

    // Hide UI
    containerApp.style.opacity = 0;
  }
  // clear input fields and focus
  inputCloseUsername.value = inputClosePin.value = '';
  inputClosePin.blur();
});
```

### Sorting

```js
let sorted = false;
btnSort.addEventListener('click', function (e) {
  e.preventDefault();
  // displayMovement(currentAccount.movements, true);
  displayMovement(currentAccount.movements, !sorted); // same as true. b/c initially sorted is false

  // if click then flip the sorted value
  sorted = !sorted; // amazing!!!!

  // But here we have to do when ever user click sort button again it should return in normal form (-unsorted form)/
  // How we do this type of functionality???

  // We will solve this problem by using state variable it will monitor, currently sorted or not.
  // That variable should define outside of function. b/c we want to preserve that sorted state
});
```

---

## Arrays In JavaScript

## Table of Contents

1. [SOME_BASIC_ARRAY_METHODS](#some_basic_array_methods)
   1. [Slice_Method](#slice_method)
   2. [Splice_Method](#splice_method)
   3. [Reverse_Method](#reverse_method)
   4. [Concat](#concat)
   5. [Join_Method](#join_method)
   6. [Slice_Method](#slice_method)
   7. [At_Method](#at_method)
2. [forEach_Method](#foreach_method)
3. [insertAdjacentHTML_Method](#insertadjacenthtml_method)
4. [innerHTML_And_textContent](#innerhtml_and_textcontent)
5. [Data_Transformation_Methods](#data_transformation_methods)
   1. [MAP_METHOD](#map_method)
   2. [FILTER_METHOD](#filter_method)
   3. [REDUCE_METHOD](#reduce_method)
6. [CHAINING_METHODS](#chaining_methods)
7. [FIND_METHOD](#find_method)
8. [FIND_INDEX_METHOD](#find_index_method)
9. [SOME_AND_EVERY_METHODS](#some_and_every_methods)
10. [Separate_Callback_Function](#separate_callback_function)
11. [FLAT_AND_FLAT-MAP_METHOD](#flat_and_flat-map_method)
12. [SORTING_ARRAYS](#sorting_arrays)
13. [WAYS_TO_CREATING_AND_FILLING_ARRAYS](#ways_to_creating_and_filling_arrays)

---

Array has a countless methods. we've seen that a methods are a function in an objects, so array is also an object.  
**Method are just as tools.**

## SOME_BASIC_ARRAY_METHODS

let's talk about some basic methods.

### Slice_Method

**Slice Method:** It's just like a slice method in string. we can extract part of any array without changing the original one.

```js
let arr = ['a', 'b', 'c', 'd', 'e'];
console.log(arr.slice(2)); // c, d, e. will create new array.
console.log(arr.slice(2, 4)); // c, d end(4) not include in output
console.log(arr.slice(-1)); // e last one
console.log(arr.slice(-3)); // c,d,e
console.log(arr.slice(1, -2)); // b, c
```

**Also can use for shallow copy.** Just call without any argument.

```js
const cpyArr = arr.slice();
console.log(cpyArr);
```

We have also used **spread operator** to shallow copy

```js
const shallowCopy = [...arr];
console.log(shallowCopy);
```

---

### Splice_Method

Almost same but it change original array. mutate original array (**we can say it use to delete array elements**)

```js
const splicedArr = arr.splice(2);
console.log(arr); // a, b changed.
console.log(splicedArr); // c, d, e
```

To remove last element of array

```js
console.log(arr.splice(-1));
console.log(arr); // e is gone.
```

Unlike **slice method in splice method's second parameter is not an ending index but it's delete counter.**

```js
console.log(arr.splice(1, 2)); // from index 1 upto 2 elements.
console.log(arr);
```

---

### Reverse_Method

```js
arr = ['a', 'b', 'c', 'd', 'e'];
let arr2 = ['j', 'i', 'h', 'g', 'f'];
console.log(arr2.reverse());
console.log(arr2);
```

**It also changed in original one** NOTE

---

### Concat

```js
const letters = arr.concat(arr2);
console.log(letters);
console.log(arr);

// Another way (spread)
console.log([...arr, ...arr2]);
```

**Here No changes in original one.** ⤴

---

### Join_Method

**Join Method:** join all the array elements using any string value.
console.log(letters.join(' - '));

```js
console.log(letters);
```

**No changes in original one** NOTE

---

### At_Method

[A New Method in ES22] [AT METHOD]

```js
const arr = [23, 11, 64];
console.log(arr[0]);
// Alternative to this!!!
console.log(arr.at(0));
```

One useful application of at method is to log last array element

```js
console.log(arr[arr.length - 1]);
// alternative to this!!! ⤵

// alternative by using slice method
console.log(arr.slice[-1](0)); // last element
```

**Alternative by using at method** [quite simple]

```js
console.log(arr.at(-1)); // last element
console.log(arr.at(-2)); // second last element....

for (let i = 1; i <= arr.length; i++) {
  console.log(arr.at(-i));
}
```

#### At method also works on string

```js
console.log('Muhammad Ahmad'.at(0));
console.log('Muhammad Ahmad'.at(-1));
```

---

## forEach_Method

We can loop over an array using for-each method

```JS
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
```

In movements array positive value are deposits and negative values are withdraws.  
We are printing whether the user deposited or withdrew.

First using **for-of loop**

```JS

for (const [i, movement] of movements.entries()) {
if (movement > 0) {
console.log(`Movement ${i + 1}: You deposited ${movement}`);
} else {
console.log(`Movement ${i + 1}: You withdrew ${Math.abs(movement)}`);
}
}
```

Now using **for-each method**

**forEach method requires a call back function. forEach is a higher order function.**  
forEach method will call that function, we are not calling it ourselves as alway in callBack functions

forEach method does, loop over the array, and in each iteration it will execute this call back function. also as a forEach method call that callback function in each iteration it will pass in the current element of the array as an argument.  
Function will be an anonymous. (without name)

```js
console.log('--- From forEach Method ---');
movements.forEach(function (movement) {
  if (movement > 0) {
    console.log(`You deposited ${movement}`);
  } else {
    console.log(`You withdrew ${Math.abs(movement)}`);
  }
});
```

**What we should do when need counter variable here??** (Index)  
We can pass in parameter. **We can use any of these three parameters, but order of those parameters are v.important.**

**First Parameter:** Always current element.  
**Second Parameter:** Always current index.  
**Third Parameter:** Always entire array.  
**We can use all three together, or one only or two, BUT order is really important.**

```js
console.log('\n--- From forEach Method ---');
movements.forEach(function (mov, i, arr) {
  if (mov > 0) {
    console.log(`Movement ${i + 1}: You deposited ${mov}`);
  } else {
    console.log(`Movement ${i + 1}: You withdrew ${Math.abs(mov)}`);
  }
});
```

When should use **forEach** and When **for-of** ?
Well, one fundamental difference between two of then that we cannot break out of a forEach loop. So **the continue and break statements do not work in forEach loop at all.** instead forEach method always loop over entire array So, if we really need to break out of a loop then use for-of loop. Otherwise forEach is perfect in any case.

---

### forEach Method on Map and Set

ForEach method is also available on maps and sets.

### On MAP

```js
const currencies = new Map([
  ['USD', 'United States dollar'],
  ['EUR', 'Euro'],
  ['GBP', 'Pound sterling'],
]);
```

First argument: current value  
Second : current key  
Third: entire map

```js
currencies.forEach(function (value, key, map) {
  console.log(`${key}: ${value}`);
});
```

### ON SET

```js
const currenciesUnique = new Set(['USD', 'PKR', 'GBP', 'EUR', 'USD', 'PKR']);
console.log(currenciesUnique);
currenciesUnique.forEach(function (value, \_, set) {
console.log(`${value}: ${value}`); // here key and value are same whay? b/c a set doesn't have any key/index
// console.log(set);
})
```

---

## insertAdjacentHTML_Method

**To insert any html elements,** We will use a method called insertAdjacentHTML. We select the element where we want to add html elements and then will add using insertAdjacentHTML method. will select container(parent) element. This method will accept two strings;

**First** one for a position in which we want to attach the html. (check mdn). If we want before the container element we use **beforebegin** and if to insert in starting of the container we use **afterbegin** and if we want insert in ending of container will use **beforeend** and for after the container will use **afterend**.

**Second** parameter for which one we want to insert.  
Using insertAdjacentHTML method all we have to create a string and then pass in parameter. String lateral will very useful here.

---

## innerHTML_And_textContent

**InnerHTML VS TextContent** Almost similar but the difference is the textContent simply returns the text itself while innerHTML returns everything, including the HTMLs(all the html tags, attributes of that tag will be included)

```js
console.log(containerMovements.innerHTML);
console.log(containerMovements.textContent);
```

## Data_Transformation_Methods

In javascript there are three big and most important array methods that we use all the time to perform data transformations  
**Transformation means transfer data from one array to another.**  
These are methods that we use to create new arrays based on transforming data from other arrays.  
In recent years these tools become very popular and for good reasons

[MAP_METHOD](#map_method)
[FILTER_METHOD](#filter_method)
[REDUCE_METHOD](#reduce_method)

### Brief Intro

1. MAP METHOD
   MAP method is yet another method we can use to loop over array. MAP method is similar to the forEach method, but difference is that map creates a brand new array based on the original array. It always return a new array.

   A MAP method takes an array, loops over that array, and in each iteration it apply a call back function that we specify in our code to the current array element.

   We say that it maps the values of the original array to a new array, that's way it's called map.  
    And it's extremely, usually, way more useful than forEach.

2. FILTER METHOD
   USED to filter elements in the original array which satisfy a certain condition.  
   It returns a new array which satisfies the given condition.  
   IN OTHER WORDS: Elements for which the condition is true will be included in new array that the filter method returns.

3. REDUCE METHOD:
   USED to boils(reduce) all array elements down to one single value (very useful in many situations).i-e: We can add all array elements and make it one single value.

   It will not return any new array BUT only new value that is reduced from array. so it returns a simple variable.

---

NOW ONE BY ONE IN MORE DETAIL:

### MAP_METHOD

const euroToUSd = 1.1;
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];

We are going to convert this usd movements into euro, by multiplying each item with 1.1, which's stored in euroToUSD.  
As we know map method returns a brand new array so we store in array variable.

```js
const mappedArray = movements.map(function (value) {
  return 66; // it simple return 66 in each iteration, and will store in mappedArray variable.
});
```

```js
const movementsUSD = movements.map(function (mov) {
  return mov * euroToUSd;
  // by this it'll multiply each element by 1.1 in each iteration.
});
console.log(movements);
console.log(movementsUSD);
```

Same thing using for-of loop⤵

```javascript
const movementsUSDfor = []; // then we push in it, in each iteration.
for (const mov of movements) {
  movementsUSDfor.push(mov * euroToUSd);
}
console.log(movementsUSDfor);
```

Now using Arrow function for callback function

```js
const movementsUSDarrow = movements.map((mov) => mov * euroToUSd); // v.simple
console.log(movementsUSDarrow);
```

Like forEach method, MAP method also give access to exact same three parameters **cur_element**, **cur_index**, **entire_array**

```js
const movementsDescriptions = movements.map((mov, i, arr) => {
  if (mov > 0) {
    return `Movement ${i + 1}: You deposited ${mov}`;
  } else {
    return `Movement ${i + 1}: You withdrew ${Math.abs(mov)}`;
  }

  // Alternative to above five lines code in just one line. : (using ternary operator)
  return `Movement ${i + 1}: You ${
    mov > 0 ? 'deposited' : 'withdrew'
  } ${Math.abs(mov)}`;
});

console.log(movementsDescriptions);
```

_**Remember: We'll not call the callback function, MAP method will call this function for each array element in the movement array Each time when map method call or callback it will simply pass in the current array element as well as current index and whole array.**_

**forEach method creates a side effect**, as we perform some actions using forEach method it will visible in console we can call this a side effect. it will create a side effect in each of the iteration. **BUT IN MAP** we didn't create any side effect in each of the iteration.  
**Side effects means doing something but can't returning any thing**

---

### FILTER_METHOD

Use to filter an element which satisfy a certain condition. **How do we specify such condition???**  
We use callback function again to add condition....

```js
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
```

Like **map** and **forEach** method, here callback function also take current_element, current_index, and entire array.  
Here we are going to create an array with only deposits.

```js
const deposits = movements.filter(function (mov) {
  return mov > 0; // we are giving a boolean value. a/c to this condition it decide return or not!!!!!!!!!
});
console.log(movements);
console.log(deposits);
```

Same functionality using for of loop

```js
const depositsfor = [];
for (const mov of movements) {
  if (mov > 0) depositsfor.push(mov);
}
console.log(depositsfor);
```

**Practice Exercise:** Creating an array of withdrawals using filter method and arrow function

```js
const withdrawals = movements.filter((mov) => mov < 0);
console.log(withdrawals);
```

---

### REDUCE_METHOD

Use to reduce(boil down) all the elements in an array to one single value.

const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
console.log(movements);

Here we are go to add all the elements from movements array using reduce method.  
In this example The result should be the total balance in the account. (Remember only one value)  
It also gets a callback function BUT some thing little bit difference here.

In reduce method first parameter of callback function is **accumulator** and rest are the same. **accumulator, cur_el, cur_index, entire-arr**

**Remember** Reduce method takes two arguments

1. Callback Function
2. Initial value of accumulator (value of acc in first iteration)

**Accumulator is like a snowball that keeps accumulating the value that we ultimately want to return.**

```js
const balance = movements.reduce(function (acc, cur, i, arr) {
  console.log(`Iteration ${i}: ${acc}`);

  return acc + cur; // accumulator will holds resultant vale in each iteration.
}, 0);
console.log(balance);
```

Using arrow function

```js
const balance = movements.reduce((acc, cur, i) => acc + cur, 0);
console.log(balance);
```

Now same code with for-of Loop

```js
let balance2 = 0;
for (const mov of movements) {
  balance2 += mov;
}
console.log(balance2);
```

One more example of Reduce method. **Find Maximum value from movements Array.**

```js
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];

const maxNum = movements.reduce(function (acc, mov, i, arr) {
  if (acc > mov) {
    return acc;
  } else {
    return mov;
  }
}, movements[0]);

console.log(maxNum);
```

Now using Ternary Operator and Arrow Function

```js
const max = movements.reduce(
  (acc, mov) => (acc > mov ? acc : mov),
  movements[0]
);
console.log(max);
```

---

## CHAINING_METHODS

Chaining means we can use multiple methods in one statement or even we can use same method multiple time in one statement. **Chaining multiple methods one after another.**  
For example from our example if we have to take all the deposit movements and then convert them to USD and then sum up all the deposits. we can do this by chaining.⤵

```JS
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
const euroToUSD = 1.1;

const totalDeposits = movements.filter(mov => mov > 0).map(mov => mov \* euroToUSD).reduce((acc, mov) => acc + mov, 0);
console.log(totalDeposits);
```

Here filter method is called on movements Array BUT MAP method called on RESULT of movement.filter and also reduce method is called on RESULT of mapped Array.

**Remember Remember:** Couple of Remarks about Chaining: (JC)

- We should not use over chaining. b/c chaining tons of methods can cause a real performance issues and also difficult to bug fixing.
- It's a bad practice in Javascript to chain methods that mutate(change) the underlying original array. e-g **splice method** or **reverse method**

---

## FIND_METHOD

We use **FIND method** to retrieve one element of an array based on a condition.  
Just like previous methods find method also accept a callback function, which will be called as the find method loops over the array

```js
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
const firstWithdrawal = movements.find((mov) => mov < 0);
console.log(movements);
console.log(firstWithdrawal); // -400, which is first element that meets the condition.
```

Remember **It seems very similar to filter method BUT unlike filter method it returns only one element,** not array. **filter method** will return an array based on certain condition.

Remember **It only return first element from the array that satisfy give conditions.** if any of all elements from entire array not satisfy the condition, then it will return **undefined**.

### FILTER METHOD V/S FIND METHOD

- **FILTER** method returns all the elements that matches the condition, while **FIND** method returns only first one.
- **FILTER** method returns an new array while **FIND** method only returns the element itself from the array. Element may be object, string, number, or any else from array.

Lets take more common **example** of FIND METHOD:  
We will find the object in the array using a property of that object. Very common use case.

```js
console.log(accounts); // which is array of objects -that is v common in JS.

// we'll find the object in the array using a property of that object
const account = accounts.find((acc) => acc.owner === 'Jessica Davis');
console.log(account);
// NOTE : This type of example is v.common in JS.

// same code using for-of loop
let accountsFor = {};
for (const acc of accounts) {
  if (acc.owner === 'Jessica Davis') accountsFor = acc;
}
console.log(accountsFor);
```

---

## FIND_INDEX_METHOD

Introducing Close Cousin of FIND Method (findIndex(function()))

findIndex method works almost same way as find method. BUT **findIndex returns the index of the found element, not element itself.**  
As all of other it will accept a callback function where we define a condition.  
It will return the index of the first element that matches the condition.

**indexOf method** is also similar to this one but in **indexOf method** we can pass any array element, we can't pass any expression/condition in it. so for a simple element search we use **indexOf method**, and to search by any boolean value we use **findIndex method.**

Both of **FIND** and **FIND_INDEX** method also can get access to the **current_element**, **current_index** and **entire_array**, as in **forEach**, **Map**, **Filter** and **Reduce** methods.  
And Both( find and findIndex ) are newer in javascript, (introduced in ES6 )  
Examples are in Bankist application.

---

## SOME_AND_EVERY_METHODS

Introducing Close Cousin of **INCLUDES Method** | **SOME AND EVERY METHODS** (some(function()), every(function()))

### SOME_METHOD

Use to check existence of any value in array using conditions. then if the condition is gone true (element found), **it'll return true else false.**

Lets look back at the includes method. It also return true or false.  
By using include method We can only check for exact same array element (equality ===), in blew it'll check for 'any array element === -30'

```js
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
console.log(movements.includes(-130));
```

What if we want to test a **condition** instead of **equality**???

So here **some method** comes into play!

we will check is there any positive number in movements array

```js
const isAnyDeposit = movements.some((mov) => mov > 0);
console.log(isAnyDeposit); // true

const maxDeposit = 5000;
const AnyDepositAboveMax = movements.some((mov) => mov > maxDeposit);
console.log(isAnyDeposit); // true
```

#### Remember some points about some method

- we've to pass callback function as we have doing in....
- we can test for any conditional statement
- will return true or false.

---

### EVERY_METHOD

Very similar to some method, difference b/w them is that **some method** will return true if one element in the array satisfy the condition ON THE OTHER HAD **every method** will return true if all the array element satisfy the condition.

In other word: If every element passes the test, in callback function, only then the every method returns true.

```js
console.log(movements.every((mov) => mov > 0)); // false
console.log(account4.movements); // all +ve here.
console.log(account4.movements.every((mov) => mov)); // true
```

---

## Separate_Callback_Function

Up until this point we written callback function directly as an argument in all of these array methods.  
HOWEVER we could also write this function separately then pass the function as a callback.

```js
const deposit = (mov) => mov > 0;

console.log(movements.every(deposit)); // false
console.log(movements.some(deposit)); // true
console.log(movements.filter(deposit)); // will return +ve.

console.log(account4.movements.every(deposit)); // true
```

---

## FLAT_AND_FLAT-MAP_METHOD

[ES19 -newer]

1. Flat method

```js
const arr = [[1, 2, 3], [4, 5, 6], 7, 8, 9];
console.log(arr.flat());
```

**Very simple and straight forward. It will simply flat a nested array to one array.**  
It means it will return an array that will contain all the array elements by removing nested. It will remove only nested array not nested array elements. From above example it will print on whole array which contains nine elements in flat form -no nested- like this. [1, 2, 3, 4, 5, 6, 7, 8, 9] -new array. **By remove nested array, NOT nested array elements.**

Very simple and no callback function.

```js
const arrDeep = [[[1, 2], 3], [4, [5, 6]], 7, 8];
console.log(arrDeep.flat());
```

⤴ This example will print like this[[1,2] 3, 4, [5, 6], 7,8] It means the flat method goes only one level deep by default -with no parameter.  
**To fix this we will pass a depth as a parameter. by default the depth is 1.**

```js
console.log(arrDeep.flat(1)); // should remain same as above
console.log(arrDeep.flat(2)); // will remain in one array -no nested.
```

**Let's take a practical example**  
Consider that. in bankist application we have to calculate all the movements (over all, not from one account.)

First we will take all the movements from objects.

```js
const accountMovements = accounts.map((acc) => acc.movements);
console.log(accountMovements);
```

Then we'll use flat method to combine all of them into one array

```js
const allMovements = accountMovements.flat();
console.log(allMovements);
```

Now add them all to find overall balance

```js
const overallBalance = allMovements.reduce((acc, mov) => acc + mov, 0);
console.log(overallBalance); // Yes.....
```

All of above steps in one statement by chaining all methods

```js
const overallBalance2 = accounts
  .map((acc) => acc.movements)
  .flat()
  .reduce((acc, mov) => acc + mov, 0);
console.log(overallBalance); // working.....
```

---

### FLAT_MAP

Using a map method first and then flating the result of map method is very common operation in javascript, exactly what we had done here ⤴ .

So here one new method comes in to play. **FLAT-MAP**  
FLAT-MAP method combines a map method and flat method into just one method for better performance.  
**flatMap will receive a callback function while flat not.**

```js
const overallBalance3 = accounts
  .flatMap((acc) => acc.movements)
  .reduce((acc, mov) => acc + mov, 0);
console.log(overallBalance3); // also giving correct result.
```

**Remember:** Using flatMap method we only can go to one level deep, if we've to go in more depth we still need flat method and then map method.

---

## SORTING_ARRAYS

### Sort Method With Strings

```js
const brothers = ['Muhammad', 'Hammad', 'Haris', 'Adeel'];
console.log(brothers.sort()); // sorted
```

Remember: **This (sort method) mutates the original array.**
console.log(brothers); // mutated

### Sort Method with Numbers

```js
const movements = [200, 450, -400, 3000, -650, -130, 70, 1300];
console.log(movements);
console.log(movements.sort());
```

**NOT WORKING!!!** This time result is not really what we are expecting.. **Why????**  
Reason for this is that sort method does the sorting based on strings -weird-  
If we look at the result it's seems correct if we are working with strings. but we are not working with strings. It sorted with only first digits. like 2, 4, 46, 6, 69, 77...

### FIXING

**We can fix this by passing in a compare callback function into the sort method.**  
This callback function will receive two arguments.

1. current value
2. next value

Sort Method will call this functions in each iteration and will compare this two numbers in each iteration.

### Sorting in Ascending Order

If we are returning some negative number then **a** will be before **b** and if we are returning some positive number then **b** will be before **a**. that's rule of comparing:  
**return < 0 then a, b**
**return > 0 then b, a**

```js
movements.sort((a, b) => {
  if (a > b) {
    return 1; // number here does't meter, should +ve number, that's rule of comparing.
    // It means if a is > b then will exchange.
  }
  if (a < b) {
    return -1; // should any -ve number. No exchange here
  }
});
console.log(movements); // Now indeed sorted.
```

_Remember: **Returning +ve means switch the order and Returning -ve means keep order.**_

**NOTE & Remember All of these are sorting in ascending order If we have to sort in descending order, condition will be opposite.**

Remember: we are working with only numbers then we can simplify this a lot by using some maths.  
**If 'a' is greater than 'b' then 'a-b' will some +ve value.**  
**If 'a' is less than 'b' then 'a-b' give some -ve number.**

```js
movements.sort((a, b) => a - b); // that's it
console.log(movements);
```

A/c to rule if return +ve then will interchange, so if a is greater than b, it gives positive so should switch order, **like 2 - 1 = 1**. Else if return -ve, then should keep their order, so if a is already smaller then b, it will give -ve, negative means there should no interchange, **like 2 - 5 = -3**

### Sorting in Descending Order

**return > 1 then a, b**  
**return <1 then b, a**

```js
movements.sort((a, b) => {
  if (a > b) {
    return -1;
  }
  if (a < b) {
    return 1;
  }
});
console.log(movements); // yes! also working.
```

For Descending order we will use b minus a. (shorter way!!)

```js
movements.sort((a, b) => b - a);
console.log(movements); // working.
```

✔ a - b For Ascending Order  
✔ b - a For Descending Order

**Reminder:** Sorting functionality are implemented in bankist application in displayMovements function.⬇⬇⏬

---

## WAYS_TO_CREATING_AND_FILLING_ARRAYS

// UP UNTIL WE USED THIS TWO METHODS TO CREATE AND FILL ARRAY
// 1.
const array1 = [2, 1, 4, 2, 5, 6, 8];
console.log(array1);
// 2.
const array2 = new Array(1, 2, 3, 8, 4, 5, 3, 6);
console.log(array2);

// In these cases we already have data, before creating an array.

// However we can also generate arrays programmatically, witout having to define all the items manually.
// There are several way to doing that

// Easiest Way. [Array Constructor]
const x = new Array(7);
console.log(x); // unexpected result. instead of an array with only one element(7), it creats an array of length 7 with empty values.
// So, Remember whenever we creates an array usign Array function and pass only one argument, it will create an array of that length with empty values. But if we pass more than one arguments it will create and array of that values.
// const y = new Array(7, 8);
// console.log(y); // [7, 8]

// this not very useful b/c we can't apply any method to this empty array like
console.log(x.map((x) => 5)); // not working
// But only work one method that's fill method.
// x.fill(4);
// console.log(x);

// ------- FILL METHOD ------- //

// Subheading: fill Method will fill entire array with the value that's passed in as argument to fill method.
// Remember : fill method will mutate original array.

// If we specify one argument then fill method will fill entiere array with that number.
x.fill(2); // it will fill entire array with 2
console.log(x);

// and if we specify two argument then first should a value, which we wants to fill and second will be begin index, where we wants to start filling. it'll fill beg to end.. like
x.fill(3, 2) // it will fill value 2 starting index = 2.
console.log(x);

// and if we specify three arguments then third will be end index. (ending index will not consider to fill)
// all is much similar to slice.
x.fill(9, 3, 5); // ending index not include
console.log(x);

// Remember : We can also fill existing filled array, it doesn't have to be an empty array.
const arr = [3, 4, 2, 1, 5, 9];
arr.fill(2, 1, 4);
console.log(arr); // [3, 2, 2, 2, 5, 9]

/////////////////////////////////////////

// Subheading: --- Array.from FUNCTION ---

// What if we want to create any array programmatically?
// for that we could use Array.from() function

// Array.from will takes two arguments one will be an object were we specity length as an property and give it value, as a length of an array and the second argument will be a mapping function, it's exactly callback function that we passed in map method. but here we will not specify any argument, only we will give new value for just filly any number but if we need than, can specify curr_el, curr_i, arr like in map method.

const y = Array.from({ length: 7 }, () => 1) // here Array is a function and from is a method.
// it'll create array with length 7 and put 1 in each index.
console.log(y); // [1, 1, 1, 1, 1, 1, 1]

const z = Array.from({ length: 7 }, (_, i) => i + 1) // Reminder underscore (_) use to throwaway variable (unneed)
console.log(z); // [1, 2, 3, 4, 5, 6, 7]

// Array.from a really made to convert other iterables (Map, sat, string ) to an array. that's whay we it's name is Array.from => (array from others)

// Remember : Like map, set & strings, result of querrySelectorAll is also an array like structure (iterable).
// Reminder : querrySelectorAll will return nodelist, which is some thing like an array, which contain all the selected elements. it's not real array so, it doesn't have methods like map(), reduce() etc.
// if we wanna apply these methods then we've to first convert that nodelist to an array And for that Array.from is perfect.

// lets see example
// consider we have't values of movements in real array, all movements are stored only on user interface. we do't have some where in our code. But for some how we have to applay some array methods on it............

// Remember
// Arrary.form => takes two arguments
// 1. may be length in object or if we are converting then should be any iterable.
// 2. should be maped callback function.

labelBalance.addEventListener('click', function () {
const movementsUI = Array.from(document.querySelectorAll('.movements\_\_value'), el => Number(el.textContent.replace('€', '')));
console.log(movementsUI);

// -- Another way to conver querrySelectorAll to array --
// we can also use spread opeator to conver into an array

const movementsUI2 = [...document.querySelectorAll('.movements__value')];
console.log(movementsUI2);
})

\*/

/\*
////////////////////////////////////////////////////
///////////////////////////////////////////////////

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////

// Heading Heading Heading

// SUMMARY OF ARRAY METHODS

// Since begining of the cousrse we have studied 23 differenct arrays methods.

// So BIG QUESTION IS THAT, WHICH ONE TO USER WHEN ??????

// to find the answer, first lets ask one more question
// What I want to do (operation)????

// Q: Do I Want to mutate origina?

// - To Add
// - push() -at end
// - unshift() -at begin

// - To remove
// - pop() -from end
// - shift() -from begin
// - splice() -from any where

// - Others
// - reverse()
// - sort()
// - fill()

////////////////////////////

// Q: Do I Want a New Array?

// - Compute from original
// - map()

// - Filter using condition
// - filter()

// - Portion of Original
// - slice()

// - Adding original to other
// - concat()

// - Flattening the original
// - flat()
// - flatMap()

////////////////////////////

// Q: Do I Want Array Index?

// - Index based on Value
// - indexOf()

// - Index based on condition
// - findIndex()

////////////////////////////

// Q: Do I Retrive Array Element?

// - Array element Based on condition
// - find()

////////////////////////////

// Q: Do I Konw If Array includes?
// ---- true OR false -----

// - Based on value:
// - includes

// - Based on condition
// - some()
// - every()

////////////////////////////

// Q: Do I Get a new string?

// - Transform array to string:
// - join()

////////////////////////////

// Q: Do I Transform array to new value?

// - Based on accumulator
// - reduce()
// Boiled value may be any type like number, string, boolean, object, or even array

////////////////////////////

// Q: Do I Simply loop over the Array?

// - Based on callback
// -forEach()
// loop over an array Without producing any resultant value
// Doesn't create any new value.
// do some operation

////////////////////////////

/////////////////////////////////////////////////
/////////////////////////////////////////////////

// Heading

// --- ARRAY EXERCISEs --- //
/\*

// - Exercise #01
// Sum of overall deposits in bank (not in one account).
const bankDepositSum = accounts.flatMap(acc => acc.movements).filter(mov => mov > 0).reduce((accu, deposit) => accu + deposit, 0)

console.log(bankDepositSum);

// - Exercise #02
// count how many deposits in back with at least 1000:

// - method 01 ( using .length propery )
const numDeposits1000 = accounts.flatMap(acc => acc.movements).filter(deposit => deposit >= 1000).length;
console.log(numDeposits1000); // 6

// - method 02 ( using .reduce method )
const numDeposits1000_2 = accounts.flatMap(acc => acc.movements).filter(deposit => deposit >= 1000).reduce((acc) => ++acc, 0);
console.log(numDeposits1000_2); // same answer

// - method 03 ( using .reduce method )
// const numDeposits1000_3 = accounts.flatMap(acc => acc.movements).reduce((count, mov) => mov >= 1000 ? count++ : count, 0);
const numDeposits1000_3 = accounts.flatMap(acc => acc.movements).reduce((count, mov) => mov >= 1000 ? ++count : count, 0);
console.log(numDeposits1000_3); // same answer

// Remember: In last methods If we add ++ after count (count++) instead of ++count, here it's not working. whay??
// To understand this lets take simple example:
let a = 10;
console.log(a++); // 10 (not incremented) why?
// Actually the ++ operator does increment the value but it still returns the previous value. if we log a again without ++ it should 11 (incremented);
console.log(a); // 11
// solution is prefixed increment operator
console.log(++a); // 12

// so conclusion: // Remember Remember
// If we add increment operator after the variable then, it will return the original value than will add by one. and if we write increment operator before variable than, it will add and after adding will return added value.

// - Exercise #03.
// using reduce method we are going to create a new object (i-e, reduce is returning an object)
// calculate sum of deposits and withdrawals

const sums = accounts.flatMap(acc => acc.movements).reduce((sums, curr) => {
curr > 0 ? sums.deposits += curr : sums.withdrawals += Math.abs(curr);
return sums;
}, { deposits: 0, withdrawals: 0 });

console.log(sums);

// destructure it
const { deposits, withdrawals } = accounts.flatMap(acc => acc.movements).reduce((sums, curr) => {
curr > 0 ? sums.deposits += curr : sums.withdrawals += Math.abs(curr);
return sums;
}, { deposits: 0, withdrawals: 0 });

console.log(deposits, withdrawals);

// to access object property, using braket notaion insted of .dot:
const { depositsBr, withdrawalsBr } = accounts.flatMap(acc => acc.movements).reduce((sums, curr) => {
sums[curr > 0 ? 'depositsBr' : 'withdrawalsBr'] += curr;
return sums;
}, { depositsBr: 0, withdrawalsBr: 0 });

console.log(depositsBr, withdrawalsBr);

// This example BUT this time returning an ARRAY.

const sums1 = accounts.flatMap(acc => acc.movements).reduce((sums, curr) => {
curr > 0 ? sums[0] += curr : sums[1] += curr;
return sums;
}, [0, 0])

console.log(sums1); // Wow Working!!!!

// Now destructuring this Arrays.
const [deposits, withdrawals] = accounts.flatMap(acc => acc.movements).reduce((sums, curr) => {
curr > 0 ? sums[0] += curr : sums[1] += curr;
return sums;
}, [0, 0]);
console.log(deposits, Math.abs(withdrawals));

// - Exercise #04:

// A function to convert any string to title case. title case means capitalize all words from a sentence, except some of them. like... (in this case except a)
// this is a nice title -> This Is a Nice Title.

const convertTitleCase = function (title) {

const capitalize = function (str) {
const upperStr = str[0].toUpperCase() + str.slice(1);
return upperStr;
}
const exceptions = ['a', 'an', 'the', 'on', 'and', 'but', 'or', 'in', 'with'];

const titleCase = title
.toLowerCase()
.split(' ')
.map(word => (exceptions.includes(word) ? word : capitalize(word))).join(' ');

return capitalize(titleCase); // to convert first letter of sentence, if 1st is from any exceptions.
};
console.log(convertTitleCase('this is a nice title'));
console.log(convertTitleCase('this is a LONG title but not too long'));
console.log(convertTitleCase('and here is ANOTHER TITLE with an example'));

\*/

/////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////

// CODING CHALENGES

/\*

// CODING CHALLENGE #01;

const checkDogs = function (dogsJulia, dogsKate) {
const dogJoliaCorrected = dogsJulia.slice();

dogJoliaCorrected.splice(0, 1);
dogJoliaCorrected.splice(-2);
// console.log(dogJoliaCorrected);

const dogs = dogJoliaCorrected.concat(dogsKate);
console.log(dogs);

dogs.forEach(function (dog, i) {
dog >= 3 ? console.log(`Dog number ${i + 1} is an adult, and is ${dog} years old`) : console.log(`Dog number ${i + 1} is still a puppy 🐶`);
})
}
const juliaDog = [3, 5, 2, 12, 7];
const kateDog = [10, 5, 6, 1, 4];
checkDogs(juliaDog, kateDog)

\*/

/\*

const letters = ['a', 'b', 'c', 'd', 'e', 'f'];
letters.forEach(function (letter, i) {
console.log(`At Position ${i + 1}: ${letter.toUpperCase()}`);
});

\*/

/\*
///////////////////////////////////////////////////////
//////////////////////////////////////////////////////

/// Heading: Coding Challenge #02 [MAP, FILTER $ REDUCE METHODS]

const caclAverageHumanAge = function (ages) {
const humagAges = ages.map(function (age, i) {
if (age <= 2) {
return 2 _age
} else {
return 16 + age_ 4;
}
})
console.log(humagAges);

const adultDogs = humagAges.filter(function (age) {
return age > 18;
})

console.log(adultDogs);

const avgAge = adultDogs.reduce(function (acc, age, i) {
return acc + age;
}, 0) / adultDogs.length

// Another way to calculate average.
// (2+3)/ 2 is same as 2/2 + 3/2 so,
const average = adultDogs.reduce(function (acc, age, i, arr) {
return acc + age / arr.length;
}, 0)

return avgAge
// return average;

};

const testData1 = [5, 2, 4, 1, 15, 8, 3];
const testData2 = [16, 6, 10, 5, 6, 1, 4];

console.log(caclAverageHumanAge(testData1));
console.log(caclAverageHumanAge(testData2));

\*/

/\*

/// Heading: Coding Challenge #03 [ CHAINING METHODS & ARROW FUNCTION ]

const caclAverageHumanAge = ages => ages.map(age => age <= 2 ? 2 _age : 16 + age_ 4).filter(humanAge => humanAge > 18).reduce((acc, age, i, arr) => acc + age / arr.length, 0);

console.log(caclAverageHumanAge([5, 2, 4, 1, 15, 8, 3]));
console.log(caclAverageHumanAge([16, 6, 10, 5, 6, 1, 4]));

\*/

////////////////////////////////////////////////////
///////////////////////////////////////////////////

// Heading : --- CODING CHALLENGE # 04 (FINAL) ---

// 1.
const dogs = [
{ weight: 22, curFood: 250, owners: ['Alice', 'Bob'] },
{ weight: 8, curFood: 200, owners: ['Matilda'] },
{ weight: 13, curFood: 275, owners: ['Sarah', 'John'] },
{ weight: 32, curFood: 340, owners: ['Michael'] },
];

dogs.forEach((dog) => {
dog.recommendedFood = Math.trunc(dog.weight \*_0.75_ 28);
})
console.log(dogs);

// 2.
const SarahDogs = dogs.find(dog => dog.owners.includes('Sarah'))

console.log(`Sarah's dog is eating ${SarahDogs.curFood > SarahDogs.recommendedFood ? 'to much' : 'to little'}`);

// 3.
const ownersEatTooMuch = dogs.filter(dog => dog.curFood > dog.recommendedFood).flatMap(dog => dog.owners);
console.log(ownersEatTooMuch);
const ownersEatTooLittle = dogs.filter(dog => dog.curFood < dog.recommendedFood).flatMap(dog => dog.owners);
console.log(ownersEatTooLittle);

// 4.
// "Matilda and
// Alice and Bob's dogs eat too much!;

console.log(`${ownersEatTooMuch.join(' and ')}'s dogs eat too much!`);
console.log(`${ownersEatTooLittle.join(' and ')}'s dogs eat too small!`);

// 5.
console.log(dogs.some(dog => dog.curFood === dog.recommendedFood));

// 6.
console.log(dogs.some(dog => dog.curFood > (dog.recommendedFood _0.90) && dog.curFood < (dog.recommendedFood_ 1.10)));

// 7.
const okayAmount = dogs.filter(dog => dog.curFood > (dog.recommendedFood _0.90) && dog.curFood < (dog.recommendedFood_ 1.10));
console.log(okayAmount);

// 8.
const dogsCopy = dogs.slice().sort((a, b) => {
return a.recommendedFood - b.recommendedFood;

});
console.log(dogsCopy);

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////
/// --- khuda khuda kr k khatm hve --- ///
/// --- Inshallah Before Ramzan it should complete --- //

````

```

```
````
