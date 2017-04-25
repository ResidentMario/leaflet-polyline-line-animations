This piece is part of a sequence of Gists I put together exploring how to animate SVG paths (more specifically, an abstraction thereof, Leaflet polylines) in a reversable way that works within React.

---

This first Gist presents a bare minimum line animation. It animates a polyline up Manhattan's West Side Drive over a span of two seconds.

In this example we:
1. Create a `path` using the `Leaflet` API. 
2. Use `d3` to select the DOM node of the `path` and run `getTotalLength` on it (note: this method is apparently deprecated) to get the length in pixels.
3. Use that length to attach, again via `d3`, a JavaScript-based transition on the path properties that "draws" it. This uses a cool, relatively well-known hack, more details on which can be found elsewhere on the web: [here](https://product.voxmedia.com/2013/11/25/5426880/polygon-feature-design-svg-animations-for-fun-and-profit) or [here](https://css-tricks.com/controlling-css-animations-transitions-javascript/) for example.

Some thoughts:
* This technique requires that we know the exact pixel length of the polyline. Obviously will depend on the line itself on a case-by-case basis, so we can't have it generated using just a classname and a static stylesheet, for example.
* It's also possible to get the pixel length without accessing the DOM directly:

```javascript
const west_side_drive_length_in_meters = L.GeometryUtil.length(polyline);
// zoom math from OpenStreetMap, cf. http://stackoverflow.com/a/31266377/1993206
const metresPerPixel = 40075016.686 * Math.abs(Math.cos(map.getCenter().lat * 180/Math.PI)) / Math.pow(2, map.getZoom()+8);
const west_side_drive_length_in_pixels = west_side_drive_length_in_meters / metresPerPixel;
```

* But this approach is conceptually simpler, fewer lines of code, and faster.
* When used within React, once we have the length information we need we can let React handle the required DOM manipulation, instead of D3.

But, try zooming in and panning around a bit...you'll notice that the line behavior is extremely janky! The usual techniques don't take into account being placed on a sticky map.

We clearly need to come up with some way of handling this problem.