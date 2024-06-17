---
Title: Overpass Queries for Roads in SLCo
Date: 2023-11-27
Modified: 2024-03-28
Tags: osm, overpass-turbo
slug: overpass-utah-roads
Summary: A collection of overpass turbo queries for reviewing roads in Utah
---

```js

[out:json][timeout:25];
// [x] Salt Lake City
// [x] Magna Township
// [x] West Valley
// [x] South Salt Lake
// [x] Millcreek
// [ ] Holladay
//area[wikidata=Q482520]->.a;
// [ ] Murray
area[wikidata=Q482822]->.a;
// [x] Taylorsville
// [ ] Kearns
//area[wikidata=Q3306750]->.a;
// [ ] West Jordan
//area[wikidata=Q52466]->.a;
// [ ] Midvale
//area[wikidata=Q482518]->.a;
// [ ] Cottonwood Heights
//area[wikidata=Q482523]->.a;
// [ ] Sandy ( + White City, Granite)
// [ ] South Jordan
// [ ] Herriman
// [ ] Riverton
// [ ] Draper
// [ ] Bluffdale


//area[wikidata=Q52475]->.a;  //Taylorsville
//area[wikidata=Q482869]->.a; //SSL
//area[wikidata=Q255187]->.a; //Manga
//area[wikidata=Q52465]->.a; // WVC

//-------------------------
// Salt Lake County
//area[wikidata=Q484556]->.a;
// Garfield County, Utah
//area[wikidata=Q26740]->.a;
// Sanpete County
//area[wikidata=Q484577]->.a;
// Juab County
//area[wikidata=Q26689]->.a;

// Badly named roads
//(
//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"]["name:prefix"!~"North|South|East|West"](area.a);
//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential|service"]["tiger:cfcc"](area.a);
//way["highway"~"primary$|secondary$|tertiary$|unclassified|residential|service"]["tiger:reviewed"="no"](area.a);
//);

// Unreviewed roads
(
way[highway]["tiger:reviewed"="no"](area.a);
//way[highway]["tiger:cfcc"](area.a);
);

out geom;
```
