---
Title: Introduction to OpenStreetMap
Date: 2024-6-6
tags: osm
slug: osm-intro
Summary: My guide and notes for the Introduction to OSM workshop at State of the Map US 2024
---

<!-- "Would like to first get an overview of the data, the tools, the people, and the challenges of OSM." -->
<!-- "Here's iD, here's how to sign up" -->

## The Data: What's on the Map?

Anything on the ground! According to [taginfo](https://taginfo.geofabrik.de/north-america:us/keys/building), as of June 2024 in just the US there are 65 million buildings and 49 million highways, which are likely the most prominent things on the map. There's also 28 thousand playgrounds, 100 thousand benches, and almost 3 million individual trees. It's a map of the world, so a lot of things fit on the map. The real question is how detailed you want to get.

## Viewing the Map

At it's core, OSM is really a database, not a map. You may notice some things that already exist on the map, but aren't actually visible on the map available at [openstreetmap.org](https://openstreetmap.org). That's because depending on your use case, there's not a one-style-fits-all way to draw the map. The default style on osm.org is carto, and it has its own repository [on Github](https://github.com/gravitystorm/openstreetmap-carto). There's a 'Layers' button on the right side to switch to other styles, like ones that highlight cycling infrastructure, or that display public transport routes.

- Salt Lake City with the default style on openstreetmap.org
    ![Salt Lake City on openstreetmap.org in the default (carto) style. One of the highlights of this style is using different colors for roads of different priority]({static}/img/osm-carto-example.png)
- Or with the Cycle Map style
    ![Salt Lake City on openstreetmap.org in the Cycle Map style. Different kinds of cycle routes, such as separate paths, or paths next to car lanes, are highlight in different shades of blue.]({static}/img/osm-cycle-example.png)
- Or with the Transport Map style
    ![Salt Lake City on openstreetmap.org in the Transport Map style. Bus routes are drawn with red lines, and rails in dashed black, with everything else more faded than other styles.]({static}/img/osm-transport-example.png)

## Editing the Map

One of the core things of OSM is that anyone can contribute to the map, whether you want to trace some buildings, add a missing business, or add details about what's available at your local park. The only requirement is that you make a free account on [openstreetmap.org](openstreetmap.org)

Once you've made an account, the easiest way to edit the map is right on [openstreetmap.org](openstreetmap.org), with the iD editor that is built into the website. It has a great tutorial for new users that goes over everything you need to know about how data is represented, has tools for snapping things to 90 degrees or circles, and has presets for a lot of existing popular brands so that you don't even have to look up the right tags. The next section is a brief on how data is represented, so if you intend to follow the tutorial, don't worry about it.

![Screenshot of the iD editor built into openstreetmap.org, looking specifically at the intersection of State Street and 200 South in Salt Lake City, with State St selected]({static}/img/id-example.png)

### Data Representations on the Map

I once saw the different ways to understand how data is represented compared to the iceberg meme, which I found very appropriate. Depending on how involved and deep into it you get, there are different things to understand.

#### Level 1: The iD Tutorial

The previously mentioned tutorial for editting built in to [openstreetmap.org](openstreetmap.org) will introduce you to the 3 types of points, lines, and areas. Although, you are more likely to hear points referred to as nodes, and lines referred to as ways. These types map well onto nearly anything. Linear things, such as roads, paths, or fences, are drawn as lines. Buildings, land use, or bodies of water are drawn as areas. Very small things, such as street lamps or bus stop signs, or things where a single location is better, such as individual shops inside a single building, are drawn as points.

#### Level 2: JOSM Types

If you spend more time with OSM data, you will learn of one more type: relations. These are used for things where a single point, line, or area cannot be accurately represent it, such as a building with a hole, requiring a line for the outside and the inside. Alternatively, relations are used for things that require multiple types, such as a bus route that holds the ways the bus drives down, and the points for the stops. Finally, relations are also used for very large things for when using a single line would be a cumbersome amount of data, such as a state boundary.

It also will not take long to find that an area is just a way where the first and last node are the same.

#### Level 3: Lists

More just as an interesting point of consideration, you may realize that the only real data type is a list, since:

- A relation is a list of ways and nodes
- A way is a list of nodes
- A node is a list of coordinates

## The Wiki

Before moving on to other things, there is another website that is absolutely essential to bring up: [wiki.openstreetmap.org](wiki.openstreetmap.org). The wiki is a great point of reference for nearly anything OSM related. It's the best place to find the definition of a tag, or to find the right tag for something you haven't encountered before. (Make sure you pay attention to the difference between a topic page and a tag's page, like [Highways](https://wiki.openstreetmap.org/wiki/Highways) vs. the [highway=](https://wiki.openstreetmap.org/wiki/Key:highway)) There's also pages for different regions used to share projects in an area, like [Utah's page](https://wiki.openstreetmap.org/wiki/Utah) where we have links to the waiver for using our state's datasets and the subpage on our current strategy for handling our unusual address system.

The wiki is also home to many collections of other things, such as:
- Proposals for new standardized tags [link](https://wiki.openstreetmap.org/wiki/Category:Proposed_features)
- OSM-based services [link](https://wiki.openstreetmap.org/wiki/List_of_OSM-based_services)
- Android apps for viewing and editing the map [link](https://wiki.openstreetmap.org/wiki/Android)
- and more!

> Note that different translations on the wiki will have notes on differences on local uses for specific tags

## Editing the Map, Part 2

The iD editor is really great, but you can only do so much with it. Many other editors exist, depending on what you want to do:

**Desktop**:

- Rapid, a fork of iD maintained by Meta. A web editor which integrates adding data from open sources, such as Microsoft's AI Buildings, along with better warnings and validations.
- JOSM, the Java OSM editor. A desktop program oriented at power users with many helpful plugins and styles that can improve your editing

**Mobile**:

- StreetComplete (Android only), an app which breaks down missing information into easy to answer 'quests' that ask simple questions like 'How many lanes does this road have?' or 'What are the opening hours of this business?'

## Things that were covered in the workshop TODO

- OSMF and OSMUS
- osmcha.org
- rapid
- streetcomplete + everydoor
- osmand + organic map
- communication channels
- imports + exports

