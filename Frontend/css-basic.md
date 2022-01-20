# CSS

## Ways to apply CSS

1. External (preferred): refer in head tag: `<link rel="stylesheet" type="text/css" href="mystyle.css">`
2. Internal: use `<style>`
3. Inline: `<h1 style="color:blue;">`

## Selectors & Pseudo classes

### Selectors

- Element selector: `h1 {color:blue;}`
- ID selector: `#id {color:blue;}`
- Class selector: `.my-class {color:blue;}`
- Attribute-based(details in the next section): `input[type="text"] {color:blue;}`
- All elements: `#id * {color:blue;}`
- Status: `a:visited {color:blue;}`
- All children: `X Y {color:blue;}`
- All direct children: `X > Y {color:blue;}`
- Immediate sibling: `X + Y {color:blue;}`
- All preceding sibling: `X ~ Y {color:blue;}`
- Not (except): `div:not(#div-i-dont-want) {color: blue;}`
- Child selectors: `X:first-child`
- `X:nth-child(n)`
- `X:last-child`
- `X:nth-last-child`
- `X:only-child` Apply if X is the only child of its parent
- Nth of type selectors: `X:nth-of-type(odd) {color:blue;}`, Only apply to node-selectors, not class names
- `X:nth-of-type(n)`
- `X:nth-last-of-type(n)`
- `X:only-of-type`
- `X:first-of-type`

Reference: [The 30 CSS Selectors You Must Memorize](https://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048)

### Pseudo classes

1. [before and after](http://www.w3schools.com/cssref/sel_before.asp) insert content before or after the selected elements (and also style it). It can be used with other pseudo classes such as hover:

   ```
   nav li a:after {
     content: "";
     height: 100%;
     left: 0;
     top: 0;
     width: 0px;
     position: absolute;
     transition: all 0.3s ease 0s;
   }

   nav li a:hover:after{ width: 100%; } // hover effect: bar will grow from left to right and occupy 100% of the element
   ```

2. `<a>` tag:

   - a:link {color: red}; - unvisited link;
   - a:visited {color: green};
   - a:hover; - MUST come after a:link and a:visited in the CSS definition in order to be effective;
   - a:active; - MUST come after a:hover in the CSS definition in order to be effective;

3. p:lang(anything) - define special rules for different language:

   ```
   	q:lang(en) {
   	  quotes:"prefix "" postfix";
   	  background:lightblue;
   	}

   	q:lang(cn) {
   	  quotes:"1""2";
   	  background:pink;
   	}

   	<p>Some text <q lang="en">English</q> Some text.</p>
   	<p>Some text <q lang="cn">Chinese</q> Some text.</p>
   ```

### Selector specificity (priority)

1. The one that is more specific path wins

   ```
   #proirity_test ul {
   	color: blue;
   }

   #proirity_test ul li:nth-of-type(3) {
   	color: green;
   }
   ```

2. !important(pink) > inline style(white) > id(orange) > external(css) loaded on bottom> class on bottom(blue) > class on top(green). PS: external reference has lower priority than !impotant and inline, but could be higher than others depending on where it's loaded. (Specificity: https://developer.mozilla.org/en/docs/Web/CSS/Specificity)

   ```
   <style>
     #orange-text {
       color: orange;
     }
     .pink-text {
       color: pink !important;
     }

     .green-text {
       color: blue;
     }

     .blue-text {
       color: blue;
     }

   </style>
   <link rel="stylesheet" type="text/css" href="mystyle.css">
   <h1 id="orange-text" class="pink-text green-text blue-text" style = "color:white">Hello World!</h1>
   ```

## CSS Properties

### Sizing

- Absolute unit:
  - px: pixel
- Relative units:
  - **rem**: relative to root element (HTML tag), recommended, usually used for mobile adaption
  - em: relative to parent element. This may result in some unexpected size in nested inheritance
  - %: relative to parent element
  - vh & vw: relative to the viewport

### Color

- Default colors `color: red`: Check all of them [here](http://colours.neilorangepeel.com/)
- Hexadecimal `color: #000000;`: The first two digits stand for red #FF0000, the next two for green #00FF00, the last two for blue #0000FF
- RGB `color: rgb(0,0,0);`: The first value stands for red rgb(255,0,0) , the next one green rgb(0,255,0), the last one for blue rgb(0,0,255)
- RGBA `color: rgba(0,0,0,0);`: The last channel is used to set transparency level from 0 to 1

### Font

- Start with the font you like, and end with a common fallback font: serif (衬线体) or sans-serif (无衬线体)
- If font-size is not specified, the default size is 16px, which is also the default of 1rem
- Introducing order: English fonts before fonts for other language (Chinese), because English fonts won't usually include styles for other languages, so avoid unwanted override. 注意：引入中文字体可能导致加载缓慢，因为中文字符多，所以字体包较大，需要预先处理。
- `@font-face`: introduce a font-family the remote sources. Google fonts are useful in this case

### border

Border actually consists of four triangles or trapezoid facing a center point. To generate a triangle, set content width and height to 0 and:

```
.trinangle {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    border-width: 5px;
    border-style: solid;
    border-color: black transparent transparent transparent;
}
```

### outline

- Outline is NOT a part of an element's dimensions. The element's total width and height is not affected by the width of the outline
- Debug layout by adding a `outline: 1px solid red` to all elements

  ![outline](https://cloud.githubusercontent.com/assets/14355257/18029801/67e9a206-6c70-11e6-9c42-01c07208a1e3.jpg)

### overflow

- `overflow: visible;`: default, won't clip, let the content leaks out
- `overflow: hidden;`: hide additional content(no scroll bar)
- `overflow: scroll;`: add scrollbar anyway
- `overflow: auto;`: add a scrollbar only when necessary

## Layout

### Box model

![boxmodel](https://cloud.githubusercontent.com/assets/14355257/18029725/1e3362f2-6c6e-11e6-8484-af0b88e33bc2.jpg)

- By default box-sizing is `box-sizing: content-box`, whose width and height only refer to the content
- It's recommended to use `box-sizing: border-box`: whose width and height include padding and border, so padding and border won't affect layout

### Flow

1. Normal flow(常规流): One box model after another. Each box has its own formatting context

   - block
   - inline-block
   - table
   - flexbox
   - grid
   - anything non-float and position is static or relative

2. Float(浮动)：Box out of the normal flow and sits next to the current box

   - float value is not none

3. Absolute positioning（绝对定位）：Box out of the normal flow and positioned through axis (left/right/top/bottom)
   - position: absolute
   - position: fixed

Reference: http://layout.imweb.io/article/positioning-scheme.html

### Formatting context

Everything on a page is a part of a formatting context.

1. Block Formatting Context (BFC)

   - The initial block formatting context for the root element (`<html>`)
   - float, absolutely positioned elements
   - flex and grid items
   - block elements where `overflow` is not `visible`
   - elements with `display: inline-block;`
   - elements with `display: flow-root;`
   - more
   - 盒子从上到下摆放
   - vertical margin collapse
   - margins from inside and outside BFC won't collapse
   - won't overlay with float elements

2. Inline Formatting Context (IFC)
   - The context of a paragraph
   - 盒子在一行内水平摆放，放不下换行
   - text-align 决定水平对齐
   - vertical-align 决定垂直对齐
   - 避开浮动 float 元素

### display

- `display: flex`: you can make almost any layout with flex, must learn
- `display: block;` (块级) a block-level element always starts on a new line and takes up the full width available;
- `display: inline;`(行级) an inline element does not start on a new line and only takes up as much width as necessary;
- `display: none;` commonly used with JavaScript to hide and show elements. It does not take up any space, whereas `visibility:hidden` does;
- `display: inline-block;` - will be affected by text-align and vertical-align

### margin

- Margin backgrounds are always transparent
- [Margin collapsing](https://www.w3.org/TR/CSS2/box.html#collapsing-margins)

  - margins only collapse between elements in the same formatting context
  - Horizontal margins never collapse
  - Floating and absolutely positioned elements (elements not in Normal Flow) never collapse

  ![ab](https://cloud.githubusercontent.com/assets/14355257/22824643/a5a60fda-efd8-11e6-99f4-fc6b68468e70.png)

  ```
  .floated {
  	background-color: yellow;
  	margin: 50px 0;
  	float:left;
  }

  .absolute {
  	background-color: pink;
  	margin: 100px 0;
  	position: absolute;
  }
  ```

#### Example 1

Both top and bottom margins of the only child are collapsed into parent's, the value equals to whichever is larger. At the same time, the collapsed bottom margin outside container collapses with the top margin of outsider's top margin;

![mc1](https://cloud.githubusercontent.com/assets/14355257/22817430/36a26c80-efb7-11e6-8e26-64b524b68868.png)

    ```
    #container {
    	background-color: gold;
    	height: 50px;
    	width: 500px;
    	margin: 30px 0;
    }

    #child {
    	background-color: red;
    	height:50px;
    	width:50px;
    	margin:50px 0;
    }

    #outsider {
    	background-color: pink;
    	margin: 50px 0;
    }
    ```

#### Example 2

Multiple empty element will collapse and overlap with each other based on the same top line.

![sm](https://cloud.githubusercontent.com/assets/14355257/22817900/f9e47024-efb9-11e6-9d34-86d5522aafa8.png)

    ```
    #outsider-large {
    	margin: 50px 0;
    }

    #outsider-small {
    	margin: 30px 0;
    }
    ```

### Padding

- Most elements do not have a default padding, so no need to use a lot of unnecessary "padding:0" that will increase burden for loading pages

### Position

1. `position:static;`: default value, no impacts
2. `position:relative;`: make elements relative to their regular position in the flow (through top/bottom/left/right), the rest of the contents will not be affected
3. `position:fixed;`: the elements will always be in the same position of the page (fixed navbar)
4. `position:absolute;`: similar to fix, but the element position is fixed to the closest ancestor that is positioned (any kind except static). However, if an absolute positioned element has no positioned ancestors, it uses the document body, and moves along with page scrolling

### z-index and opacity

Reference: https://stackoverflow.com/questions/2837057/what-has-bigger-priority-opacity-or-z-index-in-browsers

### Float

Move the element out of the Normal Flow

#### Clear

1. clear: requires (the top border edge of the) box to be below (the bottom outer edge of) any left/right/both-floating elements **earlier** in the source document
2. clearfix for image overflow. "overflow:auto" extend the border to include all contents. Example: http://www.w3schools.com/css/tryit.asp?filename=trycss_layout_clearfix

   ```
   .clearfix {
     overflow: auto;
     zoom: 1; // IE6
   }
   ```

3. display:inline-block; ~= float + clear (http://www.w3schools.com/css/tryit.asp?filename=trycss_inline-block);

**Example 1** Before/After clear is in effect [(codepen)](http://codepen.io/PhoenixPan/pen/xgQEzx):

Before: Two elements both located according to the upper-left cornor of their parents:

![beforeclear](https://cloud.githubusercontent.com/assets/14355257/22825928/ac02b408-efdf-11e6-963b-4d48e92ee142.png)

After: The element with clear property moved below the floating element as expected. Notice, margin collapsing occured here.

![afterclear](https://cloud.githubusercontent.com/assets/14355257/22825929/acec753e-efdf-11e6-9d19-23887ab351ab.png)

    ```

 	<div id="container">
 		<p class="floated">floated</p>
 		<p class="cleared">cleared</p>
 	</div>
	```
	
	```
	#container {
		background-color: gold;
		width: 500px;
		margin: 100px 0;
	}

    .cleared {
    	background-color: pink;
    	margin: 50px 0;
    	/*clear: left;*/
    }

    .floated {
    	background-color: yellow;
    	margin: 50px 0;
    	float:left;
    }
    ```

## CSS Variables

Basic

```
:root {
  --mycolor: #000000;
}

p {
  background-color: var(--mycolor);
}
```

Variables also cascade

```
:root {
  --color: red;
}
body {
  --color: orange;
}
h2 {
  color: var(--color);
}
```

Any h2 will be orange since it will be a child of body

## Filter

Nice to have it for some special effects.

```
filter: url("filters.svg#filter-id");
filter: blur(5px);
filter: brightness(0.4);
filter: contrast(200%);
filter: drop-shadow(16px 16px 20px blue);
filter: grayscale(50%);
filter: hue-rotate(90deg);
filter: invert(75%);
filter: opacity(25%);
filter: saturate(30%);
filter: sepia(60%);
```

## Transition & Transform

### Transition

Transitions provide a way to control animation speed when changing CSS properties.  
Check all animatable properties: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animated_properties

```
/*shorthand*/
transition: property1 duration1 timing-function1 delay1, property2 duration2 timing-function2 delay2;

/*complete*/
transition-property:
transition-duration:
transition-timing-function:
transition-delay:
```

For timing function, we have:

1. ease - slow start, then fast, then end slowly (this is default)
2. linear - the same speed from start to end
3. ease-in - slow start
4. ease-out - slow end
5. ease-in-out - slow start and end
6. cubic-bezier(n,n,n,n) - cubic-bezier function. Can be used to achieve bounce effects

### Transform

1. transform: translate(10px, 10px)/translateX(10px)/translateY(10px) - change the position of this element;
2. transform: scale(10px, 10px)/scaleX(10px)/scaleY(10px) - enlarge or shrink the element;
3. transform: rotate(90deg)/rotateX(90deg)/rotateY(90deg)/rotateZ(90deg) - rotate the element;
4. Perform multiple transform in one line

   ```
   transform: translate(100px) rotate(20deg);
   ```

Transition and transform can be performed at the same time on animatable elements:

    ```
    div {
        width: 100px;
        height: 100px;
        background: red;
        transition: width 2s, height 2s, transform 2s;
    }

    div:hover {
        width: 300px;
        height: 300px;
        transform: rotate(180deg);
    }
    ```

## @media

The below media query will only apply if the media type is TV, the viewport is 700px wide or wider, and the display is in landscape.

```
@media tv and (min-width: 700px) and (orientation: landscape) { ...  }
```

@media and @viewpoint: https://dev.opera.com/articles/an-introduction-to-meta-viewport-and-viewport/

## Good articles

[The invisible parts of CSS](https://madebymike.com.au//writing/the-invisible-parts-of-CSS/?utm_source=SitePoint&utm_medium=email&utm_campaign=Versioning)
