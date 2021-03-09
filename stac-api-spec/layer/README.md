# STAC API Layer Extension

**Extension [Maturity Classification](https://github.com/radiantearth/stac-api-spec/blob/master/extensions/README.md#extension-maturity): Proposal**

This API extension defines the API resources and semantics for retrieving [STAC Layers](../../../stac-spec/extensions/layer/README.md)
information defined for available STAC Items.

[STAC Items](https://github.com/radiantearth/stac-spec/tree/master/item-spec) can only have a reference to a single [STAC Collection](https://github.com/radiantearth/stac-spec/tree/master/collection-spec). To group items into some other collection without this extension is only possible by creating full items copy (with new identifiers) that are linked to the same, new collection, which can be used as a new layer or group.

STAC Layer extension allows to overcome this limitation and allows items to have references to multiple named catalogs which can be used to group items by the same (layer) name. Items in such layers can belong to the same or to different collections.

## Methods

| Path                    | Content-Type Header                                                                                         | Description                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| `GET /layers/`          | [Catalog](https://github.com/radiantearth/stac-api-spec/blob/master/stac-spec/catalog-spec/catalog-spec.md) | Catalog with links to layers.                                                      |
| `GET /layers/{layerID}` | [`GeoJSON Feature`](../../stac-spec/layer/json-schema/layer-schema.json)                                                                                       | [STAC Layer](../../stac-spec/layer/README.md) as a GeoJSON Feature. |

**!IMPORTANT** 
To query items by `layer` identifiers it is required to use the [Query API Extension](https://github.com/radiantearth/stac-api-spec/tree/master/extensions/query). See [STAC Layer Extension description](../../stac-spec/layer/README.md#item-fields) to clarify how STAC Items property fields are defined to follow [STAC Layer Extension](../../stac-spec/layer/README.md).

## Schema

- [JSON Static Layer Schema](../../stac-spec/layer/json-schema/layer-schema.json)

## How It Works

When this extension is implemented, the API supports `layers` endpoint that list all layers available. An example of the result output can be found [here](../../stac-spec/layer/examples/landsat-stac-layers/layers/catalog.json).

The specific STAC Layer is available at `/layers/{layerID}`. The example output can be found [here](../../stac-spec/layer/examples/landsat-stac-layers/layers/us.json)

## Layer ID

Layer ID is a unique identifier for a layer.

## Example

Request to `GET /layers`:

```javascript
{
    "stac_version": "1.0.0-beta.2",
    "stac_extensions": [],
    "id": "layer-us-global",
    "title": "Landsat 8 L1",
    "description": "US STAC Layers, a STAC Catalog that represents a list of STAC Layers",
    "links": [
        {
            "href": "/",
            "rel": "root"
        },
        {
            "href": "/layers",
            "rel": "self"
        },
        {
            "href": "/layers/layer-pa",
            "rel": "item"
        },
        {
            "href": "/layers/layer-us",
            "rel": "item"
        }
    ]
}
```

Request to `GET /layers/layer-us`:

```javascript
{
  "type": "Feature",
  "id": "layer-us",
  "stac_extensions": [
    "layer"
  ],
  // ... 
  // spatio-temporal extent, which can contain the time extent temporal resolution
  "extent": {
    "spatial": {
      "bbox": [
        [
          -78.32015745740821,
          37.79447027981874,
          -73.92530589507936,
          41.41037947529183
        ]
      ]
    },
    "temporal": {
      "interval": [
        [
          "2018-05-01T00:00:00Z",
          "2018-08-01T00:00:00Z"
        ]
      ]
    }
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          -101.40824987104652,
          37.79718802132125
        ],
        [
          -101.40824987104652,
          41.41061537114088
        ],
        [
          -73.94863288954222,
          41.41061537114088
        ],
        [
          -73.94863288954222,
          37.79718802132125
        ],
        [
          -101.40824987104652,
          37.79718802132125
        ]
        ]
      ]
  },
  "links": [
    {
      "rel": "self",
      "href": "./self.json"
    },
    {
      "rel": "parent",
      "href": "catalog.json"
    },
    {
        "rel": "root",
        "href": "catalog.json"
    },
    {
        "href": "item-1.json",
        "rel": "item"
    },
    {
        "href": "item-2.json",
        "rel": "item"
    },
    {
        "href": "item-3.json",
        "rel": "item"
    },
    {
        "href": "item-4.json",
        "rel": "item"
    }
  ]
}
```

```javascript
{
  // ...
  // another spatio-temporal example that contains temporal resolution
  "extent": {
    "spatial": {
      "bbox": [
        [
          -78.32015745740821,
          37.79447027981874,
          -73.92530589507936,
          41.41037947529183
        ]
      ]
    },
    "temporal": {
      "interval": [
        {
          "range": [
            "2018-05-01T00:00:00Z",
            "2018-08-01T00:00:00Z"
          ],
          // ISO 8601:1988(E) compilant period: PnYnMnD
          "period": "P1M"
        }
      ]
    }
  }

}
```
