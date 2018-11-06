# JavaScript Node Search Power Tools

## Learning Goals

1. Use `document.querySelector()` and `document.querySelectorAll()` to find nested nodes
2. Change value of targeted DOM nodes

## Introduction

One of the most essential skills in our web development toolbox is finding
elements in the DOM.

While `document.getElementById()` and `document.getElementsByClassName()` are
good, we can search even better when we use document structure (tag, `id`, `class`) **and** the tree structure of the DOM.

To practice finding elements in the DOM, we're going to make use of two methods that are useful for navigating the DOM: `document.querySelector()` and
`document.querySelectorAll()`.

## Use `document.querySelector()` and `document.querySelectorAll()` to find nested nodes

- `querySelector()` takes one argument, a string of [selectors][], and returns
the first element that matches these selectors. 

- Code example for `document.querySelector('div')`, the method would return the first `div` (whose content is "Hello!").

- More specifically explained code example for 
// get <li>2</li>
const li2 = document.querySelector('ul.ranked-list li ul li')

// get <div>4</div>
const div4 = document.querySelector('ul.unranked-list li div')



### `querySelectorAll()`

- similar to `querySelector()` - accepts an argument, searches from the element that it's called on, but returns a `NodeList` of all matching elements
- code example


## Conclusion

The DOM selection methods `document.querySelector()` and
`document.querySelectorAll()` are powerful tools for finding elements we need
to update and change. 


## Resources

- [document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)

[selectors]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
