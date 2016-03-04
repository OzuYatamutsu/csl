Catsnake Language
=================

CSL is a simple DSL for grabbing information from the web, powered by jQuery. It
is written in the form of synchronous tasks that are not designed for
performance, but for readability and ease of use.

 

Example
-------

CSL is designed to be a functional, human-readable scripting language. Here's an
example where we search for and return the species name of the domestic cat from
English Wikipedia:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
goto https://en.wikipedia.org
set jquery[#searchInput] to Cat
click jquery[#searchButton]
set result to jquery[.species].text()
return result
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

Language constructs
-------------------

### Rules!

-   Statements are executed line-by-line, with each statement on its own line.

-   Comments are delimited by the `#` operator. All values in CSL are strings.

-   All strings are unquoted and generally appear at the end of a line. The
    start of a string begins with the `to` keyword.

### `jquery[...]` 

The `jquery[...]` delimiter specifies a jQuery selector to be run on the page.
The contents of the delimited query string are unquoted. In addition, jQuery
methods on the object can be executed as normal to the left of the operator.

 

If no method or member is provided to the right of the delimiter, it is assumed
the calling method will be modifying `val()`, with the exception of `click`
(which calls `jQuery.click()`).

**Example**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Equivalent to: var val1 = jQuery('.my-class-val')[0].text();
set val1 to jQuery[.my-class-val][0].text()

# Equivalent to: jQuery('#my-text-field').val("my value");
set jquery[#my-text-field] to my value

# Equivalent to: var val2 = jQuery('#my-text-field').val();
set val2 to jquery[#my-text-field]

# Equivalent to: jQuery('.my-button-class style2').click();
click jquery[.my-button-class style2]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### Timescales

When a timescale needs to be specified, the following timescales are supported.
Timescales from left to right on the same row are all equivalent and can be used
interchangeably.

| **Long name**  | **Long pluralized name**  | **Short name**  |
|----------------|---------------------------|-----------------|
| `millisecond`  | `milliseconds`            | `ms`            |
| `second`       | `seconds`                 | `s`             |
| `minute`       | `minutes`                 | `m`             |

 

Methods
-------

### `!timeout` 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
!timeout <#> <timescale>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sets the maximum timeout between executing each statement before the entire task
is aborted. Must appear as the first line in a task. Does not apply to the
`wait` statement if a timescale is specified as an argument to `wait`.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# If any statement takes over 30 seconds to run without returning 
# (including the implicit $(document).ready()), aborts the entire task.
!timeout 30 seconds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### `goto`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
goto <url>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Navigates to a URL. By default, waits for `$(document).ready()` before moving on
to the next statement, unless `wait` is specified immediately afterwards.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Navigates to https://steakscorp.org.
goto https://steakscorp.org/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### `set`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
set <name | jquery[...]> to <val | jquery[...]>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sets a variable name or the value of a jQuery selector to a string value or the
value of another jQuery selector.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Sets a variable to the string value "cat".
set myVal to cat

# Sets a variable to the value of a jQuery selector.
set myVal2 to jquery[#id]

# Sets a jQuery selector to the string value "search term".
set jquery[#id2] to search term

# Sets a jQuery selector to the value of another jQuery selector.
set jquery[#id2] to jquery[#id1]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### `click`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
click jquery[...]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Clicks on a jQuery selector.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Calls .click() on a jQuery selector.
click jquery[.my-button-class]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### `wait`

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
wait <for jquery[...] | <#> <timescale>>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Waits until a jQuery selector returns a value, or for a given amount of time. If
a `wait` statement follows a `goto`, the `wait` statement will be executed
*instead* of waiting for `$(document).ready()` before continuing the task.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Waits for 100 milliseconds.
wait 100 milliseconds

# This statement is equivalent.
wait 100 ms

# Waits for 1 minute
wait 1 minute

# This statement is equivalent.
wait 1 m

# Waits for the given element to return a value.
wait for jquery[#my-ajaxed-area]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 

### `return` 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
return <variable_name | jquery[...]>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns the value of a previously-defined variable or the value of a jQuery
selector and ends the task.

**Example usage**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Returns the value "1".
set result to 1
return result

# Returns the value of the given selector.
return jquery[#selector]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
