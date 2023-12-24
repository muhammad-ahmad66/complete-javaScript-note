# OBJECT ORIENTED PROGRAMMING

## Table of Contents

1. [OOP_Introduction](#oop_introduction)
2. [OOP_IN_JAVASCRIPT](#oop_in_javascript)
3. [CONSTRUCTOR_FUNCTION](#constructor_function)
4. [PROTOTYPES](#prototypes)
5. [Prototype_Inheritance_And_Prototype_Chain](#prototype_inheritance_and_prototype_chain)
6. [ES6_CLASSES](#es6_classes)
7. [Getters_And_Setters](#getters_and_setters)

## OOP_Introduction

OOP is a **programming paradigm**(style of code) that is based on concept of **objects**.  
**Object** may contain data and methods. By using object we pack data and corresponding behavior into one block.  
**Objects** are use to building blocks of applications and interact with one another.  
This interactions happen through a **public interface(API)**: Methods that the code outside of the object can be access and use to communicate with the object are called Public Interface OR API.

**Why OOP?**  
**OOP** was developed with the goal of organizing code, to make it more flexible and easier to maintain.

**OOP** allow us to create new objects from our code. To do that we use classes.

**FOUR Fundamental OOP Principles**:

1. **Abstraction**
2. **Encapsulation**
3. **Inheritance**
4. **Polymorphism**

**ABSTRACTION:**  
**Abstraction** means to **ignore or to hide details that don't matter,** that allow us to get overview perspective of things we are implementing, instead of messing with details that don't really matter to our implementation.

**ENCAPSULATION:**  
**Encapsulation** means keeps some property and method private inside the class, so they are not accessible from outside the class. however some methods can be exposed as a public interface(API).  
In c++ we used private keyword.

**INHERITANCE:**  
**Inheritance** makes all properties and methods of certain class available to child class.  
When we have to class which have closely related data, we can one class inherit from the other. -ie we can have one parent class and one child classes, then the child class extends the parent class. A child class inherits all the properties and methods from its parent class.  
Like an admin class and user, both have some common functionality, but admin should have some additional one. so user is a parent class and admin is a child.

**POLYMORPHISM:**  
**Polymorphism** means that a child class can overwrite a method that's inherited from a parent class.

_**NOTE:** THESE ARE BASED ON OOP IN GENERAL, NOT ON JAVASCRIPT_

---

## OOP_IN_JAVASCRIPT

In **JavaScript** we've a something called **Prototypes**. **All objects in JavaScript are linked to a certain Prototype Object.** so we say each object has a prototype

The prototype object contains methods and properties that all the object are linked to that prototype, can access and use. this behavior is usually called **Prototypal Inheritance.**  
Objects delegate behavior to the linked prototype object.

**How we implement OOP in JavaScript?**  
There are three different ways fo implementing prototypal inheritance in JavaScript

1. Constructor Function [imp]
2. ES6 Classes [v. imp]
3. Object.create(). [not. imp]

**CONSTRUCTOR FUNCTION:**  
Technique to create objects from a function  
This is how built-in-objects like array, maps ,sets are actually implemented.

**ES6 CLASSES:**  
ES6 release introduced classes into JavaScript.  
Only syntax of class, but actual functionality of classes are not available in JavaScript. It still based on prototypal inheritance.  
ES6 classes are more modern way to constructor function syntax.

**OBJECT.CREATE() FUNCTION**  
Easiest and most straightforward way of linking an object to a prototype object.  
However, it's not as used as the other two methods.

**NOTE:** An object created form a class is called an instance.

---

## CONSTRUCTOR_FUNCTION

ONLY difference between regular and constructor function is that we call a constructor function with the **new** operator.

**In OOP usually the constructor function starts with a capital letter, as a convention.** We'll follow this. In fact other built in constructors are also follow this. like Array, Map, etc ⤵.

```js
const arr = new Array(3, 4, 5, 2, 1, 9);
console.log(arr);
```

**Function Expression** and **Declaration** are work as constructor function but not an arrow function because in arrow function **this** keyword doesn't work.

```js
const Person = function (firstName, birthYear) {
  // Instance Property
  this.firstName = firstName;
  this.birthYear = birthYear;
  console.log(this); // this will change a/c to which one is calling.

  // v.bad practice:
  this.calcAge = function () {
    return 2023 - this.birthYear;
  };
};
```

**V.bad practice:** we should never create a method inside a constructor function. basically we want to add methods to a prototype, not on every instance.

This function ⤴ is just like a regular function, But the only different is that when calling we use **new keyword**.

```js
const me = new Person('Muhammad', 2000);
console.log(me);
```

This **new operator** will call this function here but it does lot more than that. Behind the scenes, **there happen four steps:**

1. **New empty object is created**
2. **Function is called**, in this function call the **this keyword will be set to this newly created object.**
3. Newly created object is **linked to a prototype.**
4. Newly created object is automatically **returned from the constructor function.**

```js
const you = new Person('Jonas', 1991);
console.log(you);
```

**Remember:** JavaScript doesn't really have classes in the sense of traditional OOP. HOWEVER we did create an object from a constructor function. here 'me' and 'you' are object, create from Person constructor function.  
We discussed that: An object created form a class is also called an **instance**, so 'me' and 'you' also called instance of Person.

```js
const name = 'Muhammad';

// we can check for either instance or not. like this
console.log(me instanceof Person); // true.
console.log(you instanceof Person); // true.
console.log(name instanceof Person); // false
```

All the **properties** and **methods** in constructor function should be the property of all their instances.

```js
console.log(me.birthYear);
console.log(me.firstName);
console.log(me.calcAge());
```

---

## PROTOTYPES

```js
const Person = function (firstName, birthYear) {
  this.firstName = firstName;
  this.birthYear = birthYear;
};

const me = new Person('Muhammad', 2000);
const you = new Person('Jonas', 1991);
```

**Each function in javascript has automatically has a property called prototype**, that includes constructor function.  
Every object that's created by a certain constructor function will get access to all the methods and properties that we define on the constructor prototype property.

In this example

```js
console.log(Person.prototype);
Person.prototype.calcAge = function () {
  return 2023 - this.birthYear;
};
```

Remember Each object created by this constructor function(Person) will now get access to all the methods of this prototype property. So we can do:

```js
const myAge = me.calcAge();
console.log(myAge); // 23

console.log(you.calcAge()); // 32
```

Here we get correct age, although we have not write calcAge function in constructor function, but we added in prototype object.

Upr jo bad practice likea tha uska good alternative ha yea. phly methods to prototype k 7th link krin gy phr zarorat k hisab sei functions ko call krin gy.

### How and Way it works?

It works because any **object** always has access to the **methods** and **properties** from its **prototype**, And the **prototype** of 'me' and 'you' is **person.prototype**.  
we can actually confirm that because each object has a special property, called underscore underscore proto underscore underscore[**proto**]

```js
console.log(me.__proto__);
```

Prototype of the me object is the prototype property of the constructor function.

```js
console.log(Person.prototype);
console.log(me.__proto__ === Person.prototype); //true
console.log(Person.prototype.isPrototypeOf(me)); //true
console.log(Person.prototype.isPrototypeOf(you)); //true
```

**_BUT_**

```js
console.log(Person.prototype.isPrototypeOf(Person)); //false
```

**Why all of these??**  
**Person.prototype property means not a prototype of Person**, but it means prototype of all objects of that constructor function(Person)

We can also set a properties on the prototype, not just methods.

```js
Person.prototype.gender = 'male';
console.log(me.gender, '|', you.gender);
```

However this property is not directly in object.

```js
console.log(me); // only firstName & birthYear.
```

Own properties are only those that are declared directly on the object itself, not including inherited properties from prototype. lets check...

```js
console.log(me.hasOwnProperty('firstName')); // true
console.log(you.hasOwnProperty('birthYear')); // true
console.log(me.hasOwnProperty('gender')); // false
```

---

## Prototype_Inheritance_And_Prototype_Chain

```js
console.log(Person.prototype);
```

Person.prototype is also an abject and all object in js has a prototype. so Person.prototype must have a prototype. the prototype of person.prototype is object.prototype  
This link is called Prototype Chain.

```js
console.log(Person.prototype === you.__proto__);
```

**Prototype chain** is pretty similar to the scope chain, in **scope chain** whenever js find a certain variable in a scope chain it looks up into the next scope and a scope chain and tries to find the variable there. On the other had the **prototype chain** whenever javascript can find a certain property or method in a certain object it's gonna look up into the next prototype in the prototype chain, until it find, because top of the scope chain is **Object.prototype** and we go up to Object.prototype, then it will display null. (remember this **null -will display if not found any method or property of any object.**)

### Prototypal Inheritance on Build in Objects

check prototypal Inheritance on built in objects such as Arrays.

```js
console.log(me.__proto__);
console.log(Person.prototype); // both are exact same
```

Lets check prototype of me's prototype

```js
console.log(me.__proto__.__proto__); // it's prototype property of Object.
console.log(Object.prototype); // Both are exact same

console.log(me.__proto__.__proto__ === Object.prototype); // true.
```

**NOTE:** Usually Object.prototype is the top of the scope chain.

```js
console.log(me.__proto__.__proto__.__proto__); // null. bc there is no prototype top of the Object.prototype.
```

**REMEMBER**

```JS
console.log(Person.prototype.constructor); //pointing back to the person constructor function.
console.log(Person === Person.prototype.constructor); // true. b/c person.prototype is prototype of the object/instance of that constructor function. so here me.constructor
console.log(Person === me.constructor); // true.
```

### PROTOTYPE OF ARRAYS

**Arrays are also an object**, so it has also a prototype.

```js
const arr = [3, 4, 3, 6, 3, 2, 5, 2, 6, 7, 9];
console.log(arr.__proto__); // here we have all the methods of an array that we already know.
```

**_REMEMBER_**  
**_This is a reason all the arrays get access to all of these methods. Each array not contains all of these methods but instead each array will inherit these methods from it's prototype._**

```js
console.log(arr.**proto** == Array.prototype); // true
```

**Remember** prototype of the any object is always equal to the prototype property of constructor function. here **Array is a constructor function.**

```js
console.log(arr.constructor); // Array
new Array() === [];
```

now check

```js
console.log(arr.__proto__.__proto__); // Now we are back to the Object.prototype.

console.log(Object.prototype === arr.__proto__.__proto__); // ture
```

At this point we know that any Array inherits all their methods from its prototype, We can use this knowledge ko make own methods for array.

```js
Array.prototype.unique = function () {
  console.log(this);
  return [...new Set(this)];
};

console.log(arr.unique()); // working...
```

**This is not a good Idea to do.**  
**Reasons why it's bat?**

1. Next version of javascript might add a method with a same name and might work in different way.
2. When we work on a team than it might create some problem.

### PROTOTYPE OF DOM ELEMENTS

We know that all the **DOM elements are behind the scenes objects**.

```js
const h1 = document.querySelector('h1');
console.dir(h1); // see in cl very huge prototype chain.
```

### PROTOTYPE OF FUNCTION

Let's now check a prototype of function  
**Any function is also an object**. so, it has also prototype.

```js
console.dir((x) => x + 1);
```

---

## ES6_CLASSES

Doing exact same thing but using nicer and more modern syntax.

Although we use class keyword but Behind the scenes, classes are still special kind of function, there for we have class expressions and class declaration.  
_"I prefer class declaration"_

Class Expression:

```js
const Person = class {};
```

Class Declaration:

```js
class PersonCl {
  // constructor method:
  // name should constructor
  constructor(firstName, birthYear) {
    this.fName = firstName;
    this.bYear = birthYear;
  }

  // Methods
  // Methods will be added to .prototype property.
  calcAge() {
    console.log(2022 - this.bYear);
  }

  greet() {
    console.log(`Hey ${this.fName}!`);
  }
}
```

**REMEMBER:** All of these methods that we write inside the class(outside of the constructor) will be on the prototype of the objects, not on the object themselves. Like we added in constructor function.

And in constructor function, here we also call class using **new keyword**. That **new operator** will **call constructor** automatically. Also **this keyword** also be set to the newly created empty object.

```js
const me = new PersonCl('Muhammad', 2000);
console.log(me);
me.calcAge();
console.log(me.__proto__ === PersonCl.prototype); // true.
```

We can also add method manually to the prototype like we did in constructor function.

```js
PersonCl.prototype.greet = function () {
  console.log(`Welcome ${this.fName}!!!`);
};
me.greet();
```

Function declaration are hoisted, we can use them before declare in the code. (calling upr ho function niche declare hva ho)

**Remember** Couple of thing regarding Classes:

1. Classes are **not hoisted**.
2. Classes are **first-class citizens**. it means we can pass them into a functions and also return them from functions. b/c class is kind of function. behind he scenes.
3. The body of a class is always executed in **strict mode**.

**Constructor Function** OR **Classes**?  
Constructor Function are not old syntax, so 100% fine to keep using them.  
Personally prefer classes.

---

## Getters_And_Setters

// Every object in javascript can have getter and setter properties. We call these special properties 'assessor properties', while normal properties are called data properties.

// getters and setters are basically a functions that get and set a value, but they look like property.

// setter has always exactly one parameter.

// lets take an example fo object leteral
const account = {
owner: 'Muhammad',
movements: [200, 400, 700, 120, 340],

    get latest() {
        return this.movements.slice(-1).pop();
    }, // to get last element from array.

    set latest(mov) {
        this.movements.push(mov);
    }

};

console.log(account.latest);
account.latest = 390; // it is like a property, so we have to assign value like this , (NOT accout.latest(390))
console.log(account.movements);

// This is how getter and setter works in any regular object in javascript.

// NOW however, classes do also have getter and setters, and they also do works in exact same way.

class PersonCl {

    // constructor
    constructor(fullName, birthYear) {
        this.fullName = fullName;
        this.bYear = birthYear;
    }

    // methods
    calcAge() {
        console.log(2022 - this.bYear);
    }

    greet() {
        console.log(`Hey ${this.fullName}!`);
    }

    // getters
    get age() {
        return 2022 - this.bYear;
    }

    get name() {
        return this.fullName
    }

    // Getter and Setter are very usefull for data validation.
    // full name expect multiple words sapetated by space. so we can use setter to check if it's full name or not.

    set fullName(name) {
        // remember hare, we are creating a setter for a property name that does alreay exist. fullName is already a property then we also have a stter with same name, now  each time in constructor full name is executed, this setter method is also executed. that name we are passing as a fullName will become a name in setter method. lets check
        console.log(name);
        if (name.includes(' ')) this._fullName = name;// this underscore is just a convention. but necessasry to do.
        else alert(`${name} is not a full name!`)
    }

    // now fullName is not exist, bc of _fullName. so we've to use getter to get _fullName as a fullName:
    get fullName() {
        return this._fullName
    }

}
// Getter and Setter are very usefull for data validation.

const me = new PersonCl('Muhammad Ahmad', 2000);
console.log(me);
me.calcAge();
console.log(me.age); // Remember here we can read age as a peoperty, but not a property. it's method
console.log(me.fullName);

const you = new PersonCl('Jonas', '1996');
console.log(you.fullName); // undefined// bc due to condition in setter it's not assigned. (not conatians space.)

\*/

////////////////////////////////////////////////
////////////////////////////////////////////////
// lecture #12
// Heading

/\*

// -- Static Methods -- //

// remember that Array.form is used any array formed to regular array. like nodeList -> array
const h1 = document.querySelectorAll('h1');
console.log(h1); // nodeList
const h1Arr = Array.from(h1);//converted to array
console.log(h1Arr); // now it's an array

// this from method is a method that's attached to the array constructor, not with prototype. we could not use the from method on an array.like this
const array = [4, 3, 5, 2];
// array.from(); // error
// We also say that the from method is in the Array namespace.
// this from method is static method.

// We can add static methods to any class or consturctor function. lets do that

// On Constructor Function:
const PersonC = function (firstName, birthYear) {
this.firstName = firstName;
this.birthYear = birthYear;
};
PersonC.prototype.calcAge = function () {
return 2022 - this.birthYear;
}
const you = new PersonC('Jonas', 1991);
console.log(you.calcAge());

// now implement static method
PersonC.hey = function () {
console.log(`Hey there ✌`);
console.log(this);
};

// calling static method
PersonC.hey();

// we can't call with any object
// you.hey() //error

// On Class:
class Person {
constructor(fullName, birthYear) {
this.fullName = fullName;
this.birthYear = birthYear;
};

    get age() {
        return 2022 - this.birthYear;
    };

    // creating static method
    // should add static keyword
    static hey() {
        console.log('Hey there!');
        console.log(this);
    }

};
const me = new Person('Muhammad Ahmad', 2000);
console.log(me.age);

// calling static method
Person.hey();

\*/

////////////////////////////////////////////////
////////////////////////////////////////////////
// Lecture # 13
// Heading

// - Object.create:
// Third way of implementing prototypal inheritance or delegation. Which works a pretty different way then constructor and classes
// here still the idea of prototypal inheritance, but there are no prototype properties involved, and also no constructo functions and also no 'new' operator. Insted we use Object.create to manually set the prototype of an object to any other object that we want.
// If we can set the prototype to any object, lets create an object that we want to be the prototype of all the person objects.
/\*
// This object(PersonProto) is gonna be literally the prototype of all the person objects.
const PersonProto = { // just as regular object
calcAge() {
console.log(2037 - this.birthYear);
},

};

// Now creating Person object of upper's object.
const jonas = Object.create(PersonProto);
// this will return a new object, that's linked to the prototype that we passed in paramater. jonas object is liked to PersonProto object, which will be its prototype.

// now seting property to object
jonas.name = 'Jonas';
jonas.birthYear = 1991;

// verifing.
jonas.calcAge();
console.log(jonas.**proto**); // exactly PersonProto.
console.log(jonas.**proto** === PersonProto); // ture
_/
/_
// lets create onother object on PersonProto prototype. but in more efficiet way.

const PersonProto = {
calcAge() {
console.log(2022 - this.birthYear);
},

    // which'll set prperties programatically, can've any name. [ similar to constructor in class, but not]
    init(firstName, birthYear) {
        this.firstName = firstName;
        this.birthYear = birthYear;
    }

}

const me = Object.create(PersonProto);
me.init('Muhammad', 2000);
me.calcAge(); // 22 // here this is pointing to me object, that's not bc of new keyword like in constructor function, but we calling init method, which is method/function of me object, so as in regular object a this in function should point to the object that calling that method.

\*/

///////////////////////////////////////////////
///////////////////////////////////////////////
// Lecture 15
// Heading

// Inheritance Between Classes.
// Inheritance Between Constructor Functions.

// We'll build two classes 1.Person class as a parent class and 2.Student class as a child class.
// Here the Student class have specific methods that we define in it and more generic methods inhirit from Person Class.
//////////////////////////////////////////////

// Inhiretance Between Classes Using Constructor Function:
/\*
// Person Class
const Person = function (firstName, birthYear) {
this.firstName = firstName;
this.birthYear = birthYear;
};

Person.prototype.clacAge = function () {
console.log(2023 - this.birthYear);
};

// Student Class
const Student = function (firstName, birthYear, course) {
// this.firstName = firstName;
// this.birthYear = birthYear;
// these two statements are common in both Person and Student class. So, Instead
// Person(firstName, birthYear); // this not working because we are calling this constructor function as a regular function. we are not using new operator. In regular function this keyword set to undefined.so, here we use call method, that will call this method and also set this keyword manually.
Person.call(this, firstName, birthYear);

    this.course = course;

};

// Now we have to join these prototypes of these two classes, so we can share properties and methods from parent to child. To link these two prototypes we use Object.create. because defining prototypes menually is what Object.create does.
// Linking prototypes:
Student.prototype = Object.create(Person.prototype);

Student.prototype.introduce = function () {
console.log(`My name is ${this.firstName} and I study ${this.course}`);
}

const adeel = new Student('Adeel', 2012, 'Computer Science');
adeel.introduce();
adeel.clacAge(); // working

// here one thing to fix.
console.dir(Student.prototype.constructor); // it shoud point back to the Student. But here it's pointing to Person.
Student.prototype.constructor = Student;
console.dir(Student.prototype.constructor); // now correct.
\*/

//////////////////////////////////////////////
// lecture #17s

// Inhiretance Between Classes Using ES6 Classes:

/\*

class PersonCl {
constructor(fullName, birthYear) {
this.fullName = fullName;
this.birthYear = birthYear;
}

    calcAge() {
        console.log(2023 - this.birthYear);
    }

    greet() {
        console.log(`Hi, ${this.fullName}`);
    }

    get age() {
        return 2023 - this.birthYear;
    }

    set fullName(name) {
        if (name.includes(' ')) this._fullName = name;
        else alert(`${name} is not a full name!`);
    }

    get fullName() {
        return this._fullName;
    }

    static hey() {
        console.log('Hey there!!!');
    }

};

// to link any class with any other class as a parent-child class we need two things...
// In other words, to implement inheritance between ES6 classes we need two more things
// 1- extends keyword.
// 2- super function.

class studentCl extends PersonCl {
constructor(fullName, birthYear, course) {
// PersonCl.call(this, fullName, birthYear) // here we don't this, like we did before, Instead we use super function,
// Super is basically the constructor of parent class.
// Always write first in constructor.
super(fullName, birthYear); // always call super first

        this.course = course;

        // Remember: If we don't need any additional properties in child class, then do not need to write constructor method in child class. in this case if we had not course property. - also no need to call super function
    }

    introduce() {
        console.log(`My name is ${this.fullName}, my age is ${this.age}, and I am study ${this.course}`);
    };

    calcAge() {
        console.log(`I'm ${2023 - this.birthYear} years old, but as a student I feel more like ${2023 - this.birthYear + 10}`);
    }

};

const haris = new studentCl('Haris Shehbaz', 2010, 'Computer Science');

console.log(haris);
haris.introduce();
haris.calcAge();

\*/

//////////////////////////////////////////////
// lecture #18

/\*

// Inhiretance Between Classes Using Object.create:

const PersonProto = {
clacAge() {
console.log(2023 - this.birthYear);
},

    init(firstName, birthYear) {
        this.firstName = firstName;
        this.birthYear = birthYear;
    },

};

const adeel = Object.create(PersonProto);

// here PersonProto is as a prototype of all the new person objects. Now basically we want to add another prototype in middle of the chain, between PersonProto and the object.
// we will make student inherit directly from person

const StudentProto = Object.create(PersonProto)

StudentProto.init = function (firstName, birthYear, course) {
PersonProto.init.call(this, firstName, birthYear);
this.course = course;
}

StudentProto.introduce = function () {
console.log(`My name is ${this.firstName} and I study ${this.course}`);
}
const haris = Object.create(StudentProto)
// here prototype of StudentProto is PersonProto and Prototype fo haris is StudentProto, so haris is liked with PersonProto indireclty, it'll inderit all properties and methods from PersonProto.

haris.init('Haris', 2012, 'Computer Science');
console.log(haris);
haris.introduce();
haris.clacAge();

\*/

////////////////////////////////////////////////
////////////////////////////////////////////////
/\*

// Lecture #19
// Heading

// Another Class Example:
// Few more things to remember about classes

class Account {
constructor(owner, currency, pin) {
this.owner = owner;
this.currency = currency;
this.pin = pin;

        this.movements = [];
        this.locale = navigator.language;

        console.log(`Thanks for opening an account, ${owner}`);
    }

    // Public Interface of Object. (also called API)
    deposit(value) {
        this.movements.push(value);
    }

    withdrawl(value) {
        this.deposit(-value);
    }

    approveLoan(value) {
        // logic goes here to approve loan. not implemented in this case
        return true;
    }

    requestLoan(value) {
        if (this.approveLoan(value)) {
            this.deposit(value);
            console.log('Loan approved');
        }
    }

};

const acc1 = new Account('Muhammad', 'EUR', 1111);
console.log(acc1);

// pushing value to movements array
// acc1.movements.push(120);
// acc1.movements.push(-230);
// working, but not ideal.

// Instead of dealing with properties directly, we create a methods, which interact with properties.
acc1.deposit(240);
acc1.withdrawl(180);
acc1.requestLoan(1000);

acc1.approveLoan(2000); // shouln't accessable.
// we can also access this method in real world, this type of methdos should not be access from outside of the class
// also all of our values of properties are accessable from outside of the class,
console.log(acc1.pin); // 111

// but it should't be accessable from outside of the class. ritht?

// So, We really need data encalpsulation and data privacy. We will disscuss in upcommming lectures...

\*/

////////////////////////////////////////////////
////////////////////////////////////////////////
// Lecture 20

// Heading

// --- ENCAPSULATION --- //
// - Protection of Properties and Methods - //
// Keep some properties and methods private inside the class.

// Two big reasons for data encapsulation:
// 1. To pervent code from outside of a class to accidentally manipulate data inside the class.
// 2. when we expose only a small interface (small API, consisting only few buplic methods) then we can change all the other internal methods with more confidence.

// Javascript class do not yet support real privicy and encapsulation. but their is aproposal to add truly ptivate class fields and methods to the language, but it not completely ready yet. will do that in next lecture. But now we will do fake encapsulation by using a convention. we will protect movements property.

/\*
class Account {
constructor(owner, currency, pin) {
this.owner = owner;
this.currency = currency;

        // protected property
        this._pin = pin;
        this._movements = [];


        this.locale = navigator.language;
        console.log(`Thanks for opening an account, ${owner}`);
    }

    // Public Interface
    getMovements() {
        return this._movements;
    }


    deposit(value) {
        this._movements.push(value);
    }

    withdraw(value) {
        this.deposit(-value);
    }

    _approveLoan(value) {
        return true;
    }

    requestLoan(value) {
        if (this._approveLoan(value)) {
            this.deposit(value);
            console.log(`Loan approved.`);
        }
    }

}

const acc1 = new Account('Adeel', 'PKR', 1234);

acc1.deposit(200);
acc1.withdraw(800);
acc1.deposit(250);
acc1.requestLoan(500);

// acc1.\_movements.push(240);
// acc1.\_movements.push(-300);
// we still do it but it is just a convention. that any property start with underscore will consider as private. not manupalte from outside.

console.log(acc1.getMovements());

\*/

//////////////////////////////////////////////////
//////////////////////////////////////////////////

// lecture 21

// ENCAPSULATION
// private class's fields and methods.
// Here we'll talk about real encapsulation.

// Private class fields and methods are actually part of bigger propasal for improving and changing javascript classes, which is simply called 'class field' .
// this class fields proposal is currently at stage three. right now it's not yet part of javascript language. soon it will be part.

// whay it's called class field?
// in traditional OOP (java, C++) properties are usually called fields

// In this proposal there are four different kinds of fields and methods. (even eight.)
// 1. Public fields
// 2. Private fields
// 3. Public methods
// 4. Private methods
// for eight we have, static version of all these four.

/\*

class Account {

    // 1- Public fields
    locale = navigator.language;
    // Remember that all the field will added to each instance(objects.) not to prototype. WHILE all the methods will add to the prototype.


    // 2- Private fileds
    #movements = []; // remember this is syntax to add private fields.
    // we have to also private pin, but we can't define fields in the constructor, they really have to be outside of any method. so,
    #pin; // like a creating empty variable.


    constructor(owner, currency, pin) {
        this.owner = owner;
        this.currency = currency;


        this.#pin = pin;
        // this._movements = [];

        // this.locale = navigator.language;
        console.log(`Thanks for opening an account, ${owner}`);
    }

    // 3- Public Methods (all of methods)
    getMovements() {
        return this.#movements;
    }


    deposit(value) {
        this.#movements.push(value);
    }

    withdraw(value) {
        this.deposit(-value);
    }


    requestLoan(value) {
        if (this._approveLoan(value)) {
            this.deposit(value);
            console.log(`Loan approved.`);
        }
    }


    // 4- Private methods:
    // syntax is exact as private fields.
    // #approveLoan(value) {
    //     return true;
    // } // it is treated as private field, so not added to prototype. that we don't want. its means googl chorme not yet support private methods.

    // so will go back to protected (_ convertion)
    _approveLoan(value) {
        return true;
    }

}

const acc1 = new Account('Adeel', 'PKR', 1234);

acc1.deposit(200);
acc1.withdraw(800);
acc1.deposit(250);
acc1.requestLoan(500);
console.log(acc1);

// acc1.#movements.push(240);
// acc1.#movements.push(-300); // now error.
// we still do it but it is just a convention. that any property start with underscore will consider as private. not manupalte from outside.
// console.log(acc1.#pin) // also error, working.

// console.log(acc1.#approveLoan(200));
console.log(acc1.getMovements());

\*/

//////////////////////////////////////////////////
//////////////////////////////////////////////////
// Lecture 22
// Heading

// Chaining Methods
/\*

class Account {

    // 1- Public fields
    locale = navigator.language;

    // 2- Private fileds
    #movements = []; // remember this is syntax to add private fields.
    #pin; // like a creating empty variable.


    constructor(owner, currency, pin) {
        this.owner = owner;
        this.currency = currency;


        this.#pin = pin;
        // this._movements = [];

        // this.locale = navigator.language;
        console.log(`Thanks for opening an account, ${owner}`);
    }

    // 3- Public Methods (all of methods)
    getMovements() {
        return this.#movements;
    }


    deposit(value) {
        this.#movements.push(value);
        return this;
    }

    withdraw(value) {
        this.deposit(-value);
        return this;
    }


    requestLoan(value) {
        if (this._approveLoan(value)) {
            this.deposit(value);
            console.log(`Loan approved.`);
            return this;
        }
    }

    _approveLoan(value) {
        return true;
    }

}

const acc1 = new Account('Adeel', 'PKR', 1234);

acc1.deposit(250);
acc1.withdraw(900);
acc1.requestLoan(1200);
console.log(acc1.getMovements());
console.log(acc1);

// Chaining
// acc1.deposit(400).deposit(500).withdraw(100).requestLoan(2500).withdraw(1200); // right not it's error. because some functions are not returining any thing. see above in class.
// here deposit is not returing any thing. so we need to do that, we call deposit actually on account. (all of methods are returning 'this' - this is basically current calling object. )

acc1.deposit(400).deposit(500).withdraw(100).requestLoan(2500).withdraw(1200);
console.log(acc1); // now working

\*/

/////////////////////////////////////////////////
/////////////////////////////////////////////////
// lecture 23 [last one]
// Heading

// --- Quick Review --- //
// ES6 Classes Summary

//////////////////////////////////////////////////
//////////////////////////////////////////////////
//////////////////////////////////////////////////

// Heading Heading Heading

// ---- CODING CHALENGES --- //

/\*
////////////////////////////////////////////////////
// Challenge #01:

const Car = function (Name, speed) {
this.carName = Name;
this.speed = speed;
}

Car.prototype.accelerate = function () {
this.speed += 10;
console.log(`${this.carName} going at ${this.speed} km/h`);
}

Car.prototype.brake = function () {
this.speed -= 5;
console.log(`${this.carName} going at ${this.speed} km/h`);
}

const car1 = new Car('BMW', 120);
console.log(`${car1.carName} going at ${car1.speed} km/h`);
car1.accelerate();
car1.brake();

\*/

////////////////////////////////////////////////////
// Challenge #02:
/\*
class Car {
constructor(make, speed) {
this.make = make;
this.speed = speed;
}

    accelerate() {
        this.speed += 10;
        console.log(`${this.make} going at ${this.speed} km/h`);
    }

    brake() {
        this.speed -= 5;
        console.log(`${this.make} going at ${this.speed} km/h`);
    }

    // getter
    get speedUS() {
        return this.speed / 1.6;
    }

    // setter
    set speedUS(speed) {
        this.speed = speed * 1.6;
    }

}

const ford = new Car('Ford', 120);
ford.accelerate();
ford.accelerate();
ford.accelerate();
ford.brake();

// getter calling
console.log(ford.speedUS);

// setter calling
ford.speedUS = 75;
console.log(ford.speed); // 120 // changed.

\*/

////////////////////////////////////////////////////
// Challenge #03:
/\*

const Car = function (make, speed) {
this.make = make;
this.speed = speed;
};

Car.prototype.accelerate = function () {
this.speed += 10;
console.log(`${this.make} going at ${this.speed} km/h`);
};

Car.prototype.brake = function () {
this.speed -= 5;
console.log(`${this.make} going at ${this.speed} km/h`);
};

const EV = function (make, speed, charge) {
Car.call(this, make, speed);
this.charge = charge;
}

EV.prototype = Object.create(Car.prototype);

EV.prototype.chargeBattery = function (chargeTo) {
this.charge = chargeTo;
}

EV.prototype.accelerate = function () {
this.speed += 20;
this.charge--;
console.log(`${this.make} going at ${this.speed} km/h, with a charge of ${this.charge}%`);
}

const tesla = new EV('Tesla', 120, 23);

tesla.accelerate();
tesla.accelerate();
tesla.accelerate();
tesla.brake();
tesla.brake();
tesla.brake();

tesla.chargeBattery(90);
tesla.accelerate();
tesla.accelerate();

\*/

////////////////////////////////////////////////////
// Challenge #04:

///////////////////////////////////////////////////
///////////////////////////////////////////////////

// parent class
class CarCl {
constructor(make, speed) {
this.make = make;
this.speed = speed;

    }

    accelerate() {
        this.speed += 10;
        console.log(`${this.make} going at ${this.accelerate} km/h`);
    }


    brake() {
        this.speed -= 5;
        console.log(`${this.make} going at ${this.speed} km/h`);
        return this;
    }

    get speedUS() {
        return this.speed /= 1.6;
    }

    set speedUS(speed) {
        this.speed = speed * 1.6;
    }

}

// child class
class EVCl extends CarCl {

    #charge;
    constructor(make, speed, charge) {
        super(make, speed);
        this.#charge = charge;
    }

    chargeBattery(chargeTo) {
        this.#charge = chargeTo;
        return this;
    }

    accelerate() {
        this.speed += 20;
        this.#charge--;
        console.log(`Tesla going at ${this.speed} km/h, with a charge of ${this.#charge}%`);
        return this;
    }

}

const car1 = new EVCl('Rivian', 120, 23);
car1.accelerate();
car1.accelerate();
car1.accelerate();
car1.brake()
car1.chargeBattery(65);

console.log('---- CHAINING ----');

car1.accelerate().accelerate().brake().chargeBattery(92).accelerate().accelerate();

// console.log(#charge);
console.log(car1);
