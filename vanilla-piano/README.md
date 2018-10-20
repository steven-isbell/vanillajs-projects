# VanillaJS Piano

# Project Summary

The goal of this project is to build a functional piano that can be played using your computers keys. You don't need to worry about setting up the piano, the keys, or the sounds, all of that has been provided for you. Your goal will be making the piano work when their respective keys are pressed.

## Step 1

### Summary

Begin by examining the exisiting code. You'll notice that our piano is a Scalable Vector Graphic (SVG); you can find more information on SVG's <a href="https://www.w3schools.com/graphics/svg_intro.asp">here</a>.

### Instructions

* Open `./index.html`.
* Locate the opening and closing `svg` tags.
    * Each polygon in our SVG represents a key, either black or white.
    * Each key has either a `white` or `black` class and a `pianoKey` class for ease of working with the elements.
* Locate the audio tags.
    * audio tags are hidden tags that allow us to play audio in our page.
    * Each audio tag has a src that tells it what audio to play.
    * We'll use javascript to tell it when to play.
    * For more information on using audio tags, read <a href="https://www.w3schools.com/tags/ref_av_dom.asp">this</a>.
* Each polygon (pianoKey) has an associated audio tag.
    * These are tied together by the data-key property.
    * data-key is a custom html attribute that allows us to store data to HTML elements. You can find more on them <a href="https://www.w3schools.com/tags/att_global_data.asp">here</a>.
    * You can access the data-key attribute by targeting the element using `querySelector` and accessing the `data-key` property using bracket notation.
        <details>
        <summary><code> Example </code></summary>
        ```js
            // Here we're accessing the audio element with a data-key property of 65

            const element = document.querySelector('audio[data-key="65"]');
        ```
        </details>
_You now know everything you need to get this piano working!_
## Step 2

### Summary

In this step, we'll setup our application by declaring a new event listener and assigning the functionality. We want to listen for any keypress that occurs anywhere on our page. For this, we need to attach an event to the window.

### Instructions

* Open `index.html`.
* Locate the opening and closing `script` tags.
* In between the `script tags` declare a new event listener on our window object and provide it two arguments, the event to listen for and the handler to fire once that event has occured.
    * Add a single argument to the handler that will represent our `event`.
    * The `event` argument is implicitly passed with information about the keypress event that occurred.
    * One piece of information that will be passed along will be the `keyCode` of the key that was pressed.
    * This `keyCode` will correlate with the `data-key` attribute assigned to our poylgons and audio tags.

### Solution

<details>

<summary> <code> ./index.html </code> </summary>

```js
window.addEventListener('keypress', (e) => {

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
