CSS refers to Cascading Style Sheets! and specifically The *C* refers to **Cascade**.

**The cascade** is what determines which rules actually get applied to our HTML. There are different factors that the cascade uses to determine this. We will examine three of these factors, which will hopefully help you avoid those frustrating “I hate CSS” moments.

And to understand it, we have to think of it like a game of conquer! Which part is more dominant and can conquer the other part? It brings order in the rules (pun intended)

you can jump to mistakes I made in the Assignment section [[#Mistakes I made||here]]
# 1. Importance
There are four basic _types_ of rules:

1. **transition**
   Rules that apply to an active transition take the utmost importance
2. **!important**
   When we add `!important` to the end of our declaration, it jumps to this level of the cascade!
   Ideally, you reserve this level for "Please help me god", which are needed to override styles from third-party libraries.
3. **animation**
   Rules that apply to an _active animation_ jump up a level in the Cascade
4. **normal**
   - This level is where the bulk of rules live

As you can see, this top tier is mostly reserved to ensure our elements animate properly, and to help out desperate developers (`!important`).
# 2. Origin
There are three places where a rule can be defined:

1. website
	This is the only level that you have control over, as a web developer.
2. user
3. browser
	Each browser has its own set of styles, which is why things like `<button>`s have default styles.
	
	<button>Like this</button>

> Funky fact alert! The hierarchy here is actually reversed for !important rules, meaning that an !important browser default rule wins over an !important website rule (whereas a website rule normally wins over a browser default).


# 3. Specificity
A CSS declaration that is more specific will take precedence over less specific ones. Inline styles, which we went over in the previous lesson, have the highest specificity compared to selectors, while each type of selector has its own specificity level that contributes to how specific a declaration is. Other selectors contribute to specificity, but we’re focusing only on the ones we’ve gone over so far:

1. ID selectors (most specific selector)
2. Class selectors
3. Type selectors

Specificity will only be taken into account when an element has multiple, conflicting declarations targeting it, sort of like a tie-breaker. An ID selector will always beat any number of class selectors, a class selector will always beat any number of type selectors, and a type selector will always beat any number of less specific selectors. When there is no declaration with a selector of higher specificity, a rule with a greater number of selectors of the same type will take precedence over another rule with fewer selectors of the same type.

Let’s take a look at a few quick examples to visualize how specificity works. Consider the following HTML and CSS code:

```html
<!-- index.html -->

<div class="main">
  <div class="list subsection">Red text</div>
</div>
```

```css
/* rule 1 */
.subsection {
  color: blue;
}

/* rule 2 */
.main .list {
  color: red;
}
```

In the example above, both rules are using only class selectors, but rule 2 is more specific because it is using more class selectors, so the `color: red` declaration would take precedence.

Now, let’s change things a little bit:

```html
<!-- index.html -->

<div class="main">
  <div class="list" id="subsection">Blue text</div>
</div>
```

```css
/* rule 1 */
#subsection {
  color: blue;
}

/* rule 2 */
.main .list {
  color: red;
}
```

In the example above, despite rule 2 having more class selectors than ID selectors, rule 1 is more specific because ID beats class. In this case, the `color: blue` declaration would take precedence.

And a bit more complex:

```html
<!-- index.html -->

<div class="main">
  <div class="list" id="subsection">Red text on yellow background</div>
</div>
```

```css
/* rule 1 */
#subsection {
  background-color: yellow;
  color: blue;
}

/* rule 2 */
.main #subsection {
 color: red;
}
```

In this final example, the first rule uses an ID selector, while the second rule combines an ID selector with a class selector. Therefore, neither rule is using a more specific selector than the other. The cascade then checks the number of each selector type. Both rules have only one ID selector, but rule 2 has a class selector in addition to the ID selector, so rule 2 has a higher specificity!

While the `color: red` declaration would take precedence, the `background-color: yellow` declaration would still be applied since there’s no conflicting declaration for it.

## Not everything adds to specificity
When comparing selectors, you may come across special symbols for the universal selector (`*`) as well as combinators (`+`, `~`, `>`, and an empty space). The ones you have not yet seen, we will cover these later in the curriculum, so there is no need to dive into them yet. The core concept is that these symbols do not inherently convey any additional specificity.

```css
/* rule 1 */
.class.second-class {
  font-size: 12px;
}

/* rule 2 */
.class .second-class {
  font-size: 24px;
}
```

Here both rule 1 and rule 2 have the same specificity. Rule 1 uses a chaining selector (no space) and rule 2 uses a descendant combinator (the empty space). But both rules have two classes and the combinator symbol itself does not add to the specificity.

```css
/* rule 1 */
.class.second-class {
  font-size: 12px;
}

/* rule 2 */
.class > .second-class {
  font-size: 24px;
}
```

This example shows the same thing. Even though rule 2 is using a child combinator (`>`), this does not change the specificity value. Both rules still have two classes so they have the same specificity values.

```css
/* rule 1 */
* {
  color: black;
}

/* rule 2 */
h1 {
  color: orange;
}
```

In this example, rule 2 would have higher specificity and the `orange` value would take precedence for this element. Rule 2 uses a type selector, which has the lowest specificity value. But rule 1 uses the universal selector (`*`) which has no specificity value.

# 4. Inheritance
Inheritance refers to certain CSS properties that, when applied to an element, are inherited by that element’s descendants, even if we don’t explicitly write a rule for those descendants. Typography-based properties (`color`, `font-size`, `font-family`, etc.) are usually inherited, while most other properties aren’t. You can find out if a property is inherited or not by going to its docs on MDN and heading to the **Formal Definition** section. For example, the [CSS `color` property formal definition](https://developer.mozilla.org/en-US/docs/Web/CSS/color#formal_definition) indicates that `color` is an inherited property, while the [**`display`** property formal definition](https://developer.mozilla.org/en-US/docs/Web/CSS/display#formal_definition) indicates that `display` is not.

The exception to this is when directly targeting an element, as this always beats inheritance:

```html
<!-- index.html -->

<div id="parent">
  <div class="child"></div>
</div>
```

```css
/* styles.css */

#parent {
  color: red;
}

.child {
  color: blue;
}
```

Despite the `parent` element having a higher specificity with an ID, the `child` element would have the `color: blue` style applied since that declaration directly targets it, while `color: red` from the parent is only inherited.
# 5. Rule Order
The final factor, the end of the line, the tie-breaker of the tie-breakers. Let’s say that after every other factor has been taken into account, there are still multiple conflicting rules targeting an element. How does the cascade determine which rule to apply?

Whichever rule was the _last_ defined is the winner.

```css
/* styles.css */

.alert {
  color: red;
}

.warning {
  color: yellow;
}
```

For an element that has both the `alert` and `warning` classes, the cascade would run through every other factor, including inheritance (none here) and specificity (neither rule is more specific than the other). Since the `.warning` rule was the last one defined, and no other factor was able to determine which rule to apply, it’s the one that gets applied to the element.

# Mistakes I made
You know that chaining some selectors beats just one selector right? for ex:

```css
.child {
font-size: 15px;
}

.para{
font-size: 20px;
}
```

```html
<p class="para"> This is a parent paragraph!
	<p class="para child"> This is a child paragraph</p>
</p>
```

This would lead to making the child paragraph with font size **20px**. Why?
- [[#5. Rule Order]] the child actually takes the `.para` font size as it came in last.

How to fix it?
There are 2 ways:
1. You can make `.child` come in last

```css
.para{
font-size: 20px;
}

.child {
font-size: 15px;
```

2. You can chain the `.child` with a selector! like `p.child` [[Introduction to CSS#Type Selectors (div, p, etc.)|(Type Selector)]] or `.para.child` [[Introduction to CSS#Class Selectors|(Class Selectors)]] **But here** it is better to chain it with `p.child` because it makes the most sense, as a .child can be any child, but it will always be a paragraph `p`.
   If you want to style all childs the same way you can do that, but it is probably better to style each one of them for their own purpose like a a `div.child` that targets a child of a div.

