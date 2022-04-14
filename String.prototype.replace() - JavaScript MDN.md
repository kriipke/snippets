# String.prototype.replace() - JavaScript | MDN
# String.prototype.replace()

The **`replace()`** method returns a new string with some or all matches of a `pattern` replaced by a `replacement`. The `pattern` can be a string or a [`RegExp`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp), and the `replacement` can be a string or a function called for each match. If `pattern` is a string, only the first occurrence will be replaced.

The original string is left unchanged.

## [Try it](#try_it "Permalink to Try it")

## [Syntax](#syntax "Permalink to Syntax")

    replace(regexp, newSubstr)
    replace(regexp, replacerFunction)

    replace(substr, newSubstr)
    replace(substr, replacerFunction) 

Copy to Clipboard

### [Parameters](#parameters "Permalink to Parameters")

`regexp` (pattern)

A [`RegExp`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) object or literal. The match or matches are replaced with `newSubstr` or the value returned by the specified `replacerFunction`.

`substr`

A string that is to be replaced by `newSubstr`. It is treated as a literal string and is _not_ interpreted as a regular expression. Only the first occurrence will be replaced.

`newSubstr` (replacement)

The string that replaces the substring specified by the specified `regexp` or `substr` parameter. A number of special replacement patterns are supported; see the "[Specifying a string as a parameter](#specifying_a_string_as_a_parameter)" section below.

If `newSubstr` is an empty string, then the substring given by `substr`, or the matches for `regexp`, are removed (rather then being replaced).

`replacerFunction` (replacement)

A function to be invoked to create the new substring to be used to replace the matches to the given `regexp` or `substr`. The arguments supplied to this function are described in the "[Specifying a function as a parameter](#specifying_a_function_as_a_parameter)" section below.

### [Return value](#return_value "Permalink to Return value")

A new string, with some or all matches of a pattern replaced by a replacement.

## [Description](#description "Permalink to Description")

This method does not change the calling [`String`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) object. It returns a new string.

To perform a global search and replace, include the `g` switch in the regular expression.

### [Specifying a string as a parameter](#specifying_a_string_as_a_parameter "Permalink to Specifying a string as a parameter")

The replacement string can include the following special replacement patterns:

| Pattern   | Inserts                                                                                                                                                                                                                                                                                                                                             |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$$`      | Inserts a `"$"`.                                                                                                                                                                                                                                                                                                                                    |
| `$&`      | Inserts the matched substring.                                                                                                                                                                                                                                                                                                                      |
| ``$` ``   | Inserts the portion of the string that precedes the matched substring.                                                                                                                                                                                                                                                                              |
| `$'`      | Inserts the portion of the string that follows the matched substring.                                                                                                                                                                                                                                                                               |
| `$n`      | Where `n` is a positive integer less than 100, inserts the `n`th parenthesized submatch string, provided the first argument was a [`RegExp`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) object. Note that this is `1`-indexed. If a group `n` is not present (e.g., if group is 3), it will be replaced as a literal (e.g., `$3`). |
| `$<Name>` | Where `Name` is a capturing group name. If the group is not in the match, or not in the regular expression, or if a string was passed as the first argument to `replace` instead of a regular expression, this resolves to a literal (e.g., `$<Name>`). Only available in browser versions supporting named capturing groups.                       |

### [Specifying a function as a parameter](#specifying_a_function_as_a_parameter "Permalink to Specifying a function as a parameter")

You can specify a function as the second parameter. In this case, the function will be invoked after the match has been performed. The function's result (return value) will be used as the replacement string. (**Note:** The above-mentioned special replacement patterns do _not_ apply in this case.)

Note that the function will be invoked multiple times for each full match to be replaced if the regular expression in the first parameter is global.

The arguments to the function are as follows:

| Possible name | Supplied value                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `match`       | The matched substring. (Corresponds to `$&` above.)                                                                                                                                                                                                                                                                                                               |
| `p1, p2, ...` | The \_n_th string found by a parenthesized capture group (including named capturing groups), provided the first argument to `replace()` was a [`RegExp`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) object. (Corresponds to `$1`, `$2`, etc. above.) For example, if `/(\a+)(\b+)/`, was given, `p1` is the match for `\a+`, and `p2` for `\b+`. |
| `offset`      | The offset of the matched substring within the whole string being examined. (For example, if the whole string was `'abcd'`, and the matched substring was `'bc'`, then this argument will be `1`.)                                                                                                                                                                |
| `string`      | The whole string being examined.                                                                                                                                                                                                                                                                                                                                  |
| `groups`      | In browser versions supporting named capturing groups, will be an object whose keys are the used group names, and whose values are the matched portions (`undefined` if not matched).                                                                                                                                                                             |

(The exact number of arguments depends on whether the first argument is a [`RegExp`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp) object—and, if so, how many parenthesized submatches it specifies.)

The following example will set `newString` to `'abc - 12345 - #$*%'`:

    function replacer(match, p1, p2, p3, offset, string) {
      // p1 is nondigits, p2 digits, and p3 non-alphanumerics
      return [p1, p2, p3].join(' - ');
    }
    let newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
    console.log(newString);  // abc - 12345 - #$*% 

Copy to Clipboard

## [Examples](#examples "Permalink to Examples")

### [Defining the regular expression in replace()](#defining_the_regular_expression_in_replace "Permalink to Defining the regular expression in replace()")

In the following example, the regular expression is defined in `replace()` and includes the ignore case flag.

    let str = 'Twas the night before Xmas...';
    let newstr = str.replace(/xmas/i, 'Christmas');
    console.log(newstr);  // Twas the night before Christmas... 

Copy to Clipboard

This logs `'Twas the night before Christmas...'`.

**Note:** See [this guide](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) for more explanations about regular expressions.

### [Using global and ignore with replace()](#using_global_and_ignore_with_replace "Permalink to Using global and ignore with replace()")

Global replace can only be done with a regular expression. In the following example, the regular expression includes the [global and ignore case flags](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags) which permits `replace()` to replace each occurrence of `'apples'` in the string with `'oranges'`.

    let re = /apples/gi;
    let str = 'Apples are round, and apples are juicy.';
    let newstr = str.replace(re, 'oranges');
    console.log(newstr);  // oranges are round, and oranges are juicy. 

Copy to Clipboard

This logs `'oranges are round, and oranges are juicy'`.

### [Switching words in a string](#switching_words_in_a_string "Permalink to Switching words in a string")

The following script switches the words in the string. For the replacement text, the script uses [capturing groups](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges) and the `$1` and `$2` replacement patterns.

    let re = /(\w+)\s(\w+)/;
    let str = 'John Smith';
    let newstr = str.replace(re, '$2, $1');
    console.log(newstr);  // Smith, John 

Copy to Clipboard

This logs `'Smith, John'`.

### [Using an inline function that modifies the matched characters](#using_an_inline_function_that_modifies_the_matched_characters "Permalink to Using an inline function that modifies the matched characters")

In this example, all occurrences of capital letters in the string are converted to lower case, and a hyphen is inserted just before the match location. The important thing here is that additional operations are needed on the matched item before it is given back as a replacement.

The replacement function accepts the matched snippet as its parameter, and uses it to transform the case and concatenate the hyphen before returning.

    function styleHyphenFormat(propertyName) {
      function upperToHyphenLower(match, offset, string) {
        return (offset > 0 ? '-' : '') + match.toLowerCase();
      }
      return propertyName.replace(/[A-Z]/g, upperToHyphenLower);
    } 

Copy to Clipboard

Given `styleHyphenFormat('borderTop')`, this returns `'border-top'`.

Because we want to further transform the _result_ of the match before the final substitution is made, we must use a function. This forces the evaluation of the match prior to the [`toLowerCase()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) method. If we had tried to do this using the match without a function, the [`toLowerCase()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) would have no effect.

    let newString = propertyName.replace(/[A-Z]/g, '-' + '$&'.toLowerCase());  // won't work 

Copy to Clipboard

This is because `'$&'.toLowerCase()` would first be evaluated as a string literal (resulting in the same `'$&'`) before using the characters as a pattern.

### [Replacing a Fahrenheit degree with its Celsius equivalent](#replacing_a_fahrenheit_degree_with_its_celsius_equivalent "Permalink to Replacing a Fahrenheit degree with its Celsius equivalent")

The following example replaces a Fahrenheit degree with its equivalent Celsius degree. The Fahrenheit degree should be a number ending with `"F"`. The function returns the Celsius number ending with `"C"`. For example, if the input number is `"212F"`, the function returns `"100C"`. If the number is `"0F"`, the function returns `"-17.77777777777778C"`.

The regular expression `test` checks for any number that ends with `F`. The number of Fahrenheit degree is accessible to the function through its second parameter, `p1`. The function sets the Celsius number based on the Fahrenheit degree passed in a string to the `f2c()` function. `f2c()` then returns the Celsius number. This function approximates Perl's `s///e` flag.

    function f2c(x) {
      function convert(str, p1, offset, s) {
        return ((p1 - 32) * 5/9) + 'C';
      }
      let s = String(x);
      let test = /(-?\d+(?:\.\d*)?)F\b/g;
      return s.replace(test, convert);
    } 

Copy to Clipboard

## [Specifications](#specifications "Permalink to Specifications")

| Specification |
| ------------- |

\| [ECMAScript Language Specification  
# sec-string.prototype.replace](https://tc39.es/ecma262/multipage/text-processing.html#sec-string.prototype.replace) \|

## [Browser compatibility](#browser_compatibility "Permalink to Browser compatibility")

[Report problems with this compatibility data on GitHub](https://github.com/mdn/browser-compat-data/issues/new?body=%3C%21--+Tips%3A+where+applicable%2C+specify+browser+name%2C+browser+version%2C+and+mobile+operating+system+version+--%3E%0A%0A%23%23%23%23+What+information+was+incorrect%2C+unhelpful%2C+or+incomplete%3F%0A%0A%23%23%23%23+What+did+you+expect+to+see%3F%0A%0A%23%23%23%23+Did+you+test+this%3F+If+so%2C+how%3F%0A%0A%0A%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EMDN+page+report+details%3C%2Fsummary%3E%0A%0A*+Query%3A+%60javascript.builtins.String.replace%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FString%2Freplace%0A*+Report+started%3A+2022-04-14T06%3A38%3A48.396Z%0A%0A%3C%2Fdetails%3E&title=javascript.builtins.String.replace+-+%3CPUT+TITLE+HERE%3E "Report an issue with this compatibility data")

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

`replace`

 | Full supportChrome1

Toggle history | Full supportEdge12

Toggle history | Full supportFirefox1

Toggle history | Full supportInternet Explorer5.5

Toggle history | Full supportOpera4

Toggle history | Full supportSafari1

Toggle history | Full supportWebView Android1

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

Partial support

Partial support

The compatibility table on this page is generated from structured data. If you'd like to contribute to the data, please check out [https://github.com/mdn/browser-compat-data](https://github.com/mdn/browser-compat-data) and send us a pull request.

## [See also](#see_also "Permalink to See also")

-   [`String.prototype.replaceAll()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replaceAll)
-   [`String.prototype.match()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/match)
-   [`RegExp.prototype.exec()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec)
-   [`RegExp.prototype.test()`](/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test)

#### Found a problem with this page?

-   [Edit on **GitHub**](https://github.com/mdn/content/edit/main/files/en-us/web/javascript/reference/global_objects/string/replace/index.md "You're going to need to sign in to GitHub first (Opens in a new tab)")
-   [Source on **GitHub**](https://github.com/mdn/content/blob/main/files/en-us/web/javascript/reference/global_objects/string/replace/index.md?plain=1 "Folder: en-us/web/javascript/reference/global_objects/string/replace (Opens in a new tab)")
-   [Report a problem with this content on **GitHub**](https://github.com/mdn/content/issues/new?template=page-report.yml&mdn-url=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FString%2Freplace&metadata=%3C%21--+Do+not+make+changes+below+this+line+--%3E%0A%3Cdetails%3E%0A%3Csummary%3EPage+report+details%3C%2Fsummary%3E%0A%0A*+Folder%3A+%60en-us%2Fweb%2Fjavascript%2Freference%2Fglobal_objects%2Fstring%2Freplace%60%0A*+MDN+URL%3A+https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FString%2Freplace%0A*+GitHub+URL%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fblob%2Fmain%2Ffiles%2Fen-us%2Fweb%2Fjavascript%2Freference%2Fglobal_objects%2Fstring%2Freplace%2Findex.md%0A*+Last+commit%3A+https%3A%2F%2Fgithub.com%2Fmdn%2Fcontent%2Fcommit%2Fcd7d38b28811a0b99bec1d936ed1e36a3ed29461%0A*+Document+last+modified%3A+2022-04-12T05%3A55%3A36.000Z%0A%0A%3C%2Fdetails%3E "This will take you to GitHub to file a new issue")
-   Want to fix the problem yourself? See [our Contribution guide](https://github.com/mdn/content/blob/main/README.md).

**Last modified:** Apr 12, 2022, [by MDN contributors](/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace/contributors.txt) 
 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace) 
 [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
