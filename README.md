# AppADay 136 — Detour

**Live:** https://augustineiacopelli.github.io/appaday-136-detour/

**Portfolio:** https://augustineiacopelli.github.io/appaday/

Enter two American cities and Detour drives the route, then tells you what is standing beside it.

## What it does

Detour geocodes both cities, pulls a real driving route, and walks the road computing cumulative mileage vertex by vertex. It samples that line every 25 miles and sweeps 8 kilometers to either side of every sample for anything OpenStreetMap has tagged as an attraction, artwork, viewpoint, museum, or memorial. Each candidate is snapped back to the road twice, once coarsely against the sample list and once exactly against the full resolution vertices, which yields two numbers that matter: the mile marker where you would leave the highway, and how far off it you would wander. Anything more than five miles off the road is dropped.

What survives goes to Claude, which scores each place for weirdness and writes it a one sentence pitch. The top fifteen are restored to route order and pinned on the map as numbered mile markers, with a matching list beneath. Tap either and a card gives you the name, the mile, the detour distance, the pitch, and a link straight into Maps.

Without an API key the app still works. The ranking falls back to a local heuristic that favors roadside artwork and attractions over museums and memorials, and the stops render without pitches.

## Using it

Type a start and an end, both in the United States, and press Find stops. Trips are capped at 1,200 miles so the sweep stays fast.

The gear button holds a Claude API key, stored only in your own browser via localStorage and never sent anywhere except Anthropic. Geocoding results are cached the same way so repeated cities cost nothing.

## Built with

One `index.html` file. Inline CSS and JavaScript, no build step, no framework. Leaflet from a CDN for the map, and nothing else.

Data comes from Nominatim for geocoding, the OSRM demo server for driving routes, and the Overpass API for places. Map tiles and place data are OpenStreetMap, licensed under ODbL. The single Overpass query is posted as a request body and carries a multi coordinate around filter, with an automatic retry against a mirror on rate limiting or gateway timeouts.

## Notes

Every network call has a timeout and a distinct failure message, and the app stays usable after any of them. It needs to run from a real http server or GitHub Pages rather than a sandboxed preview, because it reaches three third party origins.

---

*Part of AppADay. One complete app, shipped every day.*
