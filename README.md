![Deloitte Digital](docs/deloittedigital-logo-white.png)

# DDBreakpoints
Breakpoints SCSS Mixin and JavaScript library, used to accelerate and simplify media query development during the development process of responsive pages.

The SCSS and JS also allow for the ability to create static (non-responsive) stylesheets as well by setting a variable allowing backwards support for non responsive browsers (like IE8) easily.

## Getting Started

### Install via Bower

To install via the [bower](http://twitter.github.com/bower/) package repository, enter the following at the command line:

```
bower install ddbreakpoints
```

### SCSS

Import the SCSS into your own project

```scss
@import 'dd-breakpoints';
```

#### Usage

At the most basic level, everything comes from a single mixin:

```scss
@include bp($min, $max:0, $property:width) {
    // your styles here
}
```

The recommended usage for the mixin is to go mobile first:

```scss
.module {
    // base styles

    @include bp(m) {
        // medium styles
    }
    
    @include bp(l) {
        // large styles
    }

    @include bp(xl) {
        // extra large styles
        // not included in the static sheet
    }
}
```

But if you have to, you can go large first too:

```scss
.module {
    // desktop styles

    @include bp(0, l) {
        // large and below styles
    }

    @include bp(0, m) {
        // medium and below styles
        // not included in the static sheet
    }

    @include bp(0, s) {
        // small and below styles
        // not included in the static sheet
    }
}
```

You can even use pixel based widths mixed with breakpoint names.

```scss
.module {
    // base styles

    @include bp(300, m) {
        // between 300px (in ems) and medium breakpoint
        // not included in the static sheet
    }

    @include bp(m, 2000) {
        // between medium breakpoint and 2000px (in ems)
    }

    @include bp(200, 250) {
        // be as specific as you need
        // not included in the static sheet
    }
}
```

And you can also check against heights too

```scss
.module {
    // base styles

    @include bp(0, 500, height) {
        // between 0 and 500px high
        // height breakpoints are never included in the static sheet
    }

    @include bph(0, 500) {
        // exactly the same as above - shortcut
    }
}
```

#### Options

You can customise a number of options in the SCSS. When doing this, if you're also using the JS library, make sure you update the values to match there as well.

##### Flags

Set these flags early in the document, they can be included after you include the breakpoint scss file, however should be set before any usage of the mixin.

```scss
$IS_RESPONSIVE: true; // [boolean] tells the mixin to either export media queries or not
$FONT_BASE: 16; // [number] base font size (in px) of your site
```

##### Breakpoints

The default breakpoints can be updated simply by editing the following variables. These should be set *before* the scss mixin is included into the page.

These default values have been chosen because they are the most common screen resolutions that we normally support.

```scss
$bp-xxs-min: 359; 
$bp-xs-min: 480; 
$bp-s-min: 640;
$bp-m-min: 768; // iPad portrait
$bp-l-min: 1024; // iPad landscape
$bp-xl-min: 1244; // 1280px screen resolution minus scrollbars
$bp-xxl-min: 1410; // 1440px screen resolution minus scrollbars
```

You can also completely customise your list by setting the following:

```scss
// customised - max numbers are the next breakpoints min minus 1px
$bp-list-min: small 359, medium 768, large 1024, xlarge 1244;
$bp-list-max: small 767, medium 1023, large 1243;
```

You don't need to set a maximum of the highest breakpoint.

### JavaScript

There are two main functions for the JS library.

#### `.get()`

Returns the media query as a string. Perfect for use with [enquire.js](http://wicky.nillia.ms/enquire.js/).

You can pass through the same values as the SCSS, however you can also pass through a single string of comma separated values which can be passed through dynamically like from `data-` attributes.

```javascript
DD.bp.get(min /* string || number */, max = 0 /* string || number */, property = 'width' /* string */);

// examples
DD.bp.get('s');
DD.bp.get('s', 'l');
DD.bp.get(0, 500);

// string notation
DD.bp.get('s,l');
```

There is also a shortcut function for height based media queries

```javascript
DD.bp.getHeight(min /* string || number */, max = 0 /* string || number */);
```

#### `.is()`

Returns a boolean indicating if the current viewport matches the requested media query. This uses `window.matchMedia().matches` so use a [polyfill](https://github.com/paulirish/matchMedia.js/) if you need one.

```javascript
DD.bp.is(min /* string || number */, max = 0 /* string || number */, property = 'width' /* string */);

// examples
DD.bp.is('s');
DD.bp.is('s', 'l');
DD.bp.is(0, 500);

// string notation
DD.bp.is('s,l');
```

There is also a shortcut function for height based media queries

```javascript
DD.bp.isHeight(min /* string || number */, max = 0 /* string || number */);
```

#### `.options()`

You can customise the JavaScript library by using the `options()` method.

```javascript
DD.bp.options(opts /* object */);
```

There are three customisable options:

* `baseFontSize` [number=16] Base font size of your site (browser default is normally 16px) this helps convert pixel widths to relative units for the media queries (using em units)
* `isResponsive` [boolean=true] Set to false if the site shouldn't get a responsive stylesheet (e.g. IE8 and below)
* `breakpoints` [Array] Use a custom list of name/pixel width breakpoints instead of the default in an array of `{ name: 'NAME', px: 000 }`

```javascript
DD.bp.options({
    baseFontSize: 16
    isResponsive: true,
    breakpoints: [
        { name: 'small', px: 400 },
        { name: 'medium', px: 800 },
        { name: 'large', px: 1200 }
    ]
});
```

Make sure to ensure that the values used here match the values used in the SCSS.

## Change log

`1.0.3` & `1.0.4` - September 2015

* Publish on npm.

`1.0.2` - March 2015

* Minor bug fix in options function, to allow setting of custom breakpoints.

`1.0.1` - Feb 2015

* Update to resolve issue with Bower getting an old tag

`1.0.0` - Feb 2015

* Public release
* Documentation
* Cleanup
* Automatic calculation of display logic for non-responsive stylesheets (previously was manual)
* Added height based breakpoints

`0.1.1` - July 2014

* Added support for inputting px based integers instead of just breakpoint names (suggested by @conhuynh)

`0.1.0` - June 2014

* Large rewrite
* Dynamically generate breakpoints without multiple IF statements
* Updated default number of breakpoints to 8
* Include printer support
* Changed the mixin name to `bp` to reduce RSI

`0.0.2` - August 2013

* Added check for responsive flag.
* Moved static and responsive code into a single mixin

`0.0.1` - May 2013

* Initial build. Inspired by http://css-tricks.com/naming-media-queries/

## Want to contribute?

* Got an amazing idea to make the plugin better?
* Found an annoying bug?

Please don't hesitate to raise an issue through GitHub or open a pull request to show off your fancy pants coding skills - we'll really appreciate it!

## Key Contributors

### Deloitte Digital Australia
* @dkeeghan
* @keeganstreet

## Background

The Breakpoints SCSS mixin and JavaScript library, which is made up of an SCSS Mixin and some JavaScript. We use this tool to accelerate our development process when creating responsive websites and webapps - it also helps us simplify consistency accross all of our projects.

We were inspired to create DDBreakpoints back in May 2013, as a result of Chris Coyier's CSS-Tricks article: [Naming Media Queries](http://css-tricks.com/naming-media-queries/).

As we built more responsive websites, our philosophy for creating responsive sites that don't look responsive (i.e. don't use a predictable standard grid) meant that we needed to add more and more breakpoints. While we always took a mobile-first approach, we would regularly find ourselves adding an "and-below" breakpoint for edge cases. This meant that our breakpoints mixin was getting overly large and repetitively complex.

Today, DDBreakpoints is used on almost every responsive project by Deloitte Digital in Australia... and now, it's available for your projects as well!





## Who is Deloitte Digital?

**Part Business. Part Creative. Part Technology. One hundred per cent digital.**

Pioneered in Australia, Deloitte Digital is committed to helping clients unlock the business value of emerging technologies. We provide clients with a full suite of digital services, covering digital strategy, user experience, content, creative, engineering and implementation across mobile, web and social media channels.

[http://www.deloittedigital.com/au](http://www.deloittedigital.com/au)

## LICENSE (BSD-3-Clause)
Copyright (C) 2015, Deloitte Digital. All rights reserved.

DDBreakpoints can be downloaded from: https://github.com/DeloitteDigital/DDBreakpoints

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
this list of conditions and the following disclaimer in the documentation
and/or other materials provided with the distribution.

* Neither the name of the copyright holder nor the names of its contributors 
may be used to endorse or promote products derived from this software without 
specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
