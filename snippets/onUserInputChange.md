---
title: Handle user input change
tags: browser,event
expertise: advanced
firstSeen: 2017-12-29T17:41:53+02:00
lastUpdated: 2020-10-21T21:54:53+03:00
---

Runs the callback whenever the user input type changes (`mouse` or `touch`).

- Use two event listeners.
- Assume `mouse` input initially and bind a `'touchstart'` event listener to the document.
- On `'touchstart'`, add a `'mousemove'` event listener to listen for two consecutive `'mousemove'` events firing within 20ms, using `performance.now()`.
- Run the callback with the input type as an argument in either of these situations.

```js
const onUserInputChange = callback => {
  let type = 'mouse',
    lastTime = 0;
  const mousemoveHandler = () => {
    const now = performance.now();
    if (now - lastTime < 20)
      (type = 'mouse'),
        callback(type),
        document.removeEventListener('mousemove', mousemoveHandler);
    lastTime = now;
  };
  document.addEventListener('touchstart', () => {
    if (type === 'touch') return;
    (type = 'touch'),
      callback(type),
      document.addEventListener('mousemove', mousemoveHandler);
  });
};
```

```js
onUserInputChange(type => {
  console.log('The user is now using', type, 'as an input method.');
});
```
