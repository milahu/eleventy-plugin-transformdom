# DOM Transforming plugin

Process & change the generated HTML output of your [eleventy](https://www.11ty.io/) site.

You give the plugin CSS selectors and transform functions, then the plugin will run them on each HTML file generated by eleventy.

Some of the things you could use it to do include:

- [Replace any text on the page](./examples/replace-text)
- [Add `loading="lazy"` attributes to your images](./examples/loading-lazy)
- [Append any script (like Google Analytics) to the page](./examples/google-analytics)
- [Set the page `title` tag to match the first heading on the page](./examples/page-title)
- [Import web components (only on pages they're used)](./examples/web-components)
- And more!

---

**Like this project? Buy me a coffee via [PayPal](https://paypal.me/liamfiddler) or [ko-fi](https://ko-fi.com/liamfiddler)**

---

## Getting started

### Install the plugin

In your project directory run:

```console
# Using npm
npm install eleventy-plugin-transformdom --save-dev

# Or using yarn
yarn add eleventy-plugin-transformdom --dev
```

Then update your project's `.eleventy.js` to use the plugin:

```javascript
const transformDomPlugin = require('eleventy-plugin-transformdom');

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(transformDomPlugin, [
    // your Transform Items go here, refer to documentation below
  ]);
};
```

### Write your Transform Items

The plugins takes an array of Transform Items as configuration.

A _Transform Item_ is an Object that contains two keys; `selector` & `transform`

Each Transform Item will be run in order. This means you can write a Transform Item that modifies the DOM, then the next Transform Item can further modify the resulting DOM, and so on.

#### selector

| Key | Type | Required? |
|---|---|:-:|
| selector | `string` | yes |

The "selector" is a [CSS selector string](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors).

Multiple selectors may be separated with commas.

#### transform

| Key | Type | Required? |
|---|---|:-:|
| transform | `(args) => void` | yes |

The "transform" is a function that will be run on the generated HTML.

The function will be passed an Object as argument. The `args` Object has the following entries:

| Key | Type | Description |
|---|---|---|
| elements | `Element[]` | An array of elements in the DOM matching the provided `selector` |
| window | `DOMWindow` | The `window` of the generated HTML |
| document | `Document` | The `window.document` of the generated HTML |
| inputPath | `string` | The source file path from which Eleventy is generating the HTML |
| outputPath | `string` | The output path/filename of the HTML file being generated |
| inputDir | `string` | The source directory from which Eleventy is building the site ([see the Eleventy docs](https://www.11ty.dev/docs/config/#input-directory)) |
| outputDir | `string` | The directory inside which the finished templates will be written to by Eleventy ([see the Eleventy docs](https://www.11ty.dev/docs/config/#output-directory)) |

### Completed example with config

Here's an example with a Transform Item that finds all the images in your HTML and adds `loading="lazy"` to the markup:

```javascript
const transformDomPlugin = require('eleventy-plugin-transformdom');

module.exports = function (eleventyConfig) {
  eleventyConfig.addPlugin(transformDomPlugin, [
    {
      selector: 'img',
      transform: ({ elements }) => {
        elements.forEach((img) => {
          img.setAttribute('loading', 'lazy');
        });
      },
    },
  ]);
};
```

## Contributing

This project welcomes suggestions and Pull Requests!

## Authors

- **Liam Fiddler** - _Initial work / maintainer_ - [@liamfiddler](https://github.com/liamfiddler)

See also the list of
[contributors](https://github.com/liamfiddler/eleventy-plugin-transformdom/contributors)
who participated in this project.

## License

This project is licensed under the MIT License -
see the [LICENSE](LICENSE) file for details

## Acknowledgments

- The wonderfully supportive team at
  [Future Friendly](https://futurefriendly.team)
- Everyone who has contributed to the
  [11ty](https://www.11ty.io/) and [JSDOM](https://github.com/jsdom/jsdom) projects, without whom
  this plugin wouldn't run

---

**Like this project? Buy me a coffee via [PayPal](https://paypal.me/liamfiddler) or [ko-fi](https://ko-fi.com/liamfiddler)**

---
