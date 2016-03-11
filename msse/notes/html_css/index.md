---
title: Web Application Development
layout: default
---

# HTML and CSS Overview

## Mike Calvo

## mike@citronellasoftware.com

---

# HTML
- Document markup language
- Defines:
  - Document Structure
  - Linking
  - Layout
  - Formatting
  - User Input

---

# XML Syntax
- Element: `<p id="description">My Paragraph</p>`
- Tag: p
- Attribute: id

---

# Basic Document Structure

``` html
<!DOCTYPE html>
<html>
  <head>
    <!-- This is a comment -->
    <!-- The stuff in the head is meta data -->
  </head>
  <body>
    <!-- Stuff you want to display has to go here -->
  </body>
</html>
```

---

# HTML Document Structure
- p: Paragraph
- div: Division Block (container of containers)
- span: Inline Text

---

# HTML Escaping
- Characters used to define HTML need to be escaped
- \< `&lt;`
- \> `&gt;`
- \& `&amp;`
- \' `&quot;`
- Generically: `&#ascii_code;`

---

# HTML Forms: User Input
- Elements that render UI controls
- INPUT tag: text fields, text areas, checkbox
- SELECT tag: dropdown
- BUTTON tag: buttons, radios
- FORM tag: bundles inputs and defines POST/PUT

---

# HTML Rendering Engines
- Implementations of the HTML Specification
- Webkit - Apple (Safari, Chrome < 28)
- Blink - Google (Chome > 28)
- Gecko - Mozilla (Firefox)
- Trident - Microsoft (IE)

---

# HTML 5
- Big additions to HTML
- Media Support (movie and audio)
- Direct Render 2D Graphics (canvas)
- Dynamic Functionality (dialog and details)
- Structure (header, footer, main, section)

---

# HTML 5 Support
- Great features
- Not available on older browsers (mainly IE 8 and below)
- Can impact internal apps
  - Corporate IT likes to standardize on stable (nee old) browsers

---

# HTML Tables
- Organize table-driven data
- TABLE element contains:
  - Heading: THEAD
  - Body: TBODY
- Heading and Body contains Rows: TR
- Rows contain Data: TD (columns)

---

# When To Use Tables
- Tables have come in and out of fashion
- Tables are great for tabular data
- Tables should be avoided for general layout purposes

---

# HTML Formatting
- Styling tags:
  - B (bold), I (italic), U (underline), CENTER
- Old remnants of bygone era
- Styling and formatting (other than tables) should be done with CSS

---

# CSS
- Cascading style sheets
- Declarative styling of HTML
- Based on selectors
  - 'Selecting' the elements the style applies to
- Vast array of styling features

---

# Stylesheet
- A file containing styling directions
- Series of selectors and corresponding style declarations
- Each declaration is a collection of property, value pairs

---

# Example Stylesheet

``` css
div: {
  color: black;
}
```

---

# CSS Selectors
- by tag: just the tag name
- by id: `#idValue`
- by class: .className (class attribute)

---

# Class attribute
- Applies CSS style classes to element
- Single value or space-separated list

---

# Decendent Selectors
- CSS supports specifying nesting of selectors
- Seperate selector components with spaces
- Example: 'all paragraphs with class error within a div':
  `div p .error { ... }`

---

# More Specific Decendents
- `div + p` => The first p within each div
- `div > p` => All direct p children of each div
- `div ~ p` => The first p anywhere below each div

- To be even more specific add a + between selectors to indicate the selctors must be adjacent

---

# Attribute Selectors
- Has an attribute: `a[src]`
- Has an attribute with specific value: `a[title="main"]`
- Has an attribute containing: `a[href*="php"]`
- Has an attribute starting with: `a[href^="http"]`
- Has an attribute with regex match: `a[href$="http"]`
- Has an attribute with a list of values that contains: `a[class~="cool"]`

---

# Pseudo selectors
- Special qualifications that apply styles based on behaviors
  - Append a ':' to the selector followed by name
- Pseudo elements: qualify specifics about children
  - Append a '::' to the selector by name

---

# Pseudo Examples
- checked (`option:checked { color: red; }`)
- hover (`.info:hover { font-weight: bold; }`)
- visited and link
- before and after
- not (`div:(#container) { color: 'blue'; }`)

---

# More Pseudo Examples
- first-child
- last-child
- nth-child(n)
- nth-last-child(n)
- nth-of-type(n) : only grab elements matching type before :

---

# Pseudo Element Examples
- first-line (p::first-line)
- first-letter (p::first-letter { font-family:cursive; })

---

# Non-Stylesheets
- CSS can appear in other places
- Internal style elements within HTML
- Every HTML element has a style attribute for inline style

---

# Adding CSS Reference To HTML
- Put a reference to stylesheet in HEAD element:
`<link rel="stylesheet" type="text/css" href="path/to/file">`
- Can be relative file or http reference

---

# Style Elements
- Can appear in HEAD or BODY
- Syntax is the same as CSS files
`<style>div { color: red; }</style>`

---

# Cascade?
- Styles applied in priority order
- Least-specific to most-specific
- Furthest from element to closest
- Browser: External Stylesheet : Internal Style : Inline
- Styles are inherited by document structure

---

# Debugging Styles
- Browser Developer Tools
- Inspect Element
- View Style/CSS Cascade
- See which style is applied and from where

---

# Force a Style
- place !important after property to insure it's honored

``` css
div {
  color: red !important;
}
```

---

# What Can I Style
- Layout: block, inline, hidden, collapsed, alignment
- Spacing: margin, padding
- Text: color, size, font, weight
- Backgrounds: color, images
- Borders
- Positioning

---

# CSS Positioning
- Control how an element is displayed on the screen
- Control how other elements behave because of the existence of the element
- Normal flow
  - Inline (span)
  - Block (p, div, table, ul)
  - Controlled by 'display' property

---

# CSS Position Property
- static - 'normal position'
- relative - adjust slightly from normal (top, bottom, left, right)
- absolute - put it exactly here (contrained by relative parent)
- float - push the element as far as possible right or left
- clear - reset the float behavior of my siblings for me

---

# CSS Units
- Some properties require a unit to be specified
- px: pixel
- em: size of 'm' in current font
- %
- pt: 1/72 of 1 inch
- 0 values do not require units

---

# Shorthand Properties
- Some groups of properties can be set with a single space seperated property

``` css
div {
  border: 1px black solid; /* sets 12 properties */
  padding: 5px; /* sets top right, bottom, left */
  margin: 1px 5px 1px 0; /* top, right, bottom, left */
  background: white url(foo.gif) no-repeat top right; /* sets 4 properties */
  font: italic bold .8em 1.2 Arial;
}
```

---

# CSS3
- Major release for modern, interactive web apps
- Color support: HSL and RGBA
- Text effects: show, overlap, word-wrap
- Web Fonts
- Backgrounds - multiple backgrounds
- Transitions and animations
- Media Queries

---

# Media Queries
- Control what media or devices a style sheet block applies to
- Media can be print, tv, screen, speech or all
- Media features that can be specified include
  - width and height
  - orientation
  - resolution
  - aspect-ratio

---

# Media Query Example

``` css
@media (max-width: 600px) {
  .sidebar {
    display: none;
  }
}

@media (min-width: 700px) and (orientation: landscape) {

}

@media screen and (min-aspect-ratio: 1/1) {

}
```
