---
title: Using Elements
summary: "Learn how to install and use Polymer Elements in your own projects."
tags: ['beginner']
updated: 2015-05-03
---

## Introduction

The Core and Paper element sets provide elements that you can use
in your web pages and apps. These elements are built with the Polymer
library.

You don't need to use Polymer directly to use these elements.
However, using Polymer you can take advantage of special
features such as data binding.

## Installing Elements

You can install elements one at a time, or install a whole collection of elements.

Polymer contains two primary collections of elements:

-   <a href="../elements/core-elements.html">Polymer Core elements</a>. A set of utility
    elements including general-purpose UI elements (such as icons, layout elements, and toolbars),
    as well as  non-UI elements providing features like AJAX, signaling and storage.

-   [Paper elements](../elements/paper-elements.html). A set of UI elements that implement the
    [material design system](../elements/material.html).

Throughout the site, you'll find component download buttons like this:

  <component-download-button org="Polymer" component="core-elements" label="GET THE Polymer CORE ELEMENTS">
  </component-download-button>

If you find an element you want while browsing the docs, simply click
the download button and choose your install method.

The component download button offers three ways to install a component or set of components:

*   Bower. **Recommended**. Bower manages dependencies, so installing a component
    also installs any missing dependencies. Bower also handles updating
    installed components. For more information, see [Installing with Bower](#using-bower).

*   ZIP file. Includes all dependencies, so you can unzip it and start using it
    immediately. The ZIP file requires no extra tools, but doesn't provide a
    built-in method for updating dependencies. For more information, see
    [Installing from ZIP files](#using-zip).

*   Github. When you clone a component from Github, you need to manage all of the dependencies
    yourself. If you'd like to hack on the project or submit a pull request, see
    [setting up Polymer with git](../../resources/tooling-strategy.html#git).

Pick your method and follow the instructions in the download dialog.

If you install one or more elements using Bower or the ZIP file, you also get the
Polymer library; as well as the [Web Components polyfills](platform.html),
which allow you to run Polymer on browsers that don't yet support
the web components standards.

### Installing with Bower

The recommended way to install **Polymer** elements
is through Bower. To install Bower, see the [Bower web site](http://bower.io/).

Bower removes the hassle of dependency management when developing or consuming
elements. When you install a component, Bower makes sure any dependencies are
installed as well.

#### Project setup

If you haven't created a `bower.json` file for your application, run this
command from the root of your project:

    bower init

This generates a basic `bower.json` file. Some of the questions, like
"What kind of modules do you expose," can be ignored by pressing Enter.

The next step is to install one or more Polymer packages:

    bower install --save Polymer/core-icons

Bower adds a `bower_components/` folder in the root of your project and
fills it with the element and its dependencies.

**Tip:** `--save` adds the item as a dependency in *your* app's bower.json:
```
{
  "name": "my-project",
  "version": "0.0.0",
  "dependencies": {
    "polymer": "Polymer/core-icons#~{{site.latest_version}}"
  }
}
```
{: .alert .alert-success }

#### Selecting packages

Using the component download button, click the **Bower** tab
and cut and paste the Bower install command.

You can also choose one of the commonly-used packages:

-   `Polymer/polymer`. Just the Polymer library
    and web components polyfills.

-   `Polymer/core-elements`. The
    [Polymer Core elements](../elements/core-elements.html)
    collection.

-   `Polymer/paper-elements`. The
    [Paper elements](../lements/paper-elements.html) collection.

For example, if you'd like to install Polymer’s collections
of pre-built elements, run the following commands from the terminal:

    bower install --save Polymer/core-elements
    bower install --save Polymer/paper-elements

#### Updating packages {#updatebower}

When a new version of Polymer is available, run `bower update`
in your app directory to update your copy:

    bower update

This updates all packages in `bower_components/`.

### Installing from ZIP files

When you download a component or component set as a ZIP file, you get all of
the dependencies bundled into a single archive. It's a great way to get
started because you don't need to install any additional tools.

Expand the ZIP file in your project directory to create a `bower_components` folder.

![](/images/zip-file-contents.png)

If you download multiple component sets as ZIP files, you'll usually end up with
multiple copies of some dependencies. You'll need to merge the contents of the
ZIP files.

Unlike Bower, the ZIP file doesn't provide a built-in method
for updating dependencies. You can manually update components with a new ZIP
file.

### Using git

Because there are a number of dependencies we suggest you install
Polymer with Bower instead of git. If you'd like to hack on
the project or submit a pull request check out our guide on
[setting up Polymer with git](../../resources/tooling-strategy.html#git).

## Using elements

To use elements, first load `webcomponents.js`. Many browsers have yet to
implement the various web components APIs. Until they do, `webcomponents.js`
provides [polyfill support](platform.html). **Be sure to include
this file before any code that touches the DOM.**

Once you have some elements installed and you've loaded `webcomponents.js`,
using an element is simply a matter of loading the element file using an
[HTML Import](../../platform/html-imports.html).

An example `index.html` file:

    <!DOCTYPE html>
    <html>
      <head>
        <!-- 1. Load webcomponents.min.js for polyfill support. -->
        <script src="bower_components/webcomponentsjs/webcomponents.min.js"></script>

        <!-- 2. Use an HTML Import to bring in the element. -->
        <link rel="import"
              href="bower_components/core-ajax/core-ajax.html">
      </head>
      <body>
        <!-- 3. Declare the element. Configure using its attributes.
        Replace '//example.com/json' with valid json file -->
        <core-ajax url="//example.com/json"
                   handleAs="json"></core-ajax>

        <script>
          // Wait for 'polymer-ready'. Ensures the element is upgraded.
          window.addEventListener('polymer-ready', function(e) {
            var ajax = document.querySelector('core-ajax');

            // Respond to events it fires.
            ajax.addEventListener('core-response', function(e) {
              console.log(this.response);
            });

            ajax.go(); // Call its API methods.
          });
        </script>
      </body>
    </html>

**Note:** You must run your app from a web server for the [HTML Imports](../../platform/html-imports.html)
polyfill to work properly. This requirement goes away when the API is available natively.
{: .alert .alert-info }

###  Passing object and array values in attributes

[HTML attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes) are string values, but sometimes you need to pass more complicated values into a custom element, such as objects or arrays. Ultimately, it's up to the element author to decide how to decode values passed in as attributes, but many Polymer elements understand attribute values that are a JSON-serialized object or array. For example:

    <roster-list persons='[{"name": "John"}, {"name": "Bob"}]'></roster-list>

For Polymer elements, you can find the expected type for each attribute listed in the [Elements reference](../elements/). If you pass the wrong type, it may be decoded incorrectly.

When creating your own Polymer elements, you can choose to expose properties as attributes, as described in [Published properties](../polymer/polymer.html#published-properties).

## Next steps

Now that you've got the basic idea of using and installing elements, it's time to start
building something!

In the next section we'll cover using the Core layout elements
to structure an application's layout.  Continue on to:

<p>
<a href="../elements/layout-elements.html">
  <paper-button raised><core-icon icon="arrow-forward" ></core-icon>Layout elements</paper-button>
</a>
</p>

To learn about building your own elements using the Polymer library, see
[Polymer in 10 minutes](creatingelements.html).

If you'd rather browse the existing elements, check out the
<a href="../elements/core-elements.html">Polymer Core elements</a>
and <a href="../elements/paper-elements.html">Paper elements</a> catalogs.
