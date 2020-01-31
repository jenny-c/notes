# jQuery 

**initial**
```html
<script>
    $(document).ready(function() {

    });
</script>
// the code inside the function will run as soon as the browser has loaded the page (without it, the code might run before the html has rendered)
```

- all jQuery functions start with $
- jQuery often selects an HTML element and then does something 

## selecting 
```js
$("button") // selects all buttons (selection by type of element)
$(".well") // selects all elements with the class "well"
$("#target") // selects the element with the id "target"

// jQuery uses CSS selectors to target elements 
$(".target:nth-child(3)") // selects the third elements with the class "target"
$(".target:odd") // selects all the odd elements with the class "target" (NOTE: jQuery is 0-indexed; :even selects all the even elements)
```


## functions 
```js
.addClass("animated bounce") // adds the classes "animated" and "bounce" to the selected elements 
.removeClass("animated bounce") // does what you expect it to
.css("color", "red"); // adds/modifies CSS to the element
.prop("disabled", true); // allows you to adjust properties of elements ("disabled" is the property name) 
.html("<em>new element and text</em>"); // lets you add HTML tags and text within an element 
.text("new text"); // similar to .html, but won't evaluate HTML tags passed to it 
.remove() // removes the HTML element 
.appendTo("#left-div"); // appends the selected element to the element with the id "left-div"
.clone() // makes a copy of an element (note: can  chain another function to the end like ".clone().appendTo())"
.parent() // accesses the parent of the selected element 
.children() // accesses the children of the selected element 
```