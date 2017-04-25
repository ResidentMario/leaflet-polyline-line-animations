This piece is part of a sequence of Gists I put together exploring how to animate SVG paths (more specifically, an abstraction thereof, Leaflet polylines) in a reversable way that works within React.

---

This simple solution using `d3` `onWheel` events is roughly the behavior we want, except:

* It needs to be debounced, because `onWheel` events vary in magnitude between platforms to an extreme degree.
* It totally explodes when you zoom in or out. The trick we were using before to deal with this won't work because we have to stay in an intermediate state for the flow to work. But, calculating a new version for these cases shouldn't be too hard.
* There is a huge amount of lag on the scroll event. There are a couple of culprits here: (1) there are a lot of CSS calculations here, again we need to debounce things and (2) scroll events apparently have a default wait period for `preventDefault` incorporated in. See [here](https://github.com/WICG/EventListenerOptions/blob/gh-pages/explainer.md) and [here](http://gregfranko.com/blog/javascript-performance-tips/).