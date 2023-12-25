# Mapty Project

_Implementing **OOP in Real world Project**. And also Practicing **Geolocation API**._

## Table of Contents

1. [HOW_TO_PLAN_A_WEB_PROJECT](#how_to_plan_a_web_project)
2. [Using_Geolocation_API](#using_geolocation_api)
3. [DISPLAYING_MAP_ON_PAGE](#displaying_map_on_page)
   1. [DISPLAYING_A_MAP_MARKER](#displaying_a_map_marker)
   2. [Rendering_Workout_Input_Form](#rendering_workout_input_form)
4. [Project_Architecture_With_Full_Code](#project_architecture_with_full_code)
5. [Project_Architecture_With_Full_Code](#project_architecture_with_full_code)
6. [Storing_Data_in_LocalStorage](#storing_data_in_localstorage)

---

---

## HOW_TO_PLAN_A_WEB_PROJECT

Concept of **'USER STORIES'**  
**A user story is a basically a description of the application's functionality from the user's perspective** And then all the user stories put together will clearly describe the functionality of the entire application.  
User stories are basically a high lever overview of the whole application, which allow us to determine the exact features that we need to implement. Then after that to visualize the different actions that a user can take, we usually put all these features into a nice flow-chart.

And then comes Project Architecture  
**Project Architectures** means how we will organize our code, and what javascript features we will use.  
Than finally we move to development. their we gonna implement.

**User Sorties** -> **Features** -> **Flowchart** -> **Architecture** -> **Development**

Fist four are planing steps and then Development.

1. USER STORIES: functionality from user's perspective.
   common format to write user stories:
   As a [type of user], I want [an action] so that [a benefit]

   This format of format answers: WHO, WHAT AND WHY
   WHO? USER, WHAT? FUNCTION? WHY? BENEFIT

For detail check out **PDF file** [Theory_lecture](./theory-lectures-v2.pdf)

---

## Using_Geolocation_API

**Geolocation API** [one of modern API] is called API because it is in fact, **a browser API**, just like for example internationalization or timers, or really anything that the browser gives us.

```js
navigator.geolocation.getCurrentPosition().
```

**getCurrentPosition** function takes two callback functions as an input.  
First **callback** function will be called on **success**. -whenever browser successfully got the coordinates of the current position of the user. And always we pass parameter, usually called position.  
second **callback** is the **Error** callback, which called when there happened an error while getting the coordinates.

```js
// if navigator.geolocation exist
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(
    function (position) {
      console.log(position);
      // lets get latitude and longitude form the the coords object. (seen when displaying cl(position))
      // const latitude = position.coords.latitude // OR destructure it
      const { latitude } = position.coords;
      const { longitude } = position.coords;
      console.log(`https://www.google.com/maps/@${latitude},${longitude} `);
      // just copied a link from google and pasted here, and replaced the latitude value and longitude values with variables. https://www.google.com/maps/@33.148216,73.6719532,12z

      // Remember: latitude and longitude are used together as a coordinate pair to specify a location on the earth.
    },
    function () {
      alert('Could not get your position');
    }
  );
}
// actual code niche (only code. no comments)
```

---

## DISPLAYING_MAP_ON_PAGE

Displaying Map on page using third party library called **'leaflet'** library  
leafletjs.com/download.html. from here copied these codes and paste in html head, before our own script.

```html
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.3/dist/leaflet.css"
  integrity="sha256-kLaT2GOSpHechhsozzB+flnD+zUyjE2LlfWPgU04xyI="
  crossorigin=""
/>
<script
  src="https://unpkg.com/leaflet@1.9.3/dist/leaflet.js"
  integrity="sha256-WBkoXOwTeyKclOHuWtc+i2uENFpDZ9YPdf5Hf+D7ewM="
  crossorigin=""
></script>
```

Remember: we should never put any javascript in the header without any of the **differ** or **async** attributes. so alway use **differ** or **async** when adding JavaScript in head.  
And then go overview page and copy v simple code from there and pase in callback function when a browser is successful getting the coordinates. like this, and modify the code according to our need.  
Remember, whenever we pass a string into a map function, must be the **id name of our html element**, that element where map will be displayed.  
Also we will put our latitude and longitude in **setView method** and also in **marker method**, and second parameter of **setView** method is **zoom** in/out level.

**Now lets talk about tileLayer:** Map we see is a basically made up of tiles, they come from this URL here which is in tileLayer function. we can change look of map by changing in it.

```js
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(
    function (position) {
      console.log(position);
      const { latitude } = position.coords;
      const { longitude } = position.coords;
      console.log(`https://www.google.com/maps/@${latitude},${longitude} `);

      const coords = [latitude, longitude];

      const map = L.map('map').setView(coords, 13); // in map function string should be the html element where we want to display map.

      L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(map);

      L.marker(coords)
        .addTo(map)
        .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
        .openPopup();
    },
    function () {
      alert('Could not get your position');
    }
  );
}
```

---

### DISPLAYING_A_MAP_MARKER

**Display a marker wherever we click on the map**. For that we are gonna use one more time, leaflet library.  
First we will add an **event handler on map**, If we attach the eventListener to whole map, then it will not work (-no way to find where user is clicked on the map. -can't find GPS coordinates or clicked area) As a result we can't use addEventListener simply, that we are doing all the time, **But Instead** we use something similar, from a leaflet library,

Here on map variable we add **'on' method**. Remember that, this 'on' method is not coming from javascript itself, instead coming from the leaflet library.

```js
if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(
    function (position) {
      console.log(position);
      const { latitude } = position.coords;
      const { longitude } = position.coords;
      console.log(`https://www.google.com/maps/@${latitude},${longitude}`);

      const coords = [latitude, longitude];

      const map = L.map('map').setView(coords, 13); // in map function string should be the id of html element where we want to display map.

      L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(map);

      // it will create a marker (default)
      // L.marker(coords).addTo(map)
      //     .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
      //     .openPopup();

      // console.log(map); // lot of things.
      // 'on' method is available of leaflet library, not on js
      map.on('click', function (mapEvent) {
        console.log(mapEvent); // here we see latitude and longitude.

        const { lat, lng } = mapEvent.latlng;

        // it will create marker
        L.marker([lat, lng])
          .addTo(map)
          .bindPopup(
            L.popup({
              maxWidth: 250,
              minWidth: 100,
              autoClose: false,
              closeOnClick: false,
              className: 'running-popup',
            })
          )
          .setPopupContent('Workout ')
          .openPopup();

        // here we we wants to show custom popup on every marker. So instead of just passing a string in bindPopup method,
        // We will pass a popup method which is available in leaflet library, in there we pass an object. all of properties are available popup's object. see documentation.
      });
    },
    function () {
      alert('Could not get your position');
    }
  );
}
```

**REMEMBER:** Every library/package has own documentation, always read that documentation to use their features. this is very essential

---

### Rendering_Workout_Input_Form

In this we Implementing, **whenever click on map**, **form should display on page.** and **cursor** should focus on distance input field.  
Then will add event handler on form for submitting it, and wherever form submitted, the marker should appear on map. (no button here for submit, we simply wants to submit, whenever click enter key)

```js
let map, mapEvent;

if (navigator.geolocation) {
  navigator.geolocation.getCurrentPosition(
    function (position) {
      console.log(position);
      const { latitude } = position.coords;
      const { longitude } = position.coords;
      console.log(`https://www.google.com/maps/@${latitude},${longitude}`);

      const coords = [latitude, longitude];

      map = L.map('map').setView(coords, 13);

      L.tileLayer('<https://tile.openstreetmap.org/{z}/{x}/{y}.png>', {
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }).addTo(map);

      // Handling clicks on map.
      map.on('click', function (mapE) {
        mapEvent = mapE;
        form.classList.remove('hidden');
        inputDistance.focus();

        // shifted to form event handler fun
        console.log(mapEvent); // here we see latitude and longitude.

        const { lat, lng } = mapEvent.latlng;

        // it will create marker
        L.marker([lat, lng])
          .addTo(map)
          .bindPopup(
            L.popup({
              maxWidth: 250,
              minWidth: 100,
              autoClose: false,
              closeOnClick: false,
              className: 'running-popup',
            })
          )
          .setPopupContent('Workout ')
          .openPopup();
      });
    },
    function () {
      alert('Could not get your position');
    }
  );
}

form.addEventListener('submit', function (e) {
  e.preventDefault();

  // clearing input fields
  inputDistance.value =
    inputDuration.value =
    inputCadence.value =
    inputElevation.value =
      '';

  console.log(mapEvent); // here we see latitude and longitude.

  const { lat, lng } = mapEvent.latlng;
  // it will create marker
  L.marker([lat, lng])
    .addTo(map)
    .bindPopup(
      L.popup({
        maxWidth: 250,
        minWidth: 100,
        autoClose: false,
        closeOnClick: false,
        className: 'running-popup',
      })
    )
    .setPopupContent('Workout ')
    .openPopup();
  // here we have two problem, we are trying to access two variables which are not in scope(map and mapEvent). we'll declare it in global.
});
```

According to type field cadence field should change (steps/min and meters). to do that we use change event, what will change.

---

## Project_Architecture_With_Full_Code

**Architecture means giving project a structure.** We use **Object Oriented Programming (OOP)** with classes here.

### Parent Class

```js
class Workout {
  data = new Date();

  clicks = 0;

  // unique identifier for each object.
  id = (Date.now() + '').slice(-10);

  constructor(coords, distance, duration) {
    this.coords = coords; // [lat, lng]
    this.distance = distance; // in km
    this.duration = duration; // in min
  }

  _setDescription() {
    // prettier-ignore
    const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

    this.description = `${this.type[0].toUpperCase()}${this.type.slice(1)} on ${
      months[this.data.getMonth()]
    } ${this.data.getDate()}`;
  }

  click() {
    this.clicks++;
  }
}
```

### Child Class #1

```js
class Running extends Workout {
  type = 'running';
  constructor(coords, distance, duration, cadence) {
    super(coords, distance, duration);
    this.cadence = cadence;

    // Calling Method
    this.calcPace();
    this._setDescription();
  }

  calcPace() {
    // min/km
    this.pace = this.duration / this.distance;
    return this.pace;
  }
}
```

### Child Class #2

```js
class Cycling extends Workout {
  type = 'cycling';
  constructor(coords, distance, duration, elevationGain) {
    super(coords, distance, duration);
    this.elevationGain = elevationGain;

    this.calcSpeed();
    this._setDescription();
  }

  calcSpeed() {
    // km/h
    this.speed = this.distance / this.duration / 60;
    return this.speed;
  }
}
```

#### Testing

```js
const run1 = new Running([39, -12], 5.2, 24, 178);
const Cycling1 = new Cycling([39, -12], 27, 95, 543);
console.log(run1, Cycling1);
```

---

```js
const form = document.querySelector('.form');
const containerWorkouts = document.querySelector('.workouts');
const inputType = document.querySelector('.form__input--type');
const inputDistance = document.querySelector('.form__input--distance');
const inputDuration = document.querySelector('.form__input--duration');
const inputCadence = document.querySelector('.form__input--cadence');
const inputElevation = document.querySelector('.form__input--elevation');
```

```js
class App {
  #map;
  #mapEvent;
  #workout = [];
  #mapZoomLevel = 13;

  constructor() {
    // Get users position
    this._getPosition();

    // get data from local storage.
    this._getLocalStorage();

    // Attach Event Handlers
    form.addEventListener('submit', this._newWorkout.bind(this));

    inputType.addEventListener('change', this._toggleElevationField);

    containerWorkouts.addEventListener('click', this._movToPopup.bind(this));
  }

  _getPosition() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        this._loadMap.bind(this),
        function () {
          alert('Could not get your position');
        }
      );
    }
  }

  _loadMap(position) {
    const { latitude } = position.coords;
    const { longitude } = position.coords;
    console.log(`https://www.google.com/maps/@${latitude},${longitude} `);

    const coords = [latitude, longitude];

    this.#map = L.map('map').setView(coords, this.#mapZoomLevel);

    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(this.#map);

    // Handling clicks on map.
    this.#map.on('click', this._showForm.bind(this));

    this.#workout.forEach((work) => {
      this._renderWorkoutMarker(work);
    });
  }

  _showForm(mapE) {
    this.#mapEvent = mapE;
    form.classList.remove('hidden');
    inputDistance.focus();
  }

  hideForm() {
    // Empty input fields
    inputDistance.value =
      inputDuration.value =
      inputCadence.value =
      inputElevation.value =
        '';

    // hide form
    form.style.display = 'none';
    form.classList.add('hidden');
    setTimeout(() => (form.style.display = 'grid'), 1000);
  }

  _toggleElevationField() {
    inputElevation.closest('.form__row').classList.toggle('form__row--hidden');

    inputCadence.closest('.form__row').classList.toggle('form__row--hidden');
  }

  _newWorkout(e) {
    // small helper function to check input validations
    const validInputs = (...inputs) =>
      inputs.every((inp) => Number.isFinite(inp));
    const allPositive = (...inputs) => inputs.every((inp) => inp > 0);

    e.preventDefault();

    // lec #12

    // Get data from the form
    const type = inputType.value;
    const distance = +inputDistance.value; // + => convert str to num
    const duration = +inputDuration.value;

    const { lat, lng } = this.#mapEvent.latlng;
    let workout;

    // if workout running, then create running object
    if (type === 'running') {
      const cadence = +inputCadence.value;

      // Check if data is valid
      if (
        // !Number.isFinite(distance) || !Number.isFinite(duration) || !Number.isFinite(cadence)
        // refactored in validInput function

        !validInputs(distance, duration, cadence) ||
        !allPositive(distance, duration, cadence)
      )
        return alert('Input have to be positive number');

      workout = new Running([lat, lng], distance, duration, cadence);
    }

    // if cycling, create cycling object
    if (type === 'cycling') {
      const elevation = +inputElevation.value;

      if (
        !validInputs(distance, duration, elevation) ||
        !allPositive(distance, duration)
      )
        return alert('Input have to be positive number');

      workout = new Cycling([lat, lng], distance, duration, elevation);
    }

    // Add new object to workout object
    this.#workout.push(workout);

    // Render workout on map as marker
    this._renderWorkoutMarker(workout);

    // it will create marker

    // Render workout on list
    this._renderWorkout(workout);

    // Hide form and clear input fields.
    this.hideForm();

    // console.log(mapEvent); // here we see latitude and longitude.

    // Set local Storage to all workouts.

    this._setLocalStorage();
  }

  _renderWorkoutMarker(workout) {
    L.marker(workout.coords)
      .addTo(this.#map)
      .bindPopup(
        L.popup({
          maxWidth: 250,
          minWidth: 100,
          autoClose: false,
          closeOnClick: false,
          className: `${workout.type}-popup`,
        })
      )
      .setPopupContent(
        `${workout.type === 'running' ? '🏃‍♂️' : '🚴🏻‍♂️'} ${workout.description}`
      )
      .openPopup();
  }

  _renderWorkout(workout) {
    let html = `
        <li class="workout              workout--${
          workout.type
        }"                 data-id=${workout.id}>
            <h2 class="workout__title">${workout.description}</h2>
            <div class="workout__details">
                <span class="workout__icon">${
                  workout.type === 'running' ? '🏃‍♂️' : '🚴🏻‍♂️'
                }</span>
                <span class="workout__value">${workout.distance}</span>
                <span class="workout__unit">km</span>
            </div>
            <div class="workout__details">
                <span class="workout__icon">⏱</span>
                <span class="workout__value">${workout.duration}</span>
                <span class="workout__unit">min</span>
            </div>
        `;
    if (workout.type === 'running') {
      html += `<div class="workout__details">
            <span class="workout__icon">⚡️</span>
            <span class="workout__value">${workout.pace.toFixed(1)}</span>
            <span class="workout__unit">min/km</span>
          </div>
          <div class="workout__details">
            <span class="workout__icon">🦶🏼</span>
            <span class="workout__value">${workout.cadence}</span>
            <span class="workout__unit">spm</span>
          </div>
        </li >`;
    }

    if (workout.type === 'cycling') {
      html += `<div class="workout__details">
            <span class="workout__icon">⚡️</span>
            <span class="workout__value">${workout.speed.toFixed(1)}</span>
            <span class="workout__unit">km/h</span>
          </div>
          <div class="workout__details">
            <span class="workout__icon">⛰</span>
            <span class="workout__value">${workout.elevationGain}</span>
            <span class="workout__unit">m</span>
          </div>
        </li>`;
    }

    form.insertAdjacentHTML('afterend', html);
  }

  _movToPopup(e) {
    const workoutEl = e.target.closest('.workout');
    // console.log(workoutEl);

    if (!workoutEl) return;

    const workout = this.#workout.find(
      (work) => work.id === workoutEl.dataset.id
    );
    // console.log(workout);

    this.#map.setView(workout.coords, this.#mapZoomLevel, {
      animate: true,
      pan: {
        duration: 1,
      },
    }); // this is a method of leaflet.

    // workout.click();
  }

  _setLocalStorage() {
    localStorage.setItem('workout', JSON.stringify(this.#workout));
  }
  // In localStorage the data will store in key value pair.
  // In this the first argument is key/ or name and the second argument need to a string that we want to a store associated with that key.
  // So, we will convert object to string and then pass to this setItem function.
  // To convert into string, we use JSON.stringify(), here we pass our object.

  _getLocalStorage() {
    const data = JSON.parse(localStorage.getItem('workout'));
    console.log(data);

    if (!data) return;

    this.#workout = data;

    this.#workout.forEach((work) => {
      this._renderWorkout(work);
    });
  }

  reset() {
    localStorage.removeItem('workout');

    // to reload automatically
    location.reload();
  }
}

const app = new App();
// app._getPosition();
```

Remember that, constructor methods always executed whenever any object is created for that class, not need of call init. So here we wants to get position whenever page lode, hence we put this \_getPosition in constructor.  
we can replace app.\_getPosition() to this.\_getPosition() and put in constructor.

---

## Storing_Data_in_LocalStorage

**In localStorage the data will store in key value pair**.  
In this the **first argument is key** OR name and the **second argument need to a string** that we want to a store associated with that key.  
So, we will convert object to string and then pass to this setItem function.  
**To convert into string, we use JSON.stringify()**, here we pass our object.

**Working with localStorage.** Using local storage API to make workout data persist across multiple page reloads.

**localStorage is basically a place in the browser**, where we can store data that will stay there even after close the page. So **basically the data will linked to the url**.

Whenever user create a new workout it should store to local storage. and then whenever tha pase is load all the workouts from the local storage, are render them on the map and also on the list.
