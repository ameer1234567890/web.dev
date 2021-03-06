---
page_type: guide
title: Control focus with tabindex
description: |
  Native HTML elements such as <button> or <input> have keyboard accessibility
  built-in for free. If you're building custom interactive components, use
  tabindex to ensure that they're keyboard accessible.
author: robdodson
web_lighthouse:
  - tabindex
  - focusable-controls
wf_blink_components: Blink>Accessibility
---

# Control focus with tabindex

Native HTML elements such as `<button>` or `<input>` have keyboard accessibility
built-in for free. If you're building _custom_ interactive components, use
`tabindex` to ensure that they're keyboard accessible.

<div class="aside note">
  Whenever possible, use a native HTML element rather than building your
  own custom version. &lt;button&gt;, for example, is very easy to style and
  already has full keyboard support. This will save you from needing to manage
  <code>tabindex</code> or add additional semantics with ARIA.
</div>

## Check if your controls are keyboard accessible
A tool like Lighthouse is great at detecting certain accessibility issues, but
some things can only be tested by a human.

Try pressing the `TAB` key to navigate through your site. Are you able to reach
all of the interactive controls on the page? If not, you may need to use
[`tabindex`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
to improve the focusability of those controls.

<div class="aside warning">
  If you don't see a focus indicator at all, it may be hidden by your
  CSS. Check for any styles that mention <code>:focus { outline: none; }</code>.
  You can learn how to fix this in our guide on <a href="#">styling focus</a>.
</div>

## Insert an element into the tab order
Insert an element into the natural tab order using `tabindex="0"`. For example:

```
<div tabindex="0">Focus me with the TAB key</div>
```

The element can be focused by pressing the `TAB` key and by calling its
`focus()` method.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe
    src="https://glitch.com/embed/#!/embed/tabindex-zero?path=index.html&previewSize=100&attributionHidden=true"
    alt="tabindex-zero on Glitch"
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

## Remove an element from the tab order
Remove an element using `tabindex="-1"`. For example:
```
<button tabindex="-1">Can't reach me with the TAB key!</button>
```

This removes an element from the natural tab order, but the element can still be
focused by calling its `focus()` method.

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe
    src="https://glitch.com/embed/#!/embed/tabindex-negative-one?path=index.html&previewSize=100&attributionHidden=true"
    alt="tabindex-negative-one on Glitch"
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

## Avoid `tabindex > 0`
Any `tabindex` greater than 0 jumps the element to the front of the natural tab
order. If there are multiple elements with a `tabindex` greater than 0, the tab
order starts from the lowest value greater than zero and works its way up.

Using a `tabindex` greater than 0 is considered an **anti-pattern** because
screen readers navigate the page in DOM order, not tab order. If you need an
element to come sooner in the tab order, it should be moved to an earlier spot
in the DOM.

Lighthouse makes it easy to identify elements with a `tabindex` > 0. Run the
Accessibility Audit (Lighthouse > Options > Accessibility) and look for the
results of the “No element has a [tabindex] value greater than 0” audit.

## Create accessible components with "roving `tabindex`"
If you're building a complex component, you may need to add additional keyboard
support beyond focus. Consider the native `select` element. It is focusable and
you can use the arrow keys to expose additional functionality (the selectable
options).

To implement similar functionality in your own components, use a technique known
as "roving `tabindex`". Roving tabindex works by setting `tabindex` to -1 for
all children except the currently-active one. The component then uses a keyboard
event listener to determine which key the user has pressed.

When this happens, the component sets the previously focused child's `tabindex`
to -1, sets the to-be-focused child's `tabindex` to 0, and calls the `focus()`
method on it.

**Before**
<pre class="prettyprint devsite-disable-click-to-copy">
&lt;div role=&quot;toolbar&quot;&gt;
  &lt;button tabindex=&quot;-1&quot;&gt;Undo&lt;/div&gt;
  <strong>&lt;button tabindex=&quot;0&quot;&gt;Redo&lt;/div&gt;</strong>
  <strong>&lt;button tabindex=&quot;-1&quot;&gt;Cut&lt;/div&gt;</strong>
&lt;/div&gt;
</pre>

**After**
<pre class="prettyprint devsite-disable-click-to-copy">
&lt;div role=&quot;toolbar&quot;&gt;
  &lt;button tabindex=&quot;-1&quot;&gt;Undo&lt;/div&gt;
  <strong>&lt;button tabindex=&quot;-1&quot;&gt;Redo&lt;/div&gt;</strong>
  <strong>&lt;button tabindex=&quot;0&quot;&gt;Cut&lt;/div&gt;</strong>
&lt;/div&gt;
</pre>

<div class="glitch-embed-wrap" style="height: 346px; width: 100%;">
  <iframe
    src="https://glitch.com/embed/#!/embed/roving-tabindex?path=index.html&previewSize=100&attributionHidden=true"
    alt="tabindex-negative-one on Glitch"
    style="height: 100%; width: 100%; border: 0;">
  </iframe>
</div>

<div class="aside note">
  Curious what those role="" attributes are for? They let you change the
  semantics of an element so it will be announced properly by a screen reader.
  You can learn more about them in our guide on
  <a href="#">screen reader basics</a>.
</div>

## Keyboard access recipes
If you're unsure what level of keyboard support your custom components might
need, you can refer to the [ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/). This handy guide lists
common UI patterns and identifies which keys your components should support.
