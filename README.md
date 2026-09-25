# snapchat-tinkerguide
# Snapchat Web Tinkering
<p align="center">
  <img src="header.png" width="900">
</p>

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
Experiment 1: True Black Theme

Snapchat's dark theme isn't completely black.

Snapchat Web uses CSS variables to control many of its colors. Some useful variables include:

--sigMain
--sigSurface
--sigBackgroundPrimary
--sigBackgroundSecondary
--sigTextPrimary
--sigTextSecondary

You can override several of them to create a much darker theme.

:root {
    --sigMain: #000000 !important;
    --sigSurface: #000000 !important;

    --sigTextPrimary: #ffffff !important;
    --sigTextSecondary: #ffffff !important;
    --sigTextTertiary: #ffffff !important;
    --sigTextPlaceholder: #ffffff !important;

    --sigBackgroundPrimary: #000000 !important;
    --sigBackgroundSecondary: #000000 !important;

    --sigBackgroundFeedHover: #111111 !important;
    --sigBackgroundMessageHover: #111111 !important;
    --sigBackgroundMessageSaved: #111111 !important;
}

html,
body {
    background: #000000 !important;
    color: #ffffff !important;
}

The exact result can depend on which Snapchat theme is currently active.

Experiment 2: Change the Friends Feed

One of the useful discoveries from inspecting Snapchat Web was that the Friends area uses --sigSurface.

Try:

document.documentElement.style.setProperty(
    "--sigSurface",
    "red",
    "important"
);

If the relevant UI is using that variable, its surface will change to red.

You can replace red with another CSS color or a HEX value.

For example:

document.documentElement.style.setProperty(
    "--sigSurface",
    "#000000",
    "important"
);
Experiment 3: Solid Camera Background

The camera background uses an image element.

The element found during testing was:

document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

First, hide the original image:

const img = document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

img.style.opacity = "0";

Then change its parent's background:

img.parentElement.style.background = "#000000";

You can replace #000000 with any color.

For example:

img.parentElement.style.background = "#ff0000";
Experiment 4: Custom Camera Background

You can also use an image stored on your computer.

This version opens a file picker.

const selector =
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img";

const input = document.createElement("input");
input.type = "file";
input.accept = "image/*";

input.onchange = () => {
    const file = input.files[0];
    if (!file) return;

    const reader = new FileReader();

    reader.onload = () => {
        const img = document.querySelector(selector);
        if (!img) return;

        img.style.opacity = "0";

        const parent = img.parentElement;

        parent.style.backgroundImage = `url("${reader.result}")`;
        parent.style.backgroundSize = "cover";
        parent.style.backgroundPosition = "center";
        parent.style.backgroundRepeat = "no-repeat";
    };

    reader.readAsDataURL(file);
};

input.click();

After running it, choose an image from your computer.

GIFs can also be selected because the file picker accepts images.

Experiment 5: Persistent Camera Background

There is one problem with the previous experiment.

If you leave the camera and come back, Snapchat may recreate the camera elements.

Because the old <img> element was destroyed, the modification disappears.

A MutationObserver can watch the page for changes and reapply the background.

const selector =
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img";

const input = document.createElement("input");
input.type = "file";
input.accept = "image/*";

input.onchange = () => {
    const file = input.files[0];
    if (!file) return;

    const reader = new FileReader();

    reader.onload = () => {
        const imageData = reader.result;

        const applyBackground = () => {
            const img = document.querySelector(selector);
            if (!img) return;

            img.style.opacity = "0";

            const parent = img.parentElement;

            parent.style.backgroundImage =
                `url("${imageData}")`;

            parent.style.backgroundSize = "cover";
            parent.style.backgroundPosition = "center";
            parent.style.backgroundRepeat = "no-repeat";
        };

        applyBackground();

        new MutationObserver(applyBackground).observe(
            document.body,
            {
                childList: true,
                subtree: true
            }
        );
    };

    reader.readAsDataURL(file);
};

input.click();

This allows the custom background to be reapplied when Snapchat recreates the camera UI.

How the Camera Experiment Works

The camera background is an actual <img> element.

The selector:

const img = document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

finds that element.

Instead of replacing the image, the experiment:

Finds the image.
Makes the image invisible.
Finds its parent element.
Gives the parent a new background.
Uses background-size: cover so the replacement fills the area.

This was useful because directly changing the image's src didn't produce the desired result during testing, while modifying the parent background did.

Using Your Own Colors

CSS colors can be written in several ways.

Named color:

red

RGB:

rgb(255, 0, 0)

HEX:

#ff0000

Some example HEX colors:

Black       #000000
White       #ffffff
Dark Gray   #202020
Navy        #101a3a
Purple      #6c2bd9
Pink        #ff4fa3
Red         #e53935
Orange      #ff7a00
Yellow      #ffd600
Green       #20c878
Cyan        #00cfe8
Blue        #2979ff

HEX values use six characters:

#RRGGBB

The first two control red, the next two green, and the last two blue.

Why External Images May Not Work

During testing, attempting to use an external image URL produced:

Blocked by client

This means the request was blocked somewhere on the client side, such as the browser or a privacy/security filter.

Using a local image with FileReader avoids making an external request for the image.

The file is converted into a data URL and then used as the background.

Data URL Experiment

You can also create a background entirely inside the browser without selecting a file.

For example, this creates a black SVG background:

const img = document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

img.style.opacity = "0";

img.parentElement.style.backgroundImage =
    "url(\"data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='1000' height='1000'%3E%3Crect width='1000' height='1000' fill='%23000000'/%3E%3C/svg%3E\")";

img.parentElement.style.backgroundSize = "cover";

You can make more complicated SVG backgrounds by changing the SVG inside the data URL.

A Simple Test

If you're unsure whether you've found the correct element, you can temporarily make it obvious.

For example:

const img = document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

img.style.filter = "brightness(0)";

If the camera background changes, you've found the correct image element.

Another useful test:

const img = document.querySelector(
    "#root > div.Fpg8t > div.Vbjsg.WJjwl > div > div > div > img"
);

img.style.opacity = "0";
img.parentElement.style.background = "red";

If the background becomes red, you've found the correct parent layer too.

Resetting Everything

Most of these experiments are temporary.

The easiest way to undo them is simply:

Refresh the Snapchat Web page.

If Snapchat recreates the relevant elements, temporary modifications may also disappear automatically.

Things to Investigate

More experiments could include:

Finding more --sig* CSS variables
Changing chat backgrounds
Changing message bubbles
Changing hover effects
Finding more camera elements
Experimenting with Bitmoji UI
Finding shorter and more reliable selectors
Testing other React elements
Creating a complete custom theme
Documenting newly discovered variables
