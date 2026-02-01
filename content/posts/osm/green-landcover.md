---
Title: Notes on green landcover tags in OpenStreetMap
Date: 2024-02-04
Tags:
- OpenStreetMap
slug: osm-green-landcover
Summary: The official OSM wiki feels overly verbose sometimes, so I wrote down my concise understanding of everything that counts as green landcover
---

**Natural (unmanaged)**:
- Wood (`natural=wood`): Unmanaged land covered by trees
- Scrub (`natural=scrub`): Land covered by uncultivated shrubs, bushes, or stunted trees of medium height. Note difference from `natural=shrub`, which is for single plants
- Heath (`natural=heath`): Characterized by open, low-growing woody vegetation, e.g. sagebrush or heather
- Meadow (`landuse=meadow`): Land primarily covered by grass and other non-woody plants, mainly used for hay or grazing.
    - See subtag `meadow=`
        - `=agricultural`: Hay
        - `=pasture`: Grazing
        - `=paddock`: animals are kept here
        - `=wildflower`: primarily a conservation habitat than production of grass.
- Grassland (`natural=grassland`): Land dominated by grass and other non-woody plants which is left to be wild
- Tundra

**Artificial (managed)**:
- Forest (`landuse=forest`): Managed land covered by trees
- Shrubbery (`natural=shrubbery`): Manicured/maintained shrubs
- Grass (`landuse=grass`): Mowed grass
- Farmland (`landuse=farmland`): crops. See additional tag:
    - `crop=`: grass, hay, alfalfa, etc.

