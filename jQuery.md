jQuery
======

## CSS-like selector

### $("selector")
Select an element according to a css selector (element name, #id, .class, etc.)

### $("selector children")
Select a children element

### $("selector > children")
Select a direct descendant

### $("selector, selector")
Select multiple elements with different rules

### $("selector:first, selector:last")
Select the first and last corresponding elements

### $("selector:odd, selector:event")
Select odd or even elements (first is 0, so even)


## Methods selectors (faster)

### $("ul").find("li")
Select all li children elements in ul element

### $("selector").first() / .last()
Select the first / last corresponding element

### $("element").prev() / .next()
Select the previous / next element

### $("element").parent()
Select the direct ancester element

### $("ul").children("li")
Select only direct li descendants of ul (unlike **.find()**)

### $("element").parents("li")
Select all ancesters elements to li

### $("element").closest("li")
Select the closest ancester element to li

### $(".class1").filter(".class2")
Select all classes that have .class1, then filter to keep only those who have also .class2


## DOM manipulation

### var element = $("&lt;li&gt;item&lt;/li&gt;")
Create a new HTML element in the DOM

### $("ul").prepend(element) / .append(element)
Place element as first / last children of ul

### $("p").after(element) / .before(element)
Place element before of after p

### element.prependTo($("ul")) / .appendTo($("ul"))
Place element as first / last children of ul

### element.insertAfter($("p")) / .insertBefore($("p"))
Place element before of after p

### $("element").remove()
Remove #element form the DOM

### $("element").attr('attr') / .attr('attr', 'val')
Set / get element's attribute attr with value val

### $("element").data('key') / .data('key', 'val')
Set / get element's data-key attribute with value val

### $("element").html() / .html(value)
Set / get element content
* value can either be raw HTML code, a jQuery object or an array of jQuery object

### $("element").empty()
Remove all HTML inside *element*

### $("element").detach()
Temporarly remove an element from the DOM for quicker manipulation. Needs to be reinsereted into the DOM after


## Classes/CSS manipulation

### $("element").addClass('classe')
Add a class to the element

### $("element").removeClass('classe')
Remove an element's class

### $('element').toggleClass('class')
Toggle an element's class

### $('element').hasClass('class')
Return true if element has the specified class

### $("element").css('background-color', '####ff6600')
Change the element's backgroud-color css property

### $("element").css({'background-color': '####ff6600'})
Change the element's backgroud-color css property using an object


## Animate

### $("element").animate({'top': '-10px'}, speed)
Same as CSS but animate the change
* speed: optional animation duration in millisecond (default: 400, slow: 200, fast: 400)

### $("element").show() / .hide() / .toggle()
Show / hide / toggle element

### $("element").slideDown() / .slideUp() / .slideToggle() 
Show / hide / toggle element with a slide effect

### $("element").fadeIn() / .fadeOut() / .fadeToggle()
Show / hide / toggle element with a fade effect


## Events

### $("element").on('event.ns', handler)
Call *handler* function when *event* occures on *element*
* event : the event listened to (among events listed above)
* .ns : the namespace for this event (useful for **.off()** and **.trigger()**)

### $(".class").on('event.ns', 'element', handler
Call handler function when event occures on *element* if it is located in an *.class* element. Useful if inner element is loaded via AJAX after initial page load.

### $("element").off("event.ns")
Deactivate the *event* event with namespace *ns* for *element*
* .ns : the namespace of the event, given by **.on("event.ns")**
* .off('click') : remove all click event for this element
* .off('.ns') : remove all event with namespace ns for this element

### $("element").trigger("event.ns")
Trigger *event* on *element* as if it really occured. Can be used to create custom events with custom events name, that will be caught by **.on()**

### $(this)
Select the triggered element inside of the callback function

### event.preventDefault()
Prevent the event's default behavior to be triggered

### Mouse events
* click/dblclick  
* focusin/focusout  
* mousedown/mouseup
* mouseenter/mouseleave
* mousemove

### Keyboard events
* keypress
* keydown/keyup

### Form events
* blur/focus
* select/change
* submit


## Ajax

### $.ajax('url', options)  
Make an ajax request. *options* is a javascript object with the values:
* **data**: a JS object for GET parameters
* **success(response)**: called if the request was a success
* **error(request, errorType, errorMessage)***: called if the request didn't succeeded
* **timeout**: Time before timeout in millisecondes
* **beforeSend()**: called before sending the request (useful for loading message)
* **complete()**: called after the request, failed ou succesful (use ful for hiding loading message)
* **context**: set the value of *this* inside callback functions
* **type**: request method (POST or default GET)
* **dataType**: 'json' parses the response as JSON
* **contentType**: 'application/json' asks the server for JSON

### $.get('url', success)
Shorthand method for **.ajax()** with type GET
* success(): a function called if the request was a success

### $.getJSON('url', success)
Shorthand method for **.get()** with dataType json and contentType application/json
* success(): a function called if the request was a success

## Forms

### $('form').serialize()
Get all form fields and value as a JS object


## Utility methods

### $.each(collection, function(index, object))
Iterates on each item in the collection and send it as object in the callback function

### $.map(collection, function(item, index))
Returns a new array after having executed the function on each item in the collection

### $(".class").each( function() { ... } )
Iterates on each element contained in the jQuery object (each element with *.class.* class and execute callback function with this as the iterated element.

### $.extend(target, object)
Will combine one or more *objects* with the *target* object. If a key is already set in *target*, it will be replaced. Allow to easily defined and override default values.


## Plugins

### $.fn.plugin = function() {}
Create a plugin named *plugin()* that can be used on a jQuery element like *$("element").plugin()*. This in the callback function will refer to the element on which the plugin function was called.














End of file
