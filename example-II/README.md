This piece is part of a sequence of Gists I put together exploring how to animate SVG paths (more specifically, an abstraction thereof, Leaflet polylines) in a reversable way that works within React.

---

An easy workaround for the issue we discovered in the previous Gist. Here we add a little bit of extra code that (1) silently removes the stroke properties when the path is done being drawn and (2) loudly aborts the animation and presents the finished product when you zoom in mid-animation.

The former of these is great because it's transparent to the user, however, the latter workaround is less than optimal. Can we do better?

A quick check seems to show that the answer is yes, but with significant difficulty and complexity: by detecting a `Leaflet` `zoomstart`, pausing the animation, extracting the current state of the node, extracting the percentage length complete at that point in time, removing the existing node, proceeding with the zoom, and creating a new node that has that percentage of the animation complete at the new resolution and is proceeding with the remainder.

This is overmuch. Leaflet already has the unfixable issue that lines that fall of the screen aren't there when you pan around until you release the pan, so just defaulting to a full draw is meh, alright.