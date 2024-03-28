---
Title: Overpass Queries for Roads in SLCo
Date: 2023-11-27
Modified: 2024-03-28
Tags: osm, overpass-turbo
Summary: A collection of overpass turbo queries for reviewing roads in Utah
---

```js

[out:json][timeout:25];
// [ ] Salt Lake City
// [x] Magna Township
// [ ] West Valley
//area[wikidata=Q52465]->.a;
// [x] South Salt Lake
// [ ] Millcreek
area[wikidata=Q1829357]->.a;
// [ ] Holladay
// [ ] Murray
// [x] Taylorsville
// [ ] Kearns
// [ ] West Jordan
//area[wikidata=Q52466]->.a;
// [ ] Midvale
// [ ] Cottonwood Heights
// [ ] Sandy ( + White City, Granite)
// [ ] South Jordan
// [ ] Herriman
// [ ] Riverton
// [ ] Draper
// [ ] Bluffdale


//area[wikidata=Q52475]->.a;  //Taylorsville
//area[wikidata=Q482869]->.a; //SSL
//area[wikidata=Q255187]->.a; //Manga

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
(
way["highway"~"primary$|secondary$|tertiary$|unclassified|residential"]["name:prefix"!~"North|South|East|West"](area.a);
way["highway"~"primary$|secondary$|tertiary$|unclassified|residential|service"]["tiger:cfcc"](area.a);
way["highway"~"primary$|secondary$|tertiary$|unclassified|residential|service"]["tiger:reviewed"="no"](area.a);
);

// Unreviewed roads
//(
//way["tiger:reviewed"="no"](area.a);
//way["tiger:cfcc"](area.a);
//area[wikidata=Q484577]({{bbox}});
//);

out geom;

```