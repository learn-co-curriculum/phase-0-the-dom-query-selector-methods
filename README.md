# JavaScript Query Selector Methods

## Learning Goals

- Use `querySelector()` and `querySelectorAll()` to find nested nodes
- Modify attributes of DOM nodes

## Introduction

One of the most essential skills in our web development toolbox is finding
elements in the DOM.

In the previous lesson, we learned about three of the methods we can use to
access DOM nodes: `getElementById()`, `getElementsByClassName()`, and
`getElementsByTagName()`. These methods are straightforward to use: you simply
pass the value of the `id` or `class` or `tag` name you're looking for (in
quotes) as the argument. The method returns either a single element
(`getElementById()`), or an `HTMLCollection` containing all the elements that
match what you specified (`getElementsByClassName()` and
`getElementsByTagName()`).

However, as we saw with one of the examples in the last lesson, if we're trying
to access a nested element that doesn't happen to have an `id` or `class` to
help us, we may need to string together several steps to get to the element we
want.

Recall from the CSS section earlier in this course that we can write very
specific, complex [_CSS selectors_][selectors] to ensure that a CSS rule is
applied to the exact element or elements we want. We can take advantage of CSS
selectors in our JavaScript code as well by using the `querySelector()` and
`querySelectorAll()` methods. Rather than taking a simple string as an argument,
these methods accept a string containing one or more CSS selectors as their
argument. This allows us to use document structure (tag, `id`, `class`) **along
with** the tree structure of the DOM to access the element(s) we need.

## Accessing Nested Nodes

To follow along in the console, fork and clone this lesson, open the files in
your text editor, and open `index.html` in Google Chrome. As you go, copy each
HTML example into `index.html`.

### `querySelector()`

The `querySelector()` method takes one argument, a string of one or more
CSS-compatible [selectors][], and returns the _first_ element that matches the
_full set of selectors_.

Given a document like:

```html
<body>
  <div>Hello!</div>
  <div>Goodbye!</div>
</body>
```

If we called `document.querySelector('div')`, the method would return the first
`div`. If we check its `innerHTML`, we should see `Hello!`.

Selectors aren't limited to one tag name, though. Otherwise, why not just use
`document.getElementsByTagName('div')[0]`? We can get very specific.

```html
<body>
  <div>
    <ul class="ranked-list">
      <li>1</li>
      <li>
        <div>
          <ul>
            <li>2</li>
          </ul>
        </div>
      </li>
      <li>3</li>
    </ul>
  </div>

  <div>
    <ul class="unranked-list">
      <li>6</li>
      <li>2</li>
      <li>
        <div>4</div>
      </li>
    </ul>
  </div>
</body>
```

```js
const li2 = document.querySelector("ul.ranked-list li ul li");
li2;
//=> <li>2</li>
```

Here's how to interpret the query above:

1. Start from `document`, the object we've called `querySelector()` on.
2. Find a `ul` with a `className` of `ranked-list`. (Recall from CSS that the
   `.` indicates that `ranked-list` is a `className`. Note also that the HTML
   property `class` is referred to as `className` in JavaScript. This is because
   `class` is a reserved word in JavaScript.)
3. Next, find an `li` that is a descendant of that `ul`.
4. Next, find a `ul` that is a descendant of that `li`.
5. Finally, find an `li` that is a descendant of that `ul`."

Note that, if we stopped with step 3, `querySelector()` would have returned the
_first_ `li` element, containing the `1`. However, that element doesn't have a
`ul` as a descendant so, when the method gets to the next selector in step 4, it
instead selects the _second_ `li`, which _does_ have a `ul` as a descendant.
Because that `ul` contains an `li`, it is the first (and, in this case, only)
element that matches the CSS selector string, so that element is returned.

Another way to phrase it is: "Starting from `document`, find a `ul` with a
`className` of `ranked-list`, then find an `li` descendant of that element that
has a `ul` descendant that in turn has an `li` descendant." The selector string
is interpreted in its entirety, and the first element that matches the whole
string is returned.

**Question**: What does the query below say? What element will it return?

```js
document.querySelector("ul.unranked-list li div");
```

<details><summary><b>Answer</b></summary>
<p>The query says, "Starting from <code>document</code>, find a <code>ul</code> with a <code>className</code> of <code>unranked-list</code>. Then find an <code>li</code> descendant of that <code>ul</code> that itself has a <code>div</code> descendant."</p>
<p>The query will return the <code>div</code> that contains '4'.</p>
</details>

#### CSS Selectors

If using CSS to target elements isn't feeling natural, now might be a good time
to brush up on [selectors][]. Play around on the MDN page, then come back when
you're ready.

### `querySelectorAll()`

`querySelectorAll()` works a lot like `querySelector()` — it accepts a string
containing one or more selectors as its argument, and it searches starting from
the object that it's called on (either `document` or an element). However,
instead of returning the first match, it returns a `NodeList` collection of all
matching elements. A `NodeList` is similar to an `HTMLCollection`: it is an
array-like structure containing, in this case, a list of DOM nodes. Like
`HTMLCollection`, you can iterate through it with a loop.

Given a document like:

```html
<body>
  <main id="app">
    <ul class="ranked-list">
      <li>1</li>
      <li>2</li>
    </ul>

    <ul class="ranked-list">
      <li>10</li>
      <li>11</li>
    </ul>
  </main>
</body>
```

If we called:

```js
document.getElementById("app").querySelectorAll("ul.ranked-list li");
```

We'd get back a list of nodes corresponding to:

```txt
<li>1</li>, <li>2</li>, <li>10</li>, <li>11</li>
```

**Challenge**: See if you can come up with a selector that would list just the
`li` elements in the _second_ `ul`.

<details><summary><b>Hint</b></summary>
<p>If you aren't sure how to do it, break it down into steps. For example, you could first get a list of the <code>ul</code>s, then get just the second one, then get the list of <code>li</code>s it contains. You do not need to do this with a single line of code!</p>
</details>

<details><summary><b>Answer</b></summary>
<p>Here's one way to do it:</p>
<pre>
const secondUl = document.getElementsByClassName("ranked-list")[1];
secondUl.querySelectorAll("li");
</pre>
<p>If you want to verify that you got the right nodes, you can do so by expanding the <code>NodeList</code> that your code returns and make sure the list includes two nodes. Next, expand one of the nodes and scroll down until you find the <code>innerText</code> property. If it's either <code>10</code> or <code>11</code>, you got the right nodes!<p>
</details>

## Conclusion

The DOM selection methods `document.querySelector()` and
`document.querySelectorAll()` are powerful tools for finding the elements we
need to update and change. They use the familiar CSS selector syntax and allow
us to create very specific queries that give us access to elements in complex
DOM trees.

## Resources

- [document.querySelector()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)
- [document.querySelectorAll()](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
- [CSS Selectors][selectors]
- [CSS Diner](https://flukeout.github.io/)

[selectors]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
