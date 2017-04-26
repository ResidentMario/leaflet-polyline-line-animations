This piece is part of a sequence of Gists I put together exploring how to animate SVG paths (more specifically, an abstraction thereof, Leaflet polylines) in a reversable way that works within React.

---

Mouse over the map and then scroll down (or up) to animate a polyline going up and down West Side Drive.

The lag turned out to just be my own fault, to get smooth animations you have to use a large transition duration that seems to take forever to start with default easing. We can implement a different easing to handle it, if need be. So, OK.