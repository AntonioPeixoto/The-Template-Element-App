The `template` element
======================

Sometimes we need to make a web page whose content is not known at
design time, we only know its structure, but not the actual values.

The **HTML Content Template element** (`<template>`) was designed to let
us address this need. With it, we can design a template that describes
the content/data structure.

The template built with `<template>` tag isn\'t rendered by the browser,
it will be used by JavaScript code. The JavaScript code will receive the
data - possibly from a server - and will populate the template with it.

> The `template` element is used to declare fragments of HTML that can
> be cloned and inserted in the document by script.
>
> In a rendering, the
> `                         template                     `element
> [represents](https://www.w3.org/TR/html52/dom.html#represent) nothing.
>
> The [template
> contents](https://www.w3.org/TR/html52/semantics-scripting.html#template-contents)
> of a `                         template` element [are not children of
> the element
> itself](https://www.w3.org/TR/html52/syntax.html#template-syntax).
>
> in [The template
> element](https://www.w3.org/TR/html52/semantics-scripting.html#the-template-element)
> by HTML 5.2 W3C Recommendation

Example
-------

In this example, we will structure a table without real data in it. The
content structure will be described by **HTMLContent Template element**,
`<template>` for short.

    HTML code

    <table id="productCatalog">
        <thead>
            <tr>
                <th>Product Name</th>
                <th>Product Description</th>
            </tr>
        </thead>
        <tbody id="products">
            <!-- existing data could optionally be included here -->
            <template id="product">
                <tr>
                    <td id="productName"></td>
                    <td id="productDescription"></td>
                </tr>
            </template>
        </tbody>
    </table>

We begin by defining a table that represents the data structure we want
to represent. The actual data is modelled by the HTML tags between
\<template\> and \</template\> and will not be rendered by the browser.

The next step is to write the JavaScript code the will populate the
template with actual data. In the example, we created a JSON object that
simulates the data sent by the server.

The following steps describe the process used to populate the template
with actual data:

1.  Verify if the browser supports templates if no execute fallback
2.  Get a reference to the table body, in our case a list of products we
    want to show
3.  Get a reference to our template
4.  Read the template (clone it)
5.  Iterate over an array of objects and put its properties in the
    template fields described by its id attribute

Follow the code below:

    JavaScript code

    if ("content" in document.createElement("template")) {
      // Instantiate the table with the existing HTML tbody
      let tbody = document.getElementById("products");

      // Instantiate the table row with the template
      let template = document.getElementById("product");

      // Insert each product in the products table
      for (product of products) {

        // Clone the template
        let clone = template.content.cloneNode(true);

        // Populate template with the data received in JSON
        let productName = clone.getElementById("productName");
        let productDescription = clone.getElementById("productDescription");
        productName.textContent = product.productName;
        productDescription.textContent = product.productDescription;

        // Append the template populated with data at the end of the table body
        tbody.appendChild(clone);
      }
    } else {
      document.write("Your browser do not supports templates. Update it!");
      // Find another way to add the rows to the table because
      // the HTML template element is not supported.
    }          
 
[See the results](https://antoniopeixoto.github.io/The-Template-Element-App/)
