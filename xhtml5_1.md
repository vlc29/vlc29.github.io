# VLC 2.9 XHTML5 Implementation Guide

VLC 2.9 Foundation XHTML5 (aka XHTML5 or VLCXHTML5) is an extended HTML5 standard using **XML syntax** while keeping all HTML5 functionality. Its purpose is to **maximize readability and flexibility for humans and machines and improve code quality**. This is not the final standard or polyfill and is under **active development**.

---

## 1. Basic Setup

Use the proper **XML declaration** and **DOCTYPE**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en">
<head>
    <title>My XHTML5 Page</title>
    <meta charset="UTF-8" />
    <meta http-equiv="Content-Type" content="application/xhtml+xml; charset=UTF-8" />
</head>
<body>
    <!-- Your content here -->
</body>
</html>
```
Alternately, this is another proposed doctype:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE xhtml5>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en">
<head>
    <title>My XHTML5 Page</title>
    <meta charset="UTF-8" />
    <meta http-equiv="Content-Type" content="application/xhtml+xml; charset=UTF-8" />
</head>
<body>
    <!-- Your content here -->
</body>
</html>
```
Alternately, this is yet another proposed doctype:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE vlcxhtml5>
<html xmlns="http://www.w3.org/1999/xhtml" lang="en">
<head>
    <title>My XHTML5 Page</title>
    <meta charset="UTF-8" />
    <meta http-equiv="Content-Type" content="application/xhtml+xml; charset=UTF-8" />
</head>
<body>
    <!-- Your content here -->
</body>
</html>
```

* All elements must be **properly closed** (`<br />`, `<img />`, `<audio />`, etc.).
* Attributes must be **quoted**.
* All HTML5 elements are supported, just use proper XML/XHTML syntax.

---

## 2. CSS and Media Types

* Use **`type="text/css"`** for `<style>` blocks.
* New media types supported by XHTML5:

```css
@media glass {
    /* AR / transparent overlay displays */
}

@media xr {
    /* VR / XR headsets */
}

@media round {
    /* Circular displays */
}

@media handheld {
    /* Low-power devices / engines (not polyfilled) */
}
```

* Self-contained CSS for readability and compatibility is recommended.
* Polyfill automatically patches CSS for unsupported media queries.

---

## 3. Audio / Video

### Self-closing tags

```xml
<audio controls="controls" alt="Audio was not found" src="https://www.nyan.cat/music/dub.mp3" />
<video controls="controls" alt="Video was not found" src="https://www.example.com/video.mp4" />
```

* Self-closing tags work like `<img />`.
* Embed should also work like this as well, but is not in the polyfill currently and therefore does not
* Saves space in code when only one source exists
* In the future, this functionality could be applied to **all elements** allowing **any** element to be a self-closing tag, even ones like `<canvas>` which are normally not.
* Also this would allow things like `alt=` to be used if the audio/video/etc could not be rendered, allowing the element and error text to be on a single line.
* **`<onerror>` is not supported** on self-closing tags; use container tags for fallback.

### Container tags with `<onerror>`

```xml
<audio controls="controls">
    <source src="https://www.nyan.cat/music/dub.mp3" type="audio/mpeg" />
    <onerror>
        Your browser cannot play this audio.
    </onerror>
</audio>

<video controls="controls">
    <source src="https://www.example.com/video.mp4" type="video/mp4" />
    <onerror>
        Your browser cannot play this video.
    </onerror>
</video>
```

* `<onerror>` wraps fallback content for unsupported media.
* Polyfill moves any non-`<source>`/`<onerror>` children **outside** the element to prevent rendering issues.

---

## 4. Embed & Object

XHTML5 resurrects `<object>` and allows `<embed>`:

```xml
<embed src="file.svg" type="image/svg+xml" />
<embed src="file.svg" type="image/svg+xml"></embed>
<object data="file.svg" type="image/svg+xml"></object>
```

* Proposal: `<img>`, `<embed>`, and `<object>` could also support `<source>` and `<onerror>`.
* Keeps XML compliance while embedding SVG, PDF, or other content.

---

## 5. JavaScript
### `<script>` self closing tags
```
<script src="https://www.example.com/example.js" type="text/javascript" />
```

* Faster declaration of JavaScript 

### `elt()` — Element Builder

```javascript
var node = document.elt("div", { class: "box" },
    "Score: ", 42,
    document.elt("span", { style: "color:blue;" }, " points")
);
```

* Supports strings, numbers, arrays (recursively), and DOM nodes.
* Safely constructs XML-style DOM nodes.

### `document.q` and `document.qs`

* `document.q(selector)`: alias for `document.querySelector()`
* `document.qs(selector)`: alias for `document.querySelectorAll()`

### Function Syntax

* **Arrow functions** (`() => {}`) are discouraged for readability.
* Use traditional function declarations:

```javascript
function example() {
    console.log("Readable syntax preferred");
}
```
* Or for anonymous function declarations:

```javascript
var thisfunction = function () {
    console.log("Readable syntax preferred");
}
```

---

## 6. Polyfill

XHTML5 is not implemented yet, so for testing, include the polyfill in your page:

```html
<script type="text/javascript" src="https://vlc29.neocities.org/xhtml5.js"></script>
```

* Sets `document.xhtml5 = true` if not already present.
* Implements:

  * `elt()`
  * `q()` and `qs()`
  * Patches `<audio>` and `<video>` container elements for fallback handling
  * Fixes CSS for new media types (`glass`, `xr`, `round`)
* Logs actions for debugging and ensures the page does not lag.

---

## 7. Summary

* XHTML5 = **HTML5 + XML syntax + extended functionality**
* Self-closing audio/video tags simplify embedding, like `<img />`
* `<onerror>` provides fallback content for media
* Embed and object elements are fully XML-compliant
* CSS supports **glass, XR, round, handheld**
* Polyfill ensures compatibility on unsupported engines
* Marquee is standard (supported in all major browsers)
* Focus is on **readability and machine-friendliness**

---

**Polyfill used:** [xhtml5.js](https://vlc29.neocities.org/xhtml5.js)
**Demo:** [xhtml5](https://vlc29.neocities.org/xhtml5)
