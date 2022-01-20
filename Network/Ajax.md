# Asynchronous JavaScript and XML

Ajax isn't a technology, it's more of a pattern.

## Features

1. Dynamically update a website without reloading the entire page.
2. Dramatically decrease the data that needs to be transferred and allow a part of execution been completed on clint side.
3. Dropback: hard to move back the page.

## Workflow and code example

1. On client side, create a JavaScript object called XMLHttpRequest (set on which the HTML method for GET or POST request and the destination URL) to perform HTTP request and parse server response;
2. Register a callback function for each XMLHttpRequest and then dispatch XMLHttpRequest asynchronously;
3. The control returns to the browser, which keeps interacting with the user for other actions;
4. On server, the Java Web server parses the request just like any other HttpServletResponse: the serlet invokes necessary steps, serialize its response into XML, and write it to the HttpServletResponse;
5. Back on client side, the server's response arrives and calls the callback function to process the XML document arrived;
6. Update the user interface using JavaScript to manipulate HTML DOM.

##### Code example

1. Download the sample code from the end of [this article](http://www.ibm.com/developerworks/library/j-ajax1/#Listing 1);
2. Create a new Java Web project;
3. Put "src" - "developerworks" under "Source Packages";
4. Put files in "web" folder under "Web Pages";
5. Put "web.xml" under "Web Pages" - "WEB-INF".
6. Run the program!

## Questions

Why "application/xml" not "text/xml"?

```
res.setContentType("application/xml");
res.getWriter().write(cartXml);
```

To make the program work for remove action, what does "remove&item" means?

```
 req.send("action=remove&item="+itemCode);
```

## Complete code with JSON with explanation

ajax.js

```
/*
 * Returns an new XMLHttpRequest object, or false if the browser
 * doesn't support it
 */
function newXMLHttpRequest() {
  var xhr = false;
  // Create XMLHttpRequest object in non-Microsoft browsers
  if (window.XMLHttpRequest) {
    xhr = new XMLHttpRequest();
  } else if (window.ActiveXObject) {
    try {
      // Try to create XMLHttpRequest in later versions of Internet Explorer
      xhr = new ActiveXObject("Msxml2.XMLHTTP");
    } catch (e1) {
      // Failed to create required ActiveXObject
      try {
        // Try version supported by older versions of Internet Explorer
        xhr = new ActiveXObject("Microsoft.XMLHTTP");
      } catch (e2) {
        // Unable to create an XMLHttpRequest by any means
        xhr = false;
      }
    }
  }
return xhr;
}

 /*
  * Returns a function that waits for the specified XMLHttpRequest
  * to complete, then passes it XML response to the given handler function.
  * req - The XMLHttpRequest whose state is changing
  * responseXmlHandler - Function to pass the XML response to
  * It is important to learn how to use this function
  */
 function getReadyStateHandler(req, responseXmlHandler) {

   // Return an anonymous function that listens to the XMLHttpRequest instance
   return function () {
     // If the request's status is 4, "complete"
     if (req.readyState == 4) {
       // Check that we received a successful response from the server
       if (req.status == 200) {
         // Pass the XML payload of the response to the handler function.
         responseXmlHandler(req.responseText);
       } else {
         // An HTTP problem has occurred
         alert("HTTP error "+req.status+": "+req.statusText);
       }
     }
   }
 }
```

cart.js

```
// Timestamp of cart that page was last updated with
var lastCartUpdate = 0;

/*
 * Adds the specified item to the shopping cart, via Ajax call
 * itemCode - product code of the item to add
 */
function addToCart(itemCode) {

// Obtain an XMLHttpRequest instance
    var req = newXMLHttpRequest();

// Set the handler function to receive callback notifications from the request object
    req.onreadystatechange = getReadyStateHandler(req, updateCart);

// Open an HTTP POST connection to the shopping cart servlet.
    req.open("POST", "cart.do", true);  // "true" specifies the request is asynchronous.

// Specify that the body of the request contains form data
    req.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

// Send form encoded data stating that which item should be added to the cart.
    req.send("action=add&item=" + itemCode);
}


/*
 * Removes the specified item from the shopping cart, via Ajax call
 * itemCode - product code of the item to add
 */
function removeFromCart(itemCode) {

// Obtain an XMLHttpRequest instance
    var req = newXMLHttpRequest();

// Set the handler function to receive callback notifications from the request object
    req.onreadystatechange = getReadyStateHandler(req, updateCart);

// Open an HTTP POST connection to the shopping cart servlet.
    req.open("POST", "cart.do", true);  // "true" specifies the request is asynchronous.

// Specify that the body of the request contains form data
    req.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

// Send form encoded data stating that which item should be removed from the cart.
    req.send("action=remove&item=" + itemCode);
}

/*
 * Parses the json Cart response and updates the page via ajax.
 * **/
function updateCart(jsonResponse) {

    // Get a cart object from JSON format
    var cart = jsonParse(jsonResponse).cart;
    var generated = cart.generated;

    // Check that a more recent cart document hasn't been processed already
    if (generated > lastCartUpdate) {
        lastCartUpdate = generated;

        // Clear the HTML list of cart contents
        var contents = document.getElementById("contents");
        contents.innerHTML = "";

        // Loop over the items in the cart
        var items = cart.item;
        for (var I = 0; I < items.length; I++) {

            // Extract the text nodes from the name and quantity elements
            var item = items[I];
            var name = item.name;
            var quantity = item.quantity;

            // Create and add a list item HTML element for this cart item
            var listItem = document.createElement("li");
            listItem.appendChild(document.createTextNode(name + " x " + quantity));
            contents.appendChild(listItem);
        }
    }

    document.getElementById("total").innerHTML = cart.total;
}
```

index.jsp

```
<%@ page import="java.util.*" %>
<%@ page import="developerworks.ajax.store.*" %>
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<meta name="save" content="history" />
<script type="text/javascript" language="javascript" src="ajax1.js"></script>
<script type="text/javascript" language="javascript" src="cart.js"></script>
<script type="text/javascript" language="javascript" src="json_sans_eval.js"></script>

</head>
<body>
<div style="float: left; width: 500px">
<h2>Catalog</h2>
<!-- Table of products from store's catalog, one row per item -->
<table border="1">
  <thead><th>Name</th><th>Description</th><th>Price</th><th></th><th></th></thead>
  <!-- Item details & buttons -->
  <tbody>
  <%
    for (Iterator<Item> I = new Catalog().getAllItems().iterator() ; I.hasNext() ; ) {
      Item item = I.next();
  %>
    <tr>
        <td><%= item.getName() %></td>
        <td><%= item.getDescription() %></td>
        <td><%= item.getFormattedPrice() %></td>
        <td><button onclick="addToCart('<%= item.getCode() %>')">Add to Cart</button></td>
        <td><button onclick="removeFromCart('<%= item.getCode() %>')">Remove From Cart</button></td>
    </tr>
    <% } %>
  </tbody>
</table>
<!-- Representation of shopping cart, updated asynchronously -->
<div style="position: absolute; top: 0px; right: 0px; width: 250px">
<h2>Cart Contents</h2>
<ul id="contents">
</ul>
<!-- Total cost of items in cart displayed inside span element -->
Total cost: <span id="total">$0.00</span>
</div>
</body>
</html>
```

Cart.java

```
import java.math.BigDecimal;
import java.util.*;

/**
 * A very simple shopping Cart
 */
public class Cart {

  private HashMap<Item,Integer> contents;

  /**
   * Creates a new Cart instance
   */
  public Cart() {
    // initialize the cart, with names of the items and their quantity
    contents = new HashMap<Item,Integer>();
  }

  /**
   * Adds a named item to the cart
   * @param itemName The name of the item to add to the cart
   */
  public void addItem(String itemCode) {
    Catalog catalog = new Catalog();

    if (catalog.containsItem(itemCode)) {
      Item item = catalog.getItem(itemCode);

      int newQuantity = 1;
      // If item is already in cart, add one more unit
      if (contents.containsKey(item)) {
        Integer currentQuantity = contents.get(item);
        newQuantity += currentQuantity.intValue();
      }
      // If not in cart, put one unit in
      contents.put(item, new Integer(newQuantity));

    }
  }

  /**
   * Removes the named item from the cart
   * @param itemName Name of item to remove
   */
  public void removeItem(String itemCode) {
     Catalog catalog = new Catalog();

    if (catalog.containsItem(itemCode)) {
      Item item = catalog.getItem(itemCode);

      // If item is already in cart, remove one unit
      if (contents.containsKey(item) && contents.get(item).intValue() > 1) {
        int newQuantity = contents.get(item).intValue() - 1;
        contents.put(item, new Integer(newQuantity));
      }
      // If the quantity is 1, remove it. If 0, do nothing.
      else {
        contents.remove(item);
      }
    }
  }
  /**
   * @return XML representation of cart contents
   */
  public String toJSON() {
    StringBuffer newJson = new StringBuffer();
    newJson.append("{\"cart\": {");
    newJson.append("\"generated\":\"" + System.currentTimeMillis() + "\",");
    newJson.append("\"total\":\"" + this.getCartTotal() + "\",");
    newJson.append("\"item\" : [");
    //  Traverse the map and print out their names, quantities, and units
    for (Iterator<Item> I = contents.keySet().iterator() ; I.hasNext() ; ) {
      Item item = I.next();
      int itemQuantity = contents.get(item).intValue();
      newJson.append("{");
      newJson.append("\"code\":\"" + item.getCode() + "\",");
      newJson.append("\"name\":\"" + item.getName() + "\",");
      newJson.append("\"quantity\":\"" + itemQuantity + "\"");
      newJson.append("},");
    }
//    newJson.setLength(newJson.length() - 1); // remove the last ","
    newJson.append("]"); // close item
    newJson.append("}}"); // close cart
    return newJson.toString();
  }

  /**
   * Calculate the total price
   * @return Total amount spent
   */
  private String getCartTotal() {
    int total = 0;

    for (Iterator<Item> I = contents.keySet().iterator() ; I.hasNext() ; ) {
      Item item = I.next();
      int itemQuantity = contents.get(item).intValue();

      total += (item.getPrice() * itemQuantity);
    }

    return "$"+new BigDecimal(total).movePointLeft(2);
  }
}
```

These three classes have no changes comparing with the oringinal ones in the demo code:  
CartServlet.java  
Catelog.java  
Item.java

http://www.ibm.com/developerworks/library/j-ajax1/
