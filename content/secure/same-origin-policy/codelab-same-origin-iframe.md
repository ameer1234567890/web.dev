---
page_type: glitch
title: Same Origin Policy & iframe
author: kosamari
description: |
  In this codelab, learn how the same-origin policy works when accessing data
  inside an iframe.
glitch: same-origin-policy-iframe
---
# Same-origin Policy & iframe

In this codelab, see how the same-origin policy works when accessing data inside an iframe.

## Set up: Page with a same-origin iframe
This page embeds an `iframe`, called `iframe.html`, in the same origin.  
Since the host and the iframe share the same origin, the host site is able to access data inside of the iframe and expose the secret message like blow.
```
const iframe = document.getElementById('iframe');
const message = iframe.contentDocument.getElementById('message').innerText;
```

## Change to cross-origin iframe
Try changing the `src` of the `iframe` to `https://other-iframe.glitch.me/`.
Can the host page still access the secret message? 

Since the host and embeded `iframe` do not have the same origin, access to the data is restricted. 
