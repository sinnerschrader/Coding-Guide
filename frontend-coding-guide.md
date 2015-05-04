# Frontend Coding Guide

The guide takes inspiration from a few sources, credited in the inspiration section.

Our Boilerplate project is based on these rules.


* * *


## General

### Be consistent

This coding guide aims to shape a common ground for developing frontend web projects at SinnerSchrader. Everything in this document is optional to use but we strongly recommend to rely on this ruleset in order to establish a consistent codebase between projects. Consider this document as crucial part of your project, add it to the repository of the project and optionally update the paragraphs that you and your team like to do in a different way.


### Know the spec!

It is essential that you know the specification of all standards that you use. It provides answers to almost every question on how to write your code and what to use.


### Separation of presentation, content and behavior

Separate presentation, content and behavior to ensure the upmost interoperability. Thereby each layer can be modified without having to update the other layers.


### Write readable code

Prefer readability and maintainability over premature optimization. Use comments where appropriate to describe the intention behind your code.


### Indent code consistently

More important than the eternal discussion of whether to use tabs or spaces is a project wide consistent use of indentation across all developers. Our recommendation is to use tabs to indent your code. The advantage is that every developer can set the tab width to the preferred value.

One possible solution to tackle the issue of an inconsistent indentation is the use of an [editorconfig file](http://editorconfig.org/).


* * *


## Markup

### Use as little markup as possible

Markup should be used to describe content provide elements used for styling. Try to avoid empty elements or wrapping divs wherever possible. The cleaner the HTML, the better!


### Use the semantically most appropriate element

Choose semantically specific elements over `div` or `span`. This enables search engines and everyone else to get a semantic context for the information inside an element.


### Close all tags

Despite it is not mandatory to close void elements in HTML5 you should close all tags. Use `<input type="text" />` instead of `<input type="text">`


### No shorthand attributes

Despite it is ok to use shorthand attributes in HTML5 you should write the XML compatible long form. Use `<input type="text" required="true" />` instead of `<input type="text" required>`


### Use double quotes for attributes

Again, there is no single "right" way for quoting attributes in HTML in the [specification](http://www.w3.org/TR/html4/intro/sgmltut.html#h-3.2.2) but to stay consistent you should use double quotes in HTML.


### Avoid inline styles

Using inline styles conflicts with the rule to separate presentation, content and behavior and is a sign of poor code quality. Just don't do it.


### Avoid inline event handlers

Using inline event handlers such as `<span onclick="myFunc">Link</span>` conflicts with the rule to separate presentation, content and behavior. JavaScript should progressively enhance the user experience. Use classnames with a `js_` prefix and bind your event handlers to these classes in your JavaScript files.


### No protocol but "//&lt;path&gt;"

Try to strip the protocol whenever possible. Most CDNs serve their assets via `http` and `https`. Dropping the protocol from your reference forces the browser to use the same protocol the site is served with, which helps to avoid invalid https implementation.


* * *


## Styles

### Avoid IDs

It is never safe to assume that you use one element only once. Even for logos, pages or any other element. Furthermore, an element with an ID will create a global variable in JavaScript holding a reference to the element in the DOM (see ["named elements" documented in the HTML standard](http://www.whatwg.org/specs/web-apps/current-work/multipage/browsers.html#named-access-on-the-window-object)).


### Initial layout is independent from JavaScript

The initial layout of a page should look same with JavaScript enabled and disabled.
All JavaScript should be at the end of page (see [JavaScript](#javascript)), this is including Modernizr feature-tests. Prefer "Graceful Degradation" over feature-tests. When layout updates (depending on feature-tests) are applied to the page, they should be imperceptible: e.g. replacing `display:none` with `opacity:0` and `pointer-events:none`.


### Animate an interface using classes not inline styles

Animations belong to the presentation layer and therefore the necessary properties should be declared in your stylesheet rather than in the JavaScript code. Use JavaScript just to toggle the classes that define the the visual target state. Let CSS handle the transitions between the states. If you implement a more complex UI behavior try to use a minimum of DOM maniplations synchronized with the animation frame, and animate CSS properties that don't trigger a repaint.


### Selectors should be max. four levels deep

Long selector chains are slower for the browser to evaluate and lead to specificity issues. If you feel like you need more than four levels in a selector then it's probably the better choice to add an additional class to the markup.

Nice to read: [The inception rule](http://thesassway.com/beginner/the-inception-rule)


### Double check compiled stylesheets

Use preprocessors to make your CSS easier to maintain but you should always double check the compiled output. It's easy to underestimate the resulting code bloat of overly specific selector nesting.


### Prefer normalize over reset

You should use the normalize.less from the boilerplate: [Read why normalize is better than reset](http://nicolasgallagher.com/about-normalize-css/).

> A global reset is a convenient way of ensuring consistent styling cross browser but it is often overkill and it makes CSS harder to debug in developer tools.


### Order properties: box, border, background, text & other

Again, the only _correct_ way to order properties is a consistent way. So pick a method and make sure the entire team is consistent in using that method. The recommended way of ordering properties is by impact ([CSScomb](http://csscomb.com/) can help you here) and placing mixins at the end:

```css
.example-property-order{
	display: inline-block;
	font-size: 10px;
	line-height: 14px;
	#boilerplate-tools > .ir;
}
```


### Avoid the universal selector *

It results in bad render performance. We prefer not using the universal selector because it might become a performance issue and it definitely is a very bad style. Use the normalize stylesheet and you should never feel the need for the * selector. It can be [ok](http://www.paulirish.com/2012/box-sizing-border-box-ftw/) to use it once.

```css
html{
	box-sizing: border-box;
}
*, *:before, *:after{
	box-sizing: inherit;
}
```


### Use shorthand properties unless you overwrite

CSS shorthand properties are preferred because of its terseness and flexibility. Use fully qualified properties if you overwrite inherited values to indicate that you were aware of the inheritance.


### Whitespace between : and the value; No space between classname and {

This coding style is a proposition. Make sure you are consistent within the project.

```css
.classname{
	property: value;
}
```


### Use vendor prefixes correctly & painlessly

Use [autoprefixer](https://github.com/ai/autoprefixer) for vendor prefixes whenever possible. Since we use a CSS preprocessor (LESS > SCSS) you may also implement vendor prefixes with mixins based on the browser matrix you are developing for. This provides high flexibility and maintainability.


### Avoid CSS hacks

Always avoid the use of non standard compliant CSS hacks. CSS hacks are bad practice since most of them can be avoided by writing clean and clutterless code. If you feel the need to use a CSS hack think of feature detection libraries e.g. modernizr.


### Split CSS into logical units during development

Split CSS (LESS) files in logical units that are as small as possible but as big as necessary.


### Class names should be self explanatory but short

It should be possible to look at a class name and instantly have an idea what it represents. Keep it short and do not provide a detailed description in the class name, just a hint. If a detailed description is needed it belongs in a comment at the top of the class. If you abbreviate the class name make sure the abbreviation makes sense to everyone, not only you. E.g. `.nav` for `.navigation` is okay but `.atr` for `.author` is probably not.


### Don't use styling in class names

The visual appearance will change, the class name will not.
Don't use s.th. like `.greenBorder`, `.bigMargin`, `.redText`, `.floatLeft`. Try find the context for the class like `.highlight`, `.errormsg`.


### Don't re-invent class names (use selectors)

If you have teasers in a sidebar and the footer you can use the same class name by making it unique through it's selector.
`.footer .teaser` and `.sidebar .teaser` are better then `.footer-teaser` and `.sidebar-teaser`. Especially in CMS context it makes sense to have modules (markup) that are fully re-usable.


### Don't abuse class names

Just don't: `.iReallyLikeToKeepSelectorsOnlyOneLevelBecauseIRead5YearsAgoSomeWhereBySomeOneThatsItsMuchMuchBetter-YeahThatsAGoodName`


### Avoid CamelCaseClassNames

Not all people write camelcase the same way. You should just use lowercase and lean class names.
`.smallbreadcrumb` is better to remember then `.smallBreadcrumb`.


### Don't qualify class selectors

If you already use a class name, there is no need to qualify it. Unqualified selectors are independent from the (semantic) elements and help you separate presentation and content.


### !!important

Never use `!important`! If you want to overwrite a property and you don't know how read about [selector specificity](http://www.w3.org/TR/css3-selectors/#specificity). It is way better to break the rule "_Selectors should be max. four levels deep_" than to use `!important`.


### Avoid floating where possible

If you use floats for layout you need clearfixes, which are CSS hacks and should be avoided. There are only one case in which floating makes sense: If you want text to actually float around another element e.g. an image, figure, table. Try to avoid them for layout whenever possible; use `display` instead.


### Avoid explicit definition of width and height

The containing elements and their contents should define the width and hight of a module. You never know how much content a module will contain, and you often can't tell which container it will be placed in. Keep all module as flexible and reusable as possible.

* * *


## Javascript

### Don't over-optimize

Write readable and maintainable code in favor of "optimized" code. Only optimize if there is a significant boost of performance. Over-optimized code is not intuitive.


### Use whitespace

Don't hold back with whitespace and line-breaks, it makes the code easier to scan and will be stripped out on the live system via minification anyway! It helps creating logically grouped code.


### CamelCase

Variables and classes are named in camel case. Variables start with a lowercase letter and classes with an uppercase letter.
Only constants are written in all caps and separated with underscores.


### var

Each new variable should get it's own var statement. This increases readability and makes the code less error prone (forget a comma and the variable is in the global scope). Also variables should be defined where they are needed or where it makes sense instead of in a huge pile somewhere in the code.


### Brackets

For functions, loops, etc. the opening bracket is placed at the end of the line. The closing bracket is placed at the beginning of a new line

#### Style examples:

```javascript
var foo;
var bar = 'foo';
var myObject = {
	foo: 'bar'
};
var MyClass = function() {
	if ( foo ) {
		doSomething();
	} else {
		doSomethingElse();
	}
};
var MY_CONSTANT = 42;
```


### Use Semicolons

Even though semicolons are optional in JavaScript, you should use them! Leaving them out can have undesired side effects, also some weak minifiers might corrupt your code in the process.


### Comments

Code should be self commenting by use of senseful function and variable names. If you need to explain what a function does, you might consider to rewrite it.

But you should comment special codelines which are not obvious for everyone, like fixes.
Place these comments in an extra line above the code you describe.

```javascript
// fix for Safari 6
window.setTimeout(foo, 0);
```


### Closures & Namespace

You do not write code in the global namespace, always use closures!
```javascript
(function(window) { /* your code here */ })(this);
```

If possible, you should use the commonJS module pattern and browserify to compile your code. This will automatically wrap your logic in closures and prevent local variables to be exposed to the global scope.

```javascript
var MyModule = function(args, ...) {
	/* your code here */
};
module.exports = MyModule;
```

If some part of your code has be globally accessible you explicitly assign it to the `window` object under a namespace that suits the project.

```javascript
window.s2.myVariable = [ ... ]
```


### Literals

Prefer to use literals over constructor functions, they are more compact, can be initialized with values and are spotted more easily while reading.

```javascript
// instead new Array();
[1, 2, 3];

// instead new Object();
{'one': 1, 'two': 2};
// instead new RegExp();
/^[a-z]$/;
```


### Quotes

Use single-quotes (') for strings in JavaScript. This way, it is more convenient to use HTML fragments in a string, because they use double-quotes (") and don't have to be escaped.

```javascript
var awesomeButton = '<button class="awesome">Awesome!</button>';
```


### (Strict) Equal

Prefer strict or typed equal (===) over the lose equal (==), it could prevent unexpected behavior. Exceptions are checks for truthy/falsy values or values that you can be certain of to return a specific type (e.g. Array.length always returns a number). You should always cast mixed types in the type you need.


### Libraries/Plugins

Be as minimal as possible! Most of the time it is not smart to use a monolithic library or plugin, but rather write a custom version for your needs. This should save a lot of file size and - which is even more important - drastically increase performance. If you consider taking a plugin and rewrite it, you should be arware of license issues. A list of the best practices can be found in the Wiki. These blueprints are better than plugins because they only include the code that is always common, but don't implement special behavior. They also match perfectly the project's browser matrix.


### Always JavaScript?

Don't try to achieve things with JavaScript that can be done better with other techniques or tools. Prefer native elements and don't reinvent the wheel. Also animations/transitions have shifted from JavaScript to CSS, only trigger their start/end!


* * *


## Performance

>[...] for most web pages, less than 10–20% of the end user response time is spent getting the HTML document from the web server to the browser. If you want to dramatically reduce the response times of your web pages, you have to focus on the other 80–90% of the end user experience. (Souders, Steve. 2007. _High Performance Web Sites_. O’Reilly Media, Inc.)

Performance optimization of websites is the responsibility of every frontend developer. Make sure to read both books of Steve Souders on website performance.[^1]
[^1]: [High Performance Web Sites](http://www.amazon.de/High-Performance-Web-Sites-Faster-Loading/dp/0596529309/) and [Even Faster Web Sites](http://www.amazon.de/Even-Faster-Web-Sites-Performance/dp/0596522304/) by Steve Souders.

The rules summarized in this coding guide just give you a rough understanding what to is essential to know.


### Concat an minify all the things!

Concatenation and minification should be a essential part of your process. Serving the minimum of files possible that are as small as possible helps you speed up loading time.


### Put CSS in HEAD

Reference the CSS file in the HEAD (use the `link` tag, not `@import`). This prevents [FOUC](http://en.wikipedia.org/wiki/Flash_of_unstyled_content) and all downsides.


### JavaScript before closing BODY

Include the minified version of the JavaScript right before the closing BODY tag. Load scripts in the HEAD will [block the rendering](http://www.stevesouders.com/blog/2010/09/30/render-first-js-second/) of the page during the time the script is retrieved form the server and interpreted by the JavaScript engine of the browser. Think about asynchronous and deferred loading of your scripts.


### Reduce HTTP requests

Reducing the number of HTTP requests will help improve loading times, since browsers can only handle a number of parallel downloads. Again, minify and concatenate your assets and use sprites. This issue may become irrelevant in HTML2.0/Speedy.


### Set appropriate cache headers and use cache busting

**Cache is King!** Configure the cache headers to the max and use [cache busting](http://html5boilerplate.com/html5boilerplate-site/built/en_US/docs/cachebusting/). Consider a CDN like cloudfront.


* * *


## Images

### Use retina optimized JPGs for photos
[Read how and why](http://blog.netvlies.nl/design-interactie/retina-revolution/)


### Optimize images
Use tools like [imageOptim](http://imageoptim.com/), [imageAlpha](http://pngmini.com/) or the respective command-line tools to losslessly reduce the file size of images.


### Use IconFonts or SVGs whenever possible
If possible use IconFonts or SVGs for icons. The browser support is quite good, you need no retina optimization and you can easily fall back to sprites.


### Use CSS sprites
To minimize HTTP requests try to use one CSS sprite instead multiple small images.


### Progressive JPG
Progressive JPGs have a much better perceived speed as they appear faster even though the actual loading speed is not faster. [Read more about progressive JPGs](http://calendar.perfplanet.com/2012/progressive-jpegs-a-new-best-practice/).


* * *


## Inspiration and further reading

* [https://github.com/byrichardpowell/CSS-Style/](https://github.com/byrichardpowell/CSS-Style/)
* [https://smacss.com/](https://smacss.com/)
* [http://isobar-idev.github.io/code-standards/](http://isobar-idev.github.io/code-standards/)
* [http://www.yellowshoe.com.au/standards/](http://www.yellowshoe.com.au/standards/)
* [https://github.com/stevekwan/best-practices](https://github.com/stevekwan/best-practices)
* [https://github.com/airbnb/javascript](https://github.com/airbnb/javascript)
* [https://github.com/rwldrn/idiomatic.js/](https://github.com/rwldrn/idiomatic.js/)
