jQuery
======

## CSS-like selector

**$("selector")**  
Select an element according to a css selector (element name, #id, .class, etc.)

**$("selector children")**  
Select a children element

**$("selector > children")**  
Select a direct descendant

**$("selector, selector")**  
Select multiple elements with different rules

**$("selector:first, selector:last")**  
Select the first and last corresponding elements

**$("selector:odd, selector:event")**  
Select odd or even elements (first is 0, so even)


## Methods selectors (faster)

**$("ul").find("li")**  
Select all li children elements in ul element

**$("selector").first() / .last()**  
Select the first / last corresponding element

**$("element").prev() / .next()**  
Select the previous / next element

**$("element").parent()**  
Select the direct ancester element

**$("ul").children("li")**  
Select only direct li descendants of ul (unlike **.find()**)

**$("element").parents("li")**  
Select all ancesters elements to li

**$("element").closest("li")**  
Select the closest ancester element to li

**$(".class1").filter(".class2")**  
Select all classes that have .class1, then filter to keep only those who have also .class2


## DOM manipulation

**var element = $("&lt;li&gt;item&lt;/li&gt;")**  
Create a new HTML element in the DOM

**$("ul").prepend(element) / .append(element)**  
Place element as first / last children of ul

**$("p").after(element) / .before(element)**  
Place element before of after p

**element.prependTo($("ul")) / .appendTo($("ul"))**  
Place element as first / last children of ul

**element.insertAfter($("p")) / .insertBefore($("p"))**  
Place element before of after p

**$("#element").remove()**  
Remove #element form the DOM


## Classes/CSS manipulation

**$("#element").addClass('classe')**  
Add a class to the element

**$("#element").removeClass('classe')**  
Remove an element's class

**$('#element').toggleClass('class')**  
Toggle an element's class

**$('#element').hasClass('class')**  
Return true if element has the specified class

**$("element").css('background-color', '#ff6600')**  
Change the element's backgroud-color css property

**$("element").css({'background-color': '#ff6600'})**  
Change the element's backgroud-color css property using an object


## Animate

**$("element").animate({'top': '-10px'}, speed)**
Same as CSS but animate the change
speed: optional animation duration in millisecond (default: 400, slow: 200, fast: 400)

**$("element").show() / .hide() / .toggle()**  
Show / hide / toggle element

**$("element").slideDown() / .slideUp() / .slideToggle()**   Show / hide / toggle element with a slide effect

**$("element").fadeIn() / .fadeOut() / .fadeOut()**  
Show / hide / toggle element with a fade effect


## Events

**$("element").on('event', handler)**  
Call handler function when event occures on element

**$(".class").on('event', 'element', handler**  
Call handler function when event occures on element if it is located in an .class element

**$(this)**  
Select the triggered element inside of the callback function

**event.preventDefault()**  
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






















End of file
