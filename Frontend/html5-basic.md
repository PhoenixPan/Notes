# HTML5

## Basic structrue  
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8"/>
<title>Title of the document</title>
</head>

<body>
Content of the document......
</body>

</html>
```

## What's new?  
New semantic elements like <**header**>, <**footer**>, <**article**>, and <**section**>.  
New form control attributes like number, date, time, calendar, and range.  
New graphic elements: <**svg**> and <**canvas**>.  
New multimedia elements: <**audio**> and <**video**>.  

## Elements removed
Tags removed since HTML5 and their replacements  

|    Removed     | Replacement |
| -------------- |:-----------:|
| <**acronym**>  | <abbr>      |
| <**applet**>   | <object>    |
| <**basefont**> | CSS         |
| <**big**>      | CSS         |
| <**center**>   | CSS         |
| <**dir**>      | <ul>        |
| <**font**>     | CSS         |
| <**frame**>    |             |
| <**frameset**> |             |
| <**noframes**> |             |
| <**strike**>   | CSS         |
| <**tt**>       | CSS         |

<!--##Added-->
<!--|   New Elements    | Definition                                                                                	|-->
<!--|-----------------	|:-------------------------------------------------------------------------------------------:|-->
<!--| <**article**>    	| Defines an article in the document                                                        	|-->
<!--| <**aside**>      	| Defines content aside from the page content                                               	|-->
<!--| <**bdi**>        	| Defines a part of text that might be formatted in a different direction from other text   	|-->
<!--| <**details**>    	| Defines additional details that the user can view or hide                                 	|-->
<!--| <**dialog**>     	| Defines a dialog box or window                                                            	|-->
<!--| <**figcaption**> 	| Defines a caption for a <figure> element                                                  	|-->
<!--| <**figure**>     	| Defines self-contained content, like illustrations, diagrams, photos, code listings, etc. 	|-->
<!--| <**footer**>     	| Defines a footer for the document or a section                                            	|-->
<!--| <**header**>     	| Defines a header for the document or a section                                            	|-->
<!--| <**main**>       	| Defines the main content of a document                                                    	|-->
<!--| <**mark**>       	| Defines marked or highlighted text                                                        	|-->
<!--| <**menuitem**>   	| Defines a command/menu item that the user can invoke from a popup menu                    	|-->
<!--| <**meter**>      	| Defines a scalar measurement within a known range (a gauge)                               	|-->
<!--| <**nav**>        	| Defines navigation links in the document                                                  	|-->
<!--| <**progress**>   	| Defines the progress of a task                                                            	|-->
<!--| <**rp**>         	| Defines what to show in browsers that do not support ruby annotations                     	|-->
<!--| <**rt**>         	| Defines an explanation/pronunciation of characters (for East Asian typography)            	|-->
<!--| <**ruby**>       	| Defines a ruby annotation (for East Asian typography)                                     	|-->
<!--| <**section**>    	| Defines a section in the document                                                         	|-->
<!--| <**summary**>    	| Defines a visible heading for a <details> element                                         	|-->
<!--| <**time**>       	| Defines a date/time                                                                       	|-->
<!--| <**wbr**>        	| Defines a possible line-break                                                             	|-->

## Arrtibute syntax
Type: Empty, Unquoted, Single-quoted, and Double-quoted.  

| Syntax       | Result      |
|--------------|:-----------:|
| ''John Doe'' | None        |
| '"John Doe"' | "John Doe"  |
| "'John Doe'" | 'John Doe'  |
| ""John Doe"" | None        |

## New Semantic Elements
Why?  
1. Readability is way much better than a cluster of <**div**>;  
2. Allow search engines to identify the correct pages;  
3. Allows data to be shared and reused across applications, enterprises, and communities.  
<**div**>: A generic container element for styling purposes or as a convenience for scripting;   
<**section**>: Used to represent a group of related elements, consisting a part of a program;  
<**article**>: Used to hold an INDEPENDENT unit, making sense on its own.  



## Migration from HTML4 to HTML5  
(http://www.w3schools.com/html/html5_migration.asp)  
(Browser support: http://www.w3schools.com/html/html5_browsers.asp )  

##### Change semantic elements and corresponding CSS names
|     Typical HTML4      |   Typical HTML5   |
|------------------------|-------------------|
| <**div id="header"**>  | <**header**>      |
| <**div id="menu"**>    | <**nav**>         |
| <**div id="content"**> | <**section**>     |
| <**div id="post"**>    | <**article**>     |
| <**div id="footer"**>  | <**footer**>      |  
  
##### Change Doctype  
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
to
<!DOCTYPE html>
```  
  
##### Change to HTML5 Encoding  
```
<meta http-equiv="Content-Type" content="text/html;charset=utf-8">
to
<meta charset="utf-8">
```  
  
##### Add The Shiv  
Teach old browser HTML5 
```
<!--[if lt IE 9]>
  <script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif]-->
```  
  
## Conventions
1. use lower case. use lower case. use lower case.   
2. Always use lower case file names to avoid consistency issue on different servers;   
3. Always close empty html elements <**meta charset="utf-8" /**>;  
4. Always quote attribute values;  
5. No space needed around "=";  
6. Indentation: use two spaces, do not use TAB;  
7. Do not omit <**html lang="en-US"**> and <**body**> tags, it reduces search engine exposure and errors;  
8. Omit <**head**> tag is still an unfamiliar approach. Even if you do it, all elements before <**body**> will still be added to a default head tag; 
9. <**title**> is required and has to be meaningful;  
10. Always give images alt value in case of failure. Always define image size as it reduces flickering because the browser can reserve space for images before they are loaded;  
```
<**img src="html5.gif" alt="HTML5" style="width:128px;height:128px"**>
```
Comments example:  
```
// Single line with one space before and after
<!-- This is a comment --> 

// Multiple lines
<!-- 
  This is a long comment example. This is a long comment example. This is a long comment example.
  This is a long comment example. This is a long comment example. This is a long comment example.
-->
```
Simple style can be compressed on one line:   
```
p.into {font-family: Verdana; font-size: 16em;}
```
  

  
## Others
h1 and p will be displayed in default format  
http://html5.group.iteye.com/group/wiki/3293-html5  
```
<div class="outer">
     <aaa>
         <h1>title</h1>
         <p>text</p>
     </aaa>
</div>
```

## More About New Tags

### data-type

It takes three steps to transform a custom data attribute into a DOMStringMap key:  
1. The data- prefix is removed from the attribute name
2. Any hyphen followed by a lower case letter is removed from the name and the letter following it is converted to uppercase
3. Other characters will remain unchanged. This means that any hyphen that is not followed by a lowercase letter will also remain unchanged.

```
/*html & css*/
<li data-type="veg" data-distance="2miles" data-identifier="10318">Salad King</li>

li[data-type='veg'] {
  background: #8BC34A;
}
```

```
// JS manipulation
var ratings = restaurant.getAttribute("data-ratings"); // or
var ratings = restaurant.dataset.ratings;

restaurant.setAttribute("data-owner-name", "someName"); // or
restaurant.dataset["owner-name"] = newOwner;
```

Reference: https://www.sitepoint.com/how-why-use-html5-custom-data-attributes/?utm_source=SitePoint&utm_medium=email&utm_campaign=Versioning




### video

```
<video width="420" controls>
  <source src="mov_bbb.mp4" type="video/mp4">
  <source src="mov_bbb.ogg" type="video/ogg">
 Your browser does not support the video tag.
</video>
```
