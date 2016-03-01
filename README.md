# Catsnake Language
CSL is a simple DSL for grabbing information from the web. An implementation is currently in progress.

TODO run synchronous after document.ready

## Example
CSL is designed to be a functional, human-readable scripting language. Here's an example where we search for and return the top result for a search for "cat videos" on YouTube:
```
goto https://youtube.com
set jquery['#masthead-search-term'] to cat videos
click jquery['#search-btn']
click jquery['.yt-lockup-title'][0]
return url
```

## Methods
### `goto`
### `wait`
