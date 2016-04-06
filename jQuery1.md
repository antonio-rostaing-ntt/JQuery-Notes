---
title: jQuery. DOM manipulation.
author: Antonio Rostaing
email: arostaing@outlook.com
web: http://arostaing.github.io/jquery-notes
date: January, 2014  
keywords: javascript, jQuery
abstract: Notes from jQuery course from codeschool.
css: jQuery.css
format: complete
---

<article title="Introduction to jQuery" class="static-content">

# Chapter 1: Introduction to jQuery

## What is jQuery?
What you can do with jQuery:

- reveal interface elements  
- change content based on user actions  
- toggle CSS classes to highlight an element  

jQuery make it easy to:

- **find** elements in an HTML document
- **change** HTML content
- **listen** to what a user does and react accordingly
- **animate** content on the page
- **talk** over the network to fetch new content

### Element Selector
Basic jQuery usage:  
`jQuery(<code>);`  

```javascript
jQuery(document); //document -> DOM
```
But how can we search through the DOM?

### Using the jQuery function to find nodes

Selecting by HTML element name...

```javascript
jQuery("h1");
$("h1");

jQuery("p");
$("p");
```

### The text() Method. Changing text.
Fetching an element’s text...
```javascript
$("h1").text()
```

Modifying an element’s text...
```javascript
$("h1").text("Where to?");
```

### DOM Ready
JavaScript may execute before the DOM loads.   
We need to make sure the DOM has finished loading the HTML content before we can reliably use jQuery.  
How can we listen for this signal?  Listening for document ready...  
```javascript
jQuery(document).ready(function(){

  //Will only run this code once the DOM is “ready”

});
```

## Using jQuery
- Download jQuery
- Load it in your HTML document
```javascript
<script src="jquery.min.js"></script>
```
- Start using it
```javascript
<script src="application.js"></script>
```

### Modifying Multiple Elements  
```javascript
// Select all <li> elements
$("li");

// Modifying each of their text nodes
$("li").text("New text");
```

### ID Selector & Class Selector
We can find elements by **ID** or **Class**   

<!--figure-->

| Select | CSS | jQuery |
| --- | --- | --- |
| by node type | p { ... } | $("p"); |
| by ID | #container { ... } | $("#container"); |
| by class | .articles { ... } | $(".articles"); |   

<!--/figure-->

```html
<h1>Where do you want to go?</h1>
<h2>Travel Destinations</h2>
<p>Plan your next adventure.</p>
<ul id="destinations">
  <li>Rome</li>
  <li>Paris</li>
  <li class='promo'>Rio</li>
</ul>
```

Selecting by unique ID:
```javascript
$("#destinations");
```

Selecting by Class Name:
```javascript
$(".promo");
```

</article>

<article title="Traversing the DOM" class="static-content">
# Chapter 2: Traversing the DOM

## Searching the DOM

Descendant Selector:
```javascript
// $("parent descendant")  
$("#destinations li");
```

Selecting only direct children:
```javascript
// $("parent > children")  
$("#destinations > li");
```

Selecting multiple elements:
```html
<h1>Where do you want to go?</h1>
<h2>Travel Destinations</h2>
<p>Plan your next adventure.</p>
<ul id="destinations">
  <li>Rome</li>
  <li>
    <ul id="france">
      <li>Paris</li>
    </ul>
  </li>
  <li class='promo'>Rio</li>
</ul>
```

How do we find only elements with either a *“promo”* class or a *“france”* ID?
```javascript
$(".promo, #france"); // The comma matters
```

Filters:
```javascript
$("#destinations li:first");
$("#destinations li:last");
$("#destinations li:odd");  // watch out, the index starts at 0
$("#destinations li:even");
```

## Traversing: filtering

Filtering by traversing:
It takes a bit more code, but it’s faster
```javascript
$("#destinations li");
// ->
$("#destinations").find("li");
```
```javascript
$("#destinations > li");
// ->
$("#destinations").children("li");
```
```javascript
$("li:first");
// ->
$("li").first();
```
```javascript
$("li:last");
// ->
$("li").last();
```

<!--figure-->

jQuery | Action
--- | ---
.eq() | Reduce the set of matched elements to the one at the specified index.
.filter() | Reduce the set of matched elements to those that match the selector or pass the function’s test.
.first() | Reduce the set of matched elements to the first in the set.
.has() | Reduce the set of matched elements to those that have a descendant that matches the selector or DOM element.
.is() | Check the current matched set of elements against a selector, element, or jQuery object and return true if at least one of these elements matches the given arguments.
.last() | Reduce the set of matched elements to the final one in the set.
.map() | Pass each element in the current matched set through a function, producing a new jQuery object containing the return values.
.not() | Remove elements from the set of matched elements.
.slice() | Reduce the set of matched elements to a subset specified by a range of indices.

<figcaption>
[<b>jQuery documentation</b> | Category: Filtering](https://api.jquery.com/category/traversing/filtering)
</figcaption>

<!--/figure-->

## Traversing: Walking the DOM.
```javascript
$("li").first();
$("li").first().next();
$("li").first().next().prev();
```

Traversing Up: Walking up the DOM.
```javascript
$("li").first();
$("li").first().parent(); // retur <ul> element
```

Traversing Down: Walking down the DOM.
*children()*, unlike *find()*, only selects direct children
```javascript
$("#destinations").children("li"); // return first level <li> elements
```

<!--figure-->

jQuery | Action
--- | ---
.children() | Get the children of each element in the set of matched elements, optionally filtered by a selector.
.closest() | For each element in the set, get the first element that matches the selector by testing the element itself and traversing up through its ancestors in the DOM tree.
.find() | Get the descendants of each element in the current set of matched elements, filtered by a selector, jQuery object, or element.
.next() | Get the immediately following sibling of each element in the set of matched elements. If a selector is provided, it retrieves the next sibling only if it matches that selector.
.nextAll() | Get all following siblings of each element in the set of matched elements, optionally filtered by a selector.
.nextUntil() | Get all following siblings of each element up to but not including the element matched by the selector, DOM node, or jQuery object passed.
.offsetParent() | Get the closest ancestor element that is positioned.
.parent() | Get the parent of each element in the current set of matched elements, optionally filtered by a selector.
.parents() | Get the ancestors of each element in the current set of matched elements, optionally filtered by a selector.
.parentsUntil() | Get the ancestors of each element in the current set of matched elements, up to but not including the element matched by the selector, DOM node, or jQuery object.
.prev() | Get the immediately preceding sibling of each element in the set of matched elements, optionally filtered by a selector.
.prevAll() | Get all preceding siblings of each element in the set of matched elements, optionally filtered by a selector.
.prevUntil() | Get all preceding siblings of each element up to but not including the element matched by the selector, DOM node, or jQuery object.
.siblings() | Get the siblings of each element in the set of matched elements, optionally filtered by a selector.

<figcaption>
[<b>jQuery documentation</b> | Category: Traversing](https://api.jquery.com/category/traversing/)
</figcaption>

<!--/figure-->

</article>

<article title="Working with the DOM" class="static-content">
# Chapter 3: Working with the DOM

## Manipulating the DOM

### Appending to the DOM
Create a node (but doesn't add it to the DOM)
```javascript
var price = $('<p>From $399.99</p>');
```

Ways to add this *price* node to the DOM

<!--figure-->

jQuery | Action
--- | ---
$(\<#node\>).**before**(\<elementToAppend\>) | Puts the \<elementToAppend\> node before \<#node\>
$(\<#node\).**after**(\<elementToAppend\>) | Puts the \<elementToAppend\> node after \<#node\>
$(\<#node\>).**prepend**(\<elementToAppend\>) | Adds the \<elementToAppend\> node to the top of \<#node\>
$(\<#node\>).**append**(\<elementToAppend\>) | Puts the \<elementToAppend\> node at the bottom of \<#node\>

<figcaption>
Existing node as reference
</figcaption>
<!--/figure-->

<!--figure-->

jQuery | Action
--- | ---
$(\<elementToAppend\>).**insertBefore**(\<#node\>) | Puts the \<elementToAppend\> node before \<#node\>
$(\<elementToAppend\>).**insertAfter**(\<#node\>) | Puts the \<elementToAppend\> node after \<#node\>
$(\<elementToAppend\>).**prependTo**(\<#node\>) | Adds the \<elementToAppend\> node to the top of \<#node\>
$(\<elementToAppend\>).**appendTo**(\<#node\>) | Puts the \<elementToAppend\> node at the bottom of \<#node\>

<figcaption>
Node to append as reference
</figcaption>
<!--/figure-->

```javascript
$(document).ready(function()

  //Create a node
  var price = $('<p>From $399.99</p>');

  // Puts the price node before .vacation
  $('.vacation').before(price);
  // or
  price.insertBefore($('.vacation'));

  // Puts the price node after .vacation
  $('.vacation').after(price);
  // or
  price.insertAfter($('.vacation'));

  // Adds the node to the top of .vacation
  $('.vacation').prepend(price);
  // or
  price.prependTo($('.vacation'));

  // Puts the price node at the bottom of .vacation
  $('.vacation').append(price);
  // or
  price.appendTo($('.vacation'));

});
```

### Removing from the DOM

jQuery | Action
--- | ---
$('#node').**remove**() | Remove <#node>


## Acting on Interaction
jQuery Object Methods  
`.on(<event>, <event handler>)`

The <event handler> function contains a reference to the object that invoke the function...  
`$(this)`

### Tackling the HTML
All data attributes begin with ‘data-’
```html
<li class="vacation onsale" data-price='399.99'>
  <h3>Hawaiian Vacation</h3>
  <button>Get Price</button>
  <ul class='comments'>
    <li>Amazing deal!</li>
    <li>Want to go!</li>
  </ul>
</li>
```
jQuery Object Methods  
`.data(<name>)`  
`.data(<name>, <value>)`  
```javascript
$('.vacation').first().data('price');
"399.99"
```

### Watching for Click
```javascript
$('button').on('click', function() {
  // runs when any button is clicked
});
```
**On With a Selector**  
If we add new buttons anywhere, they will trigger this click handler.
```javascript
$('.vacation').on('click', 'button', function() {
  // Only target a ‘button’ if it’s inside a ‘.vacation’.
});
```

### Class manipulation  
`.addClass(<class>)`  
`.removeClass(<class>)`
```javascript
$('.vacation').filter('.onsale').addClass('highlighted');
```

</article>

<article title="Listening to DOM events" class="static-content">

# Chapter 4: Listening to DOM events

## Show and Hide

<!--figure-->

jsQuery | Action
--- | ---
.slideDown() | Display the matched elements with a sliding motion.
.slideToggle() | Display or hide the matched elements with a sliding motion.
.slideUp() | Hide the matched elements with a sliding motion.

<figcaption>
[<b>jQuery documentation</b> | Category: Sliding](https://api.jquery.com/category/effects/sliding/)
</figcaption>

<!--/figure-->

```html
<li class="confirmation">
  ...
  <button>FLIGHT DETAILS</button>
  <ul class="ticket">...</ul>
</li>
```
```CSS
.ticket {
  display: none;
}
```
```javascript
$('.confirmation').on('click', 'button', function() {
  $(this).closest('.confirmation').find('.ticket').slideDown();
});
```

<!--figure-->

jsQuery | Action
--- | ---
.fadeIn() | Display the matched elements by fading them to opaque.
.fadeOut() | Hide the matched elements by fading them to transparent.
.fadeTo() | Adjust the opacity of the matched elements.
.fadeToggle() | Display or hide the matched elements by animating their opacity.

<figcaption>
[<b>jQuery documentation</b> | Category: Fading](https://api.jquery.com/category/effects/fading/)
</figcaption>

<!--/figure-->

## Mouse events

<!--figure-->

jQuery | Action
--- | ---
.click() | Bind an event handler to the “click” JavaScript event, or trigger that event on an element.
.dblclick() | Bind an event handler to the “dblclick” JavaScript event, or trigger that event on an element.
.hover() | Bind one or two handlers to the matched elements, to be executed when the mouse pointer enters and leaves the elements.
.mousedown() | Bind an event handler to the “mousedown” JavaScript event, or trigger that event on an element.
.mouseenter() | Bind an event handler to be fired when the mouse enters an element, or trigger that handler on an element.
.mouseleave() | Bind an event handler to be fired when the mouse leaves an element, or trigger that handler on an element.
.mousemove() | Bind an event handler to the “mousemove” JavaScript event, or trigger that event on an element.
.mouseout() | Bind an event handler to the “mouseout” JavaScript event, or trigger that event on an element.
.mouseover() | Bind an event handler to the “mouseover” JavaScript event, or trigger that event on an element.
.mouseup() | Bind an event handler to the “mouseup” JavaScript event, or trigger that event on an element.
.toggle() | Bind two or more handlers to the matched elements, to be executed on alternate clicks.

<figcaption>
[<b>jQuery documentation</b> | Category: Mouse Events](https://api.jquery.com/category/events/mouse-events/)
</figcaption>

<!--/figure-->

## Keyboard events

<!--figure-->

jQuery | Action
--- | ---
.keydown() | Bind an event handler to the “keydown” JavaScript event, or trigger that event on an element.
.keypress() | Bind an event handler to the “keypress” JavaScript event, or trigger that event on an element.
.keyup() | Bind an event handler to the “keyup” JavaScript event, or trigger that event on an element.

<figcaption>
[<b>jQuery documentation</b> | Category: Keyboard Events](https://api.jquery.com/category/events/keyboard-events/)
</figcaption>

<!--/figure-->

## Form events

<!--figure-->

jQuery | Action
--- | ---
.blur() | Bind an event handler to the “blur” JavaScript event, or trigger that event on an element.
.change() | Bind an event handler to the “change” JavaScript event, or trigger that event on an element.
.focus() | Bind an event handler to the “focus” JavaScript event, or trigger that event on an element.
.focusin() | Bind an event handler to the “focusin” event.
.focusout() | Bind an event handler to the “focusout” JavaScript event.
.select() | Bind an event handler to the “select” JavaScript event, or trigger that event on an element.
.submit() | Bind an event handler to the “submit” JavaScript event, or trigger that event on an element.

<figcaption>
[<b>jQuery documentation</b> | Category: Form Events](http://api.jquery.com/category/events/form-events/)
</figcaption>

<!--/figure-->

```html
<div class="vacation" data-price='399.99'>
  <h3>Hawaiian Vacation</h3>
  <p>$399.99 per ticket</p>
  <p>
    Tickets:
    <input type='number' class='quantity' value='1' />
  </p>
</div>
<p>Total Price: $<span id='total'>399.99</span></p>
```
```javascript
$(document).ready(function() {
  $('.vacation').on('keyup', '.quantity', function() {

    // Get the price for this vacation
    // Use + to convert the string to a number
    var price = +$(this).closest('.vacation').data('price');

    // Get the quantity entered
    // Use + to convert the string to a number
    var quantity = +$(this).val();

    // Set the total to price * quantity
    $('#total').text(price * quantity);

  });
});
```

## Lynk Layover

```CSS
.comments {
  display: none;
}
```
```html
<ul>
  <li class="vacation" data-price='399.99'>
    <a href='#' class='expand'>Show Comments</a>
    <ul class="comments">
      ...
    </ul
  </li>
</ul>
```
```javascript
$(document).ready(function() {
  $('.vacation').on('click', '.expand', function() {
    // Find the comments ul
    var comment = $(this).closest('.vacation').find('.comments')
    // Show the comments ul
    comment.fadeToggle();
  });
});
```
Why does the page jump to the top?  

- The click event will “bubble up” to each parent node
- Follows the link! (goes to the top of the page)

Solution: Add the event parameter  

```javascript
$(document).ready(function() {
  $('.vacation').on('click', '.expand', function(event) {

    // Will not work: The browser will still handle the click event
    // but will prevent it from “bubbling up” to each parent node.
    event.stopPropagation();

    // Will work!
    // The click event will “bubble up” but the browser won’t handle it
    event.preventDefault();

    $(this).closest('.vacation').find('.comments').fadeToggle();

  });
});
```

</article>

<article title="CSS manipulation" class="static-content">

# Chapter 5: CSS manipulation

## Taming CSS

To style something based on user interaction, which would we use?  

Separation of concerns  
* <em>content</em>: <strong>HTML</strong>  
* <em>style</em>: <strong>CSS</strong>  
* <em>behaviour</em>: <strong>javaScript</strong>   

### Changing the style

```html
<div id="vacations">
  <ul>
    <li class="vacation">
      <p class="price">
      ...
      </p>
    </li>
  </ul>
</div>
```

jQuery Object Methods  
- `.css(<attr>, <value>)`  
- `.css(<attr>)`  
- `.css(<object>)`  

```javascript
$(document).ready(function() {
  $('#vacations').on('click', '.vacation', function() {

    $(this).css('background-color', '#252b30');
    $(this).css('border-color', '1px solid #967');

    // Is equivalent to

    $(this).css({'background-color': '#252b30',
                 'border-color': '1px solid #967'});

    // Passing in a JavaScript Object as an argument
    // is a common jQuery pattern
  });
});
```

jQuery Object Methods  
- `.show()`  
- `.hide()`  

```javascript
$(this).find('.price').css('display', 'block');

// Same as CSS syntax, but easier to read and understand
$(this).find('.price').show();
```

### Moving Styles to external CSS

jQuery Object Methods  
- `.toggleClass()`  
- `.addClass(<class>)`  
- `.removeClass(<class>)`  

Bad practice: Mixing behaviour and style...  

```javascript
$(document).ready(function() {
  $('#vacations').on('click', '.vacation', function() {
    /// Can we move these to a CSS stylesheet?
    $(this).css({'background-color': '#563',
                 'border-color': '1px solid #967'});
    $(this).find('.price').show();
  });
});
```

Good practice: Separation of concerns: css + javascript  
It’s now much easier to manipulate with external CSS styles  

```css
.highlighted {
  background-color: #563;
  border-color: 1px solid #967;
}
.highlighted .price {
  display: block;
}
```

```javascript
$(document).ready(function() {
  $('#vacations').on('click', '.vacation', function() {
    $(this).addClass('highlighted');
  });
});
```

We can only show price, but how would we hide price?  
`toggleClass`  
Adds the class if \$(this) doesn’t have it, removes it if \$(this) already has it
```javascript
  $(this).toggleClass('highlighted');
```

## Animation

### Adding Movement

```html
<div id="vacations">
  <ul>
    <li class="vacation">
      <p class="price">
      ...
      </p>
    </li>
  </ul>
</div>
```

```css
.highlighted {
  background-color: #563;
  border-color: 1px solid #967;
}
.highlighted .price {
  display: block;
}
```

jQuery Object Methods     
- `.animate(<object>)`   

Takes in a JavaScript object similar to the .css() method.  
Will adjust a CSS property pixel by pixel in order to animate it.  
<i>“If statements”</i> allow your code to make decisions at runtime.  

```javascript
if(<vacation has the class highlighted>) {  
	// animate the vacation up  
	// set 'top' property to '-10px'  
}  
else {  
	// animate the vacation back down  
	// 'top' to '0px' if a second click occurs  
}  
```

```javascript
$(document).ready(function() {
	$('#vacations').on('click', '.vacation', function() {
		$(this).toggleClass('highlighted');

		if($(this).hasClass('highlighted')) {
			$(this).animate({'top': '-10px'});
		} else {
			$(this).animate({'top': '0px'});
		}
	});
});
```

### Changing the speed
We can optionally pass in the speed as a second argument to animate().  
Efects methods like animate(), slideToggle() and fadeToggle() can also be given a specific speed as a String or in milliseconds.

```javascript
$(this).animate({'top': '-10px'});
$(this).animate({'top': '-10px'}, 400);

$(this).animate({'top': '-10px'}, 'fast');
$(this).animate({'top': '-10px'}, 200);

$(this).animate({'top': '-10px'}, 'slow');
$(this).animate({'top': '-10px'}, 600);
```

### Moving Animations to external CSS
Isn’t this still styling? Shouldn’t it be inside of a stylesheet?
```javascript
$(document).ready(function() {
	$('#vacations').on('click', '.vacation', function() {
		$(this).toggleClass('highlighted');
	});
});
```
```css
.vacation {
	// Will only work with browsers that implement CSS transitions
	transition: top 0.2s;

	// Unlike jQuery, with CSS we have to account for specific browsers
	-moz-transition: top 0.2s;
	-o-transition: top 0.2s;
	-webkit-transition: top 0.2s;
}
.highlighted {
	top: -10px;
}
```

</article>

<article title="References" class="static-content">

# References

</article>
