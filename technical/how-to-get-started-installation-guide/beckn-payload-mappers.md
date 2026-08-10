# Beckn Payload Mappers

Beckn Payload Mapping Specification\
<br>

In Beckn, these fields should not go into the context. They belong in the intent section of the search request, typically under message.intent.fulfillment.stops\[0].location for location filters and message.intent.tags (or item descriptors depending on your domain schema) for custom search filters.&#x20;

Agrovet Provider API

1. Endpoint GET /service-points

| API Field       | Type            | Beckn Field                                                 | Notes                                                                                     |
| --------------- | --------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| county\_code    | string          | message.intent.fulfillment.end.location.area\_code          | County filter (KE-001 format). Validation regex ^KE-\[0-9]{3}$ should happen at API layer |
| crop            | CropCode        | message.intent.tags\[].descriptor.code = crop               | Matches items where item contains this crop in crop\_codes                                |
| category        | string          | message.intent.item.category.descriptor.code                | Item category: fertiliser, seed, pesticide, fungicide, herbicide, veterinary, equipment   |
| growth\_stage   | GrowthStageCode | message.intent.tags\[].descriptor.code = growth\_stage      | Matches item growth stages                                                                |
| item\_query     | string          | message.intent.item.descriptor.name                         | Case-insensitive item name search                                                         |
| in\_stock\_only | boolean         | message.intent.tags\[].descriptor.code = in\_stock\_only    | Default value true                                                                        |
| type            | string          | message.intent.provider.category\_id                        | Service point type: agrovet, aggregation\_centre, extension\_office, vet\_clinic          |
| lat             | number          | message.intent.fulfillment.end.location.gps                 | Must combine with longitude                                                               |
| lon             | number          | message.intent.fulfillment.end.location.gps                 | Format: "lat,lon"                                                                         |
| radius\_km      | number          | message.intent.fulfillment.end.location.circle.radius.value | Valid only with lat + lon                                                                 |
| limit           | integer         | message.intent.tags\[].descriptor.code = limit              | Pagination extension                                                                      |
| offset          | integer         | message.intent.tags\[].descriptor.code = offset             | Pagination extension                                                                      |

Endpoint  :   GET /items

| API Field                       | Type            | Exact Beckn Field                                                | Remarks                                                                                      |
| ------------------------------- | --------------- | ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| crop                            | CropCode        | message.intent.item.tags\[].descriptor.code = crop               | Matches if returned item has this crop in item.tags (crop\_codes)                            |
| category                        | string          | message.intent.item.category.descriptor.code                     | Item category filter                                                                         |
| growth\_stage                   | GrowthStageCode | message.intent.item.tags\[].descriptor.code = growth\_stage      | Matches item growth stage metadata                                                           |
| item\_query                     | string          | message.intent.item.descriptor.name                              | Case-insensitive substring search on item name                                               |
| item\_query (active ingredient) | string          | message.intent.item.tags\[].descriptor.code = active\_ingredient | Search extension for agricultural metadata                                                   |
| in\_stock\_only                 | boolean         | message.intent.item.tags\[].descriptor.code = in\_stock\_only    | Default true; filters available inventory                                                    |
| max\_price\_kes                 | number          | message.intent.item.price.maximum\_value (domain extension)      | Inclusive upper price filter; recommended as tag if schema does not support price constraint |
| limit                           | integer         | message.intent.tags\[].descriptor.code = limit                   | Page size. Default: 20. Maximum: 100. Domain extension                                       |
| offset                          | integer         | message.intent.tags\[].descriptor.code = offset                  | <p>Records to skip. Default: 0. Must be >= 0. Domain extension<br><br><br></p>               |

Example Payload:

\
\
&#x20; {

&#x20; "context": {

&#x20;   "domain": "agri:input-marketplace",

&#x20;   "action": "search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bap\_uri": "[https://bap.example.org](https://bap.example.org)",

&#x20;   "transaction\_id": "tx-123456789",

&#x20;   "message\_id": "msg-123456789",

&#x20;   "timestamp": "2026-08-03T12:00:00Z",

&#x20;   "ttl": "PT30S"

&#x20; },

&#x20; "message": {

&#x20;   "intent": {

&#x20;  "item": {

&#x20;    "descriptor": {

&#x20;      "name": "Roundup"

&#x20;    },

&#x20;    "category": {

&#x20;      "descriptor": {

&#x20;        "code": "HERBICIDE"

&#x20;      }

&#x20;    },

&#x20;    "tags": \[

&#x20;      {

&#x20;        "descriptor": {

&#x20;          "code": "crop"

&#x20;        },

&#x20;        "list": \[

&#x20;          {

&#x20;            "descriptor": {

&#x20;              "code": "value"

&#x20;            },

&#x20;            "value": "MAIZE"

&#x20;          }

&#x20;        ]

&#x20;      },

&#x20;      {

&#x20;        "descriptor": {

&#x20;          "code": "growth\_stage"

&#x20;        },

&#x20;        "list": \[

&#x20;          {

&#x20;            "descriptor": {

&#x20;              "code": "value"

&#x20;            },

&#x20;            "value": "VEGETATIVE"

&#x20;          }

&#x20;        ]

&#x20;      },

&#x20;      {

&#x20;        "descriptor": {

&#x20;          "code": "active\_ingredient"

&#x20;        },

&#x20;        "list": \[

&#x20;          {

&#x20;            "descriptor": {

&#x20;              "code": "value"

&#x20;            },

&#x20;            "value": "Glyphosate"

&#x20;          }

&#x20;        ]

&#x20;      },

&#x20;      {

&#x20;        "descriptor": {

&#x20;          "code": "in\_stock\_only"

&#x20;        },

&#x20;        "list": \[

&#x20;          {

&#x20;            "descriptor": {

&#x20;              "code": "value"

&#x20;            },

&#x20;            "value": "true"

&#x20;          }

&#x20;        ]

&#x20;      },

&#x20;      {

&#x20;        "descriptor": {

&#x20;          "code": "max\_price\_kes"

&#x20;        },

&#x20;        "list": \[

&#x20;          {

&#x20;            "descriptor": {

&#x20;              "code": "value"

&#x20;            },

&#x20;            "value": "2500"

&#x20;          }

&#x20;        ]

&#x20;      }

&#x20;    ]

&#x20;  },

&#x20;  "tags": \[

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "limit"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "20"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "offset"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "0"

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

Response Objects

ServicePoint

| API Field            | Type            | Exact Beckn Field                                                                          | Remarks                                                                           |             |             |             |
| -------------------- | --------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- | ----------- | ----------- | ----------- |
| id                   | string          | message.catalog.providers\[].id                                                            | Stable provider identifier (KE-SP-####)                                           |             |             |             |
| type                 | string          | message.catalog.providers\[].categories\[].descriptor.code                                 | Service point type (agrovet, aggregation\_centre, extension\_office, vet\_clinic) |             |             |             |
| name                 | string          | message.catalog.providers\[].descriptor.name                                               | Business/service point name                                                       |             |             |             |
| county\_code         | string          | message.catalog.providers\[].locations\[].address.area\_code                               | CountyCode (KE-001 format)                                                        |             |             |             |
| county\_name         | string          | message.catalog.providers\[].locations\[].address.state                                    | Denormalised county name                                                          |             |             |             |
| sub\_county          | string          | message.catalog.providers\[].locations\[].address.city                                     | Constituency mapping                                                              |             |             |             |
| ward                 | string          | message.catalog.providers\[].locations\[].address.locality                                 | Ward mapping                                                                      |             |             |             |
| geo.lat              | number          | message.catalog.providers\[].locations\[].gps                                              | GPS format "lat,long"                                                             |             |             |             |
| geo.lon              | number          | message.catalog.providers\[].locations\[].gps                                              | GPS format "lat,long"                                                             |             |             |             |
| phone                | string          | message.catalog.providers\[].contacts.phone                                                | Business contact (not masked)                                                     |             |             |             |
| opening\_hours       | string          | message.catalog.providers\[].tags\[].descriptor.code = opening\_hours                      | Domain metadata                                                                   |             |             |             |
| licence\_number      | string          | message.catalog.providers\[].tags\[].descriptor.code = licence\_number                     | Synthetic licence metadata                                                        |             |             |             |
| services             | string\[]       | message.catalog.providers\[].tags\[].descriptor.code = services                            | Service capabilities                                                              |             |             |             |
| delivery\_available  | boolean         | message.catalog.providers\[].fulfillments\[].tags\[].descriptor.code = delivery\_available | Delivery capability                                                               |             |             |             |
| delivery\_radius\_km | integer \| null | message.catalog.providers\[].fulfillments\[].stops\[].location.circle.radius.value         | Delivery radius. Present only when delivery\_available=true                       |             |             |             |
| items                | object\[]       | message.catalog.providers\[].items\[] or message.catalog.items\[]                          | Stocked items associated with provider                                            |             |             |             |
| last\_verified\_on   | string          | message.catalog.providers\[].tags\[].descriptor.code = last\_verified\_on                  | YYYY-MM-DD verification metadata                                                  |             |             |             |
| distance\_km         | number \| null  | message.catalog.providers\[].tags\[].descriptor.code = distance\_km                        | Calculated Haversine distance from query location                                 |             |             |             |
| <p><br></p>          | <p><br></p>     | <p><br></p>                                                                                | <p><br></p>                                                                       |             |             |             |
| <p><br></p>          | <p><br></p>     | <p><br></p>                                                                                | <p><br></p>                                                                       | <p><br></p> | <p><br></p> | <p><br></p> |

Example Reponse:

\
{

&#x20; "context": {

&#x20;   "domain": "agri:input-marketplace",

&#x20;   "action": "on\_search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456789",

&#x20;   "message\_id": "msg-123456789",

&#x20;   "timestamp": "2026-08-03T12:00:01Z"

&#x20; },

&#x20; "message": {

&#x20;   "catalog": {

&#x20;  "descriptor": {

&#x20;    "name": "Agricultural Service Providers"

&#x20;  },

&#x20;  "providers": \[

&#x20;    {

&#x20;      "id": "KE-SP-0205",

&#x20;      "descriptor": {

&#x20;        "name": "Lakeview Agrovet Ltd"

&#x20;      },

&#x20;      "categories": \[

&#x20;        {

&#x20;          "id": "CAT-AGROVET",

&#x20;          "descriptor": {

&#x20;            "code": "agrovet",

&#x20;            "name": "Agrovet"

&#x20;          }

&#x20;        }

&#x20;      ],

&#x20;      "locations": \[

&#x20;        {

&#x20;          "id": "LOC-0205",

&#x20;          "gps": "-0.278,36.0362",

&#x20;          "address": {

&#x20;            "area\_code": "KE-032",

&#x20;            "state": "Nakuru",

&#x20;            "city": "Kuresoi North",

&#x20;            "locality": "Kamara"

&#x20;          }

&#x20;        }

&#x20;      ],

&#x20;      "contacts": {

&#x20;        "phone": "+254722213974"

&#x20;      },

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "opening\_hours"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "Mon-Sat 07:30-18:00"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "licence\_number"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "AGV/032/2024/8185"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "services"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "service"

&#x20;              },

&#x20;              "value": "training"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "service"

&#x20;              },

&#x20;              "value": "veterinary\_consultation"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "service"

&#x20;              },

&#x20;              "value": "spraying\_service"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "last\_verified\_on"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "2026-06-15"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "distance\_km"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": ""

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ],

&#x20;      "fulfillments": \[

&#x20;        {

&#x20;          "id": "FUL-DELIVERY",

&#x20;          "type": "Delivery",

&#x20;          "stops": \[

&#x20;            {

&#x20;              "type": "end",

&#x20;              "location": {

&#x20;                "circle": {

&#x20;                  "radius": {

&#x20;                    "unit": "km",

&#x20;                    "value": "10"

&#x20;                  }

&#x20;                }

&#x20;              }

&#x20;            }

&#x20;          ],

&#x20;          "tags": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "delivery\_available"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "true"

&#x20;                }

&#x20;              ]

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ],

&#x20;      "items": \[

&#x20;        {

&#x20;          "id": "KE-SP-0205-IT-02",

&#x20;          "descriptor": {

&#x20;            "name": "CAN Fertiliser 50kg"

&#x20;          },

&#x20;          "category\_ids": \[

&#x20;            "CAT-FERTILISER"

&#x20;          ],

&#x20;          "price": {

&#x20;            "currency": "KES",

&#x20;            "value": "2604.00"

&#x20;          },

&#x20;          "tags": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "crop\_codes"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "maize"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "beans"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "wheat"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "potato"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "sorghum"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "millet"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "growth\_stages"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "stage"

&#x20;                  },

&#x20;                  "value": "vegetative"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "pack\_size"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "50kg"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "in\_stock"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "true"

&#x20;                }

&#x20;              ]

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "id": "KE-SP-0205-IT-10",

&#x20;          "descriptor": {

&#x20;            "name": "Maize Fungicide 1L"

&#x20;          },

&#x20;          "category\_ids": \[

&#x20;            "CAT-FUNGICIDE"

&#x20;          ],

&#x20;          "price": {

&#x20;            "currency": "KES",

&#x20;            "value": "1413.00"

&#x20;          },

&#x20;          "tags": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "crop\_codes"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "maize"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "crop"

&#x20;                  },

&#x20;                  "value": "wheat"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "growth\_stages"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "stage"

&#x20;                  },

&#x20;                  "value": "vegetative"

&#x20;                },

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "stage"

&#x20;                  },

&#x20;                  "value": "flowering"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "pack\_size"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "1L"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "active\_ingredient"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "mancozeb"

&#x20;                }

&#x20;              ]

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "in\_stock"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "true"

&#x20;                }

&#x20;              ]

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

Item

| API Field          | Type               | Exact Beckn Field                                                                     | Remarks                                                                  |
| ------------------ | ------------------ | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| item\_id           | string             | message.catalog.items\[].id                                                           | Stable item identifier ({service\_point\_id}-IT-{NN})                    |
| name               | string             | message.catalog.items\[].descriptor.name                                              | Product name                                                             |
| category           | string             | message.catalog.items\[].category.descriptor.code                                     | fertiliser, seed, pesticide, fungicide, herbicide, veterinary, equipment |
| crop\_codes        | CropCode\[]        | message.catalog.items\[].tags\[].descriptor.code = crop                               | Agriculture-specific metadata                                            |
| growth\_stages     | GrowthStageCode\[] | message.catalog.items\[].tags\[].descriptor.code = growth\_stage                      | Agriculture-specific metadata                                            |
| unit               | InputUnitCode      | message.catalog.items\[].quantity.unitized.measure.unit                               | kg, g, l, ml, sachet, bag, bottle                                        |
| pack\_size         | string             | message.catalog.items\[].quantity.unitized.measure.value                              | Example: 50kg, 500ml                                                     |
| price\_kes         | number             | message.catalog.items\[].price.value                                                  | Product price                                                            |
| currency           | KES                | message.catalog.items\[].price.currency                                               | Set "KES"                                                                |
| in\_stock          | boolean            | message.catalog.items\[].quantity.available.count OR message.catalog.items\[].tags\[] | Recommended: quantity count                                              |
| active\_ingredient | string/null        | message.catalog.items\[].tags\[].descriptor.code = active\_ingredient                 | Null for seed/fertiliser/equipment                                       |

Example Reponse:

\
{

&#x20; "context": {

&#x20;   "domain": "agri:input-marketplace",

&#x20;   "action": "on\_search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456",

&#x20;   "message\_id": "msg-123456",

&#x20;   "timestamp": "2026-08-03T12:00:01Z"

&#x20; },

&#x20; "message": {

&#x20;   "catalog": {

&#x20;  "items": \[

&#x20;    {

&#x20;      "id": "KE-SP-0001-IT-01",

&#x20;      "descriptor": {

&#x20;        "name": "Sorghum Seed Gadam"

&#x20;      },

&#x20;      "category": {

&#x20;        "descriptor": {

&#x20;          "code": "seed"

&#x20;        }

&#x20;      },

&#x20;      "price": {

&#x20;        "currency": "KES",

&#x20;        "value": "345.0"

&#x20;      },

&#x20;      "quantity": {

&#x20;        "unitized": {

&#x20;          "measure": {

&#x20;            "unit": "kg",

&#x20;            "value": "1kg"

&#x20;          }

&#x20;        },

&#x20;        "available": {

&#x20;          "count": 1

&#x20;        }

&#x20;      },

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "crop"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "sorghum"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "growth\_stage"

&#x20;          },

&#x20;          "list": \[]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "active\_ingredient"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": null

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

\
\
\
Farmers Registry API

Endpoint  :   GET /farmers/{farmer\_id}

| API Field      | Type      | Exact Beckn Field                                                                                            | Remarks                                     |
| -------------- | --------- | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------- |
| farmer\_id     | string    | message.catalog.items\[].id                                                                                  | Farmer unique identifier (KE-FR-#####)      |
| farmer\_token  | string    | message.catalog.items\[].tags\[].descriptor.code = farmer\_token                                             | Opaque UUID passed downstream, not identity |
| name\_masked   | string    | message.catalog.items\[].descriptor.name                                                                     | Only masked name should be exposed          |
| phone\_masked  | string    | message.catalog.items\[].tags\[].descriptor.code = phone\_masked                                             | Beckn Item has no phone field               |
| county\_code   | string    | message.catalog.items\[].location\_ids\[] OR message.catalog.items\[].tags\[].descriptor.code = county\_code | Prefer location reference                   |
| county\_name   | string    | message.catalog.items\[].location\_ids\[] → location.address.state                                           | Denormalised county name                    |
| sub\_county    | string    | message.catalog.items\[].location\_ids\[] → location.address.city                                            | Constituency                                |
| ward           | string    | message.catalog.items\[].location\_ids\[] → location.address.locality                                        | Ward                                        |
| geo\_coarse    | object    | message.catalog.items\[].location\_ids\[] → location.gps                                                     | Use coarse location only                    |
| aez\_code      | string    | message.catalog.items\[].tags\[].descriptor.code = aez\_code                                                 | Domain metadata                             |
| farm\_size\_ha | number    | message.catalog.items\[].tags\[].descriptor.code = farm\_size\_ha                                            | Domain metadata                             |
| land\_holdings | object\[] | message.catalog.items\[].tags\[].descriptor.code = land\_holdings                                            | Domain extension                            |
| crops          | object\[] | message.catalog.items\[].tags\[].descriptor.code = crops                                                     | Domain extension                            |
| livestock      | object\[] | message.catalog.items\[].tags\[].descriptor.code = livestock                                                 | Domain extension                            |

| API Field       | Beckn Search Mapping                                               | Remarks                           |
| --------------- | ------------------------------------------------------------------ | --------------------------------- |
| county\_code    | message.intent.fulfillment.stops\[0].location.address.area\_code   | Administrative area/county code   |
| crop            | message.intent.item.descriptor.code                                | Crop identifier                   |
| category        | message.intent.item.category.descriptor.code                       | Fertiliser, Seed, Pesticide etc.  |
| growth\_stage   | message.intent.item.tags                                           | Custom tag (growth\_stage)        |
| item\_query     | message.intent.item.descriptor.name                                | Free text search                  |
| in\_stock\_only | message.intent.item.tags                                           | Custom tag (in\_stock\_only=true) |
| type            | message.intent.provider.categories.descriptor.code or Provider tag | Agrovet, Vet clinic etc.          |
| lat             | message.intent.fulfillment.stops\[0].location.gps                  | GPS coordinates                   |
| lon             | message.intent.fulfillment.stops\[0].location.gps                  | Combined with latitude            |
| radius\_km      | message.intent.fulfillment.tags                                    | Search radius                     |
| limit           | message.intent.tags                                                | Pagination metadata               |
| offset          | message.intent.tags                                                | Pagination metadata               |

Crop\
\
<br>

| API Field              | Type            | Exact Beckn Field                                                         | Remarks                         |
| ---------------------- | --------------- | ------------------------------------------------------------------------- | ------------------------------- |
| crop\_code             | CropCode        | message.catalog.items\[].tags\[].descriptor.code = crop\_code             | Controlled crop code            |
| variety                | string          | message.catalog.items\[].tags\[].descriptor.code = variety                | Local variety (e.g. H614)       |
| area\_ha               | number          | message.catalog.items\[].tags\[].descriptor.code = area\_ha               | Area under cultivation          |
| growth\_stage          | GrowthStageCode | message.catalog.items\[].tags\[].descriptor.code = growth\_stage          | Current crop stage              |
| planting\_month        | string          | message.catalog.items\[].tags\[].descriptor.code = planting\_month        | Format YYYY-MM                  |
| production\_purpose    | string \| null  | message.catalog.items\[].tags\[].descriptor.code = production\_purpose    | subsistence/commercial/mixed    |
| production\_system     | string \| null  | message.catalog.items\[].tags\[].descriptor.code = production\_system     | monocropping/intercropping      |
| uses\_certified\_seeds | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_certified\_seeds | Boolean metadata                |
| uses\_fertilizer       | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_fertilizer       | Boolean metadata                |
| uses\_pesticides       | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_pesticides       | Boolean metadata                |
| water\_source          | string \| null  | message.catalog.items\[].tags\[].descriptor.code = water\_source          | rain\_fed, borehole\_pump, etc. |
| harvest\_year          | integer \| null | message.catalog.items\[].tags\[].descriptor.code = harvest\_year          | Expected/actual harvest year    |

Livestock

| API Field           | Type            | Exact Beckn Field                                                      | Remarks                            |
| ------------------- | --------------- | ---------------------------------------------------------------------- | ---------------------------------- |
| type                | string          | message.catalog.items\[].tags\[].descriptor.code = livestock\_type     | e.g. dairy\_cattle, goats, poultry |
| male\_count         | integer         | message.catalog.items\[].tags\[].descriptor.code = male\_count         | Number of male livestock           |
| female\_count       | integer         | message.catalog.items\[].tags\[].descriptor.code = female\_count       | Number of female livestock         |
| total\_count        | integer         | message.catalog.items\[].tags\[].descriptor.code = total\_count        | Total livestock count              |
| production\_system  | string \| null  | message.catalog.items\[].tags\[].descriptor.code = production\_system  | zero\_grazing, free\_range, etc.   |
| uses\_ai            | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_ai            | Artificial insemination            |
| uses\_vaccination   | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_vaccination   | Vaccination status                 |
| uses\_deworming     | boolean         | message.catalog.items\[].tags\[].descriptor.code = uses\_deworming     | Deworming status                   |
| production\_purpose | string \| null  | message.catalog.items\[].tags\[].descriptor.code = production\_purpose | dairy, meat, eggs                  |
| hive\_count         | integer \| null | message.catalog.items\[].tags\[].descriptor.code = hive\_count         | Beekeeping only                    |
| hive\_type          | string \| null  | message.catalog.items\[].tags\[].descriptor.code = hive\_type          | Beekeeping only                    |
| active\_units       | integer \| null | message.catalog.items\[].tags\[].descriptor.code = active\_units       | Aquaculture ponds/cages            |

Example Reponse:

\
{

&#x20; "context": {

&#x20;   "domain": "agri:farmer-registry",

&#x20;   "action": "on\_search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456",

&#x20;   "message\_id": "ee55d89e-e37f-4bad-b298-ced7b770d1ef",

&#x20;   "timestamp": "2026-07-27T20:56:09.289200+00:00"

&#x20; },

&#x20; "message": {

&#x20;   "catalog": {

&#x20;  "locations": \[

&#x20;    {

&#x20;      "id": "KE-014",

&#x20;      "gps": "-0.5565,37.414",

&#x20;      "address": {

&#x20;        "state": "Embu",

&#x20;        "city": "Manyatta",

&#x20;        "locality": "Mbeti North"

&#x20;      }

&#x20;    }

&#x20;  ],

&#x20;  "items": \[

&#x20;    {

&#x20;      "id": "KE-FR-00001",

&#x20;      "descriptor": {

&#x20;        "name": "J\*\*\* A\*\*\*\*\*\*\*"

&#x20;      },

&#x20;      "location\_ids": \[

&#x20;        "KE-014"

&#x20;      ],

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "farmer\_token"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "3b982ef8-daf6-4a26-946d-3f31fc377a4c"

&#x20;           }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "phone\_masked"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "+2547\*\*\*\*\*810"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "aez\_code"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;             "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "UM2"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "farm\_size\_ha"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": 0.5

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "land\_holdings"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": {

&#x20;                "tenure\_type": "OWNED\_NO\_TITLE",

&#x20;                "land\_size\_hectares": 0.5,

&#x20;                "land\_for\_crops": 0.5,

&#x20;                "land\_for\_livestock": 0.0,

&#x20;                "land\_for\_other": 0.0,

&#x20;                "land\_idle": 0.0,

&#x20;               "documentation\_status": "untitled",

&#x20;                "parcel\_number": null

&#x20;              }

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "crops"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "crop\_code"

&#x20;              },

&#x20;              "value": "maize"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "variety"

&#x20;              },

&#x20;              "value": "local"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "area\_ha"

&#x20;              },

&#x20;              "value": 0.3

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "growth\_stage"

&#x20;              },

&#x20;              "value": "grain\_fill"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "planting\_month"

&#x20;              },

&#x20;              "value": "2026-03"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "production\_purpose"

&#x20;              },

&#x20;              "value": "subsistence"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "production\_system"

&#x20;              },

&#x20;              "value": "intercropping"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "uses\_certified\_seeds"

&#x20;              },

&#x20;              "value": true

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "uses\_fertilizer"

&#x20;              },

&#x20;              "value": false

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "uses\_pesticides"

&#x20;              },

&#x20;              "value": false

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "water\_source"

&#x20;              },

&#x20;              "value": "canal\_system"

&#x20;            },

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "harvest\_year"

&#x20;              },

&#x20;              "value": 2026

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "livestock"

&#x20;          },

&#x20;          "list": \[]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

Knowledge Search API

Endpoint  :   GET /api/v1/knowledge/search

| API Parameter | Type   | Exact Beckn Field                                 | Remarks                                                                         |
| ------------- | ------ | ------------------------------------------------- | ------------------------------------------------------------------------------- |
| q             | string | message.intent.item.descriptor.name               | Natural-language search query (e.g., "What fertiliser should I use for maize?") |
| topK          | number | message.intent.tags\[].descriptor.code = topK     | Domain-specific extension for maximum results                                   |
| domain        | string | message.intent.tags\[].descriptor.code = domain   | Knowledge domain filter (e.g., agronomy)                                        |
| language      | string | message.intent.tags\[].descriptor.code = language | Language preference (e.g., en, sw)                                              |

Example Payload:

\
\
{

&#x20; "context": {

&#x20;   "domain": "agri:knowledge-base",

&#x20;   "action": "search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456",

&#x20;   "message\_id": "msg-123456",

&#x20;   "timestamp": "2026-08-03T12:00:00Z"

&#x20; },

&#x20; "message": {

&#x20;   "intent": {

&#x20;  "item": {

&#x20;    "descriptor": {

&#x20;      "name": "What fertiliser should I use for maize?"

&#x20;    }

&#x20;  },

&#x20;  "tags": \[

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "topK"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "5"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "domain"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "agronomy"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "language"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "en"

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

Response

| API Field       | Type    | Beckn Field                                              | Remarks                                                                                                                                                                                                                                                                                                 |
| --------------- | ------- | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| success         | boolean | HTTP Status + message / error                            | ❌ Beckn has no success field. A successful response is indicated by a valid Beckn response (typically HTTP 200 with message). Errors are represented using the error object. ([Beckn](https://developers.becknprotocol.io/docs/logistics-specification/schema-reference/error/?utm_source=chatgpt.com)) |
| statusCode      | number  | HTTP Status Code                                         | ❌ Not part of the Beckn payload. Keep it at the HTTP protocol layer. ([Beckn](https://developers.becknprotocol.io/docs/logistics-specification/schema-reference/error/?utm_source=chatgpt.com))                                                                                                         |
| data.query      | string  | message.catalog.tags\[].descriptor.code = "query"        | Original query used to retrieve the results. Domain-specific metadata.                                                                                                                                                                                                                                  |
| data.chunks     | array   | message.catalog.items\[]                                 | Each retrieved knowledge chunk becomes a Beckn Item. ([GitExtract](https://gitextract.com/beckn/protocol-specifications?utm_source=chatgpt.com))                                                                                                                                                        |
| data.totalFound | number  | message.catalog.tags\[].descriptor.code = "total\_found" | Domain-specific metadata.                                                                                                                                                                                                                                                                               |
| data.searchMode | string  | message.catalog.tags\[].descriptor.code = "search\_mode" | Example: hybrid, vector, keyword.                                                                                                                                                                                                                                                                       |
| data.latencyMs  | number  | message.catalog.tags\[].descriptor.code = "latency\_ms"  | Search execution metadata.                                                                                                                                                                                                                                                                              |
| traceId         | string  | context.message\_id (preferred)                          | Correlation identifier for the request/response cycle. Beckn already defines message\_id for this purpose. ([Beckn](https://developers.becknprotocol.io/docs/core-specification/schema-reference/context/?utm_source=chatgpt.com))                                                                      |
| timestamp       | string  | context.timestamp                                        | <p>Standard Beckn context field (RFC3339/ISO-8601 timestamp). (<a href="https://developers.becknprotocol.io/docs/core-specification/schema-reference/context/?utm_source=chatgpt.com">Beckn</a>)<br><br><br><br></p>                                                                                    |

Example Reponse:

\
\
{

&#x20; "context": {

&#x20;   "domain": "agri:knowledge-service",

&#x20;   "action": "on\_search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456",

&#x20;   "message\_id": "trace-987654321",

&#x20;   "timestamp": "2026-08-03T12:00:01Z"

&#x20; },

&#x20; "message": {

&#x20;   "catalog": {

&#x20;  "items": \[

&#x20;    {

&#x20;      "id": "chunk-001",

&#x20;      "descriptor": {

&#x20;        "name": "Knowledge Chunk 1"

&#x20;      },

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "content"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "Apply Nitrogen fertilizer during the vegetative stage of maize for optimal growth."

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "id": "chunk-002",

&#x20;      "descriptor": {

&#x20;        "name": "Knowledge Chunk 2"

&#x20;      },

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "content"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "Use DAP fertilizer at planting and CAN fertilizer for top dressing."

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ],

&#x20;  "tags": \[

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "query"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "What fertiliser should I use for maize?"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "total\_found"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "2"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "search\_mode"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "hybrid"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "latency\_ms"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "145"

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

Open-Meteo Weather API

Endpoint  :   GET /v1/forecast

| API Parameter  | Type                     | Exact Beckn Field                                         | Remarks                                      |
| -------------- | ------------------------ | --------------------------------------------------------- | -------------------------------------------- |
| latitude       | number                   | message.intent.fulfillment.end.location.gps               | GPS format: "latitude,longitude"             |
| longitude      | number                   | message.intent.fulfillment.end.location.gps               | Combined with latitude in a single gps field |
| hourly         | string (comma-separated) | message.intent.tags\[].descriptor.code = "hourly"         | Requested hourly weather variables           |
| daily          | string (comma-separated) | message.intent.tags\[].descriptor.code = "daily"          | Requested daily forecast variables           |
| forecast\_days | number                   | message.intent.tags\[].descriptor.code = "forecast\_days" | Number of forecast days                      |

| API Field              | Type   | Exact Beckn Field                                                               | Remarks                                                                                                                                                                                      |
| ---------------------- | ------ | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| latitude               | number | message.catalog.providers\[].locations\[].gps                                   | Returned model/grid latitude as part of GPS ("lat,lon"). ([Beckn](https://developers.becknprotocol.io/docs/core-specification/schema-reference/location/?utm_source=chatgpt.com))            |
| longitude              | number | message.catalog.providers\[].locations\[].gps                                   | Returned model/grid longitude as part of GPS. ([Beckn](https://developers.becknprotocol.io/docs/core-specification/schema-reference/location/?utm_source=chatgpt.com))                       |
| generationtime\_ms     | number | message.catalog.tags\[].descriptor.code = "generationtime\_ms"                  | Weather-provider metadata. ([Beckn](https://developers.becknprotocol.io/docs/core-specification/schema-reference/item/?utm_source=chatgpt.com))                                              |
| utc\_offset\_seconds   | number | message.catalog.tags\[].descriptor.code = "utc\_offset\_seconds"                | Timezone metadata.                                                                                                                                                                           |
| timezone               | string | message.catalog.tags\[].descriptor.code = "timezone"                            | e.g. Africa/Nairobi.                                                                                                                                                                         |
| timezone\_abbreviation | string | message.catalog.tags\[].descriptor.code = "timezone\_abbreviation"              | e.g. EAT.                                                                                                                                                                                    |
| elevation              | number | message.catalog.providers\[].locations\[].tags\[].descriptor.code = "elevation" | Elevation of the weather model location.                                                                                                                                                     |
| hourly\_units          | object | message.catalog.tags\[].descriptor.code = "hourly\_units"                       | Units for returned hourly variables.                                                                                                                                                         |
| hourly                 | object | message.catalog.items\[]                                                        | Each hourly forecast (or forecast series) represented as catalog items. ([Beckn](https://developers.becknprotocol.io/docs/core-specification/schema-reference/item/?utm_source=chatgpt.com)) |

Example Reponse:

{

&#x20; "context": {

&#x20;   "domain": "weather-advisory:oan",

&#x20;   "action": "on\_search",

&#x20;   "version": "1.1.0",

&#x20;   "bap\_id": "bap.example.org",

&#x20;   "bpp\_id": "bpp.example.org",

&#x20;   "transaction\_id": "tx-123456",

&#x20;   "message\_id": "msg-123456",

&#x20;   "timestamp": "2026-08-03T12:00:01Z"

&#x20; },

&#x20; "message": {

&#x20;   "catalog": {

&#x20;  "providers": \[

&#x20;    {

&#x20;      "id": "weather-provider-001",

&#x20;      "descriptor": {

&#x20;        "name": "Weather Forecast Service"

&#x20;      },

&#x20;      "locations": \[

&#x20;        {

&#x20;          "id": "LOC-001",

&#x20;          "gps": "-1.3005272,36.824646",

&#x20;          "tags": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "elevation"

&#x20;              },

&#x20;              "list": \[

&#x20;                {

&#x20;                  "descriptor": {

&#x20;                    "code": "value"

&#x20;                  },

&#x20;                  "value": "1677"

&#x20;                }

&#x20;              ]

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ],

&#x20;  "items": \[

&#x20;    {

&#x20;     "id": "hourly-forecast",

&#x20;      "descriptor": {

&#x20;        "name": "Hourly Weather Forecast"

&#x20;      },

&#x20;      "tags": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "time"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "temperature\_2m"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "relative\_humidity\_2m"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "dew\_point\_2m"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "apparent\_temperature"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "cloud\_cover"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;       },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "precipitation"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "precipitation\_probability"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "soil\_temperature\_0cm"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "wind\_direction\_10m"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "wind\_speed\_10m"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "rain"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        },

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "visibility"

&#x20;          },

&#x20;          "list": \[

&#x20;            {

&#x20;              "descriptor": {

&#x20;                "code": "value"

&#x20;              },

&#x20;              "value": "\[ ... ]"

&#x20;            }

&#x20;          ]

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ],

&#x20;  "tags": \[

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "generationtime\_ms"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "1.2930631637573242"

&#x20;       }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "utc\_offset\_seconds"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "0"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "timezone"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "GMT"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "timezone\_abbreviation"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "GMT"

&#x20;        }

&#x20;      ]

&#x20;    },

&#x20;    {

&#x20;      "descriptor": {

&#x20;        "code": "hourly\_units"

&#x20;      },

&#x20;      "list": \[

&#x20;        {

&#x20;          "descriptor": {

&#x20;            "code": "value"

&#x20;          },

&#x20;          "value": "{\\"time\\":\\"iso8601\\",\\"temperature\_2m\\":\\"°C\\",\\"relative\_humidity\_2m\\":\\"%\\",\\"dew\_point\_2m\\":\\"°C\\",\\"apparent\_temperature\\":\\"°C\\",\\"cloud\_cover\\":\\"%\\",\\"precipitation\\":\\"mm\\",\\"precipitation\_probability\\":\\"%\\",\\"soil\_temperature\_0cm\\":\\"°C\\",\\"wind\_direction\_10m\\":\\"°\\",\\"wind\_speed\_10m\\":\\"km/h\\",\\"rain\\":\\"mm\\",\\"visibility\\":\\"m\\"}"

&#x20;        }

&#x20;      ]

&#x20;    }

&#x20;  ]

&#x20;   }

&#x20; }

}

<br>
