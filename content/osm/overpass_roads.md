---
Title: Overpass Queries for Roads in SLCo
Date: 2023-11-27
Modified: 2024-08-31
Tags: osm, overpass-turbo
slug: overpass-utah-roads
Summary: A collection of overpass turbo queries for reviewing roads in Utah
---

### Improperly Named Roads

```js
[out:json][timeout:25];
// [ ] Salt Lake City 
// [ ] Magna Township    
// [ ] West Valley
// [ ] South Salt Lake
// [ ] Millcreek
// [ ] Holladay 
// [ ] Murray 
// [ ] Taylorsville 
// [ ] Kearns
// [ ] West Jordan
// [ ] Midvale
// [ ] Cottonwood Heights
// [ ] Sandy ( + White City, Granite)
// [ ] Herriman 
// [ ] South Jordan 
// [ ] Riverton
// [ ] Draper    
// [ ] Bluffdale  

// [ ] Copperton 
// [ ] Emigration Canyon  
// [ ] Cottonwood Canyons  
// [ ] GSL Marshes  
 
//-------------------------
// Salt Lake County
area[wikidata=Q484556]->.a; 
// Garfield County, Utah
//area[wikidata=Q26740]->.a;
// Sanpete County 
//area[wikidata=Q484577]->.a;
// Juab County
//area[wikidata=Q26689]->.a; 


// Badly named roads
(
way[highway=primary]["name:prefix"!~"North|South|East|West"](area.a);
way[highway=primary][!"name:full"](area.a);
way[highway=primary]["name_1"](area.a);

//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"]["name:prefix"!~"North|South|East|West"](area.a);
//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"][!"name:full"](area.a);

//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential|service"]["tiger:cfcc"](area.a);
); 
 

out geom;
```

### Very Detailed Roads

```js
---
style:
  extends: https://styles.trailsta.sh/protomaps-black.json
  layers:
    - type: line
      filter:
        - any
        - ['has', 'highway']
      paint:
        line-width: 1
        line-color: green
    - type: line
      filter:
        - any
        - [ all,
            ['!has', 'sidewalk'],
            ['!has', 'sidewalk:left'],
            ['!has', 'sidewalk:right'],
            ['!has', 'sidewalk:both'],
          ]
      paint: &paint
        line-width: 2
        line-color: magenta
    - type: line
      filter:
        - any
        - ['!has', 'lanes']
      paint:
        line-width: 2
        line-color:  "red"
    - type: line
      filter:
        - any
        - ['!has', 'surface']
      paint:
        line-width: 2
        line-color: "#f1c40f"
    - type: line
      filter:
        - any
        - ['!has', 'maxspeed']
      paint:
        line-width: 2
        line-color: "#ec7063"
---
[out:json][timeout:25];
way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"]({{bbox}});
out geom;
```

### Roads glued to Landuse

```js
[out:json][timeout:25]; 
(wr({{geocodeBbox:Bountiful, UT}})[landuse];) -> .land;
way(r.land) -> .members; 
(way.land; .members;) -> .all;
node(w.all)->.landpts;  
way({{geocodeBbox:Bountiful, UT}})[highway][area!=yes] -> .roads;
node(w.roads) -> .roadspts;  
node.landpts.roadspts -> .intersecting;  
node.landpts.intersecting -> .landptsintersecting;
node.roadspts.intersecting -> .roadsptsintersecting; 
way.land(bn.landptsintersecting) -> .landmatchw;
way.members(bn,landptsintersecting) -> .membersmatch; 
rel.land(br.membersmatch)->.landmatchr; 
way.roads(bn.roadsptsintersecting) -> .roadsmatch; 
foreach.landmatchw->.thisland
{
  node.landptsintersecting(w.thisland)->.thisintersectingpts;
  node.thisintersectingpts.roadsptsintersecting->.thismatchpts;
  if (thismatchpts.count(nodes)>1)
  { 
    way.roadsmatch(bn.thismatchpts)->.thismatchroads;
    foreach.thismatchroads->.thisthismatchroads
    {
      node.thismatchpts(w.thisthismatchroads);
      if (count(nodes)>1)
      {
        .thisthismatchroads out geom;
        .thisland out geom;
      };
    };
  };
};
foreach.landmatchr->.thisland
{
  way.members(r.thisland) -> .thismembers;
  node.landptsintersecting(w.thismembers) -> .thisintersectingpts;
  node.thisintersectingpts.roadsptsintersecting -> .thismatchpts;
  if (thismatchpts.count(nodes)>1)
  { 
    way.roadsmatch(bn.thismatchpts)->.thismatchroads;
    foreach.thismatchroads->.thisthismatchroads
    {
      node.thismatchpts(w.thisthismatchroads);
      if (count(nodes)>1)
      {
        .thisthismatchroads out geom;
        .thisland out geom;
      };
    };
  }; 
};
```

### Unreviewed Roads (finished)

```js
[out:json][timeout:25];

//-------------------------
// Salt Lake County
area[wikidata=Q484556]->.a;

// Unreviewed roads
(
way[highway]["tiger:reviewed"="no"](area.a);
);

out geom;
```
