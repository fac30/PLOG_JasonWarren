# Documentation Tools

## Links

| Name    | Repo                                            | Other                               |
| ------- | ----------------------------------------------- | ----------------------------------- |
| jsDoc   | [jsDoc](https://github.com/jsdoc/jsdoc)         | [@use JSDoc](https://jsdoc.app/)    |
| GitBook | [gitbook](https://github.com/GitbookIO/gitbook) | [GitBook](https://www.gitbook.com/) |

### Documentation & Guides

- [@use JSDoc](https://jsdoc.app/)
- [JS Docs: A Quickstart Guide](https://medium.com/@martink_rsa/js-docs-a-quickstart-guide-da6ce5df4a73)
- [Jsdoc Cheatsheet](https://devhints.io/jsdoc)
- [Integrating GitBook with JSDoc to Document Your Open Source Project](https://medium.com/@kevinast/integrate-gitbook-jsdoc-974be8df6fb3)

### Utilities

- jsDoc-to-markdown • [Repo](https://github.com/jsdoc2md/jsdoc-to-markdown): *Generate markdown documentation from jsdoc-annotated javascript*
- jsdoc-action • [Repo](https://github.com/andstor/jsdoc-action) • [Marketplace](https://github.com/marketplace/actions/jsdoc-action): *GitHub Action to build JSDoc documentation*
- better-docs • [Repo](https://github.com/SoftwareBrothers/better-docs) • [Example](https://softwarebrothers.github.io/example-design-system/index.html): *Beautiful toolbox for jsdoc generated documentation - with 'typescript', `category` and `component` plugins

---

## Guides

### Getting Started with JSDoc 3

#### Getting started

JSDoc 3 is an API documentation generator for JavaScript, similar to Javadoc or phpDocumentor. You add documentation comments directly to your source code, right alongside the code itself. The JSDoc tool will scan your source code and generate an HTML documentation website for you.

#### Adding documentation comments to your code

JSDoc's purpose is to document the API of your JavaScript application or library. It is assumed that you will want to document things like modules, namespaces, classes, methods, method parameters, and so on.

JSDoc comments should generally be placed immediately before the code being documented. Each comment must start with a `/**` sequence in order to be recognized by the JSDoc parser. Comments beginning with `/*`, `/***`, or more than 3 stars will be ignored. This is a feature to allow you to suppress parsing of comment blocks.

##### The simplest documentation is just a description

```js
/** This is a description of the foo function. */
function foo() {
}
```

Adding a description is simple—just type the description you want in the documentation comment.

Special "JSDoc tags" can be used to give more information. For example, if the function is a constructor for a class, you can indicate this by adding a @constructor tag.

##### Use a JSDoc tag to describe your code

```js
/**
 * Represents a book.
 * @constructor
 */
function Book(title, author) {
}
```

More tags can be used to add more information. See the home page for a complete list of tags that are recognized by JSDoc 3.

##### Adding more information with tags

```js
/**
 * Represents a book.
 * @constructor
 * @param {string} title - The title of the book.
 * @param {string} author - The author of the book.
 */
function Book(title, author) {
}
```

#### Generating a website

Once your code is commented, you can use the JSDoc 3 tool to generate an HTML website from your source files.

By default, JSDoc uses the built-in "default" template to turn the documentation into HTML. You can edit this template to suit your own needs or create an entirely new template if that is what you prefer.

##### Running the documentation generator on the command line

```bash
jsdoc book.js
```

This command will create a directory named out/ in the current working directory. Within that directory, you will find the generated HTML pages.

---
