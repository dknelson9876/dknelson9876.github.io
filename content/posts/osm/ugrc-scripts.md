---
Title: UGRC to OSM Scripts
Date: 2025-07-25
tags:
- OpenStreetMap
slug: ugrc-osm-scripts
Summary: A list of scripts for converting data downloaded from UGRC for use with OSM
---

## road-name-transform.py

```py
import json
import sys

# Define a dictionary to map abbreviations to full words for street types and directions
abbr_lookup = {
    "ALY": "Alley",
    "AVE": "Avenue",
    "BAY": "Bay",
    "BLVD": "Boulevard",
    "CIR": "Circle",
    "COR": "Corner",
    "CRES": "Crescent",
    "CRK": "Creek",
    "CT": "Court",
    "CTR": "Center",
    "CV": "Cove",
    "CYN": "Canyon",
    "DR": "Drive",
    "EST": "Estate",
    "ESTS": "Estates",
    "EXPY": "Expressway",
    "FLT": "Flat",
    "FRK": "Fork",
    "FWY": "Freeway",
    "GLN": "Glen",
    "GRV": "Grove",
    "GTWY": "Gateway",
    "HL": "Hill",
    "HOLW": "Hollow",
    "HTS": "Heights",
    "HWY": "Highway",
    "JCT": "Junction",
    "LN": "Lane",
    "LNDG": "Landing",
    "LOOP": "Loop",
    "MDW": "Meadow",
    "MDWS": "Meadows",
    "MNR": "Manor",
    "PARK": "Park",
    "PASS": "Pass",
    "PATH": "Path",
    "PKWY": "Parkway",
    "PL": "Place",
    "PLZ": "Plaza",
    "PT": "Point",
    "RD": "Road",
    "RDG": "Ridge",
    "RNCH": "Ranch",
    "ROW": "Row",
    "RTE": "Route",
    "RUN": "Run",
    "SPUR": "Spur",
    "SQ": "Square",
    "ST": "Street",
    "TER": "Terrace",
    "TRCE": "Trace",
    "TRL": "Trail",
    "VIS": "Vista",
    "VLG": "Village",
    "VW": "View",
    "WAY": "Way",
    "XING": "Crossing",
    "S": "South",
    "N": "North",
    "E": "East",
    "W": "West"
}

direction_mapping = {
    "N": "North",
    "S": "South",
    "E": "East",
    "W": "West"
}

cartocode_lookup = {
    "1": "interstate",
    "2": "us-highway-separated",
    "3": "us-highway-unseparated",
    "4": "ut-highway-separated",
    "5": "ut-highway-unseparated",
    "6": "ut-highway-institutional",
    "7": "ramp",
    "8": "major-local-paved",
    "9": "major-local-unpaved",
    "10": "other-local",
    "11": "other-local",
    "12": "other",
    "13": "non-road",
    "14": "driveway",
    "15": "proposed",
    "16": "4WD-high-clearance",
    "17": "service-access",
    "18": "general-access"
}

bike_lookup = {
    "1A": "At grade track, protected by parking",
    "1B": "track protected with barrier",
    "1C": "track separated by raised curb",
    "1D": "track bidirectional",
    "1E": "track center-running",
    "2A": "lane buffered",
    "2B": "lane",
    "2C": "lane bidirectional buffered",
    "3A": "shoulder bikeway",
    "3B": "shared painted",
    "3C": "shared signed",
    "1": "track unspecified",
    "2": "lane unspecified",
    "3": "other unspecified"
}

def unabbreviate_name(name):
    """Transform the street name with unabbreviated street type and direction."""
    parts = name.split()
    for i, part in enumerate(parts):
        if part in abbr_lookup:
            parts[i] = abbr_lookup[part]
    return ' '.join(parts).title()

def unabbreviate_direction(dir_abbrev):
    """Transform the direction abbreviation to the full word."""
    return direction_mapping.get(dir_abbrev, "")

def process_feature(feature):
    """Process a single GeoJSON feature to add the required attributes."""
    properties = feature["properties"]

    # Extract relevant fields from the original GeoJSON properties
    an_name = properties.get("AN_NAME", "")
    an_postdir = properties.get("AN_POSTDIR", "")
    fullname = properties.get("FULLNAME", "")
    predir = properties.get("PREDIR", "")
    max_speed = properties.get("SPEED_LMT", "")


    # Transform the FULLNAME and PREDIR
    name = unabbreviate_name(fullname)
    name_prefix = unabbreviate_direction(predir)
    name_full = f"{name_prefix} {name}".strip()

    # Update the properties with the new fields
    properties["name"] = name
    properties["name:prefix"] = name_prefix
    properties["name:full"] = name_full

    # If AN_NAME is populated, process alt_name attributes
    if an_name:
        alt_name = unabbreviate_name(f"{an_name} {an_postdir}")
        alt_name_prefix = name_prefix
        alt_name_full = f"{alt_name_prefix} {alt_name}".strip()

        properties["alt_name"] = alt_name
        properties["alt_name:prefix"] = alt_name_prefix
        properties["alt_name:full"] = alt_name_full

    properties["maxspeed"] = f"{max_speed} mph"

    cartocode = properties["CARTOCODE"]
    properties["CARTOCODE"] = cartocode_lookup.get(cartocode, cartocode)

    bikel = properties["BIKE_L"]
    properties["BIKE_L"] = bike_lookup.get(bikel, bikel)

    biker = properties["BIKE_R"]
    properties["BIKE_R"] = bike_lookup.get(biker, biker)

    oneway = properties["ONEWAY"]
    if oneway == "0":
        del properties["ONEWAY"]
    elif oneway == "1":
        properties["ONEWAY"] = "yes"
    elif oneway == "2":
        properties["ONEWAY"] = "-1"
    
    unused = ["ACCESSCODE",
              "BIKE_PLN_L", "BIKE_PLN_R",
              "BIKE_REGPR",
              "COUNTY_L", "COUNTY_R",
              "CREATED",
              "CREATOR",
              "CUSTOMTAGS",
              "DOT_AADT", "DOT_AADTYR",
              "DOT_CLASS", "DOT_FCLASS", "DOT_F_MILE",
              "DOT_HWYNAM",
              "DOT_OWN_L", "DOT_OWN_R",
              "DOT_RTNAME", "DOT_RTPART",
              "DOT_SRFTYP",
              "DOT_T_MILE",
              "EDITOR",
              "ER_CAD_ZONES",
              "ESN_L", "ESN_R",
              "FROMADDR_L", "FROMADDR_R",
              "GlobalID",
              "LOCAL_UID",
              "MSAGCOMM_L", "MSAGCOMM_R",
              "OBJECTID",
              "PARITY_L", "PARITY_R",
              "POSTCOMM_L", "POSTCOMM_R",
              "QUADRANT_L", "QUADRANT_R",
              "SOURCE",
              "SPEED_LMT",
              "STATE_L", "STATE_R",
              "TOADDR_L", "TOADDR_R",
              "UNINCCOM_L", "UNINCCOM_R",
              "UNIQUE_ID",
              "UPDATED",
              "UTAHRD_UID",
              "ZIPCODE_L", "ZIPCODE_R"]
    for prop in unused:
        del properties[prop]


    return feature

def process_geojson(input_path, output_path):
    """Load, process, and save the GeoJSON file."""
    # Load GeoJSON file
    with open(input_path, 'r') as f:
        geojson_data = json.load(f)

    # Process each feature in the GeoJSON
    for feature in geojson_data["features"]:
        feature = process_feature(feature)

    # Save the modified GeoJSON
    with open(output_path, 'w') as f:
        json.dump(geojson_data, f, indent=2)

# Specify input and output file paths

if len(sys.argv) != 3:
    print("Usage: py road-name-transform.py <input_file> <output_file>")
    sys.exit(1)

input_geojson_path = sys.argv[1]
output_geojson_path = sys.argv[2]

# Process the GeoJSON file
process_geojson(input_geojson_path, output_geojson_path)

print("GeoJSON file processed successfully.")

```

## addr-name-transform.py

```py
import json
import sys

# Define a dictionary to map abbreviations to full words for street types and directions
abbr_lookup = {
    "ALY": "Alley",
    "AVE": "Avenue",
    "BAY": "Bay",
    "BLVD": "Boulevard",
    "CIR": "Circle",
    "COR": "Corner",
    "CRES": "Crescent",
    "CRK": "Creek",
    "CT": "Court",
    "CTR": "Center",
    "CV": "Cove",
    "CYN": "Canyon",
    "DR": "Drive",
    "EST": "Estate",
    "ESTS": "Estates",
    "EXPY": "Expressway",
    "FLT": "Flat",
    "FRK": "Fork",
    "FWY": "Freeway",
    "GLN": "Glen",
    "GRV": "Grove",
    "GTWY": "Gateway",
    "HL": "Hill",
    "HOLW": "Hollow",
    "HTS": "Heights",
    "HWY": "Highway",
    "JCT": "Junction",
    "LN": "Lane",
    "LNDG": "Landing",
    "LOOP": "Loop",
    "MDW": "Meadow",
    "MDWS": "Meadows",
    "MNR": "Manor",
    "PARK": "Park",
    "PASS": "Pass",
    "PATH": "Path",
    "PKWY": "Parkway",
    "PL": "Place",
    "PLZ": "Plaza",
    "PT": "Point",
    "RD": "Road",
    "RDG": "Ridge",
    "RNCH": "Ranch",
    "ROW": "Row",
    "RTE": "Route",
    "RUN": "Run",
    "SPUR": "Spur",
    "SQ": "Square",
    "ST": "Street",
    "TER": "Terrace",
    "TRCE": "Trace",
    "TRL": "Trail",
    "VIS": "Vista",
    "VLG": "Village",
    "VW": "View",
    "WAY": "Way",
    "XING": "Crossing",
    "S": "South",
    "N": "North",
    "E": "East",
    "W": "West"
}

direction_mapping = {
    "N": "North",
    "S": "South",
    "E": "East",
    "W": "West"
}

def unabbreviate_direction(dir_abbrev):
    """Expand a one-letter cardinal direction abbreviation."""
    return direction_mapping.get(dir_abbrev, "")

def process_feature(feature):
    """
    Process a single GeoJSON feature for an address point:
      - Map "AddNum" to "addr:housenumber"
      - Create "addr:street" by concatenating:
          * Expanded "PrefixDir"
          * "StreetName" in title case
          * Expanded "StreetType" (or, if missing, expanded "SuffixDir")
      - Map "UnitID" to "addr:unit"
      - Remove any property key which contains "id" (case insensitive)
    """
    properties = feature["properties"]

    new_properties = {}

    # Convert "AddNum" to "addr:housenumber" (if available)
    housenum = properties.get("AddNum", "").strip()
    if housenum:
        new_properties["addr:housenumber"] = housenum

    # Prepare street address parts
    prefix = properties.get("PrefixDir", "").strip()
    street_name = properties.get("StreetName", "").strip()
    street_type = properties.get("StreetType", "").strip()
    suffix = properties.get("SuffixDir", "").strip()

    # Expand the directional prefix using direction_mapping
    if prefix:
        expanded_prefix = unabbreviate_direction(prefix)
    else:
        expanded_prefix = ""

    # Convert the street name from all caps to title case
    if street_name:
        street_name = street_name.title()
    else:
        street_name = ""

    # For street type, if provided, use the lookup; if not, try SuffixDir
    if street_type:
        expanded_type = abbr_lookup.get(street_type.upper(), street_type.title())
    elif suffix:
        expanded_type = unabbreviate_direction(suffix)
    else:
        expanded_type = ""

    # Build "addr:street" by concatenating available parts
    street_components = []
    if expanded_prefix:
        street_components.append(expanded_prefix)
    if street_name:
        street_components.append(street_name)
    if expanded_type:
        street_components.append(expanded_type)

    street_full = " ".join(street_components).strip()
    if street_full:
        new_properties["addr:street"] = street_full

    # Convert "UnitID" to "addr:unit" (if available)
    unit = properties.get("UnitID", "").strip()
    if unit:
        new_properties["addr:unit"] = unit

    city = properties.get("City", "").strip()
    if city:
        new_properties["addr:city"] = city

    state = properties.get("State", "").strip()
    if state:
        new_properties["addr:state"] = state

    # Copy over any other property that we want to keep, but remove
    # any keys that contain "id" (in any case) or keys already processed.
    ignore_keys = {"AddNum", "PrefixDir", "StreetName",
                   "StreetType", "SuffixDir", "UnitID"}
    for key, value in properties.items():
        if key in ignore_keys:
            continue
        if "id" in key.lower():
            continue
        new_properties[key] = value

    feature["properties"] = new_properties
    return feature

def process_geojson(input_path, output_path):
    """Load, process, and save the GeoJSON file."""
    with open(input_path, 'r') as f:
        geojson_data = json.load(f)

    for i, feature in enumerate(geojson_data.get("features", [])):
        geojson_data["features"][i] = process_feature(feature)

    with open(output_path, 'w') as f:
        json.dump(geojson_data, f, indent=2)

if len(sys.argv) != 3:
    print("Usage: py address-point-transform.py <input_file> <output_file>")
    sys.exit(1)

input_geojson_path = sys.argv[1]
output_geojson_path = sys.argv[2]

process_geojson(input_geojson_path, output_geojson_path)
print("GeoJSON file processed successfully.")
```
