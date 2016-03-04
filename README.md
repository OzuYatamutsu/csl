# Catsnake Language
CSL is a simple DSL for grabbing information from the web, powered by jQuery.

## Example
CSL is designed to be a functional, human-readable scripting language. Here's an example where we search for and return the species name of the domestic cat from English Wikipedia:
```
goto https://en.wikipedia.org
set jquery['#searchInput'] to Cat
click jquery['#searchButton']
set result to jquery['.species'].text()
return result
```

## Methods
### `goto`
#### Syntax
```
goto <url>
```
Navigates to a URL. By default, waits for `$(document).ready()` before moving on to the next statement, unless `wait` is specified immediately afterwards.
### `set`
```
set <name | jquery[...]> to <val | jquery[...]>
```
Sets a variable name or the value of a jQuery selector to a string value or the value of another jQuery selector.
### `click`
```
click jquery[...]
```
Clicks on a 
### `wait`
### `return`