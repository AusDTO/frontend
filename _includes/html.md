Generally speaking, you should always follow the [W3C HTML5 standard].

## Document Type Declaration
Always use the [HTML5 doctype], and place it before any other markup
(usually the `<html>` element):

  ```html
  <!DOCTYPE html>
  ```

The lowercase variation is also valid:

  ```html
  <!doctype html>
  ```

> **Here's why:** The [DTD], or "doctype", tells browsers (and other
programs that read HTML) what to expect in your document. In some browsers,
the presence or absence of certain DTDs can drastically change the way that
your markup, style, and scripts are interpreted. In "[quirks mode]",
browsers lack many modern web features and emulate the buggy behavior of
sometimes ancient versions.

**Note:** The doctype _must_ be followed immediately (ignoring whitespace)
by an element such as `<html>`, and not a comment. See
[the spec](https://www.w3.org/TR/html51/syntax.html#optional-tags)
for more information.


## Void Elements
**Do not** close [void][void elements] ("self-closing") elements with a
trailing slash, as in `<br/>`. For example, mark them up like so:

  ```html
  <link rel="stylesheet" href="foo.css">
  <img src="foo.gif">
  <embed src="foo.svg">
  ```
  
  HTML5's full list of void elements is:

  ```html
  <area>
  <base>
  <br>
  <col>
  <embed>
  <hr>
  <img>
  <input>
  <keygen>
  <link>
  <meta>
  <param>
  <source>
  <track>
  <wbr>
  ```

:warning: **Note:** [SVG](#inline-svg) elements are essentially XML, and
therefore follow different rules **TODO: citation needed**.

## Nesting
1. Always close elements that aren't [void or "self-closing"](#void-elements):

  ```html
  <ul>
    <li>Item 1</li>
    <li>Item 2 <a href="#foo">link</a></li>
  </ul>
  ```

  > :warning: If you learned HTML in the 90s, you may remember not having to
  > close some elements, such as `<li>`:
  > 
  > ```html
  > <ul>
  >   <li>Item 1
  >   <li>Item 2
  > </ul>
  > ```
  > 
  > This _is_ [valid in HTML4](https://www.w3.org/TR/html-markup/li.html)
  > (note: this linked spec has been deprecated), but is no longer valid in HTML5.

1. Be sure to nest start and close tags properly:

  ```html
  <section>
    <h1>Section Title</h1>
    <ul>
      <li>Item 1</li>
      <li>Item 1</li>
      <li>Item 3
        <ul>
          <li>Sub-item A</li>
          <li>Sub-item B</li>
        </ul>
      </li>
    </ul>
  </section>
  ```

Improper nesting order may cause unexpected display issues. Using consistent
[whitespace](#whitespace) and an editor that understands HTML can help you
maintain the proper nesting order.


## Inline SVG
SVG is an XML dialect for describing vector graphics, and can be used either
"inline" (included directly in the HTML) or by reference to external resources
with the `<img>` or `<object>` elements. Inline SVG should conform first and
foremost to the [W3C spec](http://dev.w3.org/SVG/proposals/svg-html/svg-html-proposal.html),
then to the rest of these guidelines, with the following exceptions:

1. You *may* omit XML namespace declarations in static HTML. This
[StackOverflow answer](http://stackoverflow.com/a/23322429) explains why, and also
notes that SVG elements in JavaScript must be created and manipulated with the
namespace-aware DOM methods, namely [`document.createElementNS()`][createElementNS]
and [`setAttributeNS()`][setAttributeNS].

1. Because SVG is an XML dialect, some elements without content (such as `<rect>`
and `<path>`) *may* self-close. In this case, you must include the trailing slash:

  ```html
  <svg>

    <!-- if the closing tag were omitted, the following elements wouldn't render -->
    <path d="M10,10h40v40h-40v-40Z"></path>

    <!-- this is also valid -->
    <rect width="20" height="30"/>

  </svg>
  ```

### Links in Inline SVG
You can wrap any SVG element in an `<a>` element to turn it into an active link,
but if you do so you must use the `xlink:href` attribute to specify its target:

```html
<svg>
  <a xlink:href="#foo">
    <rect width="100%" height="100%"></rect>
  </a>
</svg>
```

If you forget the `xlink:` prefix, the link will not be usable. Remember that if
you're creating SVG links in JavaScript you will also need to use the namespace-aware
DOM methods [`document.createElementNS()`][createElementNS] and
[`setAttributeNS()`][setAttributeNS].

```js
var svg = document.querySelector('svg');
var a = document.createElementNS('a', 'http://www.w3.org/2000/svg');
a.setAttributeNS('href', 'http://www.w3.org/1999/xlink');
svg.appendChild(a);
``` 

## Whitespace
For consistency and ease of editing HTML, we suggest using **2-space indentation**.

## Comment Hints
Complex HTML documents with deep nesting or long blocks of content can be hard to
work with, especially in editors that don't provide shortcuts for moving between
matching open and close tags. The following convention can help you identify
closing tags when the matching start tag isn't visible in your editor:

```html
<element>

  content

</element><!-- CSS selector -->
```

Where the `CSS selector` is an class, ID, or attribute selector, as in:

```html
<div class="foo">

  ...

</div><!-- .foo -->

<section id="bar">

  ...

</section><!-- #bar -->

<textarea name="content">

  ...

</textarea><!-- [name=content] -->
```


[DTD]: https://en.wikipedia.org/wiki/Document_type_definition
[HTML5 doctype]: http://dev.w3.org/html5/html-author/#doctype-declaration
[XHTML]: https://en.wikipedia.org/wiki/XHTML
[doctype switching]: http://www.webdevout.net/doctype-switching
[quirks mode]: https://en.wikipedia.org/wiki/Quirks_mode
[W3C HTML5 standard]: https://www.w3.org/TR/html5/
[void elements]: https://www.w3.org/TR/html5/syntax.html#void-elements
[createElementNS]: https://developer.mozilla.org/en-US/docs/Web/API/Document/createElementNS
[setAttributeNS]: https://developer.mozilla.org/en-US/docs/Web/API/Element/setAttributeNS
