# OPR STAC Extension

A [STAC](https://stacspec.org/) extension for [Open Polar Radar](https://github.com/englacial/xopr) metadata.

- **Title:** OPR Extension
- **Identifier:** `https://englacial.github.io/opr-stac-extension/v1.0.0/schema.json`
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions#extension-maturity):** Proposal

This extension adds radar-specific metadata and [morton geometry indices](https://github.com/englacial/mortie) to STAC Items and Collections for ice-penetrating radar data.

## Fields

| Field Name       | Type           | Item | Collection | Required   | Description |
|------------------|----------------|:----:|:----------:|------------|-------------|
| opr:provider     | string (enum)  | Y    | Y          | Yes        | Data provider: `awi`, `cresis`, `dtu`, `utig` |
| opr:mbox         | array[4 ints]  | Y    | N          | Yes (item) | Morton bounding box (4 prefix-cell characteristics) |
| opr:mpolygon     | array[12 ints] | N    | Y          | Yes (coll) | Morton polygon (12 prefix-cell characteristics) |
| opr:frame        | integer        | Y    | N          | No         | Frame number within the segment |
| opr:date         | string         | Y    | N          | No         | Acquisition date (YYYYMMDD) |
| opr:bandwidth    | integer        | Y    | N          | No         | Radar chirp bandwidth in Hz |
| opr:frequency    | integer        | Y    | N          | No         | Radar center frequency in Hz |
| opr:segment      | integer        | Y    | N          | No         | Segment number within the flight day |
| opr:hemisphere   | string (enum)  | Y    | Y          | No         | `north` or `south` |

## Morton Geometry Indices

`opr:mbox` and `opr:mpolygon` are computed using the [mortie](https://github.com/englacial/mortie) library. They encode spatial extent as HEALPix morton prefix-cells, enabling efficient spatial indexing and cross-catalog queries without requiring traditional geometry intersection.

- **`opr:mbox`** (item level): 4 prefix-cells forming a coarse bounding box of the flight line geometry.
- **`opr:mpolygon`** (collection level): 12 prefix-cells forming a tighter polygon around all items in the collection.

Each integer is the `characteristic` attribute of a `MortonChild` node from the mortie prefix trie.

## Examples

- [Item example](examples/item.json)
- [Collection example](examples/collection.json)

## Schema

- [JSON Schema](json-schema/schema.json)
