---
title: jQuery. Ajax
author: Antonio Rostaing
email: arostaing@outlook.com
web: http://arostaing.github.io/jquery-notes
date: January, 2014  
keywords: javascript, jQuery
abstract: Notes from jQuery course from codeschool.
css: jQuery.css
format: complete
---

<article title="AJAX Basic">

# Chapter 1: AJAX Basic

## Ajax call example
AJAX: Asynchronous Javascript And XML.  
A client side technique for communicating with a web server.  

**jQuery Methods**   
`$.ajax(url[, settings])`

```javascript
$('.confirmation').on('click', 'button', function(){

  // AJAX call
  $.ajax('http://example.org/confirmation.html', {

     // sucess: run only when the server return a successful response
     success: function(response) {
       $('.ticket').html(response).slideDown();
     }

  });

});
```

`response`: The response which get returned. Not a full page.   

```html
<!-- http://example.org/confirmation.html-->
￼<div>
  <strong>Boarding Pass: </strong>
  <a href='#' class='view-boarding-pass'>View Boarding Pass</a>
  <img src='ticket.png' alt='Your boarding pass' class='boarding-pass' />
</div>
```

## Shorthand with $.get

**jQuery Methods**   
`$.get(url, success)`   

```javascript
// Equivalent to previous call
￼￼￼$.get('http://example.org/confirmation.html', function(response) {
    $('.ticket').html(response).slideDown();
});
```

## Sending parameters with requests

```javascript
// How would we do a request with a Confirmation Number?
// ￼confirmation.html?confNum=1234

$.ajax('confirmation.html?confNum=1234', { success: function(response) {
     $('.ticket').html(response).slideDown();
   }
});

// Equivalent to $.ajax call above
$.ajax('confirmation.html', {
   success: function(response) {
     $('.ticket').html(response).slideDown();
   },
   data: { "confNum": 1234 }   // JavaScript object!
});
```

Often the data in the request is dynamic. The `confNum` could be pulled from the HTML.   

```html
<div class='ticket' data-confNum='1234'>
```

```javascript
$.ajax('confirmation.html', {
     success: function(response) {
       $('.ticket').html(response).slideDown();
     },
     data: { "confNum": $(".ticket").data("confNum")
     }
});
```

</article>

<article title="Ajax options">

# Chapter 2: Ajax options

## Handling failed AJAX request

Member | Action
--- | ---
success | Run only when the server return a successful response
error | Runs this callback if there is a timeout, abort or server error
timeout | Setting the timeout speed
beforeSend | Runs before the Ajax request
complete | Runs after both success and error

```javascript
$('.confirmation').on('click', 'button', function(){
  $.ajax('confirmation.html', {
    success: function(response) {
      // Run only when the server return a successful response
      $('.ticket').html(response).slideDown();
    },
    error: function(request, errorType, errorMessage) {
      // Runs this callback if there is a timeout, abort or server error
      alert('Error: ' + errorType + ' with message: ' + errorMessage);
    },
    timeout: 3000, // In ms
    beforeSend: function() {
      // Runs before the Ajax request
      $('.confirmation').addClass('is-loading'); // Add a loading animation
    },
    complete: function() {
      // Runs after both success and error
      $('.confirmation').removeClass('is-loading'); // Remove loading animation
    }
  });
});
```

## Using event delegation

```html
<!-- http://example.org/confirmation.html -->
<div> ...
   <strong>Boarding Pass: </strong>
   <a href='#' class='view-boarding-pass'>View Boarding Pass</a>
   <img src='ticket.png' alt='Your boarding pass' class='boarding-pass' />
</div>
```

```javascript
// Do NOT work!
$('.confirmation .view-boarding-pass').on('click', function(){ ... });
```

This was run once when the page loaded.  
But when the page loaded `.view-boarding-pass` didn’t exist.  
It only existed after our AJAX request.  

```javascript
// Work!
$('.confirmation').on('click', '.view-boarding-pass', function(){ ... });
```
Listen for click events inside `.confirmation`   
When they happen, check if the target was `.view-boarding-pass`

</article>

<article title="Refactoring to Objects">

# Chapter 3: Refactoring to Objects

How can we organize this?   
That’s a big document ready method!   

```javascript
$(document).ready(function() {

  ￼￼$('.confirmation').on('click', 'button', function() {
    $.ajax('confirmation.html', {
      timeout: 3000,
      success: function(response) {
        $('.ticket').html(response).slideDown();
      },
      error: function(request, errorType, errorMessage) {
        alert('Error: ' + errorType + ' with message: ' + errorMessage);
      },
      beforeSend: function() {
        $('.confirmation').addClass('is-loading');
      },
      complete: function() {
        $('.confirmation').removeClass('is-loading');
        }
    });
  });

  $('.confirmation').on('click', '.view-boarding-pass', function(event) {
    event.preventDefault();
    $('view-boarding-pass').hide();
    $('boarding-pass').show();
  });

});
```

Within init, we can run all the code that was in our document ready function.  
Much easier to read what happens on load.   

```javascript
var confirmation = {
  init: function() {

    ￼￼￼￼// Our existing event handlers

    $('.confirmation').on('click', 'button', function() {
      $.ajax('confirmation.html', {
        //...
      });
    });

    $('.confirmation').on('click', '.view-boarding-pass', function(event) {
      //...
    });

  }
￼};

//Kick things off on page load
(document).ready(function() {
  confirmation.init();
￼});
```

We can also put functions out of `init` to make easier to read.   

```javascript
var confirmation = {
  init: function() {
    $('.confirmation').on('click', 'button', this.loadConfirmation);
    $('.confirmation').on('click', '.view-boarding-pass', this.showBoardingPass);
  },
  loadConfirmation: function() {
    $.ajax('confirmation.html', { ... });
  },
  showBoardingPass: function(event) { ... }
};

$(document).ready(function() {
  confirmation.init();
});
```
</article>

<article title="Refactoring to Fuctions">

# Chapter 4: Refactoring to Functions

## Object vs Functions
- **Object**: Limited to 1 confirmation on the page
- **Function**: Can have multiple vacations

```javascript
//  Object
var vacation = {
  init: function() {
    // init vacation
  }
};

$(document).ready(function() {
    vacation.init();
});

// Function
function Vacation(destination) {
    // init vacation to destination
}

$(document).ready(function() {
    var paris = new Vacation('Paris');
    var london = new Vacation('London');
});
```

## Refactoring code

Lets move this object into a reusable function.   

```javascript
var confirmation = {
    init: function() {
        $('.confirmation').on('click', 'button', this.loadConfirmation);
        $('.confirmation').on('click', '.view-boarding-pass', this.showBoardingPass);
    },
    loadConfirmation: function() { ... }
    showBoardingPass: function(event) { ... }
};

$(document).ready(function() {
    confirmation.init();
});
```

Refactoring...   

```javascript
// 'el' refers to the element
function Confirmation (el) {

    //Save a reference to the passed in element
    this.el = el;

    // Fixing the target element
    this.ticket = this.el.find('.ticket');

    // helper methods go here
    this.loadConfirmation = function() {
        $.ajax('confirmation.html', {
        timeout: 3000,
        success: function(response) {

            // Be careful! Sometimes jQuery changes the value of ‘this’
            // Inside AJAX callbacks, this is set to the AJAX settings.
            // This refers to ajax settings and not our function’s el
            // So...  The next line of code will not work
            this.ticket.html(response).slideDown();
        }
     });
}

this.showBoardingPass = function(event) { ... }

// event handlers go here
this.el.on('click', 'button', this.loadConfirmation);
this.el.on('click', '.view-boarding-pass', this.showBoardingPass);
}

$(document).ready(function() {
    // Create a new confirmtion for each ticket
    var paris = new Confirmation($('#paris'));
    var london = new Confirmation($('#london'));
});

```

## Setting the proper `this`   

`context` Allows us to set the value of “this” inside our callbacks.   

```javascript
//...
this.el = el;
this.ticket = this.el.find('.ticket');

// Allows us to set the value of “this” inside our callbacks.
var confirmation = this;

this.loadConfirmation = function() {

    $.ajax('confirmation.html', {
      timeout: 3000,
      context: confirmation,
      success: function(response) {
        this.ticket.html(response).slideDown();
      }
    });
}
```

## Refactoring hardcoded DOM elements   

```javascript
// Run when the link is clicked
this.showBoardingPass = function(event) {

  event.preventDefault();

  // In the context of an event callback ‘this’ is set to the dom object
  console.log(this);

  // References the confirmation object in the parent scope
  console.log(confirmation);
}
```

```html
<a href="#" class="view-boarding-pass" style="display: none;">View Boarding Pass</a>
```

```javascript
Confirmation {
  loadConfirmation:
  function, showBoardingPass:
  function, el:b.fn.b.init[1],
  ticket: b.fn.b.init[1]
}
```

Refactoring to functions.   
Remember, `this` will be the link that was clicked.   
Able to use variables from the parent scope.   

```javascript
this.showBoardingPass = function(event) {
  event.preventDefault();
  $(this).hide();
  confirmation.el.find('.boarding-pass').show();
}
```
￼
</article>

<article title="Ajax Forms">

# Chapter 5: Ajax Forms

Submiting a form via AJAX:

```html
<form action='/book'>
  <select id='destination' name='destination'>
  ...
  </select>
  <input type='text' id='quantity' name='quantity' value='1' />
</form>
```

Will do a POST request to the server, which is used for forms.

```javascript
$('form').on('submit', function(e) {

  // Prevents browser from submitting
  event.preventDefault();

  $.ajax('/book', {
    type: 'POST'
  });
)
```

This would do the AJAX request, but it’s not sending any form data over.   
How would we pass the values of this form via `$.ajax`?

```javascript
$('form').on('submit', function(event) {
  event.preventDefault();
  $.ajax('/book', {
    type: 'POST'
    data: { "destination": $('#destination').val(),
            "quantity": $('#quantity').val() }
  });
});
```

Works, but there’s a better way...   

**jQuery Object Methods**   
`serialize()` Used to merge all form fields for submission

```javascript
$('form').on('submit', function(event) {
  event.preventDefault();
  var form = $(this);
  $.ajax('/book', {
    type: 'POST',
    data: form.serialize(),
    success: function(result) {
      // Like our previous $.ajax request, success is called with the HTML result
      form.remove();
      $('#vacation').hide().html(result).fadeIn();
    }
  });
});
```

</article>

<article title="Ajax with JSON">

# Chapter 6: Ajax with JSON

```javascript
$('form').on('submit', function(event) {
  event.preventDefault();
  var form = $(this);
  $.ajax('/book', {
    type: 'POST',
    data: form.serialize(),

    // Parse the response as JSON
    dataType: 'json',

    // Ask the server to respond with JSON
    contentType: 'application/json',

    success: function(result) {
      // Result isn’t HTML, we’ll need to create a DOM node here
      form.remove();
      $('#vacation').hide().html(result).fadeIn();
    }
  });
});
```

##Reading the JSON result and creating HTML

`result`: JSON is converted into a JavaScript Object.

```javascript
{
 location: 'Paris',
 totalPrice: 1196.00,
 nights: 4,
 confirmation: '345feab'
 }
```

Create and add a new DOM node.   

```javascript
success: function(result) {
  form.remove();
  var msg = $("<p></p>");
 msg.append("Destination: " + result.location + ". ");
 msg.append("Price: " + result.totalPrice + ". ");
 msg.append("Nights: " + result.nights + ". ");
 msg.append("Confirmation: " + result.confirmation+ ".");

 $('#vacation').hide().html(msg).fadeIn();
}
```

```html
<p>Destination: Paris. Price: $2196.00. Nights: 4. Confirmation: 345feab.</p>
```

*Don´t repeat yourself!*   
Form URL is in both HTML and JavaScript   

```html
<form action='/book' method='POST'>
  ...
</form>
```
```javascript
    $.ajax('/book', { ... });
```

**jQuery Object Methods**    

- `attr(<attribute>)`   
- `attr(<attribute>, <value>)`   

Will use the action from the form HTML   

```javascript
$.ajax($('form').attr('action'),{ ... });
```

</article>

<article title="Chapter 7: Utility Methods">

# Chapter 7: Utility Methods

## Multiple results from JSON

Result is now an array of objects   

```javascript
[
  {
    image: "images/paris.png",
    name: "Paris, France",
  },
  {
    image: "images/london.png",
    name: "London, UK"
  },
  {
    image: "images/madrid.png",
    name: "Madrid, Spain"
  }
]
```

Need to ‘loop’ over each favorite   

**Utility Method**   
`$.each(collection, function(<index>, <object>) {})`   

Calls the function for each object in the collection   

```javascript
success: function(result) {
   $.each(result, function(index, city) {
     var favorite = $('.favorite-' + index);
     favorite.find('p').html(city.name);
     favorite.find('img')
             .attr('src', city.image);
   }
}
```
```html
<div class='favorite-0'>
 ...
</div>
<div class='favorite-1'>
 ...
</div>
<div class='favorite-2'>
 ...
</div>
```

## Transforming an array of objects into HTML

**Utility Method**   
`$.getJSON(url, success);`   

Equivalent calls...   

```javascript
$('.update-status').on('click', function() {
  $.ajax('/status', {
    contentType: 'application/json',
    dataType: 'json',
    success: function(result) {
      // result will be an array of JavaScript Objects
      ...
    }
  });
});
```

```javascript
   $.getJSON('/status', function(result) {
     // result will be an array of JavaScript Objects
     ...
   });
```

```javascript
[
  {
    name: 'JFK - New York, NY',
    status: 'Departing Location'
  },
  {
    name: 'DEN - Denver, CO',
    status: 'Connecting Flight'
  },
  {
    name: 'SFO - San Francisco, CA',
    status: 'Destination'
  }
]
```

We need a good way to create an array of list item HTML elements from the result of the AJAX call.   

```html
<li>
  <h3>name</h3>
  <p>status</p>
</li>

<li> ... </li>  

<li> ... </li>
```

**Utility Method**   
`$.map(collection, function(<item>, <index>){});`   

You can think of a collection as an array.   
Map returns an array modified by what is returned in the function passed as an argument.   

```Javascript
// myNumbers -> [1,2,3,4]
var myNumbers = [1,2,3,4];
// newNumbers -> [2,3,4,5]
var newNumbers = $.map(myNumbers, function(item, index){ return item + 1 });
```

```javascript
$.map(result, function(status, i) {
  var listItem = $('<li></li>');
  $('<h3>'+status.name+'</h3>').appendTo(listItem);
  $('<p>'+status.status+'</p>').appendTo(listItem);
  return listItem;
}
```

So, the complete code:

```javascript
$('.update-flight-status').on('click', function() {
   $.getJSON('/status', function(result) {
     var statusElements = $.map(result, function(status, i) {
       var listItem = $('<li></li>');
       $('<h3>'+status.name+'</h3>').appendTo(listItem);
       $('<p>'+status.status+'</p>').appendTo(listItem);
       // Make sure to return the list item.
       return listItem;
     });
     // statusElements is an array list items
     $('.status-list').html(statusElements);
   });
});
```

## $.each vs $.map
So what’s the difference between $.each and $.map?

- `$.each` runs the function for each item in the array, but returns the original array unchanged.
- `$.map` runs the function for each item in the array and creates a new array from the returned results.

## Give the DOM a break with .detach()  
**Utility Method**   
`detach()`   

Removes an element from the DOM, preserving all data and events.     
This is useful to minimize DOM insertions with multiple html elements.  

```javascript
$('.update-flight-status').on('click', function() {
   $.getJSON('/status', function(result) {
     ...
   });

    // .detach() removes the list from the DOM,   
    // then it can be modified and reinserted into the status element.
    $('.status-list').detach()
                      .html(statusElements)
                      .appendTo('.status');
   });

});
```

</article>

<article title="Advanced Events">

# Chapter 8: Advanced Events

</article>
