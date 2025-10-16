---
Title: Overpass Queries for Points of Interest
Date: 2025-10-15
Tags: osm, overpass-turbo
slug: overpass-poi
Summary: A query for overpass for finding where POIs are out of date
---

Searches for all POIs on screen, then color them by how recently they were check (from purple as most recent to red as years old). A pink outline is added around the point if the date is an assumption based on when the POI was last edited and a `check_date=*` tag is not present. (*This query is not very robust. POI tags and year to color matching is hard coded*)



Overpass Turbo link: [click me](https://overpass-ultra.us/#q=LQhQGcBcE8BsFMBcoAEL4A9LwHYBNxEUALSSAB0IHoqo55wA6SAJwEMBLWKNx8YquRYB7SMIC2bSsDxsWAa0YArcMJyoUsNtHgtCGtMBQxySFAGMOLcwgNoUAMy7YWRANoAiALxePAGhRPAHN4CXhWaGATeA8AXQCPAAVhDhxIOLs0ck405Ht8iysbeGB2PA4AV0IUADZM+0trBGBzYVhhV3r8o3M2cHgu+yMjYj7B7otieHN5AH1ZbHGUYYs+gYKN5cDUgIAmAAZdgFYAtxDIAPMpmfm2bFjYpcMUcgqWcltNgqM3HZQD3YAFlO50u1zmC3gDyeWwARrAKusvkNtjg9ocAMwg8Jg6YQu5Qx7IlFBFjwXAwn5-AG7bEXSZ4273InErYdNg4EKUlBkvBLFa9frc35o-6HE6BUEoDwAAUgHHEDEgbHE5DiLOJRle70+rKpooBwMlOOlcoVSpVauhrLhCKRmtR6N2WON9Nl8sVPFV6u5pPJ6ht+qdtNdCTNnuV3utepQ7M59uRRl5g0axWAUBE8hKAHcOHhIMQiLsU0VmhnhFmWm0OnlE6shQ6RmNAwybpD+dKWEFYWwABT7AIDlBD-YASg8HfIqXkoBAwFAbmEFUgiBUalibg9oWXiAOsQA3KBexocNmWG4VbgODAAH4eAB6vbJPDeHMgAB9eg54O+eyx368sLvg4fSQLMDjCMIeC-hy8jvnc4jAYisAAaMLCSOY0DvsQwjgFOyooXguDylA74AG7hLoqRyFh5C4WBwgOE45g-nRsAcCxwFWPAsw8PKaijgAJHEvYAN6ibCsLCBgAC+MmjoeaCnue-DCOQd6PuAFSmOhchZh+rQ4BROAcLgnE9lmLBYbCy5XLon7tAWDDvqpznwAg5isGoHHgL+kHyKp5DAe0LAcKRoxWHgz79P+sLwGwy5YWp8qWBy75KPA2buVZ77iMIsJcDx5A4TgP4FZhxSfnIVUsLMZLZFYQkieJknSXJCknmem5LqF4DiBpvY4dgKFBIiUCzDhVQ-jhUDublU0VIhdysGwnkcGo76pBBun8Tg5GmdmdGpJATWxGJElSbJ8mKSgyluAgYVvPAA1OJApXgOAswsWkZIAXpLl0SwkCfd9rA-kEbQOF9PX9J+qTwJI75OXcZKnedrVXR1SldYxzHPQ+vatKqHJYVo2Y6P+q2tBUaRvu+FrYLMbAhGkm04Fp7A4JxchXNe0wfhDFEsDgippGjLWXe1h4dUukAWMRugoIqyr7kAA)

```js
---
style:
  extends: https://styles.trailsta.sh/protomaps-dark.json
  layers:
    - type: circle
      filter: ["==", ["geometry-type"], "Point"]
      paint:
        circle-radius: 6
        circle-color:
          - case
          - - has
            - check_date
          - - case
            - [in, 2025, [get, check_date]]
            - purple
            - [in, 2024, [get, check_date]]
            - blue
            - [in, 2023, [get, check_date]]
            - green
            - [in, 2022, [get, check_date]]
            - orange
            - red
          - - case
            - [in, 2025, [get, "@timestamp"]]
            - purple
            - [in, 2024, [get, "@timestamp"]]
            - blue
            - [in, 2023, [get, "@timestamp"]]
            - green
            - [in, 2022, [get, "@timestamp"]]
            - orange
            - red
        circle-stroke-width: 2
        circle-stroke-color:
          - case
          - - has
            - check_date
          - "rgba(0, 0, 0, 0)"
          - pink
---
[out:json][timeout:20];
(
  nwr[amenity~"^(restaurant|cafe|bar|pub|fast_food|bank|atm|fuel|pharmacy|hospital|dentist|veterinary|post_office|police|fire_station)$"]({{bbox}});
  nwr[shop~"^(supermarket|convenience|bakery|butcher|clothes|shoes|electronics|bookshop|florist|hairdresser|beauty|optician|jewelry|mobile_phone|bicycle|car|car_repair)$"]({{bbox}});
  nwr[tourism~"^(hotel|guest_house|hostel|museum|attraction|information|viewpoint)$"]({{bbox}});
  nwr[leisure~"^(fitness_centre|park|sports_centre|golf_course|cinema|theatre)$"]({{bbox}});
  nwr[office~"^(company|lawyer|accountant|estate_agent|insurance|architect|government)$"]({{bbox}});
);
out center meta;
```
