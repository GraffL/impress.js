impress.js with substeps!
============

It's a presentation framework based on the power of CSS3 transforms and
transitions in modern browsers and inspired by the idea behind prezi.com.

This includes a fork of [impress.js][1] that adds sub-step capability.  For documentation on everything else about impress.js, read that [documentation][1].

You can see an example of this code in action here: http://tehfoo.github.io/impress.js

## Using substeps
This is pretty easy.  There are two parts, the HTML part and the CSS part.

### Getting Started
Just like [impress.js][1], you really only need the `impress.js` file.  You can learn all about using `impesss.js` by viewing the source of the main [index.html](https://github.com/tehfoo/impress.js/blob/master/index.html) file.  This just builds on what is already there, and doesn't break anything... I think ;)

### In your HTML
This is pretty simple. In an element marked with the `step` class, you can add the `substep` class to whatever elements you want to be a substep.  On init, the  modified `impress.js` script will find all elements with class `substep`, and mark them with the class `future`.  This is just like regular steps.  

As you navigate forward or backward through your steps, any substep found will be treated as a navigation step.  While viewing an element with substeps, navigation forward finds the next substep,  swapping `future` for `present` and `active`.  When a moving forward from a substep, the `present` substep has `present` removed, and the class `past` is added.  This is much like the behavior for steps.  If there are no more substeps ahead, the navigation moves to the next step.

An important key difference between regular steps and substeps is that substeps keep their `active` class when they are marked as `past`.  This is different from regular steps, which have the `active` class removed when they are marked as `past`.  I might make this behavior configurable in the future. 

The reverse is applied when navigating backward; if there are substeps they last one will get focus, which adds removes `past` and adds `present`.  Continue backward, and both `present` and `active` classes are removed, while the class `future` is set.  If there is another substep, that gets focus and is treated as above.  If there are no more substeps, backward navigation moves to the previous step.

To get all this magic on, all you have to do is set the `substep` class on whatever you want to have act as a substep.  Of course, this has to be withing a `step` enabled element.

Example

    <div id="introduction" class="step" data-x="0" data-y="0">
      <h1 class="line">Can Haz Substep?</h1>
        <ul>
          <li class="substep">Sure!</li>
          <li class="substep">Why Not?</li>
          <li class="substep">Just Add the Substep Class</li>
          <li class="substep">And Amaze Friends</li>
          <li class="substep">With Substep Goodness</li>
        </ul>
      </div>

That will get you substep elements with CSS classes that change as you navigate.  Now you need to style them.

### In your CSS
To put the substeps to work, you need to style them.  They don't really do anything by default. You might be thinking ['Y U NO HIDE AS DEFAULT?'](#y-u-no-hide-as-default). Ask me that later. This is easy to make work with some CSS. 

A simple (and possibly ['Powerpointish'](https://github.com/bartaz/impress.js/issues/81#issuecomment-3700541)) behavior is to set the substep `opacity: 0` by default, and then transition them to `opacity: 1` when they are `active`.

Example

    .impress-enabled .substep {
      opacity: 0;
      -webkit-transition: all 1s;
      -moz-transition:    all 1s;
      -ms-transition:     all 1s;
      -o-transition:      all 1s;
      transition:         all 1s;  
    }

    .impress-enabled .substep.active {
      opacity: 1;
    }

    .impress-enabled .substep.present { 
      color: rgb(100, 135, 195);
    }

This would style the HTML example above to have substeps hidden by default, become visible when a substep becomes `active`, and get colored when the substep becomes `present`.  All the property changes get a 1 second transition.

### More Help
If you need more help, try viewing source on the [substep.html](https://github.com/tehfoo/impress.js/blob/master/substep.html) example file, and looking at the [substep.css](https://github.com/tehfoo/impress.js/blob/master/css/substep.css) as well.


See this exact code working here: http://tehfoo.github.io/impress.js

### Y U NO HIDE AS DEFAULT?
You may notice there is no default behavior to hide substeps, and reveal them when they get focus.  That's because I wanted to mimic the default `impress.js` behavior for steps.  By default, all steps are visible; they may be out of the viewport, but they are visible.  If you want steps to act otherwise, you need to style them.  Same applies for substeps.  You could pretty easily fork this and hack in default hiding if you really want it. 


### Y U NO EVENTS?
I haven't added "impress:substep" events dispatching yet.  I actually have a day job. :)

If you've found a bug you can [open an issue]
(http://github.com/tehfoo/impress.js/issues/new). Better yet, fork this and fix it yourself, or even just fix something from the [issues list](https://github.com/tehfoo/impress.js/issues).

### SUPPORT
This is a largely unsupported fork of impress.  

If you have fixed a bug or implemented a feature that you'd like to share, send your pull request against [dev branch]
(http://github.com/tehfoo/impress.js/tree/dev).  I'll do the best I can.  Impress might even have a thriving community somewhere, I'll look into it and see what the deal is.


ABOUT THE NAME
----------------

impress.js name in [courtesy of @skuzniak](http://twitter.com/skuzniak/status/143627215165333504).

It's an (un)fortunate coincidence that a Open/LibreOffice presentation tool is called Impress ;)


BROWSER SUPPORT
-----------------

### TL;DR;

Currently impress.js works fine in latest Chrome/Chromium browser, Safari 5.1 and Firefox 10.
With addition of some HTML5 polyfills (see below for details) it should work in Internet Explorer 10, 11 and Edge.
It doesn't work in Opera, as it doesn't support CSS 3D transforms.

If you find impress.js working on other browsers, feel free to tell us and we'll update this documentation.

As a presentation tool it was not developed with mobile browsers in mind, but some tablets are good
enough to run it, so it should work quite well on iPad (iOS 5, or iOS 4 with HTML5 polyfills) and
Blackberry Playbook. Inform us of any bug and we will try to fix this.

### Still interested? Read more...

Additionally for the animations to run smoothly it's required to have hardware
acceleration support in your browser. This depends on the browser, your operating
system and even kind of graphic hardware you have in your machine.

For browsers not supporting CSS3 3D transforms impress.js adds `impress-not-supported`
class on `#impress` element, so fallback styles can be applied to make all the content accessible.


### Even more explanation and technical stuff

Let's put this straight -- wide browser support was (and is) not on top of my priority list for
impress.js. It's built on top of fresh technologies that just start to appear in the browsers
and I'd like to rather look forward and develop for the future than being slowed down by the past.

But it's not "hard-coded" for any particular browser or engine. If any browser in future will
support features required to run impress.js, it will just begin to work there without changes in
the code.

From technical point of view all the positioning of presentation elements in 3D requires CSS 3D
transforms support. Transitions between presentation steps are based on CSS transitions.
So these two features are required by impress.js to display presentation correctly.

Unfortunately the support for CSS 3D transforms and transitions is not enough for animations to
run smoothly. If the browser doesn't support hardware acceleration or the graphic card is not
good enough the transitions will be laggy.

Additionally the code of impress.js relies on APIs proposed in HTML5 specification, including
`classList` and `dataset` APIs. If they are not available in the browser, impress.js will not work.

Fortunately, as these are JavaScript APIs there are polyfill libraries that patch older browsers
with these APIs.

For example IE10 is said to support CSS 3D transforms and transitions, but it doesn't have `dataset`
APIs implemented at the moment. So including polyfill libraries *should* help IE10 with running
impress.js.


### And few more details about mobile support

Mobile browsers are currently not supported. Even Android browsers that support CSS 3D transforms are
forced into fallback view at this point.

Fortunately some tablets seem to have good enough hardware support and browsers to handle it.
Currently impress.js presentations should work on iPad and Blackberry Playbook.

In theory iPhone should also be able to run it (as it runs the same software as iPad), but I haven't
found a good way to handle its small screen.

Also note that iOS supports `classList` and `dataset` APIs starting with version 5, so iOS 4.X and older
requires polyfills to work.

Copyright 2011-2016 Bartek Szopka - Released under the MIT [License](LICENSE)

LICENSE
---------
Released under the MIT and GPL (version 2 or later) Licenses.


[1]: https://github.com/bartaz/impress.js
