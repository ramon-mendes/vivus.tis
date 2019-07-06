# vivus.tis

Vivus.tis is a port of JavaScript **[Vivus.js](https://github.com/maxwellito/vivus)** library for **[Sciter/TIScript](http://sciter.com/)**. See the original library to know more about what it does.

Goal is to have this API exactly the same as vivus.js, so you can grab any existent JS/SVG sample and have it working in Sciter just by copy/pasting. However, there are still some adaptations needed in such copied code, mainly adding the lengths of every SVG `<path>` (more details below).

This way, just search for any Vivus.js sample and you will be amazed with what you can achieve with SVG stroke animations: [Codepen](http://codepen.io/search/pens?q=vivus&limit=all&type=type-pens)

# Samples

![](https://github.com/ramon-mendes/vivus.tis/blob/master/samples/gifs/index.gif?raw=true)

![](https://github.com/ramon-mendes/vivus.tis/blob/master/samples/gifs/cheese.gif?raw=true)
![](https://github.com/ramon-mendes/vivus.tis/blob/master/samples/gifs/lettering.gif?raw=true)

![](https://github.com/ramon-mendes/vivus.tis/blob/master/samples/gifs/stamps.gif?raw=true)

# Usage

```html
<script type="text/tiscript">
  new Vivus(self#my-svg, { duration: 200 }, myCallback);
</script>

<svg id="my-svg">
  <path length="100" ...>
  <path length="150" ...>
  <path length="180" ...>
</svg>
```

## Constructor

The Vivus constructor asks for 3 parameters:

- Element instance or CSS/string selector of the DOM SVG element to interact with.<br/>
  It can be an inline SVG or a wrapper element to append an object tag from the option `file`
- Option object (described in the following table) (optional)
- Callback to call at the end of the animation (optional)

## Option list

Options are the same as in vivus.js, however I removed some of them because they are really not necessary in Sciter.

| Name       | Type     | Description |
|------------|----------|-------------|
|`type`      | string   | Defines what kind of animation will be used: `delayed`, `async`, `oneByOne`, `script`, `scenario` or `scenario-sync`. [Default: `delayed`] |
|`file`      | string   | Link to the SVG to animate. If set, Vivus will create an object tag and append it to the DOM element given to the constructor. Be careful, use the `onReady` callback before playing with the Vivus instance. |
|`start`     | string   | Defines how to trigger the animation (`inViewport` once the SVG is in the viewport, `manual` gives you the freedom to call draw method to start, `autostart` makes it start right now). [Default: `inViewport`] |
|`duration`  | integer  | Animation duration, in frames. [Default: `200`] |
|`delay`     | integer  | Time between the drawing of first and last path, in frames (only for `delayed` animations). |
|`pathTimingFunction` | function | Timing animation function for each path element of the SVG. Check the [timing function part](#timing-function). |
|`animTimingFunction` | function | Timing animation function for the complete SVG. Check the [timing function part](#timing-function). |
|`dashGap`   | integer  | Whitespace extra margin between dashes. Increase it in case of glitches at the initial state of the animation. [Default: `2`] |
|<s>`onReady`</s>   | N/A | N/A |
|<s>`forceRender`</s> | N/A | N/A |

## Methods

Methods table is 99% equal to vivus.js one. Unlike JS, in TIScript integers and floats are distinct types, so you must respect the parameter type in each method.

| Name          | Description         |
|---------------|---------------------|
| `play(speed:Float)` | Plays the animation with the speed given in parameter. This value can be negative to go backward, between 0 and 1 to go slowly, or superior to 1 to go fast. [Default: `1.0`] |
| `stop()`      | Stops the animation. |
| `reset()`     | Reinitialises the SVG to the original state: undrawn. |
| `finish()`    | Set the SVG to the final state: drawn. |
| `setFrameProgress(progress:Float)` | Set the progress of the animation. Progress must be a Float number between 0 and 1. |
| `getFrameProgress()` | Get the progress of the animation. |
| `getStatus() : Symbol` | Get the status of the animation between `#start`, `#progress`, `#end` |
| `destroy()`   | Reset the SVG but make the instance out of order. |

These methods return the Vivus instance so you can chain the actions.

```js
var myVivus = new Vivus("#my-svg-id");
myVivus
  .stop()
  .reset()
  .play(2.0)
```

# Adapting JS/SVG code from Vivus.js

Sciter currently doesn't supports calculating `<path>`'s length, so you need to tell in your SVG markup the length of each `<path>` by adding a `length="123"` attribute.

The good news is that I've made a .html 'script' page to automate this process, which you will find in `calc-length-script/calc.html`. It uses a standard browser (tested in Chrome and Firefox) to calculate the length of each path and gives you the output SVG markup.

A problem with this method: while testing, I noticed that sometimes the returned length is greater than the actual length. So the browser is returning the wrong measurement, or in Sciter, when you scale your SVG, the length gets incorrect. Anyway, you may have to manually tweak the value.

# REFERENCE / TODO 

- http://surbhioberoi.com/a-complete-guide-to-svg/
- http://gionkunz.github.io/chartist-js/
- https://github.com/maxwellito/triangulr
- https://github.com/lmgonzalves/segment