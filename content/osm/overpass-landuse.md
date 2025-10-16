

```js

[out:json][timeout:50]; 
(wr({{geocodeBbox:Davis County, UT}})[landuse];) -> .land;
way(r.land) -> .members; 
(way.land; .members;) -> .all;
node(w.all)->.landpts;  
way({{geocodeBbox:Davis County, UT}})[highway][area!=yes] -> .roads;
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
