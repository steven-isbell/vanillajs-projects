# VanillaJS Typeahead

    const selected = [];
    const search = document.querySelector('input');
    const list = document.getElementById('list');

    

    function filterText() {
        if (!this.value) {
            list.innerHTML = "";
            list.style.display = 'none';
            return;
        }
        const filtered = characters
            .filter(val => val.name.toLowerCase().includes(this.value.toLowerCase()) && selected.findIndex(val => val.toLowerCase().includes(this.value.toLowerCase())) === -1)
            .map(val => `<li onclick="handleSelect(this.innerText)">${val.name}</li>`);
        if (filtered.length) {
            list.style.display = 'block';
            render(filtered, list)
        }
    }

    function handleSelect(e) {
        const selectedContainer = document.querySelector('.selected-container');
        const search = document.querySelector('input');
        const chosenOne = characters.filter(val => val.name === e)[0];
        selected.push(`<div class="card-container"><h2>${chosenOne.name}</h2></div>`)
        search.value = '';
        list.style.display = 'none';
        render(selected, selectedContainer);
    }

    function render(data, container) {
        const html = data.join('');
        container.innerHTML = html;
    }

    search.addEventListener('keyup', filterText);




    <img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250" align="right">

# Project Summary

The goal of this project is to build a typeahead similar to the Twitter Typeahead found <a href="https://twitter.github.io/typeahead.js/">here.</a> We'll cover how to access DOM nodes, how to make AJAX requests, and how to dynamically add HTML, all without using any additional libraries.

## Step 1

### Summary

To begin, we need data to search through. While there are many ways to interact with the data in a typeahead, we're going to load the data into the client and filter through it there using the fetch API.

### Instructions

* Open `./index.html`.
* Locate the opening and closing `script` tags.
* Between the script tags, declare a variable called characters and set its value to an empty array.
* Using `fetch` make a request to the SWAPI API at `/api/people`s and store the resulting data in the people array.
  * Usage of the fetch API can be found <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch">here</a>
  * Usage of the SWAPI API can be found <a href="https://www.swapi.co">here</a>

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
const characters = [];

fetch('https://www.swapi.co/api/people')
    .then(response => response.json())
    .then(response => characters.push(...response.results))

```

</details>

## Step 2

### Summary

In this step, we'll make use of `axios` to get the `Increase Price` and `Decrease Price` buttons to work. When modifying/updating data on a server you always use a PUT request.

### Instructions

* Open `./src/App.js`.
* Locate the pre-made `updatePrice` method.
* Using `axios` and the API documentation make a PUT request to either increase or decrease the price.
  * If the request is successful, use `this.setState()` to update the value of `vehiclesToDisplay` and use `toast.success`.
    * Hint: Inspect the returned data object.
  * If the request is unsuccessful, use `toast.error`.

### Solution

<details>

<summary> <code> ./src/App.js ( updatePrice method ) </code> </summary>

```js
updatePrice( priceChange, id ) {
  axios.put(`https://joes-autos.herokuapp.com/api/vehicles/${ id }/${ priceChange }`).then( results => {
    toast.success("Successfully updated price.");
    this.setState({ 'vehiclesToDisplay': results.data.vehicles });
  }).catch( () => toast.error("Failed at updating price") );
}
```

</details>

## Step 3

### Summary

In this step, we'll make use of `axios` to get the `Add vehicle` button to work. When creating new data on a server you should always use a POST request.

### Instructions

* Open `./src/App.js`.
* Locate the pre-made `addCar` method.
* Using `axios` and the API documentation make a POST request to create a new vehicle.
  * If the request is successful, use `this.setState()` to update the value of `vehiclesToDisplay` and use `toast.success`.
    * Hint: Inspect the returned data object.
  * If the request is unsuccessful, use `toast.error`.

### Solution

<details>

<summary> <code> ./src/App.js ( addCar method ) </code> </summary>

```js
addCar() {
  let newCar = {
    make: this.make.value,
    model: this.model.value,
    color: this.color.value,
    year: this.year.value,
    price: this.price.value
  };

  axios.post('https://joes-autos.herokuapp.com/api/vehicles', newCar).then( results => {
    toast.success("Successfully added vehicle.");
    this.setState({ vehiclesToDisplay: results.data.vehicles });
  }).catch( () => toast.error('Failed at adding new vehicle.') );
}
```

</details>

## Step 4

### Summary

In this step, we'll make use of `axios` to get the `SOLD!` button to work. When deleting data on a server you should always use a DELETE request.

### Instructions

* Open `./src/App.js`.
* Locate the pre-made `sellCar` method.
* Using `axios` and the API documentation make a DELETE request to delete ( "sell" ) a vehicle.
  * If the request is successful, use `this.setState()` to update the value of `vehiclesToDisplay` and use `toast.success`.
    * Hint: Inspect the returned data object.
  * If the request is unsuccessful, use `toast.error`.

### Solution

<details>

<summary> <code> ./src/App.js ( sellCar method ) </code> </summary>

```js
sellCar( id ) {
  axios.delete(`https://joes-autos.herokuapp.com/api/vehicles/${ id }`).then( results => {
    toast.success("Successfully sold car.");
    this.setState({ 'vehiclesToDisplay': results.data.vehicles });
  }).catch( () => toast.error("Failed at selling car.") );
}
```

</details>

## Black Diamond

If there is extra time during the lecture, try to complete the remaining methods. The remaining methods can also be used as `axios` and `CRUD` practice on your own time.

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.

## Copyright

© DevMountain LLC, 2017. Unauthorized use and/or duplication of this material without express and written permission from DevMountain, LLC is strictly prohibited. Excerpts and links may be used, provided that full and clear credit is given to DevMountain with appropriate and specific direction to the original content.

<p align="center">
<img src="https://s3.amazonaws.com/devmountain/readme-logo.png" width="250">
</p>
