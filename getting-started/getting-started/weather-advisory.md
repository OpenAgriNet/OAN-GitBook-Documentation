# Weather Advisory

**Version 0.1**

Version History

<table><thead><tr><th width="179">Date</th><th>Version</th><th>Description</th></tr></thead><tbody><tr><td>08-03-2025</td><td>0.1</td><td>Initial Version</td></tr><tr><td>08-03-2025</td><td>0.1</td><td>Current Version</td></tr></tbody></table>

This section will provide all the details to implement and integrate the Weather Advisory use case on to the network

### Flow diagrams

#### General Beckn message flow and error handling

This section is relevant to all the messages flows illustrated below and discussed further in the document.

Beckn is a asynchronous protocol at its core.

* When a network participant(NP1) sends a message to another participant(NP2), the other participant(NP2) immediately returns back an ACK/NACK(Acknowledgement or Negative Acknowledgement in case of error - usually with wrongly formed messages).
* An ACK is an indicator that the receiving participant(NP2) will process this message and dispatch an on\_xxxxxx message to original NP (NP1)
* Subsequently after processing the message NP2 sends back the real response in the corresponding on\_xxxxxx message, to which again the first participant(NP1).
* This message can contain a message field (for success) or error field (for failure)
* NP1 when it receives the on\_xxxxxx message, sends back an ACK/NACK (Here in both the cases NP1 will not send any subsequent message).
* In the Use case diagrams, this ACK/NACK is not illustrated explicitly to keep the diagrams crisp.
* However when writing software we should be prepared to receive these NACK messages as well as error field in the on\_xxxxxx messages
* While this discussion is from a Beckn perspective, Adapters can provide synchronous modes. For example, the Protocol Server which is the reference implementation of the Beckn Adapter provides a synchronous mode by default. So if your software calls the support endpoint on the BAP Protocol Server, the Protocol Server waits till it gets the on\_support and returns back that as the response.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### API Calls and Schema

#### Discovery of schemes



<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**search**

**search request for agricultural schemes can be structured as below.**

**search by topic**

* The topic to search is specified in the message->intent->descriptor->name field.
* The desired language is specified in a tag named languages.

```
"context": {
        "domain": "weather-advisory:oan",
        "action": "search",
        "version": "1.1.0",
        "bap_id": "sample-seeker.seekerorganisation.com",
        "bap_uri": "https://sample-seeker.seekerorganisation.com"
        "location": {
            "country": {
                "name": "India",
                "code": "IND"
            },
            "city": {
                "name": "Bangalore",
                "code": "std:080"
            }
        },
        "transaction_id": "355baee6-6b1a-4dd8-b8f0-56606a4c8a91",
        "message_id":"242559b3-cd51-4980-93b3-794cd388e238",
        "timestamp": "2025-03-02T08:00:00.108Z"
    },
    "message": {
        "intent": {
            "category": {
                "descriptor": {
                    "name": "Weather-Forecast"
                }
            },
            "item": {
                "time": {
                    "range": {
                        "start": "2024-03-01T00:00:00.000Z",
                        "end": "2024-03-15T00:00:00.000Z"
                    }
                }
            },
            "fulfillment": {
                "stops": [
                    {
                        "location": {
                            "gps": "12.9767936,  77.590082"
                        }
                    }
                ]
            }
        }
    }

```



**on\_search**

**on\_search with catalog of results**

* The catalog that comes back has a list of providers.
* Each provider has a list of items.
* Each item is the catalog listing for a resource.

Providers (BPP)  can send the weather details in the catalog.  And the Seekers (BAPs) can use these details (which comes in JSON format) to display to the user.



```
        {
            "context": {
                "domain": "weather-advisory:oan",
                "action": "on_search",
                "version": "1.1.0",
                "bpp_id": "oan-weather-provider-network.tekdinext.com",
                "bpp_uri": "https://oan-weather-provider-network.tekdinext.com",
                "country": "IND",
                "city": "std:080",
                "location": {
                    "country": {
                        "name": "India",
                        "code": "IND"
                    },
                    "city": {
                        "name": "Bangalore",
                        "code": "std:080"
                    }
                },
                "bap_id": "sample-seeker.seekerorganisation.com",
                "bap_uri": "https://sample-seeker.seekerorganisation.com",
                "transaction_id": "355baee6-6b1a-4dd8-b8f0-56606a4c8a91",
                "message_id": "242559b3-cd51-4980-93b3-794cd388e238",
                "ttl": "PT10M",
                "timestamp": "2025-03-06T09:41:52.150Z"
            },
            "message": {
                "catalog": {
                    "descriptor": {
                        "name": "Weather Catalog for Weather-Forecast"
                    },
                    "providers": [
                        {
                            "id": "1",
                            "descriptor": {
                                "name": "Weather-forecast",
                                "short_desc": "Tekdi Weather Services",
                                "images": [
                                    {
                                        "url": "https://amicusagro.info/img/amicus_logo.jpg"
                                    }
                                ]
                            },
                            "categories": [
                                {
                                    "id": "c1",
                                    "descriptor": {
                                        "code": "Weather-forecast",
                                        "name": "Weather Forecast"
                                    }
                                }
                            ],
                            "fulfillments": [
                                {
                                    "id": "f1",
                                    "stops": [
                                        {
                                            "time": {
                                                "range": {
                                                    "start": "2025-03-06T09:41:50.057Z",
                                                    "end": "2025-03-13T09:41:50.057Z"
                                                }
                                            }
                                        }
                                    ]
                                }
                            ],
                            "items": [
                                {
                                    "id": "1",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04d@2x.png"
                                            }
                                        ],
                                        "name": "Current Weather",
                                        "short_desc": "broken clouds",
                                        "long_desc": "Temperature: 33.28°C, Min: 33.28°C, Max: 33.28°C, \n                      Humidity: 17%, Wind Speed: 4.77 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-06"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "location"
                                                    },
                                                    "value": "Kanija Bhavan"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.28°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.28°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "17%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.77 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "broken clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741262400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-06 12:00:00",
                                        "short_desc": "broken clouds",
                                        "long_desc": "Temperature: 32.64°C, Min: 31.82°C, Max: 32.64°C, \n                  Humidity: 17%, Wind Speed: 5.46 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-06 12:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "32.64°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "31.82°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "32.64°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "17%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.46 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "broken clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741273200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-06 15:00:00",
                                        "short_desc": "broken clouds",
                                        "long_desc": "Temperature: 28.76°C, Min: 26.61°C, Max: 28.76°C, \n                  Humidity: 17%, Wind Speed: 4.52 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-06 15:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "28.76°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "26.61°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "28.76°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "17%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.52 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "broken clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741284000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-06 18:00:00",
                                        "short_desc": "broken clouds",
                                        "long_desc": "Temperature: 24.59°C, Min: 24.59°C, Max: 24.59°C, \n                  Humidity: 18%, Wind Speed: 2.89 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-06 18:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "24.59°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "24.59°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "24.59°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "18%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.89 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "broken clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741294800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-06 21:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 21.14°C, Min: 21.14°C, Max: 21.14°C, \n                  Humidity: 38%, Wind Speed: 3.08 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-06 21:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "21.14°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "21.14°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "21.14°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "38%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.08 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741305600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 00:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 19.84°C, Min: 19.84°C, Max: 19.84°C, \n                  Humidity: 52%, Wind Speed: 2.38 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 00:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "19.84°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "19.84°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "19.84°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "52%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.38 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741316400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 03:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 23.55°C, Min: 23.55°C, Max: 23.55°C, \n                  Humidity: 45%, Wind Speed: 2.54 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 03:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "23.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "23.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "23.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "45%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.54 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741327200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 06:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 30.36°C, Min: 30.36°C, Max: 30.36°C, \n                  Humidity: 19%, Wind Speed: 1.34 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 06:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "30.36°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "30.36°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "30.36°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "19%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "1.34 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741338000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 09:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 33.33°C, Min: 33.33°C, Max: 33.33°C, \n                  Humidity: 10%, Wind Speed: 1.85 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 09:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.33°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.33°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.33°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "10%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "1.85 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741348800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 12:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 33.02°C, Min: 33.02°C, Max: 33.02°C, \n                  Humidity: 8%, Wind Speed: 2.79 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 12:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.02°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.02°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.02°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "8%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.79 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741359600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 15:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 26.28°C, Min: 26.28°C, Max: 26.28°C, \n                  Humidity: 11%, Wind Speed: 3.36 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 15:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "26.28°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "26.28°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "26.28°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "11%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.36 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741370400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 18:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 23.45°C, Min: 23.45°C, Max: 23.45°C, \n                  Humidity: 22%, Wind Speed: 3.65 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 18:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "23.45°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "23.45°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "23.45°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "22%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.65 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741381200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-07 21:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 21.11°C, Min: 21.11°C, Max: 21.11°C, \n                  Humidity: 44%, Wind Speed: 3.56 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-07 21:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "21.11°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "21.11°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "21.11°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "44%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.56 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741392000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 00:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 19.5°C, Min: 19.5°C, Max: 19.5°C, \n                  Humidity: 71%, Wind Speed: 2.63 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 00:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "19.5°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "19.5°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "19.5°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "71%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.63 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741402800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 03:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 22.99°C, Min: 22.99°C, Max: 22.99°C, \n                  Humidity: 67%, Wind Speed: 3.37 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 03:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "22.99°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "22.99°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "22.99°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "67%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.37 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741413600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 06:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 30.55°C, Min: 30.55°C, Max: 30.55°C, \n                  Humidity: 26%, Wind Speed: 0.76 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 06:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "30.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "30.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "30.55°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "26%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "0.76 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741424400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 09:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 33.25°C, Min: 33.25°C, Max: 33.25°C, \n                  Humidity: 16%, Wind Speed: 1.79 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 09:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.25°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.25°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.25°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "16%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "1.79 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741435200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 12:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 33.32°C, Min: 33.32°C, Max: 33.32°C, \n                  Humidity: 12%, Wind Speed: 2.71 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 12:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.32°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.32°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.32°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "12%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.71 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741446000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 15:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 27.34°C, Min: 27.34°C, Max: 27.34°C, \n                  Humidity: 16%, Wind Speed: 3.76 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 15:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "27.34°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "27.34°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "27.34°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "16%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.76 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741456800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 18:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 24.24°C, Min: 24.24°C, Max: 24.24°C, \n                  Humidity: 26%, Wind Speed: 3.28 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 18:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "24.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "24.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "24.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "26%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.28 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741467600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-08 21:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 21.91°C, Min: 21.91°C, Max: 21.91°C, \n                  Humidity: 38%, Wind Speed: 3.11 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-08 21:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "21.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "21.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "21.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "38%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.11 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741478400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 00:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 20.3°C, Min: 20.3°C, Max: 20.3°C, \n                  Humidity: 56%, Wind Speed: 3.56 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 00:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "20.3°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "20.3°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "20.3°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "56%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.56 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741489200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 03:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 23.93°C, Min: 23.93°C, Max: 23.93°C, \n                  Humidity: 42%, Wind Speed: 2.55 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 03:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "23.93°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "23.93°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "23.93°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "42%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.55 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741500000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 06:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 31.24°C, Min: 31.24°C, Max: 31.24°C, \n                  Humidity: 20%, Wind Speed: 2.11 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 06:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "31.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "31.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "31.24°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "20%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.11 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741510800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/03d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 09:00:00",
                                        "short_desc": "scattered clouds",
                                        "long_desc": "Temperature: 34.94°C, Min: 34.94°C, Max: 34.94°C, \n                  Humidity: 12%, Wind Speed: 4.94 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 09:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "34.94°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "34.94°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "34.94°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "12%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.94 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "scattered clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741521600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/02d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 12:00:00",
                                        "short_desc": "few clouds",
                                        "long_desc": "Temperature: 33.19°C, Min: 33.19°C, Max: 33.19°C, \n                  Humidity: 13%, Wind Speed: 6.22 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 12:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.19°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.19°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.19°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "13%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "6.22 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "few clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741532400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 15:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 26.91°C, Min: 26.91°C, Max: 26.91°C, \n                  Humidity: 22%, Wind Speed: 5.56 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 15:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "26.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "26.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "26.91°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "22%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.56 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741543200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 18:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 24.08°C, Min: 24.08°C, Max: 24.08°C, \n                  Humidity: 35%, Wind Speed: 4.84 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 18:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "24.08°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "24.08°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "24.08°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "35%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.84 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741554000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-09 21:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 21.03°C, Min: 21.03°C, Max: 21.03°C, \n                  Humidity: 50%, Wind Speed: 2.99 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-09 21:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "21.03°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "21.03°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "21.03°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "50%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.99 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741564800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 00:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 19.64°C, Min: 19.64°C, Max: 19.64°C, \n                  Humidity: 54%, Wind Speed: 2.4 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 00:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "19.64°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "19.64°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "19.64°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "54%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "2.4 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741575600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 03:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 24.16°C, Min: 24.16°C, Max: 24.16°C, \n                  Humidity: 33%, Wind Speed: 3.56 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 03:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "24.16°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "24.16°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "24.16°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "33%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.56 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741586400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 06:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 31.44°C, Min: 31.44°C, Max: 31.44°C, \n                  Humidity: 13%, Wind Speed: 4.82 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 06:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "31.44°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "31.44°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "31.44°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "13%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.82 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741597200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 09:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 33.69°C, Min: 33.69°C, Max: 33.69°C, \n                  Humidity: 11%, Wind Speed: 5.55 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 09:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.69°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.69°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.69°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "11%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.55 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741608000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/01d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 12:00:00",
                                        "short_desc": "clear sky",
                                        "long_desc": "Temperature: 32.15°C, Min: 32.15°C, Max: 32.15°C, \n                  Humidity: 12%, Wind Speed: 6.04 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 12:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "32.15°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "32.15°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "32.15°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "12%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "6.04 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "clear sky"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741618800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/03n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 15:00:00",
                                        "short_desc": "scattered clouds",
                                        "long_desc": "Temperature: 26.39°C, Min: 26.39°C, Max: 26.39°C, \n                  Humidity: 25%, Wind Speed: 5.48 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 15:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "26.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "26.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "26.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "25%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.48 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "scattered clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741629600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/03n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 18:00:00",
                                        "short_desc": "scattered clouds",
                                        "long_desc": "Temperature: 22.83°C, Min: 22.83°C, Max: 22.83°C, \n                  Humidity: 46%, Wind Speed: 5.13 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 18:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "22.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "22.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "22.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "46%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.13 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "scattered clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741640400",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-10 21:00:00",
                                        "short_desc": "overcast clouds",
                                        "long_desc": "Temperature: 20.61°C, Min: 20.61°C, Max: 20.61°C, \n                  Humidity: 64%, Wind Speed: 3.64 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-10 21:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "20.61°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "20.61°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "20.61°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "64%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.64 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "overcast clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741651200",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04n@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-11 00:00:00",
                                        "short_desc": "overcast clouds",
                                        "long_desc": "Temperature: 19.39°C, Min: 19.39°C, Max: 19.39°C, \n                  Humidity: 66%, Wind Speed: 3.31 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-11 00:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "19.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "19.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "19.39°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "66%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.31 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "overcast clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741662000",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-11 03:00:00",
                                        "short_desc": "overcast clouds",
                                        "long_desc": "Temperature: 23.83°C, Min: 23.83°C, Max: 23.83°C, \n                  Humidity: 43%, Wind Speed: 3.57 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-11 03:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "23.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "23.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "23.83°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "43%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "3.57 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "overcast clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741672800",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-11 06:00:00",
                                        "short_desc": "overcast clouds",
                                        "long_desc": "Temperature: 30.85°C, Min: 30.85°C, Max: 30.85°C, \n                  Humidity: 21%, Wind Speed: 4.54 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-11 06:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "30.85°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "30.85°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "30.85°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "21%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "4.54 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "overcast clouds"
                                                }
                                            ]
                                        }
                                    ]
                                },
                                {
                                    "id": "forecast-1741683600",
                                    "descriptor": {
                                        "images": [
                                            {
                                                "url": "https://openweathermap.org/img/wn/04d@2x.png"
                                            }
                                        ],
                                        "name": "Forecast for 2025-03-11 09:00:00",
                                        "short_desc": "overcast clouds",
                                        "long_desc": "Temperature: 33.76°C, Min: 33.76°C, Max: 33.76°C, \n                  Humidity: 16%, Wind Speed: 5.18 m/s."
                                    },
                                    "matched": true,
                                    "recommended": true,
                                    "category_ids": [
                                        "c1"
                                    ],
                                    "fulfillment_ids": [
                                        "f1"
                                    ],
                                    "tags": [
                                        {
                                            "descriptor": {
                                                "code": "2025-03-11 09:00:00"
                                            },
                                            "list": [
                                                {
                                                    "descriptor": {
                                                        "code": "temperature"
                                                    },
                                                    "value": "33.76°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "min-temp"
                                                    },
                                                    "value": "33.76°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "max-temp"
                                                    },
                                                    "value": "33.76°C"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "humidity"
                                                    },
                                                    "value": "16%"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "wind-speed"
                                                    },
                                                    "value": "5.18 m/s"
                                                },
                                                {
                                                    "descriptor": {
                                                        "code": "weather-condition"
                                                    },
                                                    "value": "overcast clouds"
                                                }
                                            ]
                                        }
                                    ]
                                }
                            ]
                        }
                    ]
                }
            }
        }
    
```
