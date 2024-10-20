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
        - [ ==, [ get, 'sidewalk:left' ], separate ]
        - [ ==, [ get, 'sidewalk:right' ], separate ]
        - [ ==, [ get, 'sidewalk:both' ], separate ]
      paint: &paint
        line-width: 1
        line-color: "cyan"
    - type: line
      filter:
        - any
        - [ ==, [get, 'sidewalk' ], separate ]
      paint:
        <<: *paint
        line-width: 3
        line-dasharray: [2, 3]
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk:left' ], yes ]
        - [ ==, [ get, 'sidewalk:right' ], yes ]
        - [ ==, [ get, 'sidewalk:both' ], yes ]
      paint: &paint
        line-width: 1
        line-color: "orange"
    - type: line
      filter:
        - any
        - [ ==, [get, 'sidewalk' ], yes ]
        - [ ==, [get, 'sidewalk' ], both ]
      paint:
        <<: *paint
        line-width: 3
        line-dasharray: [2, 3]
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk' ], no ]
        - [ ==, [ get, 'sidewalk:left' ], no ]
        - [ ==, [ get, 'sidewalk:right' ], no ]
        - [ ==, [ get, 'sidewalk:both' ], no ]
      paint: &paint
        line-width: 1
        line-color: "red"
    - type: line
      filter:
        - any
        - [ ==, [ get, 'sidewalk' ], no ]
      paint:
        <<: *paint
        line-width: 3
        line-dasharray: [2, 3]
    - type: line
      filter:
        - any
        - [ ==, [ get, 'footway' ], sidewalk ]
      paint:
        line-width: 1
        line-color: "green"
---
[bbox:{{bbox}}];
(
  way[~"sidewalk"~"separate|yes|no"];
  way["footway"="sidewalk"];
  way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"];
);
out geom;

```
