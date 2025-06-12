# Document Object Model (DOM)
The `<head>` element usually contains elements that are not directly printed on the page, like the page title, while all main page elements are located under `<body>`. Other important elements include the `<style>`, which holds the page's CSS code, and the `<script>`, which holds the JS code of the page, as we will see in the next section.

Each of these elements is called a [DOM (Document Object Model)](https://en.wikipedia.org/wiki/Document_Object_Model)

Definition of DOM:
`"The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."`

The DOM standard is separated into 3 parts:

- `Core DOM` - the standard model for all document types
- `XML DOM` - the standard model for XML documents
- `HTML DOM` - the standard model for HTML documents

For example, from the above tree view, we can refer to DOMs as `document.head` or `document.h1`, and so on.

Understanding the HTML DOM structure can help us understand where each element we view on the page is located, which enables us to view the source code of a specific element on the page and look for potential issues. We can locate HTML elements by their id, their tag name, or by their class name.