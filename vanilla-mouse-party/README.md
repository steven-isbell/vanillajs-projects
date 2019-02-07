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
* We now know, programatically, the location of our mouse. Now we can add items to follow it!

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

In this step we'll take advantage of the built-in `setInterval` (more on that <a href="https://www.w3schools.com/jsref/met_win_setinterval.asp">here</a>) method to handle the creation and destruction of our dots on the page. We'll build our dots, randomize the size and color, and animate it so it disappears.

### Instructions

* Open `index.html`.
* Beneath our `mousemove` event listener.
    * Create a new function called draw.
    * It should return a `setInterval`.
    * `setInterval` accepts two arguments, a callback and an interval in milliseconds for how often we'll invoke the callback.
        * The interval we use will determine how quickly our dots are added to the page.
            * Feel free to play around with this number after everything is working to see it's effects.
        * In the callback in `setInterval`:
            * We have a `div` with an id of `wrap`. We'll want all of our dots to live in that container.
                * Create a variable called `container` whose value is our `wrap` element.
                * Add the following code beneath your new variable. This is the code that will style our dots. Feel free to examine it.
                    ```js
                        const color = `background:rgb(${getRandomNumber(0, 255)},${getRandomNumber(0, 255)}, ${getRandomNumber(0, 255)});`;
                        const ballSize = getRandomNumber(10, 30);
                        const size = `height:${ballSize}px; width:${ballSize}px;`;
                        const left = `left:${getRandomNumber(mousePosition.x - ballSize, mousePosition.x)}px;`;
                        const top = `top:${getRandomNumber(mousePosition.y - ballSize, mousePosition.y)}px; `;
                        const style = `${left}${top}${color}${size}`;
                    ```
                * Next, we need to add dots and apply our styles from above each time our callback is executed.
                    * Create a new `div` element and store it to a variable called `ball`.
                    * Add the class `ball` to our `ball` element using it's `classList`.
                    * Set the `ball` elements style to the `style` variable from above.
                * Finally, we need to add the new `ball` element to our `container` element and remove it once it's animation is complete.
                    * Add an `animationend` event listener to our `ball` element.
                    * The callback function should invoke `this.remove()` to remove the element once it's animation is complete. (remember, using an arrow function will alter the context.)
                    * Append the `ball` element to the `container` element.

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

    const draw = () => setInterval(() => {
        const container = document.getElementById('wrap');
        const color = `background:rgb(${getRandomNumber(0, 255)},${getRandomNumber(0, 255)}, ${getRandomNumber(0, 255)});`;
        const ballSize = getRandomNumber(10, 30);
        const size = `height:${ballSize}px; width:${ballSize}px;`;
        const left = `left:${getRandomNumber(mousePosition.x - ballSize, mousePosition.x)}px;`;
        const top = `top:${getRandomNumber(mousePosition.y - ballSize, mousePosition.y)}px; `;
        const style = `${left}${top}${color}${size}`;

        const ball = document.createElement('div');
        ball.classList.add('ball');
        ball.style = style;

        ball.addEventListener("animationend", function () { this.remove() })

        container.appendChild(ball);
    }, 1);
```

</details>

## Step 4

### Summary

In this step we'll start our interval by creating an event listener that will listen for our mouse to move into the browser window.

### Instructions

* Open `index.html`.
* Beneath our `draw` variable:
    * Create a new event listener on the window that will listen for a `mouseover` event.
    * The callback should invoke our `draw` function and store the resulting id to our `drawId` variable.
    * We'll use the `drawId` to cancel the interval anytime our mouse moves out of the window.
    * Add a final event listener on the window that will listen for a `mouseout` event.
    * The callback should call the built in `clearInterval` method.
        * `clearInterval` accepts a single argument, the ID of the interval we wish to stop.
        * Pass our `drawId` into the `clearInterval` invocation.
* When this is done, you should see a group of randomly colored dots following your mouse as it moves across the browser window! Feel free to play around with the random number and function and setInterval timer to see what changes you can make happen.

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

    const draw = () => setInterval(() => {
        const container = document.getElementById('wrap');
        const color = `background:rgb(${getRandomNumber(0, 255)},${getRandomNumber(0, 255)}, ${getRandomNumber(0, 255)});`;
        const ballSize = getRandomNumber(10, 30);
        const size = `height:${ballSize}px; width:${ballSize}px;`;
        const left = `left:${getRandomNumber(mousePosition.x - ballSize, mousePosition.x)}px;`;
        const top = `top:${getRandomNumber(mousePosition.y - ballSize, mousePosition.y)}px; `;
        const style = `${left}${top}${color}${size}`;

        const ball = document.createElement('div');
        ball.classList.add('ball');
        ball.style = style;

        ball.addEventListener("animationend", function () { this.remove() })

        container.appendChild(ball);
    }, 1);

    window.addEventListener('mouseover', () => drawId = draw());
    window.addEventListener('mouseout', () => clearInterval(drawId));

```

</details>

## Contributions

If you see a problem or a typo, please fork, make the necessary changes, and create a pull request so we can review your changes and merge them into the master repo and branch.
