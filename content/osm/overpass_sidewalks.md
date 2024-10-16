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


```js
---
style:
  extends: https://styles.trailsta.sh/protomaps-black.json
  layers:
    - type: line
      filter:
        - any
        - [ all, 
            [ in, 'highway', primary, secondary, tertiary, residential, unclassified ],
            ['!has', 'sidewalk'],
            ['!has', 'sidewalk:left'],
            ['!has', 'sidewalk:right'],
            ['!has', 'sidewalk:both']
          ]
      paint: &paint
        line-width: 3
        line-color: magenta
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk' ], separate ]
        - [ ==, [ get, 'sidewalk:left' ], separate ]
        - [ ==, [ get, 'sidewalk:right' ], separate ]
        - [ ==, [ get, 'sidewalk:both' ], separate ]
      paint: &paint
        line-width: 1
        line-color: "cyan"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk' ], yes ]
        - [ ==, [ get, 'sidewalk:left' ], yes ]
        - [ ==, [ get, 'sidewalk:right' ], yes ]
        - [ ==, [ get, 'sidewalk:both' ], yes ]
      paint:
        <<: *paint
        line-color: "orange"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk' ], no ]
        - [ ==, [ get, 'sidewalk:left' ], no ]
        - [ ==, [ get, 'sidewalk:right' ], no ]
        - [ ==, [ get, 'sidewalk:both' ], no ]
      paint:
        <<: *paint
        line-color: "red"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'cycleway' ], separate ]
        - [ ==, [ get, 'cycleway:left' ], separate ]
        - [ ==, [ get, 'cycleway:right' ], separate ]
        - [ ==, [ get, 'cycleway:both' ], separate ]
      paint: &paint
        line-width: 1
        line-color: "yellow"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'cycleway' ], yes ]
        - [ ==, [ get, 'cycleway:left' ], yes ]
        - [ ==, [ get, 'cycleway:right' ], yes ]
        - [ ==, [ get, 'cycleway:both' ], yes ]
      paint:
        <<: *paint
        line-color: "purple"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'cycleway' ], no ]
        - [ ==, [ get, 'cycleway:left' ], no ]
        - [ ==, [ get, 'cycleway:right' ], no ]
        - [ ==, [ get, 'cycleway:both' ], no ]
      paint:
        <<: *paint
        line-color: "blue"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'footway' ], sidewalk ]
      paint:
        line-width: 1
        line-color: "green"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'highway' ], cycleway ]
      paint:
        line-width: 1
        line-color: "cyan"
---
[bbox:{{bbox}}];
(
  way[~"sidewalk"~"separate|yes|no"];
  way[~"cycleway"~"separate|yes|no"];
  way["footway"="sidewalk"];
  way["highway"="cycleway"];
  way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"];
);
out geom;
```
