HTML 
-stands for HyperText Markup Language
-used to create web pages
-tag based language : you define the structure of the content of a document using tags.
	examples of tags
		- `<html>...</html>` Start and end HTML
		- `<head>...</head>` Head of page, not actual content
		- `<title>...</title>` Title of page
		- `<body>...</body>` Body of page, where the content goes
		- `<div>...</div>` Content section – [Block level](http://www.w3schools.com/html/html_blocks.asp)
		- `<span>...</span>` Grouping – [Inline](http://www.w3schools.com/html/html_blocks.asp)
		- `<p>...</p>` Paragraph
		- `<b>...</b>` Bold
		- `<br />` Line break (you’ll notice that this tag doesn’t have any content and therefore is both an begin and end tag, with the slash)
		- `<H1></H1>` (Also H2, H3, H4, etc..)
		- `<!-- ... -->` Comments
		- `<blink>...</blink>` Make your text blink
		- `<a href="http://...">...</a>` Link to another page. The “`href=""`” portion is an attribute. Many tags have optional attributes.

A block-level element 
-always starts on a new line
-always takes up the full width available
-ex) <p> and <div>

An inline element
-does not start on a new line
-only takes up as much width as necessary

The `<div>` element
-a container for other HTML elements.
-has no required attributes but `style`, `class` and `id` are common.

The `<span>` element
-an inline container used to mark up a part of a text, or a part of a document.

###Nesting
###Indenting
###Attrubutes
	-href, id, class,src
	-An attribute is something you add inside the opening tag to give extra information about the element.
###image
###style=CSS (cascading style sheets)


**HTML DOM** (HTML Document Object Model)

---

### HTML (HyperText Markup Language)
- Creates the **structure** of your webpage
- Written in `.html` files
- Uses tags like `<p>`, `<div>`, `<h1>`, etc.

JavaScript
- Adds **interactivity** to your webpage
- Written in script tags or .js files
- Can **manipulate** the HTML that's already on the page

**Step 1:** You write HTML (this creates the webpage structure)
**Step 2:** JavaScript accesses that HTML element
**Step 3:** JavaScript changes it

When JavaScript accesses an HTML element, it doesn't change the HTML into JavaScript. Instead, JavaScript creates a **representation** of that HTML element that you can work with in your code. This representation is called an **object**, and it has properties like `innerHTML`.


