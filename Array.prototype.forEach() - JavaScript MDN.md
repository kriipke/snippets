# Array.prototype.forEach() - JavaScript | MDN
# [Javascript] forEach()

The **`forEach()`** method executes a provided function once for each array element.

## [Try it](#try_it "Permalink to Try it")

## [Syntax](#syntax "Permalink to Syntax")

    // Arrow function
    forEach((element) => { /* ... */ })
    forEach((element, index) => { /* ... */ })
    forEach((element, index, array) => { /* ... */ })

    // Callback function
    forEach(callbackFn)
    forEach(callbackFn, thisArg)

    // Inline callback function
    forEach(function(element) { /* ... */ })
    forEach(function(element, index) { /* ... */ })
    forEach(function(element, index, array){ /* ... */ })
    forEach(function(element, index, array) { /* ... */ }, thisArg) 

Copy to Clipboard

### [Parameters](#parameters "Permalink to Parameters")

`callbackFn`

Function to execute on each element.

The function is called with the following arguments:

`element`

The current element being processed in the array.

`index`

The index of `element` in the array.

`array`

The array `forEach()` was called upon.

`thisArg` Optional

Value to use as `this` when executing `callbackFn`.

### [Return value](#return_value "Permalink to Return value")

`undefined`.

## [Description](#description "Permalink to Description")

`forEach()` calls a provided `callbackFn` function once for each element in an array in ascending index order. It is not invoked for index properties that have been deleted or are uninitialized. (For sparse arrays, [see example below](#no_operation_for_uninitialized_values_sparse_arrays).)

`callbackFn` is invoked with three arguments:

1.  the value of the element
2.  the index of the element
3.  the Array object being traversed

If a `thisArg` parameter is provided to `forEach()`, it will be used as callback's `this` value. The `thisArg` value ultimately observable by `callbackFn` is determined according to [the usual rules for determining the `this` seen by a function](/en-US/docs/Web/JavaScript/Reference/Operators/this).

The range of elements processed by `forEach()` is set before the first invocation of `callbackFn`. Elements which are assigned to indexes already visited, or to indexes outside the range, will not be visited by `callbackFn`. If existing elements of the array are changed or deleted, their value as passed to `callbackFn` will be the value at the time `forEach()` visits them; elements that are deleted before being visited are not visited. If elements that are already visited are removed (e.g. using [`shift()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)) during the iteration, later elements will be skipped. ([See this example, below](#modifying_the_array_during_iteration).)

**Warning:** Concurrent modification of the kind described in the previous paragraph frequently leads to hard-to-understand code and is generally to be avoided (except in special cases).

`forEach()` executes the `callbackFn` function once for each array element; unlike [`map()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) or [`reduce()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) it always returns the value [`undefined`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) and is not chainable. The typical use case is to execute side effects at the end of a chain.

`forEach()` does not mutate the array on which it is called. (However, `callbackFn` may do so)

**Note:** There is no way to stop or break a `forEach()` loop other than by throwing an exception. If you need such behavior, the `forEach()` method is the wrong tool.

Early termination may be accomplished with:

-   A simple [for](/en-US/docs/Web/JavaScript/Reference/Statements/for) loop
-   A [for...of](/en-US/docs/Web/JavaScript/Reference/Statements/for...of) / [for...in](/en-US/docs/Web/JavaScript/Reference/Statements/for...in) loops
-   [`Array.prototype.every()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
-   [`Array.prototype.some()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
-   [`Array.prototype.find()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
-   [`Array.prototype.findIndex()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)

Array methods: [`every()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every), [`some()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some), [`find()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find), and [`findIndex()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) test the array elements with a predicate returning a truthy value to determine if further iteration is required.

**Note:** `forEach` expects a synchronous function.

`forEach` does not wait for promises. Make sure you are aware of the implications while using promises (or async functions) as `forEach` callback.

    const ratings = [5, 4, 5];
    let sum = 0;

    const sumFunction = async (a, b) => a + b;

    ratings.forEach(async (rating) => {
      sum = await sumFunction(sum, rating);
    });

    console.log(sum);
    // Naively expected output: 14
    // Actual output: 0 

Copy to Clipboard

## [Examples](#examples "Permalink to Examples")

### [No operation for uninitialized values (sparse arrays)](#no_operation_for_uninitialized_values_sparse_arrays "Permalink to No operation for uninitialized values (sparse arrays)")

    const arraySparse = [1, 3,, 7];
    let numCallbackRuns = 0;

    arraySparse.forEach((element) => {
      console.log({ element });
      numCallbackRuns++;
    });

    console.log({ numCallbackRuns });

    // 1
    // 3
    // 7
    // numCallbackRuns: 3
    // comment: as you can see the missing value between 3 and 7 didn't invoke callback function. 

Copy to Clipboard

### [Converting a for loop to forEach](#converting_a_for_loop_to_foreach "Permalink to Converting a for loop to forEach")

    const items = ['item1', 'item2', 'item3'];
    const copyItems = [];

    // before
    for (let i = 0; i < items.length; i++) {
      copyItems.push(items[i]);
    }

    // after
    items.forEach((item) => {
      copyItems.push(item);
    }); 

Copy to Clipboard

### [Printing the contents of an array](#printing_the_contents_of_an_array "Permalink to Printing the contents of an array")

**Note:** In order to display the content of an array in the console, you can use [`console.table()`](/en-US/docs/Web/API/console/table "console.table()"), which prints a formatted version of the array.

The following example illustrates an alternative approach, using `forEach()`.

The following code logs a line for each element in an array:

    const logArrayElements = (element, index, array) => {
      console.log('a[' + index + '] = ' + element);
    };

    // Notice that index 2 is skipped, since there is no item at
    // that position in the array...
    [2, 5,, 9].forEach(logArrayElements);
    // logs:
    // a[0] = 2
    // a[1] = 5
    // a[3] = 9 

Copy to Clipboard

### [Using thisArg](#using_thisarg "Permalink to Using thisArg")

The following (contrived) example updates an object's properties from each entry in the array:

    function Counter() {
      this.sum = 0
      this.count = 0
    }

    Counter.prototype.add = function(array) {
      array.forEach(function countEntry(entry) {
        this.sum += entry;
        ++this.count;
      }, this);
    };

    const obj = new Counter();
    obj.add([2, 5, 9]);
    console.log(obj.count); // 3
    console.log(obj.sum); // 16 

Copy to Clipboard

Since the `thisArg` parameter (`this`) is provided to `forEach()`, it is passed to `callback` each time it's invoked. The callback uses it as its `this` value.

**Note:** If passing the callback function used an [arrow function expression](/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions), the `thisArg` parameter could be omitted, since all arrow functions lexically bind the [`this`](/en-US/docs/Web/JavaScript/Reference/Operators/this) value.

### [An object copy function](#an_object_copy_function "Permalink to An object copy function")

The following code creates a copy of a given object.

There are different ways to create a copy of an object. The following is just one way and is presented to explain how `Array.prototype.forEach()` works by using ECMAScript 5 `Object.*` meta property functions.

    const copy = (obj) => {
      const copy = Object.create(Object.getPrototypeOf(obj));
      const propNames = Object.getOwnPropertyNames(obj);
      propNames.forEach((name) => {
        const desc = Object.getOwnPropertyDescriptor(obj, name);
        Object.defineProperty(copy, name, desc);
      });
      return copy;
    };

    const obj1 = { a: 1, b: 2 };
    const obj2 = copy(obj1); // obj2 looks like obj1 now 

Copy to Clipboard

### [Modifying the array during iteration](#modifying_the_array_during_iteration "Permalink to Modifying the array during iteration")

The following example logs `one`, `two`, `four`.

When the entry containing the value `two` is reached, the first entry of the whole array is shifted off—resulting in all remaining entries moving up one position. Because element `four` is now at an earlier position in the array, `three` will be skipped.

`forEach()` does not make a copy of the array before iterating.

    const words = ['one', 'two', 'three', 'four'];
    words.forEach((word) => {
      console.log(word);
      if (word === 'two') {
        words.shift(); //'one' will delete from array
      }
    }); // one // two // four

    console.log(words); // ['two', 'three', 'four'] 

Copy to Clipboard

### [Flatten an array](#flatten_an_array "Permalink to Flatten an array")

The following example is only here for learning purpose. If you want to flatten an array using built-in methods you can use [`Array.prototype.flat()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat).

    const flatten = (arr) => {
      const result = [];
      arr.forEach((i) => {
        if (Array.isArray(i)) {
          result.push(...flatten(i));
        } else {
          result.push(i);
        }
      });
      return result;
    }

    // Usage
    const nested = [1, 2, 3, [4, 5, [6, 7], 8, 9]];
    console.log(flatten(nested)); // [1, 2, 3, 4, 5, 6, 7, 8, 9] 

Copy to Clipboard

## [Specifications](#specifications "Permalink to Specifications")

| Specification |
| ------------- |

\| [ECMAScript Language Specification  
# sec-array.prototype.foreach](https://tc39.es/ecma262/multipage/indexed-collections.html#sec-array.prototype.foreach) \|

## [Browser compatibility](#browser_compatibility "Permalink to Browser compatibility")

[Report problems with this compatibility data on GitHub](https://github.com/mdn/browser-compat-data/issues/new?body=%3C%21--+Tips%3A+where+applicable%2C+specify+browser+name%2C+browser+version%2C+and+mobile+operating+system+version+--%3E%0A%0A%23%23%23%23+What+information+was+incorrect%2C+unhelpful%2C+or+incomplete%3F%0A%0A%23%23%23%23+What+did+you+expect+to+see%3F%0A%0A%23%23%23%23+Did+you+test+this%3F+If+so%2C+how%3F%0A%0A%0A%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EMDN+page+report+details%3C%2Fsummary%3E%0A%0A*+Query%3A+%60javascript.builtins.Array.forEach%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FArray%2FforEach%0A*+Report+started%3A+2022-04-16T01%3A55%3A33.652Z%0A%0A%3C%2Fdetails%3E&title=javascript.builtins.Array.forEach+-+%3CPUT+TITLE+HERE%3E "Report an issue with this compatibility data")

|     | desktop | mobile | server |
| --- | ------- | ------ | ------ |
|     |         |        |        |

Chrome

 \| 

Edge

 \| 

Firefox

 \| 

Internet Explorer

 \| 

Opera

 \| 

Safari

 \| 

WebView Android

 \| 

Chrome Android

 \| 

Firefox for Android

 \| 

Opera Android

 \| 

Safari on iOS

 \| 

Samsung Internet

 \| 

Deno

 \| 

Node.js

|  |
|  |

\| 

`forEach`

 | Full supportChrome1

Toggle history | Full supportEdge12

Toggle history | Full supportFirefox1.5

Toggle history | Full supportInternet Explorer9

Toggle history | Full supportOpera9.5

Toggle history | Full supportSafari3

Toggle history | Full supportWebView Android37

Toggle history | Full supportChrome Android18

Toggle history | Full supportFirefox for Android4

Toggle history | Full supportOpera Android10.1

Toggle history | Full supportSafari on iOS1

Toggle history | Full supportSamsung Internet1.0

Toggle history | Full supportDeno1.0

Toggle history | Full supportNode.js0.10.0

Toggle history |

### Legend

Full support

Full support

The compatibility table on this page is generated from structured data. If you'd like to contribute to the data, please check out [https://github.com/mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) and send us a pull request.

## [See also](#see_also "Permalink to See also")

-   [Polyfill of `Array.prototype.forEach` in `core-js`](https://github.com/zloirock/core-js#ecmascript-array)
-   [`Array.prototype.find()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
-   [`Array.prototype.findIndex()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)
-   [`Array.prototype.map()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
-   [`Array.prototype.filter()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
-   [`Array.prototype.every()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
-   [`Array.prototype.some()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
-   [`Map.prototype.forEach()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach)
-   [`Set.prototype.forEach()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/forEach)

#### Found a problem with this page?

-   [Edit on **GitHub**](https://github.com/mdn/content/edit/main/files/en-us/web/javascript/reference/global_objects/array/foreach/index.md "You're going to need to sign in to GitHub first (Opens in a new tab)")
-   [Source on **GitHub**](https://github.com/mdn/content/blob/main/files/en-us/web/javascript/reference/global_objects/array/foreach/index.md?plain=1 "Folder: en-us/web/javascript/reference/global_objects/array/foreach (Opens in a new tab)")
-   [Report a problem with this content on **GitHub**](https://github.com/mdn/content/issues/new?template=page-report.yml&mdn-url=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FArray%2FforEach&metadata=%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EPage+report+details%3C%2Fsummary%3E%0A%0A*+Folder%3A+%60en-us%2Fweb%2Fjavascript%2Freference%2Fglobal_objects%2Farray%2Fforeach%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FArray%2FforEach%0A*+GitHub+URL%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fblob%2Fmain%2Ffiles%2Fen-us%2Fweb%2Fjavascript%2Freference%2Fglobal_objects%2Farray%2Fforeach%2Findex.md%0A*+Last+commit%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fcommit%2F83f138b147968796682b7981e2dd74101de9d86b%0A*+Document+last+modified%3A+2022-03-28T20%3A59%3A28.000Z%0A%0A%3C%2Fdetails%3E "This will take you to GitHub to file a new issue")
-   Want to fix the problem yourself? See [our Contribution guide](https://github.com/mdn/content/blob/main/README.md).

**Last modified:** Mar 28, 2022, [by MDN contributors](/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach/contributors.txt) 
 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
