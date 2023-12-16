'use strict';

///////////////////////////////////////
// Modal window

const modal = document.querySelector('.modal');
const overlay = document.querySelector('.overlay');
const btnCloseModal = document.querySelector('.btn--close-modal');
const btnsOpenModal = document.querySelectorAll('.btn--show-modal');

const openModal = function (e) {
e.preventDefault();
modal.classList.remove('hidden');
overlay.classList.remove('hidden');
};

const closeModal = function () {
modal.classList.add('hidden');
overlay.classList.add('hidden');
};

// for (let i = 0; i < btnsOpenModal.length; i++)
// btnsOpenModal[i].addEventListener('click', openModal);

// using forEach instead of for loop.
btnsOpenModal.forEach(btn => btn.addEventListener('click', openModal));

btnCloseModal.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);

document.addEventListener('keydown', function (e) {
if (e.key === 'Escape' && !modal.classList.contains('hidden')) {
closeModal();
}
});

////////////////////////////////////////////////////////
////////////////////////////////////////////////////////

#### lecture 07

// scrolling effect on learn more button

const btnScrollTo = document.querySelector('.btn--scroll-to');
const section1 = document.querySelector('#section--1');

btnScrollTo.addEventListener('click', function (e) {

section1.scrollIntoView({
behavior: 'smooth',
});

}); // all of details are below....

//////////////////////////////////////////
// Scrolling Effect on Navigation Menu links

// using Event Delegation
// in event delegation we need two steps:
// 1. we add the event listener to a common parent element.
// 2. Determine what element originated the event.

document.querySelector('.nav\_\_links').addEventListener('click', function (e) {
e.preventDefault();
// console.log(e.target); // where event is originated.

// Matching Strategy: (v. important)
// b/c when we click on parent it will also call event handler. to avoid that:
if (e.target.classList.contains('nav\_\_link')) {
// const id = this.querySelector('href'); //here this will not work. b/c this is pointing to the element where event is happened, that's container of links. so we use e.target.
const id = e.target.getAttribute('href');
document.querySelector(id).scrollIntoView({ behavior: 'smooth' })
}
});

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////

// --- Building Tabbed Component --- //

const tabs = document.querySelectorAll('.operations**tab');
const tabsContainer = document.querySelector('.operations**tab-container');
const tabsContents = document.querySelectorAll('.operations\_\_content');

/\*
// ---- First method to do that ----- [not efficient]
tabs.forEach(t => t.addEventListener('click', function (e) {
e.preventDefault();

// removing active class from tabs and content
tabs.forEach(t => t.classList.remove('operations**tab--active'));
tabsContents.forEach(c => c.classList.remove('operations**content--active'));

// adding active class to both tabs and content
e.target.closest('.operations**tab').classList.add('operations**tab--active');
const data = e.target.closest('.operations**tab').dataset.tab;
document.querySelector(`.operations**content--${data}`).classList.add('operations\_\_content--active');
// ees working!!

})) // We can do using this method BUT not efficient way.
\*/

// Second method (effiecient)
// So we use Delegation.

tabsContainer.addEventListener('click', function (e) {

// Matching Strategy first
const clicked = e.target.closest('.operations\_\_tab');

// Guard Clause
if (!clicked) return // if we clicked on parent contaner then there is no class on operations\_\_tap closest to that , so it will give null, and it cause error. so ... if clicked is falsy (null) then return immediatly. -nothing happen.
// and its called a Guard Clause we can also do
// if(clicked){ rest of code.}

// removing active class from all and then adding to clicked one
tabs.forEach(t => t.classList.remove('operations**tab--active'));
clicked.classList.add('operations**tab--active');

// Removing Active class from Content Area:
tabsContents.forEach(c => c.classList.remove('operations\_\_content--active'));

// Activing Content area of each coorsponding tab

// the information about which content should display is in the data attribute(see in html).
// console.log(clicked.dataset.tab);
document.querySelector(`.operations__content--${clicked.dataset.tab}`).classList.add('operations\_\_content--active')

})

////////////////////////////////////////////////////////
////////////////////////////////////////////////////////

#### lecture # 14:

// Hover Effect on Navigation Links ---[menu fade animation]
// -- how to pass argument to an event handler funtion -- //

const nav = document.querySelector('.nav');

/\* Refactored nyche
nav.addEventListener('mouseover', function (e) {
// mathching elemene
if (e.target.classList.contains('nav\_\_link')) {
const linkCliked = e.target;

    // selecting all sibling: we will go to the top container there all of these links and logo contains. then will select all child of that.
    const sibling = linkCliked.closest('.nav').querySelectorAll('.nav__link');
    const logo = linkCliked.closest('.nav').querySelector('img')

    // now we need to change opacity of all siblings
    sibling.forEach(el => {
      if (el !== linkCliked) {
        el.style.opacity = 0.5;
      }
    });
    logo.style.opacity = 0.5;

};
});
// mouseover & mouseenter are same but mouseenter not bubble up. while mouseout and mouseleave are opposite of these respectively.
// here we need mouseout effect. b/c when realese a mouse from link, it should return to the original style.
nav.addEventListener('mouseout', function (e) {
if (e.target.classList.contains('nav\_\_link')) {
const linkCliked = e.target;

    const sibling = linkCliked.closest('.nav').querySelectorAll('.nav__link');
    const logo = linkCliked.closest('.nav').querySelector('img');

    sibling.forEach(el => {
      if (el !== linkCliked) {
        el.style.opacity = 1;
      }
    });
    logo.style.opacity = 1;

}

});
\*/

// This code is working but both eventhandler's are almost same. So lets refactor this code.
// usually to refactor we analysis what's diff, in all functions, then all different will pass as an argument.
const handleHover = function (e, opacity) {
if (e.target.classList.contains('nav\_\_link')) {
const linkCliked = e.target;

    const sibling = linkCliked.closest('.nav').querySelectorAll('.nav__link');
    const logo = linkCliked.closest('.nav').querySelector('img');

    sibling.forEach(el => {
      if (el !== linkCliked) {
        el.style.opacity = this; // remember that we use bind method to pass and argument to a handler function. and also note that any thing we pass in bind method as an argument, this keyword will point there. here we pass a simple value.
        // console.log(this);
      }
    });
    logo.style.opacity = this;

}
};

// nav.addEventListener('mouseover', handleHover);
// nav.addEventListener('mouseout', handleHover);
// but here the problem is that we wants to pass a values into a function for opacity and event, but we can't call it here as a result we can't pass an argument. How we do that??
// The solution is, would be still a callback funtion here like in regular eventListener
/_
nav.addEventListener('mouseover', function (e) {
handleHover(e, 0.5)
});
nav.addEventListener('mouseout', function (e) {
handleHover(e, 1);
});
_/

// We can do ever better and can remove anonymous callbacks.
// That's By using Bind method.
// The bind method creates a copy of the function that it's called on, and it will set the this keyword in this function call to whatever value we pass into bind method.

nav.addEventListener('mouseover', handleHover.bind(0.5));
nav.addEventListener('mouseout', handleHover.bind(1));
// here we use the bind method to pass an argument into a hadler function.
// we always use this keyword to pass multipel arguments to hadler function by using bind method. for single value we just pass it, for multiple value we will pass an array or object.

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////

#### lecture 15

// --- Implementing Sicky Navigation --- //

// now we use a new event, which is 'scroll' event. and remember that scroll evnet is on window not document.

// this event will happend each time when we scroll
/\*
const initialCoords = section1.getBoundingClientRect();
// console.log(initialCoords);
// hre we get currnt top value of this section.

window.addEventListener('scroll', function (e) {
// console.log(window.scrollY); // remember it's, distance b/w top of page and top of viewport, initial should zero- at top page.
// question: when we add sticky class
// ans: when reached first section. outside of function krna pry ga . upr dykh
if (window.scrollY > initialCoords.top) {
nav.classList.add('sticky');
} else {
nav.classList.remove('sticky');
}

}); // not a good practice!!! 'using scroll event' Better is comming in next lec. which is 'intersection observer API'.

\*/
////////////////////////////////////////////////////////
////////////////////////////////////////////////////////

#### lecture 16

// Sticky Navigation - Intersection Observer API
// Intersection Observer API allows our code to basically observe changes to the way that a certain target element intersects another element, OR The way it intersect the viewport.

// How this API works?
// -first we need to create new intersection observer
// -We pass a callback funtion and object of options.
// -then we use this observer to observe a certain terget.
// -and pass a target element in observe() function.
// -in callback need root property: root's value is the element that the target is intersecting
// and second property we need is threshold: which is the percentage of intersection at which the observer callback will be called,
// and callback will get called each time that the observed element, or target element is intersecting the root element at the threshold that we defined in options.
// The callback funtion will receive two arguments, first entries, and observer object itself.
// entries are the array of threshold.
/\*
// the callback function
const obsCallback = function (entries, observer) {
entries.forEach(entrie => {
console.log(entrie);
})
}

// the optins object
const obsOptions = {
root: null, // null is simple viewport
// threshold: 0.1, // which is 10%
threshold: [0, 0.2] // 0% means our callback will trigger each time that the target element moves completely of the view. if we specify 0 then the callback will call when moving into the view ond when out of the view. ON THE OTHER HAND if we specify 1 then the callback will call when the target is completely visible in the view. section1 is not possible 100% in view.
}

// creating new intersection observer
const Observer = new IntersectionObserver(obsCallback, obsOptions);

// passing a taget to the observe meethod:
Observer.observe(section1);

\*/
// NOW WE ARE IMPLEMENTING STICKY NAV USING ALL OF THESE

// we want to show nav when header is completely out of the view.

const header = document.querySelector('.header');

// calculate nav height
const navHeight = nav.getBoundingClientRect().height;

const stickyNav = function (entries) { //no need of observer
// here we have only one threshod so no need of loop..
const [entry] = entries; // same as entries[0]// destructuring..
// console.log(entry);
// We will add a sticky class when header is not intersecting viewport.
if (!entry.isIntersecting) {
nav.classList.add('sticky');
} else {
nav.classList.remove('sticky');
}
}

const headerObserver = new IntersectionObserver(stickyNav, {
root: null, // interested in viewport
threshold: 0, // when heder is completely out of viewport.
rootMargin: `-${navHeight}px`, // for 90 this rootMargin is a box of 90px that will b applied outside of our target element.
});
headerObserver.observe(header);

//////////////////////////////////////////////////////
//////////////////////////////////////////////////////

#### lecture 017

// Reveal Element As scroll Close to them :
// effect of fad-in of a sections from then bottom

// The efeect is coming from the CSS, but will add that class by using intersection observer. i-e when sections are comes into viewport we will add that class.

// What already done in CSS?
// made a class nammed 'section--hidden', which'll hide the section and pushed down section 9rem. We rection is close to

const allSections = document.querySelectorAll('.section');

const revealSection = function (entries, observer) {
const [entry] = entries;
// console.log(entry);

// guard clause
if (!entry.isIntersecting) return;

entry.target.classList.remove('section--hidden')

// unoberving
observer.unobserve(entry.target)
}

const sectionObserver = new IntersectionObserver(revealSection, {
root: null, // viewport
threshold: 0.15, // 15%
})

// allSections.forEach(function (section) {
// sectionObserver.observe(section);
// section.classList.add('section--hidden')
// })

//////////////////////////////////////////////////////////
////////////////////////////////////////////////////////####

#### lecture 18

// --- LAZY LOADING IMAGE --- [great for performance]

const imgTargets = document.querySelectorAll('img[data-src]'); // selecting all the images, which has data-src property.

const loadImg = function (entries, observer) {
const [entry] = entries;
// console.log(entry);

// guard clause
if (!entry.isIntersecting) return;

// yei yadd rkhna ki js attribure k shuru mei data- ho to usko hum dataset attribure k zeryea access krty hain.
// replace src attribute to data-src attribure.
entry.target.src = entry.target.dataset.src;

// entry.target.classList.remove('lazy-img'); // it will create problem so, we use:

// we can add load event to remove class after image is loaded. (simply removing (w/o using event) will remove class before loaded).
entry.target.addEventListener('load', function () {
entry.target.classList.remove('lazy-img');
});

observer.unobserve(entry.target)
}

const imgObserver = new IntersectionObserver(loadImg, {
root: null,
threshold: 0,
rootMargin: '200px'
})

imgTargets.forEach(img => imgObserver.observe(img));

///////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////

#### lecture 29-20

// --- BUILDING A SLIDER COMPONENT --- //

// Now all three slides are in same position we use javascript style attribue to translate all ather than active, that will make other slides out of viewport.

// PUTTING ALL OF THESE IN A FUNCTION
const slider = function () {

// selecting elements:
// const slider = document.querySelector('.slider');
const slides = document.querySelectorAll('.slide');
const btnRight = document.querySelector('.slider**btn--right')
const btnLeft = document.querySelector('.slider**btn--left');
const dotContainer = document.querySelector('.dots');
let currSlide = 0;
const maxSlide = slides.length;

// FUNCTIONS
// Creating dots
const createDots = function () {
// we'll build one dot for each slide
slides.forEach(function (\_, i) {
dotContainer.insertAdjacentHTML('beforeend', `<button class="dots__dot" data-slide = "${i}"></button>`)
})
};

const activeteDots = function (slide) {

    // removing active class form all dots
    document.querySelectorAll('.dots__dot').forEach(dot => dot.classList.remove('dots__dot--active'));

    // adding to only active dot:
    document.querySelector(`.dots__dot[data-slide="${slide}"]`).classList.add('dots__dot--active')

};

const gotoSlide = function (slide) {
slides.forEach((s, i) => {
s.style.transform = `translateX(${100 * (i - slide)}%)`;
})
};

const nextSlide = function () {
if (currSlide === maxSlide - 1) {
currSlide = 0;
} else {
currSlide++;
}
gotoSlide(currSlide);
activeteDots(currSlide);
}

const prevSlide = function () {
if (currSlide === 0) {
currSlide = maxSlide - 1;
} else {
currSlide--;
};
gotoSlide(currSlide);
activeteDots(currSlide);
}

// INITIALIZAION
// Functions Calling
const init = function () {
createDots();
activeteDots(0);
gotoSlide(0);
};
init();

// EVENTS
btnRight.addEventListener('click', nextSlide);
btnLeft.addEventListener('click', prevSlide);

// ARROW KEY TO LEFT & RIGHT
// for keyboard event always have to use document.
document.addEventListener('keydown', function (e) {
// console.log(e.key);

    // if (e.key === 'ArrowLeft') prevSlide();
    // if (e.key === 'ArrowRight') nextSlide();

    // also can use short-circuting
    e.key === 'ArrowLeft' && prevSlide();
    e.key === 'ArrowRight' && nextSlide();

});

// will use event delegation to attach event
dotContainer.addEventListener('click', function (e) {
if (!e.target.classList.contains('dots\_\_dot')) return;

    // const slide = e.target.dataset.slide;
    // we can also write like this, using destructuring
    const { slide } = e.target.dataset;
    gotoSlide(slide);
    activeteDots(slide);

});

};
slider();

---

## Table of Contents

1. [IntroductionToDOM](#IntroductionToDOM)

   1. [INHERITANCE](#INHERITANCE)
   2. [DocumentNodeType](#DocumentNodeType)

2. [SelectingCreatingUpdatingElements](#SelectingCreatingUpdatingElements)
   1. [SELECTING_ELEMENTS](#SELECTING_ELEMENTS)
   2. [CREATING-ADDING_ELEMENTS](#CREATING-ADDING_ELEMENTS)
   3. [DELETING_ELEMENTS](#DELETING_ELEMENTS)
3. [STYLES_ATTRIBUTES_CLASSES](#STYLES_ATTRIBUTES_CLASSES)
   1. [STYLES](#STYLES)
   2. [ATTRIBUTES](#ATTRIBUTES)
   3. [CLASSES](#CLASSES)
4. [SMOOTH_SCROLLING](#SMOOTH_SCROLLING)

---

## IntroductionToDOM

#### How DOM Works Behind the scenes

DOM is an interface between all javascript code, and the browser, or more specifically HTML documents that are rendered in by the browser.

- Use to javascript interact with the browser.
- We can create, delete, modify elements, and set styles, classes and attributes, and listen and respond to events.
  **This works b/c DOM tree is generated from any html document, tree will contains different nodes.**

##### How does that interaction actually works???

- The DOM is a very complex API(Application Programming interface), So it's the interface use to interact with the DOM.
- DOM contains tons of methods and properties we use to interact with the DOM tree. such as querySelector, addEventListener, or createElement methods, and also the innerHtml, textContent, or children properties. and many more.
- In the DOM there are different types of nodes. for example some nodes are HTML elements and others are just texts.
- Every single node in the DOM tree is of the type, **node.**
- Each node is represented in javascript by an object.
- This object get access to node methods and properties, such as textContent, child nodes, parent node, cloneNode etc.

**Each node type has a couple of child types so to say. And these are the element type, the text type, comment type and document type.**
**Each element represented internally as an object.**

### INHERITANCE

Inheritance of Methods and properties.
Inheritance means that all the child types will also get access to the methods and properties of all their parent node types. for example an HTML element will get access to everything from the element type, like innerHTML, or classList or all other methods or properties. beside that it will also get access from the node type because that is also its parent type.

### DocumentNodeType

Document is just another type of node. So, it contains important methods, such as querySelector, createElement and getElementById etc.
**Remember:** querySelector is available on both document and element types.

DOM API allow all the node types to listen to events, and remember we usually listen for events by calling the addEventListener method on an element or on the document.
Their is a special node type called eventTarget which is parent of the node type and and also the window node type, with this we can call addEventListener to every single node type in the DOM API.

_For more information check out MDN Documentation._

---

## SelectingCreatingUpdatingElements

##### How to SELECT, CREATE and DELETE elements with javascript:

### SELECTING_ELEMENTS:

**Selecting all document element:**

- console.log(document.documentElement); // entire elements
- console.log(document.doctype);
- console.log(document.head); // select head
- console.log(document.body); // select body
  _To select these things we didn't need any selector._

**querySelector and querySelectorAll**

```js
const header = document.querySelector('.header');
// this will return first element that contain header class.
// const allSections = document.querySelectorAll('.section');
// console.log(allSections); //it'll return a NodeList. -similar to array
```

These two (querySelector & querySelectorAll) are very common to selecting elements. these are not only available on document like we did(here ⬆), but also on all the elements. we use a lot when we want to select child element.
**_to select element pass element itself, while to select id or class we pass # or . respectively._**

**getElementById**
we'll pass only id name, not with #.
document.getElementById('section--1');

**getElementsByTagName**
const allButtons = document.getElementsByTagName('button');
console.log(allButtons);
**Remember** _This methods returns an HTML collection. An HTML collection is so called life collection, that means if the DOM changes then this collection is also immediately updated automatically._ i-e if we remove some elements then it will also remove immediately from html collection. NOW the same does't happen with the nodeList, it means it didn't update itself when we add or remove that element/class/ id .

**getElementsByClassName**
document.getElementsByClassName('btn') // once again we don't need a selector like getElementById();
also returns an html collection.

_we use querySelector almost all the time._

### CREATING-ADDING_ELEMENTS

**insertAdjacentHTML**
We can create html elements using insertAdjacentHTML function.
This is a quick way to creating elements and use the most.

createElement() Method

1. use createElement method to create an element.
2. store that element in any variable, for our facility.
3. add class by using classList method, if necessary.
4. add content to the element. using textContent or innerHTML property.
5. Insert into DOM.

```js
const message = document.createElement('div');
```

_In this method we will pass a string of tag name. this will creates a DOM element into the message. this element is not yet in the DOM itself, so it's not found in our webpage. If we wanted on the page then we should manually inserted it into the page._
**Remember that the result is a dom object that is stored in message like we selecting by using query selector and then storing into any variable.**

```js
// NOW we'll add a class on it.
message.classList.add('cookie-message');

// Adding content in that element.
message.textContent =
  'We use cookies for improved functionality and analytics.';

// we can also user innerHTML
message.innerHTML =
  'We use cookies for improved functionality and analytics. <button class="btn btn--close-cookie">Got it!</button>';
```

Now our element is completed, all we have to insert it into DOM.
we'll select our header and then append our message to that element. we already selected above in query selector.

##### 4 ways to insert in DOM.

1. **USING prepend() method**
   Prepend basically add the element as a first child of selected element, here message should first child of header.
   header.prepend(message); // now it's on DOM.

2. **USING append() method**
   we can also add as a last child that's append
   header.append(message); // first one is removed!!!
   We see here, that element was insert at once. that's because this element here a message in now indeed a life element living in the DOM, therefor it can't be multiple places at the same time, like a person he also can't be at two places simultaneously.

   **Now _WHAT_ if we actually wanted to insert multiple copies of the same element?????**
   In that we actually would have to copy the first element So,
   We Use cloneNode(true) to copy
   header.append(message.cloneNode(true)); // now it's on both places.

3. **before method**
   it'll add as a sibling, but before header. -first cookie-message then header.
   header.before(message);

4. **after method**
   will add sibling but after that element.
   header.after(message);

### DELETING_ELEMENTS

we wants to remove newly created (message) element when we click a button(got it) that is in that element.
Fist select the button

```js
document
  .querySelector('.btn--close-cookie')
  .addEventListener('click', function () {
    message.remove(); // remove method is very recent. before this we use remove child element, for that first we have to select parent element then....that would look like this.
    message.parentElement.removeChild(message);
  });
```

## STYLES_ATTRIBUTES_CLASSES

### STYLES

element.style.propertyName = value **-basic syntax [ in camelCase ]**
_Remember that: these styles are set as inline style._
message.style.backgroundColor = '#37383d';
message.style.width = '120%';

```js
console.log(message.style.height); // nothing!!, although we've set height in css file, because it will print only inline styles. it actually works on only inline styles.
console.log(message.style.backgroundColor); // rgb(55, 56, 61)// now printed, it's inline style. -we added noe
```

We can still get an external style if we really wanted, using **getComputedStyle** function.
_console.log(getComputedStyle(message));_ // which will contain all of the properties with values. it's so huge, so, in practice we then do, simply take a certain property.
_console.log(getComputedStyle(message).color);_ //rgb(187, 187, 187)
this is a computed style, which means that is applied on page, even if we not declare it in our CSS file.

_console.log(getComputedStyle(message).height);_ // 43px, we didn't define ourselves, but browser needed to calculate the height.

**lets consider we wants to add some height to cookies message.**

```js
message.style.height = getComputedStyle(message).height + 40 + 'px';
```

_nothing happen b/c result of computedStyle is string._
console.log(typeof (getComputedStyle(message).height)); // string, So we have to convert it first

```js
message.style.height =
  Number.parseFloat(getComputedStyle(message).height, 10) + 30 + 'px';
```

**here we can't convert into number by using Number() function b/c it contain px at the end, so here parseFloat comes to play, which is very handy in this type of situation.**

**CSS CUSTOM PROPERTIES:** Which means CSS variables.
lets change some values from these custom properties (variables)
first we will find where these variables are define.
In this case these are define in root, which is equivalent to the documentElement, (not document, documentElement)

```js
document.documentElement.style.setProperty('--color-primary', 'orangered'); // changed to orangered in every where where this variable is used.
```

setProperty will take two parameters first name of property and second value, We can do the same to all other property(built-in). like color, margin, padding etc, BUT above one is simple and easy way to change any built in property's values.

### ATTRIBUTES

We can access and change different attributes, like src, href, id, class, alt etc.

```JS
const logo = document.querySelector('.nav\_\_logo');
console.log(logo.src);
```

**here the source(link) is different than what we have in html. why?**
Here we have a absolute url, but in html there is a relative url(relative to the folder in which index.html located.). **if we want to get only url that is in html file then we use getAttribute method**

```js
console.log(logo.getAttribute('src')); // yes indeed
```

**same is true for href attribute.**

```js
console.log(logo.alt);
console.log(logo.className); // remember className not class
```

**if we add un-standard attribute, then we can't access like this.**

```js
console.log(logo.designer); // undefined but in html it's defined, / bc of un-standard.
```

**There is another way to access un-standard attributes:**

```js
console.log(logo.getAttribute('designer')); // me, working.
```

**We can also use setAttribute to set**

```js
logo.setAttribute('company', 'bankist');
console.log(logo.getAttribute('company')); // bankist
```

**We can also set all of these attributes.**

```js
logo.alt = 'Beautiful minimalist logo'; // now check
console.log(logo.alt); // changed.
```

**_Remember for links/ source if we want relative url than always use getAttribute('href'/'src') method AND if we want absolute url then use simply .src OR .href_**

```js
const link = document.querySelector('.nav__link');
console.log(link.href); // Absolute url
console.log(link.getAttribute('href')); // Relative, which is in html
```

_Another example_

```js
const link2 = document.querySelector('.twitter-link');
console.log(link2.href);
console.log(link2.getAttribute('href'));
```

**_Data Attributes:_**
Data attributes are a special kind of the attribute that start with words data.
It has to start with data than dash- than what ever we want.

```js
console.log(logo.dataset.versionNumber); // this is very useful and use a lot, it use to store data in our html code.
```

### CLASSES

```js
logo.classList.add('c', 'j'); // can add/remove multiple classes
logo.classList.remove('c', 'j');
logo.classList.contains('c');
logo.classList.toggle('c');
```

**don't use this⤵. it will delete all other classes.**

```js
_logo.className = 'me';
_;
```

## SMOOTH_SCROLLING

// There are two ways to doing this. first one is bit more old way to doing this and second one is modern way.

// here we are going to implement smooth scrolling to 'learn more' button.

// first we will take a button and that particular section where we want to link that button
const btnScrollTo = document.querySelector('.btn--scroll-to');
const section1 = document.querySelector('#section--1');

btnScrollTo.addEventListener('click', function (e) {

// SOME STUFF SHOULD NOW:

// Finding distance b/w element and the sides of current viewport: and also width, height of element itself:
// we need to get coordinates of the element that we want to scroll to.
const section1Coords = section1.getBoundingClientRect();
// console.log(section1Coords); // here we see the top, bottom, left, right, x, y and many more properties. This is a DOM rectogle of element section.

// lets exprement more, by getting more elements's DOM rectange:

// get DOM rectangle of learn More button
// console.log(btnScrollTo.getBoundingClientRect()); // we can also write, e.target instead of btnScrollTo, bc both are same. like this:
// console.log(e.target.getBoundingClientRect());

// Remember that boundingClientRect will aloways relative to the current view port. i-e, value of x, y, top, etc will change a/c to distence from left, top, top of viewport respectively.

// Finding distence b/w current viewport and the top of the page:
// console.log('current scroll X/Y: ', window.pageXOffset, window.pageYOffset);
// it'll show current scroll from top of the page to the viewport.
// Remember : it is relative from top of the page to the viweport. remember that.
// initially very to both x & y should be zero. then according to scroll-down, the value of y will change, here x will remain 0 b/c, no horizontall scrolling in this page.

// Finding width and height of the current viewport:
// console.log('Height/Widht of veiewport: ', document.documentElement.clientHeight, document.documentElement.clientWidth);

// OLD WAY OF DOING SMOOTH SCROLLING:
// We use golobal function scrollTo, it'll take two arguments 1. left position, 2. top position.
// window.scrollTo(section1Coords.left, section1Coords.top);
// Remember that this top is always relative to the viewport, so it doesn't work in many situations. BUT the solution of this problem is to simply add the current scroll position to the top, like this...
// window.scrollTo(section1Coords.left + window.pageXOffset, section1Coords.top + window.pageYOffset);
// it's current position plus current scroll

// Now it's working but not a smooth scrolling.
// MAKING SMOOTH:
// we will pass an object instead of one argument.
/\*

window.scrollTo({
left: section1Coords.left + window.pageXOffset,
top: section1Coords.top + window.pageYOffset,
behavior: 'smooth'

    // remember key and value
    // This is a finla block to smooth scrolling.

});

// MODERAN WAY TO DINONG THIS (IN ONE LINE)

// simply use select element where we want to scroll and then .scrollIntoView function, there we'll pass an object with behavior: 'smooth'.
section1.scrollIntoView({
behavior: 'smooth',
}); // remember, this is very modern way. may be not support some browsers.

});

- /
  ///////////////////////////////////////////////////////////
  ///////////////////////////////////////////////////////////
  /#### lecture #08

// Heading /// - EVENTS - ///

// ---- Type of Events and Event Handlers ---- //

// An event is basically a signal that is generated by a certain dom node. signal means that something has happened, for example, a click somewhere or the mouse moving, or the user triggering the full screen mode, or really any thing that happened on our webpage, generates an event. And then we can listen and handle these events in our code using eventListners then handle them.

// If we handle a certain event or not that event always happen, for example if we define an event click on certain node but not handle it, the event will always occur no matter event is handle or not.

// We already know addEventListner('click');
// Two more ways to Listening an event
// check MDN to see list of different events.

// mouseenter
// mouseenter effect is little bit like as hover event in CSS.
const h1 = document.querySelector('h1');
/\*
// --- Three Ways to handling an Events ---

1. By using addEventListener method

h1.addEventListener('mouseenter', function (e) {
alert('addEventListener: mouseenter, great!!! \n You are reading the heading h1')
});

\*/

// 2. by using onEvent Property

// Another way to attaching an event listner to an element:
// using on Event property directly on element
// here we want onmouseenter (upper wala)
// first we access that property then we set that handler function to that property.
/\*
h1.onmouseenter = function (e) {
alert('addEventListener: mouseenter, great!!! \n You are reading the heading h1')
};

// Remember : For each of events ther is one onEvent property.like .onmouseenter, .onclick, onkeydonwn etc. .

document.body.onkeydown = function (e) {
console.log(e.key); // Will print pressed keyd
};

// However this way is bit old school, Now we always use .addEventListener method.

// Two cases in that addEventListener is better
// 1. We can add multiple event listener to the same event. if we add multiple even listener by using onenvnet property then always last event overwrite first ones.
// 2. We can remove an event handler in case we don't need it anymore. it's very simple and very usefull. let's try it...

// To do that (remove event hadler) first we have to define a function as a named function, it means we have to define function outsine the argument, by storing it into a variable as a function name then paste the name as an argument.

const alertH1 = function (e) {
alert('addEventListener: mouseenter, great!!! \n You are reading the heading -h1-');

// Now Removing Event Listener:

// h1.removeEventListener('click', alertH1); // first time event should listen and after that nothing will happen when clicking. this is very usefull when we are adding an event listener which listen once.
// no necessary that remove should done here.
};
h1.addEventListener('click', alertH1);

// We can remove an envent after certain time:
setTimeout(() => {
h1.removeEventListener('click', alertH1);
}, 5000);
// after 5sec nothing happening.. very usefull

// 3. using HTML attributes: [very rare use case]
// third way of handling events.
// we will do in html tag as a attribute like this
// <h1 onclick = "alert('Welcome to H1.')"></h1> -not here should put in html file. just for example

//////////////////////////////////////////////////////
//////////////////////////////////////////////////////

#### lecture #09 -event propagation theory

// Java script events have very important property.
// They have a so-called capturing phase and bubbling phase

// Explaining the capturing and bubbling phase:
// When an event(click,.,..) happed on any element, the DOM generates a click event right now. However this event is actually not generated at the target elemet(element there we set event to occur, or where the event happeded). Insted the event is actually generated at the root of document, so at the very top(document) of the DOM tree. And from there(root) the capturing phase happens, where the event travels all the way down from the document root to the target element. As the event travels down teh tree, it will pass through every single parent element of the target element. As soon as an event reaches the target the target phase begins, where events can be handled right at the target. And as soon as the event accurs, the event listener runs callback function.

// Now after raaching the target, the event then actually travels all the way up to the document root again, in the so called bubbling phase. We can say the event bubble up from the target to the document root. and just like in the capturing phase, the event passes through all its parent elements, (just parent not sibling ).

// Why all of these details is very Important.
// Basically events also happened in each of the parent elements. we can attache same event listener for any of parent. this behavior will allow us to implement really poweful pattern. (we see in next video).

// by default events can only be hadled in the target, in bubbling phase. However we can set up event listeners in a way that they listen an event in the capturing phase insead of bubbling.

// Not all type of events do have a capturing and bubbling phase. Some of them are created right on the target element, so we can only handle them there. most of the events do capture and bubble

// We can also say events propagate, which is rally what capturing and bubbling is. (event is propagating from one place to another. )

\*/
///////////////////////////////////////////////

#### lecture #10

// ---- Event Propagation Practice ----- //

// here we will attach an event listener to the navigation link, and all of it's parent elements. when we click that link, we will give all these elements random background colors. to visvilise how event bubbling is happening....

/_
// Creating Random Color:
// random color is just a string : rgb(255,255,255);
const randomInt = (min, max) => Math.floor(Math.random() _ (max - min + 1) + min) // for random number

// we use this random integer generator function to generate random color.
const randomColor = () => `rgb(${randomInt(0, 255)},${randomInt(0, 255)}, ${randomInt(0, 255)})`;
console.log(randomColor()); // yes! //rgb(69,15, 43)

// now we apply event listener to nav link
document.querySelector('.nav\_\_link').addEventListener('click', function (e) {

// remember that the 'this' key word in event handler is always point to the element where event handler is attached.
this.style.backgroundColor = randomColor(); // here it's working BUT, what if we want to perform same thing to it's parent element?
// copy this code and paste it in parent element.
// when we click this link also the parest is changing its color, due to bubbling.

// now we see eventTarget : event taget means where the event is happened
console.log('Link', e.target, e.currentTarget); // here is target is weather the event is happened. it not the element where event handler is attached.
// currnet target is the element on which event handler is attached.

// So current target is equal to this keyword, b/c both are pointin....
// console.log(e.currentTarget === this); //true.

// We can stop 'Evnent Propagation'.
// e.stopPropagation(); // now not propagating to the parents.
});

// parent of nav link
document.querySelector('.nav**links').addEventListener('click', function (e) {
this.style.backgroundColor = randomColor();
// both of these two are handling the same event that is occuring at nav**link element.

console.log('Container', e.target, e.currentTarget); //where event is occured. not where event hadler is attached.
});

// also parent of nav link
document.querySelector('.nav').addEventListener('click', function (e) {
this.style.backgroundColor = randomColor();

console.log('Nav', e.target, e.currentTarget);
});

// here we see when we click on nav**link, the event is also occuring on it's parent elements(parent also changing their colors). but when we click on nav**links then nav and nav**links itself are changing their color not nav**link, so it means when any event accur on any element then same event will occur on it's parent elements, not child elements.

// and the tager is always where the event is occured. if we click on nav**link, then e.target will nav**link element in all three event handler function. B/c all of there are hadlling exact same event. that's because of event bubbling.

// and currnet target is the element on which event handler is attached. So current target is same as 'this' keyword, b/c both are pointing to the element where event handler is attached.

// we can stop propagation using e.stopPropagation() funcion.

// we can say any element listen an event that happed on it or any of it's child element from their the event will bubbling up.

// Form here we only talk about bubling and target pahse, and now what about the capturing phase???
// Events are captured when they come dowm from the document root all the way to the target. BUT event handlers not picking up these events during the capture phase.
// event listener are only listning in the pubbling phase, but not in capturing pahse that is the defaul behavior of the addEventListener method. reason for that is the capturin phase is ususlly irrelevent for us.
// However if we want catch events during capturing phase, we can define a third parameter, in an addEventListener funtion, we can set third parameter to true or false. for example:
/\*
document.querySelector('.nav\_\_link').addEventListener('click', function (e) {
this.style.backgroundColor = randomColor();

console.log('Nav', e.target, e.currentTarget);
});
document.querySelector('.nav\_\_links').addEventListener('click', function (e) {
this.style.backgroundColor = randomColor();

console.log('Nav', e.target, e.currentTarget);
});
document.querySelector('.nav').addEventListener('click', function (e) {
this.style.backgroundColor = randomColor();

console.log('Nav', e.target, e.currentTarget);
}, true);

// in this case the event handler no longer listen to bubbling events but instead, to capturing events. In practice they gonna look same as bubbling phase. But in console we can see that the first element were an event passes in nav.

\*/
////////////////////////////////////////////////////////

#### lecture #11

// ---------- Event Delegation --------------- //
// using power of event bubbling, we can implement event delegation.
// here we are implementing the smooth scrolling effect on navigation menu. We'll discuss two ways to do that:

/\*
// -- First way without using Delegation -- [not efficient]

// document.querySelectorAll('.nav**link'); // it'll return a node list, so here we will use a forEach method, to attach event hadler to each of the elements.
document.querySelectorAll('.nav**link').forEach(el => {
el.addEventListener('click', function (e) {
// if we provide an id of any element in 'a' tage's href attribute, then we will click on 'a' tag it'll always go to that id. so we need to prevent this first from hapenning.
e.preventDefault();

    // now implement smooth scroling.
    const id = this.getAttribute(('href'));
    console.log(id);// we see that this output is exact same as we are selecting any element by id. so
    document.querySelector(id).scrollIntoView({ behavior: 'smooth' });
    // very common technique use in nav menu
    // this working very well BUT not an efficeient way .

});
});
\*/

// -- Second way using Event Delegation -- [efficient]
/\*
// In event delegation we use the fact that events bubble up. We do that by putting the eventListener on a common parent of all the elements, that we are interested in. In our example a container where all the links contain. ('.nav\_\_links')

// in event delegation we need two steps:
// 1. we add the event listener to a common parent element.
// 2. Determine what element originated the event.

document.querySelector('.nav\_\_links').addEventListener('click', function (e) {
e.preventDefault();
// console.log(e.target); // where event is originated.

// Matching Strategy: (v. important)
// b/c when we click on parent it will also call event handler. to avoid that:
if (e.target.classList.contains('nav\_\_link')) {
// const id = this.querySelector('href'); //here this will not work. b/c this is pointing to the eventhadler element, that's container of links. so we use e.target.
const id = e.target.getAttribute('href');
document.querySelector(id).scrollIntoView({ behavior: 'smooth' })
}
});

\*/ // Implementd on nav bar (upper ja)

///////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////

// Heading

// --- Traversing The DOM ---
// DOM traversing is basically walking through the DOM. which means that we can select an element based on another element.

// here we'll take h1 element then where we can do donword, upward and also sideways.

const h1 = document.querySelector('h1');

// Going Downwords: -Selecting Child
/\*
// querrySelector & querrySelectorAll [all elements]
console.log(h1.querySelectorAll('.highlight')); // it'll select all the elements of calss 'highlight' that are child of h1 element. And that would work no matter how deep these child elements would be inside of the h1 element. (imp)
// will give node list

// childNodes property. [all nodes -direct child]
// But some times we need direct children, so for that we use .childNodes
console.log(h1.childNodes); // here a lot of things, bc we already know that node can be any thing, elements, text, or even comment. (will give a node list)

// children property [only direct elements]
// But often we are interested in simple elements. so, we use .children
console.log(h1.children); // will give HTML collection -live collection.

// firstElementChild property [only first child]
// There is also first and last child element
console.log(h1.firstElementChild);
h1.firstElementChild.style.color = 'red';

// lastElementChild property [only last child]
console.log(h1.lastElementChild);
h1.lastElementChild.style.color = 'purple';
_/
///////////////////////////////////////////
/_
// Going Upword: -Selecting Parent

// direct parent:
// parentNode property [direct parent nodes]
console.log(h1.parentNode);

// direct parent element only
// parentElement property [direct parent element]
console.log(h1.parentElement); // (mosty use)

// closest method [any parent element ]
// Parent elemnt that is not direct. it means a parent, no matter how far away it is. for that we use h1.closest method
// and the closest method receives a querry string just like a querrySelector and querrySelectorAll.
console.log(h1.closest('.header')); // will print a header classed element that should be the parent of h1. but-no mater how far it is.
h1.closest('.header').style.background = 'var(--gradient-secondary)' // here we use css variable. (custom property. ) // (v.important one. oftan used.)

// Remember We can say that closest is opposite of querrySelector, b/c querrySelector will select the childern, no matter how deep and closest method finds parent also no matter how far up in the DOM, BUT both receives a strings.

_/
///////////////////////////////////////////
/_
// Going Sideways: -Selecting Siblings

// For some reason in javascript we can access only direct siblings. Basically only the previous and next one.
console.log(h1.previousElementSibling);
console.log(h1.nextElementSibling);

// for all nodes
console.log(h1.previousSibling);
console.log(h1.nextSibling);

// if we want all the sibling then we'll move upto the parent then we can assess all
console.log(h1.parentElement.children);
// it will print html collection -not an array . but it's still iterable we can implement some method or operators of array

[...h1.parentElement.children].forEach(function (el) {
//
if (el !== h1) {
el.style.transform = 'scale(0.5)';
}
})

\*/

/////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////

#### lecture #13

// Heading
// // --- Building a tabbed Component --- //

//////// upper hoga //

/////////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////////

#### lecture #14

// Heading
// // --- Building a Hovere effect on navigation --- //
// // -- How to pass argument into event hadler function.

//////// upper hoga // tabbed k nyche

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////

#### lecture 15

// --- Implementing Sicky Navigation --- //
// also uppar

///////////////////////////////////////////////////////
///////////////////////////////////////////////////////

#### lecture 18

// --- Lazy Loading Images --- //

// - Strategy of image optimization, for better performance.
// - here we add two images for each image, one is very small in size and another is original one.
// then we will add the link of compressed image in to data-src attribute, which's a special attribure we can use, this is not a standard html attribute.

// -the idea is that as we scroll down then we replace the copressed imge with original one.
// also we have add a class to blur an image.

// baqi also uppar

///////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////

#### lecture 29-20

// Heading

// --- BUILDING A SLIDER COMPONENT --- [image slider] //

// first we'll implement the image slider.

// all images are top of each other. so we use style property in js to translate all image other than active image.

// selecting element
// const imgSlider = document.querySelector('.slider');
// const imgSlides = document.querySelectorAll('.slide');
// const btnLeft = document.querySelector('.slider**btn--left');
// const btnRight = document.querySelector('.slider**btn--right');

// let curSlide = 0;
// const maxSlide = slides.length;

// // imgSlider.style.transform = 'scale(0.5)';
// // imgSlider.style.overflow = 'visible';

// // add transform property

// const gotoSlide = function (slide) {
// imgSlides.forEach((s, i) => {
// s.style.transform = `translateX(${100 * (i - slide)}%)`;
// // 1st: -100%, 2nd: 0%, 3rd: 100%, 4th: 200%......
// });
// }

// gotoSlide(0)

// // goto next slid.
// const nextSlide = function () {
// if (curSlide === maxSlide - 1) {
// curSlide = 0;
// } else {
// curSlide++;
// }
// gotoSlide(curSlide);
// }

// const prevSlide = function () {
// if (curSlide === 0) {
// curSlide = maxSlide - 1;
// } else {
// curSlide--;
// }
// gotoSlide(curSlide);
// }

// btnRight.addEventListener('click', nextSlide)
// btnLeft.addEventListener('click', prevSlide);

/////////////////////////////////////////////////////////
/////////////////////////////////////////////////////////

#### lecture 21

// Heading

// --- LIFECYCLE DOM EVENTS --- //

// Lifecycle mean, when the page first loaded, until the user leaves it.

// DOMContentLoaded Event:
// 'DOMContentLoaded' event: this event fired by the document as soon as the HTML is completely parsed, which means that the HTML has been downloaded and been converted to the DOM tree. Also all script must be downloaded and executed before the 'DOMContentDoaded' event can happen

/\*
document.addEventListener('DOMContentLoaded', function (e) {
console.log('HTML parsed and the DOM tree built!', e);
// just html (not media/images/css etc) and js loaded this event'll happen.
})

// Load Event:
// Load event fired by the window As soon as not only the HTML is parsed, but also images, external resources like CSS are loaded.
// When complete page has finished loading, this event gets fired.

window.addEventListener('load', function (e) {
console.log('Page fully loaded!', e);
})

// beforennload Event:
// which also get fired on window.
// this event will created immediately before a user is leave to page. for exampe after clicking close button on browser tab.

window.addEventListener('beforeunload', function (e) {
e.preventDefault(); // in some browser we need to preventDefault to call this event
console.log(e);

// in order to display leaving confirmation, we need to set the return value on the event to an emplt string.
e.returnValue = '';
// we ask for confirmation in popup window. like 'did you want to leave the page'
});
// this'll pretty usefull if user leave the page during fillign any form................................ etc
\*/

////////////////////////////////////////////////////////
////////////////////////////////////////////////////////

#### lecture 22

// Heading
// Efficient way to loading script:

// different way to load javascirpt code into HTML

// REGULAR WAY
{/_ <script src="script.js"></script> _/ }

// USING ASYNC ATTRIBUTE
{/_ <script async src="script.js" ></script> _/ }

// USING DEFER ATTRIBUTE
{/_ <script defer src="script.js" ></script> _/ }

// AND also remember we can write script tag in html head and body end: -we'll compre for these two:

// --- script tag in head [without any attribure]
// if we write script without any attribure in the head then firt the browser parse html code -parsing meand building dom tree- then at the certain point, it'll find script tag, start to fetch the script then execute it, and all these time the HTML parsing will actually stop, only after fetched and executing the rest of HTML can be parsed.
// This is not ideal at all b/c waiting a browser for fatching can have huge impact on the pages performance, also in this case, script will be executed before the dom is ready.
// So, never do this

// --- script tag in body-End [without any attribure]
// the html is parsed then the script tag is found at the end of the document, then the script is fetched, and finally the script gets executed.
// this is much better, BUT still not perfect, because the script could have been downloaded befre, while the html was still being parsed

// --- async attribute in head
// script is loaded at the same time as the html is parsed, in an asynchronous way. HOWEVER the html parsing still stop fot the script execution. so the script is actually downloaded asynchronously, butn then it's executed in a synchrounous way. so html code has to for beign parsed.

// --- defer attribute in head
// script is loaded at the same as the html is parsed in an asynchronous way like in async but the execution of the script is defered until the end of the html parsing. script is only executed at the end. so not wait for parsing html. In many time this is very good option.

// In body both are not make any sense. no difference in body and head

// USE CASES OF ALL THESE STRATEGIES:

// - IN Any case don't use regular in head

// DOMComtentLoaded event fired when only loaded html code not wait for javascript code in async attribure.

// and using defer attribute, the DOMContentLoaded event fired after the whole script has been downloaded and executed.

// in async there is no gurentee of the actually execution of code in the order that they are declared
// in defer there is a gurentee of the actually execution of code in the order that they are declared

// so conclusion:
// using defer in the head in overall best solution.

````

```

```
````
