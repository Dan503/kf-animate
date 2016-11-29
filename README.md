# kf-animate
A powerful sass mixin for generating keyframe css animations

##Contents

* [Installation](#installation)
* [Basic Usage](#basic-usage)
* [Auto generated keyframes](#auto-generated-keyframes)
* [Animation settings](#animation-settings)
* [Predefined keyframe animations](#predefined-keyframe-animations)
* [Pre-built animation plugins](#pre-built-animation-plugins)
  * [bob](#bob)
  * [pulse](#pulse)
  * [radar](#radar)
  * [spin](#spin)

##Installation

```````
npm install kf-animate --save-dev
```````

Then in your main scss file (making sure the path is correct)

``````scss
@import '../node_modules/kf-animate/kf-animate';
``````

I also recommend using [Autoprefixer](https://github.com/postcss/autoprefixer) in combination with this mixin for optimal browser support.

##Basic Usage

Here is an example of how to do a simple animation of something fading in and out continuously (also the text is changing color).

`````````````SCSS
//A simple SASS keyframe animation
.element {
    $fadeInOut:
        0% (opacity: 0, color: red),
        80% (opacity: 1, color: blue),
        100% (opacity: 0, color: red),
    ;
    @include kf-animate(fadeInOut, $fadeInOut);
}
```````````````````````
```````````````````````CSS
/*CSS output*/
@keyframes fadeInOut {
    0% { opacity: 0; color: red; }
    80% { opacity: 1; color: blue; }
    100% { opacity: 0; color: red; }
}
.element {
    animation: fadeInOut 1s infinite linear both;
}
```````````````````````

##Auto generated keyframes

It can get really tedious having to figure out what all the keyframe percentages need to be, especially if you just want them all to be evenly spaced and there's like 8 of them or something.

Well guess what! kf-animate is able to take away all that pain and figure out the key frames for you! As long as you are happy with the keyframes all being evenly spaced, this is a perfectly fine way of writting your SASS animation code:

`````````````SCSS
//Let kf-animate calculate the keyframes for you!

.element {
    $fadeInOut:
        (opacity: 0, color: red),
        (opacity: 1, color: blue),
        (opacity: 0, color: red),
    ;
    @include kf-animate(fadeInOut, $fadeInOut);
}
```````````````````````
```````````````````````CSS
/*CSS output*/
@keyframes fadeInOut {
    0% { opacity: 0; color: red; }
    50% { opacity: 1; color: blue; }
    100% { opacity: 0; color: red; }
}
.element {
    animation: fadeInOut 1s infinite linear both;
}
```````````````````````

##Animation settings

If you want to maybe have a slow fade in while the background changes colors, you can do this:

`````````````SCSS
//only animate the attributes you want to

.element {
    $fadeInOut:
        (background: red, opacity: 0),
        (background: blue),
        (background: green),
        (background: yellow),
        (background: orange),
        (background: grey),
        (background: black),
        (background: white, opacity: 1),
    ;
    @include kf-animate(fadeInOut, $fadeInOut, 1s, 1);
}
```````````````````````

If you are wondering what that `1` at the end is, it's the number of loops the animation will play for. Most of the time you will want this to be either `1` or `infinite`, it defaults to infinite.

This is the order that the kf-animate attributes go in and their default settings:

`````````````SCSS
@include kf-animate($name, $keyframes, $timing: 1s, $loops: infinite, $ease: linear, $fill: both)
```````````````````````

`$timing` is used for both duration and delay. Duration is always the first value and if you add a second value to the `$timing` variable it will be the delay.

If you are wondering what the `$fill` variable is, it's the `animation-fill-mode` property. If you're still confused, have a read of this excellent article: [Understanding the CSS animation-fill-mode Property](http://www.sitepoint.com/understanding-css-animation-fill-mode-property/). You shouldn't need to worry about this setting too much though. The default should work well in 99% of circumstances.

##Predefined keyframe animations

Ok, now for another scenario. What if we want to apply the same effect to a range of different elements, possibly even with different timings? If we used the kf-animate mixin to do this, yes it would work but we'd also have a whole heap of duplicated css in our output file. What we really want to be able to do is state the keyframes once but refer back to it multiple times with different timings. This is when the `kf-definition` and `kf-predefined` mixins come in handy.

`````````````SCSS
//define a set of keyframes and then refer back to it multiple times with different timings

.parent {
    &__child {
        $fadeInOut:
            (opacity: 0),
            (opacity: 1),
            (opacity: 0),
        ;
        @include kf-definition(fadeInOut, $fadeInOut);
        &--anim1 {
            @include kf-predefined(fadeInOut, 1s);
        }
        &--anim2 {
            //anim2 starts the same animation 0.5s after anim1
            @include kf-predefined(fadeInOut, 1s 0.5s);
        }
    }
}
```````````````````````
```````````````````````CSS
/*CSS output*/
@keyframes fadeInOut {
    0% { opacity: 0; }
    50% { opacity: 1; }
    100% { opacity: 0; }
}
.parent__child--anim1 {
    animation: fadeInOut 1s infinite linear both;
}
.parent__child--anim2 {
    animation: fadeInOut 1s 0.5s infinite linear both;
}
```````````````````````

The variables for `kf-definition` are simply the animation name, followed by the animation set.

The variables for `kf-predefined` are the same as `kf-animate` except without the `$keyframes` variable.

`````````````SCSS
//The variables

@include kf-definition($name, $keyframes);

@include kf-predefined($name, $timing: 1s, $loops: infinite, $ease: linear, $fill: both);
```````````````````````

##Pre-built animation plugins

To use one of these plugins, you need to load them in seperately after the core kf-animate mixin

The plugin install snippets have this format

``````````scss
//100% optional default settings
$pluginDefault-setting: value;

//required imports for plugin
@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/plugin.scss';
``````````

###bob

``````````scss
$bobDefault-distance: 3px;
$bobDefault-direction: right;
$bobDefault-timing: 1s;
$bobDefault-loops: infinite;
$bobDefault-axis: X;

@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/bob.scss';
``````````

###pulse

``````````scss
$pulseDefault-stop1: 30%;
$pulseDefault-stop2: 50%;
$pulseDefault-timing: 1s;
$pulseDefault-scale: 1.1;
$pulseDefault-name: pulse;
$pulseDefault-loops: infinite;

@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/pulse.scss';
``````````


###radar

``````````scss
$radarDefault-scale: 1.4;
$radarDefault-delay: 50%;
$radarDefault-timing: 2s;
$radarDefault-name: radar;
$radarDefault-loops: infinite;

@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/bob.scss';
``````````

###rumble

``````````scss
$rumbleDefault-rotation: 5deg;
$rumbleDefault-timing: 0.5s;
$rumbleDefault-name: rumble;
$rumbleDefault-loops: infinite;

@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/radar.scss';
``````````

###spin

``````````scss
$spinDefault-direction: clockwise;
$spinDefault-timing: 1s;
$spinDefault-loops: infinite;

@import '../node_modules/kf-animate/kf-animate';
@import '../node_modules/kf-animate/plugins/spin.scss';
``````````
