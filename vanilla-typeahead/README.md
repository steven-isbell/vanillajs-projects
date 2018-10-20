# VanillaJS Typeahead

# Project Summary

The goal of this project is to build a typeahead similar to the Twitter Typeahead found <a href="https://twitter.github.io/typeahead.js/">here.</a> We'll cover how to access DOM nodes, how to make AJAX requests, and how to dynamically add HTML, all without using any additional libraries.

## Step 1

### Summary

To begin, we need data to search through. While there are many ways to interact with the data in a typeahead, we're going to load the data into the client and filter through it there using the fetch API.

### Instructions

* Open `./index.html`.
* Locate the opening and closing `script` tags.
* We'll write all of our JavaScript between these tags.
* Between the script tags, declare a variable called characters and set its value to an empty array.
* Using `fetch` make a request to the SWAPI API at `/api/people`s and store the resulting data in the people array.
  * Usage of the fetch API can be found <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch">here.</a>
  * Usage of the SWAPI API can be found <a href="https://www.swapi.co">here.</a>

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

In this step, we'll make our data appear on the page. To dynamically add content to the view we need to target DOM nodes. DOM nodes are object representations of our HTML structure. There are several ways to access DOM Nodes including: `getElementById`, `getElementsByClassName`, `querySelector`, and `querySelectorAll`. You can find a summary of those methods <a href="https://www.digitalocean.com/community/tutorials/how-to-access-elements-in-the-dom">here.</a>

### Instructions

* Open `index.html`.
* Add another variable beneath our `characters` variable called `list` who's value is equal to the unordered list with an id of list in the `body` of our HTML.
  * We can now use this node as if it were a JavaScript object. This gives us access to all of its properties such as `innerHTML`, `event handlers`, `style`, etc.
  * Go ahead and `console.dir` our list variable to see the available properties in your browsers console.
* Now that we have access to the `ul`, let's render some `li` tags to show our character names.
  * Write a function called `render`.
  * In `render`, let's build some html.
    * Declare a variable called `html`.
    * Set `html` equal to the result of mapping over our characters array and returning a template string that contains an opening and closing `li` tag, then joining the returned strings.
    * Between the `li` tags, interpolate (${}) the name of our character.
    * Your code should look like this.
      <details>
      <summary><code> html </code></summary>
      ```js
        function render() {
          const html = characters.map(val => `<li>${val.name}</li>`).join('')
        }
      ```
      </details>
    * We can now render that content to the DOM by targeting the `innerHTML` of our `list` and setting its value to our html.
    * Invoke render in the `.then` after we push our characters into our array.

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
const characters = [];
const list = document.getElementById('list');

fetch('https://www.swapi.co/api/people')
    .then(response => response.json())
    .then(response => {
      characters.push(...response.results);
      render();
    })

function render() {
  const html = characters.map(val => `<li>${val.name}</li>`).join('');
  list.innerHTML = html;
}

```

</details>

## Step 3

### Summary

We can now see data on the view, but it's not very dynamic. Let's get our typeahead working.

### Instructions

* Open `index.html`.
* Declare a variable called `search` beneath our `list` variable who's value is our `input` element.
* Now that we have access to the input, we need a way to access the value we type into it.
  * Write a function called `filterText`.
    * When we attach this method to our input, we'll gain access to a property called `this.value` where `this` is bound to our input. So, `this.value` would be the value of our keypress.
    * In our `filterText` function, declare a variable called filtered.
      * First we want to filter through our `characters` array to search each name for the character that has been typed in. (Hint: Normalize the characters by making both the value and term lowercase.)
      * We can then grab our map from the previous step and chain it to our filter to build the HTML we want to show. (Remove the join method, we'll use that later.)
    * Next, add a condtional to check that our `filtered` has at least one value.
      * If it does, invoke the render method passing in our `filtered` array.
    * Adjust the `render` method so that it now receives one argument, an array.
      * Our `html` variable should join the passed in array.
      * Remove the `render` invocation from the `.then` above.
    * Finally, we need to utilize our `filterText` method.
      * We'll add an `eventListener` that will tell our input to listen for an event to occur.
      * For more information on available events, look <a href="https://developer.mozilla.org/en-US/docs/Web/Events">here.</a>
      * At the bottom of our `script` add a `keyup eventListener` to our `search` Node that will execute our `filterText` method.
        * When we type into our typeahead, on each `keyup` our `filterText` method will be reinvoked.

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
const characters = [];
const list = document.getElementById('list');
const search = document.querySelector('input');

fetch('https://www.swapi.co/api/people')
    .then(response => response.json())
    .then(response => characters.push(...response.results))

function filterText() {
    const filtered = characters
        .filter(val => val.name.toLowerCase().includes(this.value.toLowerCase()))
        .map(val => `<li>${val.name}</li>`)

    if (filtered.length) {
        render(filtered)
    }
}

function render(array) {
    const html = array.join('');
    list.innerHTML = html;
}

search.addEventListener('keyup', filterText);


```

</details>

## Black Diamond

There are a few ways we can improve our typeahead.
  1. On page load the `list` is empty and shows just the border. Hide this and only show it when there is HTML to be rendered in the `list`.
  2. If there is no value in the input, set the `list` Node's display to `none`, and change it to `block` when a value is present.
  3. You should also clear the innerHTML of the list if the entry no longer matches a value from the characters array.
  4. Create a container that will hold `selected` elements. So that when you click on a name, it adds that name to another list.
  5. Once you can add the name, make it so you can only add the name once.

  A completed and fully functional version can be found on the soultion branch.

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.
