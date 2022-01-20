# Concepts
1. The hierarchical structure of HTML is called the DOM (document object model);  

# Tag  
All elements:  
https://developer.mozilla.org/en-US/docs/Web/HTML/Element  
http://www.w3school.com.cn/tags/index.asp  

## `<head>` - Container of metadata
Title: define the title in browser tag and in search engine;  
Base: provides default link and target;  
Link: links to outside resources, such as style;  
Meta: used in many services and serarch engine;  
```
<head>
  <title>Title of the document</title> 
  <base href="http://www.w3school.com.cn/images/" />
  <base target="_blank" />
  <link rel="stylesheet" type="text/css" href="mystyle.css" />
  <meta name="description" content="Free Web tutorials on HTML, CSS, XML" />
  <meta name="author" content="w3school.com.cn">
</head>
```

## `<list>`  
1. Change labels: `<ol type="A">`;  
2. Change marks: style="list-style-type:square";  

## `<table>`
1. `<caption>` - table title
2. `<thead>`- table head  
  `<tr>` - table row  
  `<th>` - table heading  
3. `<tbody>`- table body  
  `<td>` - table cell  
4. `<tfoot>` - table foot
5. Use colspan and rowspan properties to merge cells

  ```
  /*This cell will take up three columns*/
  <td colspan="3">Total</td>
  ```

## `<a>`    
target=    
**_self** - default. Opens the linked document in the same frame as it was clicked;  
**_blank** - Opens the linked document in a new window or tag;  
**_parent** - Opens the linked document in the parent frame;  
**_top** - Opens the linked document in the full body of the window;  
**_framename** - Opens the linked document in a named frame;  
  
### Use `<a>`  as anchor
Use name to jump to a tag in the same page (of course other pages as well). Grant each section a name anchor to organize the page. If the anchor is not found, the page will return to the top. No errors.    
```
<a name="tips">Mainpage - Help</a>
<a href="#tips">Help</a>
```
Click to send an email example, use %20 to replace space.  
```
<p>
Another mailto link:
<a href="mailto:someone@microsoft.com?cc=someoneelse@microsoft.com&bcc=andsomeoneelse2@microsoft.com&subject=Summer%20Party&body=You%20are%20invited%20to%20a%20big%20summer%20party!">Send!</a>
</p>
```

## Text  
1. `<b>` and `<i>` are presentationl elements that convey no extra importance;  
2. `<strong>` and `<em>` are phrase elements that strength SEO, while `<strong>` is stronger than `<em>`;  
3. `<mark>` to highlight;  
4. `<sub>` and `<sup>` to subscript or superscript;  
5. `<blockquote>` to quote and cite;  
6. `<bdo dir="rtl">` to inverse a text;  
7. Provide useful information for search engine or translation: abbrevation, cite, define  
```
<p><abbr title="World Health Organization">WHO</abbr> was founded in 1948.</p>
<p><cite>The Scream</cite> by Edward Munch. Painted in 1893.</p>
<p><dfn><abbr title="World Health Organization">WHO</abbr></dfn> was founded in 1948.</p>
```

## `<image>`  
1. Use alt to display a text message when the picture is unavailable.  
  
	```
	<img src="boat.gif" alt="This image is not available">
	```

2. Insert image in other elements and align it accordingly (bottom, middle, top).  
  
	```
	<p>Picture <img src="/i/eg_cute.gif" align="bottom"> in text.</p>
	```

3. Image map: http://www.w3school.com.cn/tiy/t.asp?f=html_areamap  

	```
	<img src="/i/eg_planets.jpg" usemap="#planetmap" />
	<map name="planetmap" id="planetmap">

	<area
	shape="rect"
	coords="0,0,110,260"
	href ="/example/html/sun.html"/>

	</map>
	```

## `<form>`
form is a block-level element that will incur a new line.   
1. Action: where the form send data to  
2. Method: what HTTP method (get/post). If no action, goes to the same page which equals to refresh   
3. element name: element name in querying string  
4. button: if button element is the last thing in the form, it will submit the form  

```
<form>
  <input type="radio" name="gender" value="male"> Male
  <input type="radio" name="gender" value="female"> Female
</form>

<form action="action_page.php">
  <select name="cars">
    <option value="volvo">Volvo</option>
    <option value="ford">Ford</option>
    <option value="fiat">Fiat</option>
    <option value="audi">Audi</option>
  </select>
  <input type="submit">
</form>

<form action="action_page.php">
  <textarea name="note"></textarea>
  <input type="submit">
</form>


<form action="action_page.php">
  Quantity (between 1 and 5):
  <input type="number" name="quantity" min="1" max="5"></input>
  <input type="submit">
</form>
```

## `<input>`
1. type: text, radio, color, radio, email, password, etc.

  ```
  <input type="password" pattern=".{5,10}" title="Password has to be 5 to 10 characters" required>
  ```
2. placeholder
3. name/value: displayed in the url querying string
4. required

Other Examples
```
 	<h1>Login</h1>
 	<!-- message sent to "https://www.wikipedia.org/?username=dfgrty&password=34534535" -->
 	<form action="http://www.wikipedia.org">
 		<label for="username">Username:</label>
	 	<input id="username" name="username" type="email" placeholder="username" required>
		<label for="password">Password:</label>
		<input id="password" name="password" type="password" placeholder="password">
		<button>Submit</button>
	</form>
```
```
	<form>
		<label for="A">Option A</label>
		<input id="A" type="radio" value="A" name="options">
		<label for="B">Option B</label>
		<input id="B" type="radio" value="B" name="options">
		<button>Go</button>
	</form>
```

## `<select>`
1. option

	```
	<form>
		<select name="color">
			<option value="Yellow">Red</option>
			<option value="Green">Yellow</option>
			<option value="Red">Green</option>
		</select>
		<button>Go</button>
	</form>
	```

## `<pre>` - to maintain format
```
<p>
<pre>
   My Bonnie lies over the ocean.

   My Bonnie lies over the sea.

   My Bonnie lies over the ocean.

   Oh, bring back my Bonnie to me.
</pre>
</p>
```

## Depreciated tags (replace with CSS):
```
<center>
<font> and <basefont> (style="font-family:verdana")
<s> and <strike>	
<u>
```
http://www.w3school.com.cn/tags/html_ref_standardattributes.asp  


# Named character references
When you want to display a greater-than symbol in the text, you can use a named character reference. You should know these four common named character references:  

%20 - space  
&gt - denotes the greater-than sign (>)  
&lt - denotes the less-than sign (<)  
&amp - denotes the ampersand (&)  
&quot - denotes double quote (")  

http://w3c.github.io/html/syntax.html#named-character-references  
http://www.w3school.com.cn/tags/html_ref_entities.html  

# Quirks Mode and Standards Mode
For HTML documents, browsers use a DOCTYPE in the **beginning** of the document to decide whether to handle it in quirks mode or standards mode. For full standards mode, use:   
```
<!DOCTYPE html>
```
XHTML does not require this line as everything will be in full standards mode.  
#Charactistics  
1. Format: spaces in source code will be reduced to one and in-text newline does not work either. Use one or more `<br />`. 
2. Conditional annotation  
```
<!--[if IE 8]>
    .... some HTML here ....
<![endif]-->
```



