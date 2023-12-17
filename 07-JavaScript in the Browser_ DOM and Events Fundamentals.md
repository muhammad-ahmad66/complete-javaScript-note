# JavaScript in the Browser\_ DOM and Events Fundamentals

## Table of Contents

1. [GUESS_MY_NUMBER](#GUESS_MY_NUMBER)
2. [MODAL](#MODAL)
3. [PIG_GAME](#PIG_GAME)

## GUESS_MY_NUMBER

const guess = document.querySelector('.guess').value;
console.log(typeof guess);
**Always Remember:**

- **Whenever we get from a user's interface it always is a string**. here in this game we have to compare it with random generated number so, first we have to convert it into a number.
- **Whenever we are manipulating a style we always gives a value like a string**(in quote.)
- **All these styles are set as inline style.**
- Here we use addEventListener method and we have to add two arguments. one for when should event happened, in this case on click. and second we will pass a function, it is called event handler, in this function eventHandler we have to specify what should happen when click.
  **Remember that it is a function expression.** in function expression we write a function and assign to any variable that's just a function name, here we direct pass functions as a argument. also we han't call this function, just of because javascript will automatically call when event occur.
- **Refactoring the code:** DRY (Don't Repeat Yourself). To achieve DRY we refactoring the code and remove repeating stuff, and eliminate the duplicate codes.

```js
'use strict';

//Secret number generator
let secretNumber = Math.trunc(Math.random() * 20) + 1;
let score = 20;
let highscore = 0;

//for refactoring we make a function just like thsi:
const displayMessage = function (message) {
  document.querySelector('.message').textContent = message;
};

//Event listener

document.querySelector('.check').addEventListener('click', function () {
  const guess = Number(document.querySelector('.guess').value);
  // console.log(guess, typeof guess);

  //when there is no input
  if (!guess) {
    //if there is no value then in input number type there will always 0. and 0 is a  falsy value.
    // document.querySelector('.message').textContent = '⛔ No number!'
    displayMessage('⛔ No number!');

    //when the player wins
  } else if (guess === secretNumber) {
    // document.querySelector('.message').textContent = '🎉 Correct Number';
    displayMessage('🎉 Correct Number');

    document.querySelector('.number').textContent = secretNumber;

    document.querySelector('body').style.backgroundColor = '#60b347';
    document.querySelector('.number').style.width = '30rem';

    if (score > highscore) {
      highscore = score;
      document.querySelector('.highscore').textContent = highscore;
    }

    //When guess is wrong
  } else if (guess !== secretNumber) {
    if (score > 1) {
      // document.querySelector('.message').textContent = guess > secretNumber ? '📈 Too high!' : '📉 Too low!';
      displayMessage(guess > secretNumber ? '📈 Too high!' : '📉 Too low!');
      score--;
      document.querySelector('.score').textContent = score;
    } else {
      // document.querySelector('.message').textContent = '💥 You lost the game!';
      displayMessage('💥 You lost the game!');
      document.querySelector('.score').textContent = 0;
    }
  }
  when guess is too high
  else if (guess > secretNumber) {

      //when guess is too low
  } else if (guess < secretNumber) {
      if (score > 1) {
          document.querySelector('.message').textContent = '📉 Too low!'
          score--;
          document.querySelector('.score').textContent = score;
      } else {
          document.querySelector('.message').textContent = '💥 You lost the game!';
          document.querySelector('.score').textContent = 0;
      }
  }
});

document.querySelector('.again').addEventListener('click', function () {
  score = 20;
  secretNumber = Math.trunc(Math.random() * 20) + 1;
  // document.querySelector('.message').textContent = 'Start guessing...';
  displayMessage('Start guessing...');
  document.querySelector('.score').textContent = score;
  document.querySelector('.number').textContent = '?';
  document.querySelector('body').style.backgroundColor = '#222';
  document.querySelector('.guess').style.width = '15rem';
  document.querySelector('.guess').value = '';
});

```

---

## MODAL

- **NOTE:** In html lets check, there is three elements with 'show-modal' class. if we select this with 'querySelector' then it will select only first one. "HERE IS FIRST LIMITATION OF querySelector". SO, here we use querySelectorAll(''). it will select all that have same class.
- it opens in NodeList just as array, but not exactly same as array. we treat like an Array.
- _overlay.addEventListener('click', closeModal);_ **_Remember_** here we will't write any parentheses b/c we are not calling a function, if you write parentheses then after open a page it will immediately call it. (js will call when event occur.)

**KEY PRESS handle**<br>
There are three types of events for the keyboard.

1. keyup: when we lift our finger off the keyboard.
2. keypress: when we pressing hold
3. keydown: when press down any key. **'usually we use this'**

console.log(e);<br>
whenever we press a key a certain objet will create by JS, where we found all the properties of pressed key. we can see. let's check by clicking a random key.

```JS
'use strict';
const modal = document.querySelector('.modal');
const overlay = document.querySelector('.overlay');
const btnCloseModal = document.querySelector('.close-modal');

// const btnsOpenModal = document.querySelector('.show-modal');
// console.log(showModal);

const btnsOpenModal = document.querySelectorAll('.show-modal');
// console.log(btnsOpenModal);//see in console:
const openModal = function () {
modal.classList.remove('hidden');
overlay.classList.remove('hidden');
}
for (let i = 0; i < btnsOpenModal.length; i++) {
btnsOpenModal[i].addEventListener('click', openModal);
console.log('Button Clicked');
modal.classList.remove('hidden');
overlay.classList.remove('hidden');

}
btnCloseModal.addEventListener('click', function () {
modal.classList.add('hidden');
overlay.classList.add('hidden');
});

overlay.addEventListener('click', function () {
modal.classList.add('hidden');
overlay.classList.add('hidden');
})
// to make more efficient and clean, we make for both these block a function because both have same statements. so comment it and do....
const closeModal = function () {
modal.classList.add('hidden');
overlay.classList.add('hidden');
};
btnCloseModal.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);


document.addEventListener('keydown', function (e) {
// console.log('A Key was pressed.');

console.log(e.key); //here will print the key name.

    //NOTE: We want to add functionality, when we click escape key pressed the 'openModal' should close.
    if (e.key === 'Escape' && !modal.classList.contains('hidden')) {
        closeModal();
    }

})
```

---

## PIG_GAME

```js
'use strict';

// Selection Elements:
// Selection Elements from html tree and storing into a variables.
const player0El = document.querySelector('.player--0');
const player1El = document.querySelector('.player--1');
const score0El = document.querySelector('#score--0');
const score1El = document.getElementById('score--1');
const current0El = document.getElementById('current--0');
const current1El = document.getElementById('current--1');

const diceEl = document.querySelector('.dice');
const btnNew = document.querySelector('.btn--new');
const btnRoll = document.querySelector('.btn--roll');
const btnHold = document.querySelector('.btn--hold');

let scores, currentScore, activePlayer, playing;

// Starting Condition:
// giving all elements a starting value (content).
const init = function () { //function to initialization.

    scores = [0, 0];
    currentScore = 0;
    activePlayer = 0;
    playing = true  // It will hold the game state. isPlaying or not? if playing it's true and if any player won it would false;
    current0El.textContent = 0;
    current1El.textContent = 0;
    score0El.textContent = 0;
    score1El.textContent = 0;
    diceEl.classList.add('hidden');
    player0El.classList.add('player--active');
    player1El.classList.remove('player--active');
    player0El.classList.remove('player--winner')
    player1El.classList.remove('player--winner')

}

init(); //we should run it when this page is loaded, because we should initialize all variable when it load.

const switchPlayer = function () {
document.getElementById(`current--${activePlayer}`).textContent = 0;
currentScore = 0;
activePlayer = activePlayer === 0 ? 1 : 0;

    player0El.classList.toggle('player--active');
    player1El.classList.toggle('player--active');   //toggle will add a certain class if there not exist that class and remove that if exist there. (if exist will remove, if not there will add)

}

// Implementing Rolling Dice Functionality.
btnRoll.addEventListener('click', function () {
if (playing) {
// 1. Generating a random dice roll
const dice = Math.trunc(Math.random() \* 6) + 1;

        // 2. Display Dice
        diceEl.classList.remove('hidden');
        diceEl.src = `dice-${dice}.png`;    //Here is a logic to select an image according to random number.

        // 3. Check for rolled 1: if true switch to next player
        if (dice !== 1) {
            // add dice to current score.
            currentScore += dice;
            document.getElementById(`current--${activePlayer}`).textContent = currentScore;

        } else {
            // switch to next player.
            switchPlayer();

        }
    }

});

// Implement Hold button: When click on it the current score should add into the main score.
btnHold.addEventListener('click', function () {
if (playing) {
// Add current score to Active player's main score
scores[activePlayer] += currentScore;
document.getElementById(`score--${activePlayer}`).textContent = scores[activePlayer];

        if (scores[activePlayer] >= 20) {
            //finish the game!! You Won
            // Check if player's score is >= 100; if true finish the game
            playing = false; //change state of game to false value bc active player won
            diceEl.classList.add('hidden');
            document.querySelector(`.player--${activePlayer}`).classList.add('player--winner');
            document.querySelector(`.player--${activePlayer}`).classList.remove('player--active');

        } else {
            // switch to the next player
            switchPlayer();

        }
    }

});

btnNew.addEventListener('click', init)
```
