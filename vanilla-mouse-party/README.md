# VanillaJS Mouse Party

# Project Summary

The goal of this project is to build a collection of randomly appearing and disappearing dots that will follow your mouse movement. They will have random colors and sizes. When we're finished, we'll have the appearance of color popping balls following your mouse arround the screen.
## Step 1

### Summary

Let's begin by setting up a couple of environment variables. These will be variables or functions that we need to make the rest of the application work.

### Instructions

* Open `./index.html`.
* Locate the opening and closing `script` tags.
    * We'll need a way to keep track of our mouse position.
        * create a variable called `mousePosition` and set it equal to an object.
        * Provide it a `x` and a `y` property, both initialized as 0.
    * Next, add an undefined variable called `drawId`.
        * We'll use this variable to keep track of when the dots should be on or off the screen.
    * We'll also need a function to handle the randomizing of the color, positioning, and size of our dots.
        * Add this code beneath the `drawId` variable created above.
            ```js
                const getRandomNumber = (min, max) => Math.round(Math.random() * (max - min + 1)) + min;
            ```

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
const mousePosition = {};
let drawId;

const getRandomNumber = (min, max) => Math.round(Math.random() * (max - min + 1)) + min;

```

</details>

## Step 2

### Summary

In this step, we'll add a way for our application to `listen` for the mouse to move. We'll then capture the mouse position each time the mouse is moved so we know where to place our dots.

### Instructions

* Open `index.html`.
* Beneath our `getRandomNumber` function, add a new event listener.
    * The listener should be attached to the window.
    * It should listen for the `mousemove` event.
        * Attaching to the `window` and listening for `mousemove` will allow us to fire a callback function anytime the mouse is moved in the browser window.
    * Add a callback function to the listener.
        * The callback should receive a single argument that represents the `event` object.
        * The `event` object will contain information regarding the `mousemove` event, specifically the coordinates of our mouse.
        * Update the x and y values of our `mousePosition` object with the `pageX` and `pageY` respectively, of the `event`.
        * Feel free to console log the event values to see what happens when you move the mouse.
* We now know, programatically the location of our mouse. Now we can add items to follow it!

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
const mousePosition = {};
    let drawId;

    const getRandomNumber = (min, max) => Math.round(Math.random() * (max - min + 1)) + min;

    window.addEventListener('mousemove', e => {
        mousePosition.x = e.pageX;
        mousePosition.y = e.pageY;
    });

```

</details>

## Step 3

### Summary

In this step we'll get access to the visual key that was pressed (the unique polygon) and it's associated audio tag. As mentioned before, they share a unique `data-key` attribute.

### Instructions

* Open `index.html`.
* Within the handler of our event listener, add 2 variables.
    * One that references the `tone` or the audio tag that we want to play.
    * One that references the `pianoKey` or the polygon that should be active when the key is pressed.
    * You can access the `tone` and `pianoKey` using a selector and our `keyCode` as mentioned above.
* Add a conditon to check if our tone is `falsy`.
    * If it is, `return`.
    * This will trim down on the amount of work our application needs to do when an invalid key is pressed.

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
window.addEventListener('keypress', (e) => {
    const tone = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    if (!tone) return;
    const pianoKey = document.querySelector(`.pianoKey[data-key="${e.keyCode}"]`);
});
```

</details>

## Step 4

### Summary

In this step we'll get our piano to play a note when we press one of the valid keys seen at the top of the page.

### Instructions

* Open `index.html`.
* Within the handler of our event listener invoke the `play` method on our tone audio element.
* You should now be able to click one of the valid keys and hear the tone played.
    * But OH NO! When we press the key consecutively, it doesn't restart the tone, but instead allows the current tone to finish playing.
    * To fix this, set the `currentTime` of the tone to 0 *before* we play the tone.
        * Now, when a key is pressed, it will stop the current tone and start the fresh one!

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
window.addEventListener('keypress', (e) => {
    const tone = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    if (!tone) return;
    const pianoKey = document.querySelector(`.pianoKey[data-key="${e.keyCode}"]`);
    tone.currentTime = 0;
    tone.play();
});
```

</details>

## Step 5

### Summary

In this step we'll add an animation to make our keys work when their associated keyboard key is pressed. We'll do this by taking advantage of dynamically adding and removing classes using the element's `classList` property. You can find more on that <a href="https://www.w3schools.com/jsref/prop_element_classlist.asp">here</a>.

### Instructions

* Open `index.html`.
* Within the handler of our event listener, before `play` and after setting the `currentTime` to 0, add the class `pressed` to our `pianoKey` element.
    * If you try it now, you'll notice that each time a key on the keyboard is pressed, its associated pianoKey is changed; however, it never changes back when the key is no longer active!
    * After our tone plays, remove the class `pressed` from our `pianoKey` element.
    * You'll now notice that it appears that our key never receives the class.
        * This occurs because the class is being added an removed almost instantly.
        * To slow this down, wrap the class removal in a setTimeout, to be removed after 300ms.
    * You should now have keys that appear `pressed` when a note is played.
* You should also have a fully functional piano that plays notes and gives the appearance of pressed keys. Feel free to remap the keys using the `data-key` attribute and the associated `keyCode` from each key on the keyboard.
### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
window.addEventListener('keypress', (e) => {
    const tone = document.querySelector(`audio[data-key="${e.keyCode}"]`);
    if (!tone) return;
    const pianoKey = document.querySelector(`.pianoKey[data-key="${e.keyCode}"]`);
    tone.currentTime = 0;
    pianoKey.classList.add('pressed');
    tone.play();
    setTimeout(() => pianoKey.classList.remove('pressed'), 300);
});
```

</details>

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.
