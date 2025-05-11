---
Title: Overpass Queries for Roads in SLCo
Date: 2023-11-27
Modified: 2024-08-31
Tags: osm, overpass-turbo
slug: overpass-utah-roads
Summary: A collection of overpass turbo queries for reviewing roads in Utah
---

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
