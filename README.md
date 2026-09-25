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
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);
```

The image itself can be hidden while its parent element is given a different background.

This makes it possible to experiment with:

* Solid colors
* Custom colors
* Local images
* GIFs
* Data URLs

The custom image experiment uses a file picker and `FileReader`, meaning you can select an image directly from your computer without needing to upload it to a website.

## Why don't external image URLs always work?

During testing, external background images sometimes produced:

```text
Blocked by client
```

This means the browser or something running in the browser blocked the request. Using a local file converted into a data URL avoids needing to request the image from an external website.

## Why do some modifications disappear?

Snapchat Web uses React, so parts of the page can be destroyed and recreated when navigating around the site.

If you directly modify an element and Snapchat later recreates that element, your modification disappears with the old element.

A `MutationObserver` can watch for those changes and reapply the modification when the element appears again.

## Ideas to experiment with

Some things I'd like to investigate:

* More Snapchat CSS variables
* Different UI colors
* Chat background customization
* Message bubble customization
* Hover effects
* Bitmoji backgrounds
* Other camera elements
* Custom animations
* Finding cleaner selectors
* Making modifications survive more UI changes

## Disclaimer

This is an unofficial collection of browser-side experiments and is not affiliated with Snapchat.

Snapchat may change its website at any time, so some experiments may become outdated or stop working.

