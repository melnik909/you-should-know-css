**Pay attention, I use the Computed value term. It's a property value that you see in the DevTools Computed tab.**

## What will specificity be of the following selector?
```css
:is(#container, .content, main) {
  color: red;
}
```

The `:is()` pseudo-class function helps browsers select the highest from a given selectors list. In our example, a more high selector is `#container`. The specificity of this selector is `0, 1, 0, 0`. It will be used for the whole at-rule.

## The computed value of the `color` property is `red`. True or false?
```html 
<body>
  <span id="container" class="container">content</span>
</body>
```
```css 
.container {
  color: red;
}

:where(#container) {
  color: blue;
}
```

True. The `:where()` pseudo-class function nulles specificity.  So the `.label` selector has more specificity. It's why the computed value of the `color` property is `red`. 

## What is the computed value of the `background-color` property?
```html
<body>
  <div id="container" class="container">content</div>
</body>
```
```css
@layer basic, components;

.container { 
  width: 1rem; 
  height: 1rem; 
}

@layer components {
  .container { 
    background-color: pink; 
  }
}

@layer basic { 
  #container { 
    background-color: blue; 
  }
}
```  
Layers are defined in order of priority. The last is more high.  So the answer is `pink`.

## What color will the square be in the following example? 
```css
.container {
  display: inline;
  width: 1rem;
  height: 1rem;
  background-color: currentColor;
}
```

If the element has `display: inline` the `width` and `height` properties don't have an effect. We won't see a square. 

## What is the algorithm for calculating the computed value of the `width` property of the `.child` element?
```html
<body>
  <!-- case #1 -->
  <div class="parent">
    <div class="child">content</div>
    <div class="child">content</div>
  </div>
  <!-- case #2 -->
  <div class="parent parent-flex">
    <div class="child">content</div>
    <div class="child">content</div>
  </div>
</body>
```
```css
.parent {
  display: block;
}

.parent-flex {
  display: flex;
}
```
In the case #1 the `.child` elements are block-level elements. Their `width` property is equal to the `width` property of the parent element.

In the case #2 the `.child` elements are flex items. Their `width` property is calculated depending on content.

## What is the computed value of the `display` property of the pseudo-elements?
```css
.parent {
  display: inline-grid;
}

.parent::before {
  content: "";
  display: inline;	
}

.parent::after {
  content: "";
  display: flex;
}
```

`block` and `flex`. The `grid` or `inline-grid` values transform `inline-*` values of the `display` property of the child elements to block alternative.

```css
.parent {
  display: inline-grid;
}

.parent::before {
  content: "";
  display: inline; /* display: block will be here */		
}

.parent::after {
  content: "";
  display: flex; /* display: flex will be here */
}
```

## What is the difference between the default position of the child elements in the case with the parent element with `display: flex` and in the case with `display: grid`?

The child elements inside the parent element with `display: flex` display one by one in line.  In contrast, the elements will be displayed one below the other in the case with `display: grid`.

## What is the computed value of the `width` and `height` properties of the `.child` elements?
```html
<body>
  <div class="parent">
    <div class="child">content</div>
    <div class="child">content</div>
  </div>
</body>
```
```css
.parent { 
  display: grid; 
  width: 100rem; 
  height: 20rem; 
}
```

The `width` property of the `.child` element is equal to the `width` property of the parent element. So the computed value of the `width` property is `1600px`.

The `height` property of the child element inside of the parent with `display: grid` fills all space. If the parent has a few items space will be shared between them equally. So the computed value of the `height` property of the child element is `20rem / 2 = 10rem`, i.e `10 * 16 = 160px`.

I use `16px` like a browser's default font size.

## The margins of the `.child` element end up outside of the parent element in all cases. True or false?
```html
<body>
  <div class="parent">
    <div class="child">content</div>
  </div>
</body>
```
```css 
/* case #1 */
.parent {
  display: inline-flex;
}

.child {
  display: block;
  margin-block: 1rem;
}

/* case #2 */
.parent {
  display: grid;
}

.child {
  display: block;
  margin-block: 1rem;
}
```

False. Margins of the child elements don't end up outside the parent element with `display: flex`, `display: inline-flex`, `display: grid` and `display: inline-grid`.

## Does margin collapsing work inside elements with `display: inline-flex` and `display: inline-grid`?

No, it doesn't work. Margins will be summed up inside of the element with `display: flex`, `display: inline-flex`, `display: grid` and `display: inline-grid`.  

## The position of the pseudo-element is centered horizontally and vertically. True or false?
```css 
.container {
  display: grid;
  height: 100dvh;
}

.container::before {
  content: "";
  width: 1rem;
  height: 1rem;
  margin: auto;
}
```

True. Browsers will share all space between the childs and the parent's borders evenly.

## What is the computed value of the `min-width` property?
```html
<body>
  <div class="parent">
    <div class="child">content</div>
  </div>
</body>
```
```css
body {
  display: block;
}

.parent {
  display: grid;
  /* min-width: ? */
}

.child {
  /* min-width: ? */
}
```
The initial `min-width` value is `auto`. So the computed `min-width` value of the `.child` element is `auto`. 

But if the `block`, `inline`, `inline-block`, `table` or `table-*` value is defined for the element the computed `min-width` value of its child elements is `0`.

```css
body {
  display: block;
}

.parent {
  display: grid;
  /* min-width: 0 */
}

.child {
  /* min-width: auto */
}
```

## How can we use the `gap` property to replace the `margin` property?
```html
<body>
  <div class="parent">
    <div class="child">content</div>
  </div>
</body>
```
```css
.parent {
  display: inline-flex;
}

.parent::before,
.parent::after {
  content: "";
  width: 1rem;
  height: 1rem;
  background-color: #222;
} 

.parent::before {
  margin-right: 1rem;
}

.parent::after {
  margin-left: 1rem;
}
```

We should define the `gap` property for the `.parent` element.
```css
.parent {
  display: inline-flex;
  gap: 1rem;
}

.parent::before,
.parent::after {
  content: "";
  width: 1rem;
  height: 1rem;
  background-color: #222;
} 
```

## The computed value of the `display` property is `block`. True or false?
```css
.container {
  position: absolute;
  display: inline;
}
```
True. If the `absolute` or `fixed` value is defined browsers will transform all `inline-*` values of the `display` property to block alternatives.

```css
.container {
  position: absolute;
  display: inline; /* display: block will be here */
}
```

## Why is the computed value of the `height` property of the `.parent` element equal to `0`?
```html
<body>
  <div class="parent">
    <div class="child">content</div>
  </div>
</body>
```
```css
.child {
  position: fixed;
}
```
The element with `position: absolute` or `position: fixed` is removed from the normal document flow. So the parent elements don't see it. It's why the computed value of the `height` property is `0`. 

## What does the `isolation` property do in the following example? 
```html 
<body>
  <div class="parent">
    <div class="child">
      <span>content</span>
    </div>
  </div>
</body>
```
```css 
.parent {
  background-color: purple;
}

.child {
  position: relative;
  isolation: isolate;
}

.child::after {
  content: "";
  background-color: green;
  
  position: absolute;
  inset: 0;
  z-index: -1;
}
```

We should remember which stacking context is used by browsers when using the `z-index` property.  

By default, a root stacking context is the `html` element.  It's why the pseudo-element is behind the `.parent` element without `isolation: isolate`. 

We create a new stacking context with the `isolation` property for the `.child` element. So the pseudo-element displays behind the text but in front of the `.parent` element.

## What is the position of the pseudo-element?
```css
.container {
  display: grid;
  place-items: center;
  position: relative;
  height: 100dvh;
}

.container::before {
  content: "";
  width: 1rem;
  height: 1rem;
  position: absolute;
  bottom: 0;
}
```
First, the pseudo-elements displays in the center because `place-items: center` is applied. 

It shifts by Y axis to the bottom parent border after `position: absolute`, `bottom: 0` are applied because the `top`, `right`, `bottom` and `left` properties are more priority than the `place-items` property.

## What is the computed value of the `width` property?
```css
.container {
  flex-basis: 250px;
  max-width: 225px;
}
```

The `flex-basis` property has priority over the `width` property, but its value must also be in the range of values of the `min-width` and `max-width` properties. So the answer is `225px`.

## What is the computed value of the `padding` property?
```css 
:root {    
  --padding-vertical-start: 1rem;
  --padding-horizontal-end: 2rem;
  --padding-vertical-end: 3rem;
}

.container {
  padding: var(--padding-vertical-start) 
	       var(--padding-horizontal-end) 
	       var(--padding-vertical-end) 
	       var(--padding-horizontal-start);
}
```

We should define all parts of the shorthand when using CSS Custom Properties. If we don't make it browsers can't apply values. 

It happens in our example. The `padding` shorthand requires 4 values. But the developer defined only 3. Browsers can't set paddings. So the computed value is `0`.

## Why will the computed value of the `background-color` property be `green` for the `p` element? 
```css 
body {
  background-color: green;
}

p {
  --background-color: inherit;
  background-color: var(--background-color, inherit);
}
```

A CSS custom property inherits a value from the same custom property defined for parent elements. If a custom property is omitted browsers will use fallback. 

In our example the `--background-color` property is omitted from parent elements. So browsers use the fallback, i.e the `inherit` keyword that inherits the `green` value from the `background-color` property of the `body` element. 

## Make the `scroll-behavior` property safe with vestibular motion disorders.
```css
html {
  scroll-behavior: smooth;
}
```

We should wrap code using the `prefers-reduced-motion` media feature. It'll help to display smooth scrolling only if users allow it in OS settings.
```css
@media (prefers-reduced-motion: no-preference) {

  html {
    scroll-behavior: smooth;
  }
}
``` 

## What is the computed value of the `font-size` property?
```css
html {
  font-size: calc(1rem + 1px);
}
```

Default browser font size is `16px` in most cases. If it isn't changed the computed value of the `font-size` property will be `17px`.
