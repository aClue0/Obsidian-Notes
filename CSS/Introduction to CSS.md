# What is CSS?
CSS is what makes the *style* of the page. It is what puts the image of a "House" in the middle instead of the left like adding the img in html as in [[Recipe Project]]. 
# Basic Syntax
At the most basic level, CSS is made up of various rules. Each rule is made up of a selector (more on this in a bit) and a semicolon-separated list of declarations, with each of those declarations being made up of a property–value pair.

![[Pasted image 20260428120106.png]]

## What is a div?
A `<div>` is one of the basic HTML elements. It is an empty container. In general, it is best to use other tags such as `<h1>` or `<p>` for content in your projects, but as we learn more about CSS you’ll find that there are many cases where the thing you need is just a container for other elements. Many of the exercises use plain`<div>`s for simplicity. Later lessons will go into much more depth about when it is appropriate to use the various HTML elements.

# Selectors
A Selector is basically a name you give to some elements that you want to style in some way. 

For example if you have ^5d9eb4

```html 

<!-- index.html -->

<div> Hello, World!</div>
<div> This is also a div</div> 
<p> Hi... This is a paragraph</p>
<div> 
```

^83e0a1

```css

/* styles.css */

div {
  color: white;
}
```

Here, all three `<div>` elements would be selected and have the text color white, while the `<p>` element wouldn’t be.

## Types of Selectors
There are different types of selectors:
### Universal Selector
The universal selector will select elements of every type (as in the whole document), hence the name “universal”, and the syntax for it is a simple asterisk. In the example below, every element would have the `color: purple;` style applied to it.

```css
* {
  color: purple;
}
```

### Type Selectors (div, p, etc.)

A type selector (or element selector) will select all elements of the given element type, and the syntax is just the name of the element:

like the example [[#^83e0a1 |above]]  
### Class Selectors
Class selectors will select all elements with the given class, which is just an attribute you place on an HTML element. Here’s how you add a class to an HTML tag and select it in CSS:
```html
<!-- index.html -->

<div class="alert-text">Please agree to our terms of service.</div>
```

```css
/* styles.css */

.alert-text {
  color: red;
}
```

Note the syntax for class selectors: a period immediately followed by the case-sensitive value of the class attribute. Classes aren’t required to be specific to a particular element, so you can use the same class on as many elements as you want.

> **Class selectors** won’t work if the class name begins with a number. For example, if you give an element the class name `4lert-text`, using `.4lert-text` as a selector won’t match it.

--- 
Another thing you can do with the class attribute is to add multiple classes to a single element as a space-separated list, such as `class="alert-text severe-alert"`. Since whitespace is used to separate class names like this, you should never use spaces for multi-worded names and should use a hyphen instead.
### ID Selectors
ID selectors are similar to class selectors. They select an element with the given ID, which is another attribute you place on an HTML element.

- The major difference between classes and IDs is that an element can only have **one** ID. It cannot be repeated on a single page and should not contain any whitespace:

```html
<!-- index.html -->

<div id="title">My Awesome 90's Page</div>
```

```css
/* styles.css */

#title {
  background-color: red;
}
```

For IDs, instead of a period, we use a hashtag immediately followed by the case-sensitive value of the ID attribute. A common pitfall is people overusing the ID attribute when they don’t necessarily need to, and when classes will suffice. While there are cases where using an ID makes sense or is needed, such as taking advantage of specificity or having links redirect to a section on the current page, you should use IDs **sparingly** (if at all).

IDs are used alot in **JavaScript targeting**

IDs are heavily used in JavaScript because they’re fast and precise.

```html
<div id="title">Hello</div>
```

```js
const el = document.getElementById("title");
```

This is probably the **most important real-world use** of IDs.
#### Simple rule of thumb

- Use **IDs** for:
    - JavaScript hooks
    - Unique sections
    - Anchors
- Use **classes** for:
    - Styling
    - Reusable components

### Group Selectors
What if we have two groups of elements that share some of their style declarations?

```css
.read {
  color: white;
  background-color: black;
  /* several unique declarations */
}

.unread {
  color: white;
  background-color: black;
  /* several unique declarations */
}
```

Both our `.read` and `.unread` selectors share the `color: white;` and `background-color: black;` declarations, but otherwise have several of their own unique declarations. To cut down on the repetition, we can group these two selectors together as a comma-separated list:

```css
.read,
.unread {
  color: white;
  background-color: black;
}

.read {
  /* several unique declarations */
}

.unread {
  /* several unique declarations */
}
```

Both of the examples above (with and without grouping) will have the same result, but the second example reduces the repetition of declarations and makes it easier to edit either the `color` or `background-color` for both classes at once.

## Chaining Selectors
Another way to use selectors is to chain them as a list without any separation. Let’s say we had the following HTML:

```html
<div>
  <div class="subsection header">Latest Posts</div>
  <p class="subsection preview">This is where a preview for a post might go.</p>
</div>
```

We have two elements with the `subsection` class that have some sort of unique styles, but what if we only want to apply a separate rule to the element that also has `header` as a second class? Well, we could chain both the class selectors together in our CSS like so:

```css
.subsection.header {
  color: red;
}
```

What `.subsection.header` does is it selects any element that has both the `subsection` _and_ `header` classes. Notice how there isn’t any space between the `.subsection` and `.header` class selectors. This syntax basically works for chaining any combination of selectors, except for chaining more than one [type selector](https://www.theodinproject.com/lessons/foundations-intro-to-css#type-selectors).

This can also be used to chain a class and an ID, as shown below:

```html
<div>
  <div class="subsection header">Latest Posts</div>
  <p class="subsection" id="preview">
    This is where a preview for a post might go.
  </p>
</div>
```

You can take the two elements above and combine them with the following:

```css
.subsection.header {
  color: red;
}

.subsection#preview {
  color: blue;
}
```

> Notice in the CSS the selector is `subsection#preview` . for class and # for ID

In general, you can’t chain more than one type selector since an element can’t be two different types at once. For example, chaining two type selectors like `div` and `p` would give us the selector `divp`, which wouldn’t work since the selector would try to find a literal `<divp>` element, which doesn’t exist.

## Descendant Combinator
Combinators allow us to combine multiple selectors differently than either grouping or chaining them, as they show a relationship between the selectors. There are four types of combinators in total, but for right now we’re going to only show you the **descendant combinator**, which is represented in CSS by a single space between selectors. A descendant combinator will only cause elements that match the last selector to be selected if they also have an ancestor (parent, grandparent, etc.) that matches the previous selector.

So something like `.ancestor .child` would select an element with the class `child` if it has an ancestor with the class `ancestor`. Another way to think of it is that `child` will only be selected if it is nested inside `ancestor`, regardless of how deep that nesting is. Take a quick look at the example below and see if you can tell which elements would be selected based on the CSS rule provided:

```html
<!-- index.html -->

<div class="ancestor">
  <div class="contents">
    <div class="contents"></div>
  </div>
</div>

<div class="contents"></div>
```

```css
/* styles.css */

.ancestor .contents {
  /* some declarations */
}
```

In the above example, the first two elements with the `contents` class (on lines 4 and 5) would be selected, but the last element (on line 9) wouldn’t be. Was your guess correct?

There’s really no limit to how many combinators you can add to a rule, so `.one .two .three .four` would be totally valid. This would just select an element that has a class of `four` if it has an ancestor with a class of `three`, and if that ancestor has its own ancestor with a class of `two`, and so on. You generally want to avoid trying to select elements that need this level of nesting, though, as it can get pretty confusing and long, and it can cause issues when it comes to specificity.

> Notice the difference between them and [[#Chaining Selectors]] 
#### Chaining Selectors
```html
<div>
  <div class="subsection header">Latest Posts</div>
```

- They are next to each other separated by whitespace.
#### Descendant Combinators
```html
<div class="ancestor">
  <div class="contents">
```

- They are nested.

# Properties
These are some properties that we are going to use

## Color and background-color
The `color` property sets an element’s text color, while `background-color` sets, well, the background color of an element.

```css
p {
  /* hex example: */
  color: #1100ff;
}

p {
  /* rgb example: */
  color: rgb(100, 0, 127);
}

p {
  /* hsl example: */
  color: hsl(15, 82%, 56%);
}
```

These are some [Color Names](https://www.w3schools.com/colors/colors_names.asp) that we can use
### Legal Color Values
#### Hexadecimal Colors

A hexadecimal color is specified with: `#RRGGBB`, where the RR (red), GG (green) and BB (blue) hexadecimal integers specify the components of the color. All values must be between 00 and FF.

```css
#p1 {background-color: #ff0000;}   /* red */  
#p2 {background-color: #00ff00;}   /* green */  
#p3 {background-color: #0000ff;}   /* blue */
```

##### Hexadecimal Colors With Transparency

A hexadecimal color is specified with: #RRGGBB. To add transparency, add two additional digits between 00 and FF.

```css
#p1a {background-color: #ff000080;}   /* red transparency */  
#p2a {background-color: #00ff0080;}   /* green transparency */  
#p3a {background-color: #0000ff80;}   /* blue transparency */
```

and more you can check in [CSS Legal Color Values](https://www.w3schools.com/cssref/css_colors_legal.asp)

## Typography basics and text-align
**`font-family`** can be a single value or a comma-separated list of values that determine what font an element uses. Each font will fall into one of two categories, either a “font family name” like `"Times New Roman"` (we use quotes due to the whitespace between words) or a “generic family name” like `serif` (generic family names never use quotes).

If a browser cannot find or does not support the first font in a list, it will use the next one, then the next one and so on until it finds a supported and valid font. This is why it’s best practice to include a list of values for this property, starting with the font you want to be used most and ending with a generic font family as a fallback, e.g. **`font-family: "Times New Roman", serif;`**

**`font-size`** will, as the property name suggests, set the size of the font. When giving a value to this property, the value should not contain any whitespace, e.g. `font-size: 22px` has no space between “22” and “px”.

**`font-weight`** affects the boldness of text, assuming the font supports the specified weight. This value can be a keyword, e.g. `font-weight: bold`, or a number between 1 and 1000, e.g. `font-weight: 700` (the equivalent of `bold`). Usually, the numeric values will be in increments of 100 up to 900, though this will depend on the font.

**`text-align`** will align text horizontally within an element, and you can use the common keywords you may have come across in word processors as the value for this property, e.g. **`text-align: center`**.

## Image height and width
Images aren’t the only elements that we can adjust the height and width on, but we want to focus on them specifically in this case.

By default, an `<img>` element’s `height` and `width` values will be the same as the actual image file’s height and width. If you wanted to adjust the size of the image without causing it to lose its proportions, you would set either the `height` or `width` property to a specific value and set the other property to `auto`. For example:

```css
img {
  height: auto;
  width: 500px;
}
```

For example, if an image’s original size was 500px height and 1000px width, using the above CSS would result in a height of 250px.

These properties work in conjunction with the height and width attributes in the HTML. It’s best to include both of these properties and the HTML attributes for image elements, even if you don’t plan on adjusting the values from the image file’s original ones. When these values aren’t included, if an image takes longer to load than the rest of the page contents, it won’t take up any space on the page at first but will suddenly cause a drastic shift of the other page contents once it does load in. Explicitly stating a `height` and `width` prevents this from happening, as space will be “reserved” on the page and appear blank until the image loads.

# Adding CSS to HTML
## External CSS
External CSS is the most common method you will come across, and it involves creating a separate file for the CSS and linking it inside of an HTML’s opening and closing `<head>` tags with a void `<link>` element:

```html
<!-- index.html -->

<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

There's also a  `type=text/css` in the `link` element but it is not used as CSS generates type automatically

```css
/* styles.css */

div {
  color: white;
  background-color: black;
}

p {
  color: red;
}
```

The `href` attribute is the location of the CSS file, either an absolute URL or, what you’ll be utilizing, a URL relative to the location of the HTML file. You can revise URLs [[First Lesson|here]].

In our example above, we are assuming both files are located in the same directory. The `rel` attribute is required, and it specifies the relationship between the HTML file and the linked file.

A couple of the pros to this method are:

1. It keeps our HTML and CSS separated, which results in the HTML file being smaller and making things look cleaner.
2. We only need to edit the CSS in _one_ place, which is especially handy for websites with many pages that all share similar styles.
## Internal CSS
Internal CSS (or embedded CSS) involves adding the CSS within the HTML file itself instead of creating a completely separate file. With the internal method, you place all the rules inside of a pair of opening and closing `<style>` tags, which are then placed inside of the opening and closing `<head>` tags of your HTML file. Since the styles are being placed directly inside of the `<head>` tags, we no longer need a `<link>` element that the external method requires.

Besides these differences, the syntax is exactly the same as the external method (selector, curly braces, declarations):

```html
<head>
  <style>
    div {
      color: white;
      background-color: black;
    }

    p {
      color: red;
    }
  </style>
</head>
<body>
  ...
</body>
```

This method can be useful for adding unique styles to a _single page_ of a website, but it doesn’t keep things separate like the external method, and depending on how many rules and declarations there are it can cause the HTML file to get pretty big.

## Inline CSS

Inline CSS makes it possible to add styles directly to HTML elements, though this method isn’t as recommended:

```html
<body>
  <div style="color: white; background-color: black;">...</div>
</body>
```

The first thing to note is that we don’t actually use any selectors here, since the styles are being added directly to the opening `<div>` tag itself. Next, we have the `style=` attribute, with its value within the pair of quotation marks being the declarations.

If you need to add a _unique_ style for a _single_ element, this method can work just fine. Generally, though, this isn’t exactly a recommended way for adding CSS to HTML for a few reasons:

- It can quickly become pretty messy once you start adding a _lot_ of declarations to a single element, causing your HTML file to become unnecessarily bloated.
- If you want many elements to have the same style, you would have to copy and paste the same style to each individual element, causing lots of unnecessary repetition and more bloat.
- Any inline CSS will override the other two methods, which can cause unexpected results. (While we won’t dive into it here, this can actually be taken advantage of.)