---
Title: Overpass Queries for sidewalks
Date: 2024-08-24
Tags: osm, overpass-turbo
slug: overpass-sidewalks
Summary: A query for overpass for finding where sidewalks are complete
---

```js
[out:json][timeout:120];
(
    nwr[footway=sidewalk]({{bbox}});
    nwr[highway]["sidewalk"~"both|no|yes"]({{bbox}});
    nwr[highway]["sidewalk:right"~"yes|no"]({{bbox}});
    nwr[highway]["sidewalk:left"~"yes|no"]({{bbox}});
    nwr[highway]["sidewalk:both"~"yes|no"]({{bbox}});
);
out geom;
```
