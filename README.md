# snapchat-tinkerguide
# Snapchat Web Tinkering

A collection of client-side CSS and JavaScript experiments for customizing and experimenting with the Snapchat Web interface.

This repository documents things I've discovered while poking around Snapchat Web's HTML, CSS variables, and UI elements using browser DevTools.

## What can you do with these snippets?

* Make Snapchat Web's interface truly black
* Change the Friends Feed background
* Change camera backgrounds
* Use your own images or GIFs as camera backgrounds
* Experiment with Snapchat's CSS variables
* Learn how Snapchat Web's UI is structured
* Make your own client-side themes and modifications

## Important

These are **client-side browser modifications**.

They only change what is rendered in your own browser. They do not modify Snapchat's servers, other users' interfaces, or Snapchat's stored data.

Snapchat's website is constantly changing, so selectors, CSS variables, and other parts of these snippets may stop working after an update.

Refreshing the page will usually undo DevTools changes.

## How to use the snippets

1. Open [Snapchat Web](https://web.snapchat.com/).
2. Open your browser's Developer Tools.
3. Go to the **Console** tab for JavaScript snippets.
4. Paste a snippet into the console.
5. Press Enter.
6. Return to Snapchat and see what changed.

For CSS experiments, you can use the **Elements** tab and inspect the site's styles and variables.

## Repository contents

### `snippets/`

Contains individual CSS and JavaScript experiments.

Examples:

* `true-black-theme.css`
* `friends-feed-red.css`
* `camera-background.js`
* `camera-custom-image.js`

### `examples/`

Contains explanations and examples of things you can experiment with.

## How these were discovered

A lot of these modifications came from inspecting Snapchat Web with DevTools and testing individual elements and CSS variables.

For example, Snapchat Web uses variables such as:

```css
--sigMain
--sigSurface
--sigBackgroundPrimary
--sigBackgroundSecondary
--sigTextPrimary
```

Changing one variable can affect a surprisingly large part of the interface.

For example:

```js
document.documentElement.style.setProperty(
    "--sigSurface",
    "#000000",
    "important"
);
```

This can change surfaces that use `--sigSurface`.

## Camera background experiments

The camera background is rendered using an image element. Its current element can be found with:

```js
document.querySelector(
    "#root > div.Fpg8
```
