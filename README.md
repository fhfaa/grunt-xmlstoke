# grunt-xmlstoke

> Created/Reads/Updated/Deletes values in XML files based on XPath queries.  Similar to the `xmlstoke` task in NAnt.


## Getting Started
This plugin requires Grunt `~0.4.2`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-xmlstoke --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-xmlstoke');
```

## The "xmlstoke" task

### Overview
In your project's Gruntfile, add a section named `xmlstoke` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  xmlstoke: {
    updateTitle: {
      options: {
        xpath: '//title',
        value: 'The Good Parts'
      },
      files: {
        'dest.xml': 'src.xml'
      },
    },
  },
})
```

### Options

#### options.xpath
Type: `String`
Default value: `''`

An xpath query to select one or more nodes in the source document.

#### options.value
Type: `String` or `Function`
Default value: `''`

A string value to which the value of any matched node is set.

You can also supply a function that returns the replacement value.  The first
argument supplied to the function will be the node on which the replacement is 
being made.

#### options.replacements
Type: `Array`
Default value: `undefined`

An array of replacement options (i.e. objects with `xpath` and `value` properties)

#### options.insertions
Type: `Array`
Default value: `undefined`

An array of insertion options (i.e. objects with `xpath`, `value` and `node` properties)

The new node/attr is attached to all nodes matching the xpath selector.

`xpath` is the selector for the element(s) to be manipulated

`node` is the node name to be inserted (if-exist-update), e.g. `new-elem` or `@new-attr`

`value` is the value to be given to the new attr/note. Default to `""`

### Usage Examples

#### Basic Usage
In this example, the text content of an element is set to a static value. So if the `testing.xml` file has the content `<abc></abd>`, the generated result would be `<abc>123</abc>`.

```js
grunt.initConfig({
  setTheNumber: {
    xmlstoke: {
      options: {
        xpath: '/abc',
        value: '123'
      },
      files: {
        'dest/basic_usage.xml': 'src/testing.xml',
      },
    },
  },
})
```

#### Attribute Example
In this example, the value of an attribute is cleared. So if the `testing.xml` file has the content `<x y="999" />`, the generated result in this case would be `<x y="" />`.

```js
grunt.initConfig({
  xmlstoke: {
    updateAnAttribute: {
      options: {
        xpath: '/x/@y',
        value: ''
      },
      files: {
        'dest/attribute_example.xml': 'src/testing.xml',
      },
    },
  },
})
```

#### Function Example
In this example, the value of an attribute is modified. So if the `testing.xml` file has the content `<x y="abc" />`, the generated result in this case would be `<x y="ABC" />`.

```js
grunt.initConfig({
  xmlstoke: {
    upperCaseTheY: {
      options: {
        xpath: '/x/@y',
        value: function (node) { return node.value.toUpperCase(); }
      },
      files: {
        'dest/function_example.xml': 'src/testing.xml',
      },
    },
  },
})
```

#### options.deletions
Type: `Array`
Default value: `undefined`

An array of deletion options (i.e. objects with an `xpath`, property)

TODO: doc/examples

#### Multiple XPath Queries
In this example, the same value is put intp multiple locations. So if the `testing.xml` file has the content `<x y="999" />`, the generated result in this case would be `<x y="111">111</x>`.

```js
grunt.initConfig({
  xmlstoke: {
    updateAllTheThings: {
      options: {
        xpath: ['/x/@y','/x'],
        value: '111'
      },
      files: {
        'dest/multiple_xpath_queries.xml': 'src/testing.xml',
      },
    },
  },
})
```

#### Multiple Replacements
In this example, multiple replacements take place at once. So if the `testing.xml` file has the content `<x y="999" />`, the generated result in this case would be `<x y="111">M</x>`.

```js
grunt.initConfig({
  xmlstoke: {
    updateACoupleOfThings: {
      options: {
        replacements: [{
          xpath: '/x/@y',
          value: '111'
        }, {
          xpath: '/x',
          value: 'M'
        }]
      },
      files: {
        'dest/multiple_replacements.xml': 'src/testing.xml',
      },
    },
  },
})
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History
 - 0.1.0 &mdash; Initial release
 - 0.2.0 &mdash; Multiple replacements at once
 - 0.2.1 &mdash; Color filename when logged
 - 0.3.0 &mdash; Allow specifying replacement value as a function
 - 0.4.0 &mdash; Allow specifying namespaces
 - 0.5.0 &mdash; Allow adding elements or attributes via "insertions" option
 - 0.5.1 &mdash; TODO
 - 0.5.2 &mdash; Allow [index] selection for insertion xpath (stripped from nodeName for actual insertion)
         &mdash; Allow removing elements via "deletions" option (untested)
