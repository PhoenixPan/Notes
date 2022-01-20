# jQuery
is a popular javascript library.

## Selector 

1. We use $() to select elements in CSS style (like querySelector);
2. The selector returns a list even there's only one item. It always returns the jQuery object, which is an array-like structure that contains all the selected DOM elements. You can get raw HTML DOM element by calling the array-like using $("array")[0] or $("array").get(0);
3. Remember, what you get is raw DOM element, so you can only use js manipulation such as innerHTML rather than jQuery ones such as .text();
4. It never returns null, or another type. If one element is found, the jQuery object will have only one child. If no elements are found, the jQuery object will be empty.
5. Manipulate css: $().css("property","value") or $().css(object);

	```
	var styles = {
	backgroundColor: "pink", // It's illegal to use "-" in between, so we use camel case
	fontSize: "100px"
	};
	```
6. Use $(this) to stand for "this", use $(variable) to use a variable that contains a select result

### Select children
1. Direct children: .children()
2. All children: .find()

## Basic Methods
1. val(): Get the current value of the **first** element in the set of matched elements or set the value of **every** matched element. Case: get contents from input or select box;
2. text(): Get the **combined** (no spaces in between) text contents of each element in the set of matched elements, including their descendants, or set the text contents of **every** matched elements;
3. html(): Get the HTML contents of the **first** element in the set of matched elements or set the HTML contents of **every** matched element;
4. attr(): Get the value of an attribute for the **first** element in the set of matched elements or set one or more (through an object) attributes for **every** matched element;
5. addClass(): Adds one or more classes to **each** element in the set of matched elements;
6. removeClass(): Remove one, multiple, or all classes from **each** element in the set of matched elements;
7. toggleClass(): Add or remove one or more classes from **each** element in the set of matched elements, depending on their current status;


## jQuery Events
1. click()/dlbclick():triggered when click/double-clicked;
2. keypress():triggered between keydown and keyup (usually);  
  keyup()/keydown(): triggered when you press down / release the key;  
  
  1.If the user presses and holds a key, a keyup/keydown event is triggered once, but keypress events are triggered for each character inserted (really?);  
  2.Note that keydown and keyup provide a code indicating which key is pressed, while keypress indicates which character was entered. For example, a lowercase "a" will be reported as 65 by keydown and keyup, but as 97 by keypress. An uppercase "A" is reported as 65 by all events. Because of this distinction, when catching special keystrokes such as arrow keys, .keydown() or .keyup() is a better choice;  
  3.Reference: http://stackoverflow.com/questions/12827408/whats-the-theory-behind-jquery-keypress-keydown-keyup-black-magic-on-macs  
3. on():Attach an event handler function for **all** selected elements, similar to addEventListener() in js. It helps keep CSS for dynamically generated elements. A selector string to filter the **descendants** of the selected elements that **trigger** the event. If the selector is null or omitted, the event is always triggered when it reaches the selected element.  

	```	
	// Code from project Patatap
	<body>
		<canvas id="myCanvas" resize></canvas>
		<div id="hint-div">
			<p id="hint"><i class="fa fa-hand-o-down" aria-hidden="true"></i> Press any key to start singing </p>
		</div>
		<script src="js/patatap.js"></script>
	</body>

	// Only "body" can be used as selectors. Though "#hint-div" is the descendent of "html", it doesn't trigger the keydown even. 		// "body" does.
	$("html").on("keydown", "body", function() {
		$("#hint").html("<i class=\"fa fa-thumbs-o-up\" aria-hidden=\"true\"></i> There you go!");
		$("#hint-div").fadeOut(1500);
	});

	// If you want to take effect with "#hint-div", change code to like below and click the div 
	$("html").on("click", "#hint-div", function() {
		$("#hint").html("<i class=\"fa fa-thumbs-o-up\" aria-hidden=\"true\"></i> There you go!");
		$("#hint-div").fadeOut(1500);
	});

	```

4. click() only adds to existing elements while on() adds on future elements.
5. Event bubbling: click one place may trigger multiple events (usually from parent elements), if it's not your intention, use this stopPropagation(): 
  
  ```
  $(".list").on("click",".delete-button", function(event) {
	  $(this).parent().fadeOut(function() { // fade out and remove
		  $(this).remove(); 
	  });
	  event.stopPropagation();
  });
  ```

```
<script>
  $(document).ready(function() {
    $("button").addClass("animated bounce");       // by type, manipulate all button elements
    $(".well").addClass("animated shake");         // by class, manipulate all elements of well class
    $("#target3").addClass("animated fadeOut");    // by id, manipulate element with certain id
    $("button").removeClass("btn-default");        // remove class
    $("#target1").css("color", "blue");            // change CSS elements
    $("#target4").html("<em>#target4</em>");       // change HTML elements
    $("#target4").remove();                        // remove HTML elements
    $("#target2").appendTo("#right-well")          // move HTML elements
    $("#target5").clone().appendTo("#left-well");  // clone HTML elements
    $("#target1").parent().css("background-color","red");  // manipulate parent
    $("#right-well").children().css("color","orange");     // manipulate children
    $(".target:nth-child(2)").addClass("animated bounce"); // give the second element the bounce class
    $(".target:even").addClass("animated bounce");         // target all the even-numbered elements with class target
    $("body").addClass("animated hinge");
  });
</script>

<!-- Only change code above this line. -->
<div class="container-fluid">
  <h3 class="text-primary text-center">jQuery Playground</h3>
  <div class="row">
    <div class="col-xs-6">
      <h4>#left-well</h4>
      <div class="well" id="left-well">
        <button class="btn btn-default target" id="target1">#target1</button>
        <button class="btn btn-default target" id="target2">#target2</button>
        <button class="btn btn-default target" id="target3">#target3</button>
      </div>
    </div>
    <div class="col-xs-6">
      <h4>#right-well</h4>
      <div class="well" id="right-well">
        <button class="btn btn-default target" id="target4">#target4</button>
        <button class="btn btn-default target" id="target5">#target5</button>
        <button class="btn btn-default target" id="target6">#target6</button>
      </div>
    </div>
  </div>
</div>
```
# Selector
1. nth-child() and last-child()

	```
	<parent>
	  <son>
	    <grandson>
	    </grandson>
	  </son>
	  <son>
	    <grandson>
	    </grandson>
	  </son>
	  /*...*/
	</parent>
	```    
	How to give each son a different background?      

	```
	$("#parent .son:last-child grandson:nth-child(1)").css("background-image", "url(\'" + imageUrl + "\')");
	```

2. not() or :not: select all except the given one

	```
	var menu = $(this).siblings(".menu");
	$(".conflict-btn-group1").not(menu).css("display","none");
	```
