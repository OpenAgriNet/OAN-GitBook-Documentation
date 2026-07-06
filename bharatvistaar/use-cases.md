# BharatVISTAAR Use Cases

## 1. Weather Forecast
The Weather Forecast use case enables farmers and intermediaries to access location-specific weather forecasts through conversational interactions, supporting crop planning, sowing decisions, and risk management across web, mobile app, and IVR channels.

**Problem:** Farmers in rain-dependent regions make critical decisions — on sowing, spraying, and harvesting — based on informal or delayed weather information. Inaccurate forecasts lead to crop loss, wasted inputs, and missed windows.

**Solution:** BharatVISTAAR integrates IMD weather forecast data into a conversational interface. The AI Layer interprets location and timing context, retrieves verified short-range and hourly forecasts, and delivers them in simple, localised language suited to the farmer's channel and language preference.

### 1.1 User Journey
**Persona:** Gurpreet, a wheat and paddy farmer from Ludhiana district, Punjab
**Channel:** IVR

**Steps:**
1. Gurpreet calls the BharatVISTAAR IVR number.
2. Asks about the weather forecast for the next two days.
3. The AI Layer identifies his location based on his registered profile.
4. The weather forecast is retrieved from the IMD for the Ludhiana district.
5. Forecast is communicated via voice, including temperature, rainfall probability, and wind conditions.

### 1.2 Value Add
* Supports timely sowing and harvesting decisions
* Reduces crop loss from unseasonal rainfall or frost
* Improves input efficiency by timing sprays and fertiliser applications to dry windows
* Accessible without a smartphone or internet connection via IVR

### 1.3 API Calls & Schema

`search` → `on_search`

**search (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "search",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>",
    "location": {
      "country": {
        "code": "IND"
      },
      "state": {
        "code": "PB"
      }
    }
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "code": "WFC"
        }
      },
      "fulfillment": {
        "stops": [
          {
            "location": {
              "gps": "30.9010,75.8573"
            },
            "time": {
              "range": {
                "start": "<timestamp>",
                "end": "<timestamp+48h>"
              }
            }
          }
        ]
      }
    }
  }
}
```

**on_search (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_search",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "imd-weather-provider",
          "descriptor": {
            "name": "India Meteorological Department"
          },
          "items": [
            {
              "id": "forecast-ludhiana-48h",
              "descriptor": {
                "name": "48-Hour Forecast – Ludhiana",
                "short_desc": "Rain likely on Day 2; clear on Day 1"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "max_temp"
                  },
                  "value": "32°C"
                },
                {
                  "descriptor": {
                    "code": "min_temp"
                  },
                  "value": "21°C"
                },
                {
                  "descriptor": {
                    "code": "rainfall_probability"
                  },
                  "value": "70%"
                },
                {
                  "descriptor": {
                    "code": "wind_speed"
                  },
                  "value": "18 km/h"
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

## 2. Mandi Prices
The Mandi Price use case enables farmers to access real-time commodity price information from Agriculture Produce Market Committees (APMCs) near their location, supporting informed selling decisions across web, mobile app, and IVR.

**Problem:** Farmers often sell produce at farmgate prices without knowledge of prevailing mandi rates, leading to income losses and dependence on middlemen who exploit information asymmetry.

**Solution:** BharatVISTAAR integrates Agmarknet commodity price data into a conversational interface. The AI Layer identifies the farmer's commodity and nearest mandis, retrieves current prices, and presents them in a comparable, actionable format.

### 2.1 User Journey
**Persona:** Vandana, a cotton farmer from Yavatmal district, Vidarbha, Maharashtra
**Channel:** Mobile App (voice query)

**Steps:**
1. Vandana opens the BharatVISTAAR mobile app.
2. Asks via voice: "What is today's cotton price at the nearest mandi?"
3. AI Layer identifies her location and commodity from her profile.
4. Mandi price data is retrieved from Agmarknet for markets near Yavatmal.
5. Prices from two or three nearby mandis are displayed and read out for comparison.

### 2.2 Value Add
* Reduces income loss from uninformed farmgate sales
* Reduces dependence on intermediaries for price information
* Enables farmers to choose the most favourable market
* Supports better timing of produce sales

### 2.3 API Calls & Schema

`search` → `on_search`

**search (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "search",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>",
    "location": {
      "country": {
        "code": "IND"
      },
      "state": {
        "code": "MH"
      }
    }
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "code": "price-discovery"
        }
      },
      "item": {
        "descriptor": {
          "code": "mandi",
          "name": "cotton"
        }
      },
      "fulfillment": {
        "stops": [
          {
            "location": {
              "gps": "20.3888,78.1204"
            }
          }
        ]
      }
    }
  }
}
```

**on_search (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_search",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "agmarknet-provider",
          "descriptor": {
            "name": "Agmarknet – APMC Price Data"
          },
          "items": [
            {
              "id": "cotton-yavatmal-apmc",
              "descriptor": {
                "name": "Cotton – Yavatmal APMC",
                "short_desc": "Today's modal price at Yavatmal market"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "modal_price"
                  },
                  "value": "₹6,850 per quintal"
                },
                {
                  "descriptor": {
                    "code": "min_price"
                  },
                  "value": "₹6,600 per quintal"
                },
                {
                  "descriptor": {
                    "code": "max_price"
                  },
                  "value": "₹7,100 per quintal"
                },
                {
                  "descriptor": {
                    "code": "date"
                  },
                  "value": "<today's date>"
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

## 3. Advisory
The Advisory use case enables farmers, extension workers, and intermediaries to access validated, location- and crop-specific agricultural guidance including pest and disease management, crop practices, package of practices, and input recommendations. It also supports discovery of documents and videos related to agricultural topics.

**Problem:** Scientific advisory content is fragmented across research institutions, KVKs, and state agriculture departments and rarely reaches farmers in a timely, contextualised, and understandable form. Pest and disease outbreaks in particular require rapid, actionable guidance that the current extension system cannot consistently deliver at scale.

**Solution:** BharatVISTAAR aggregates advisory content from verified institutional sources — including ICAR, KVKs, and state agriculture departments — into a conversational interface. The AI Layer interprets the farmer's crop, location, and query to retrieve and contextualise the most relevant advisory content, covering pest and disease management, crop-stage recommendations, package of practices, and related documents or videos.

### 3.1 User Journey
**Persona:** Ramesh, a maize and ragi farmer from Hassan district, Karnataka
**Channel:** Mobile App (text query)

**Steps:**
1. Ramesh opens the BharatVISTAAR app.
2. Asks: "There are white patches appearing on my maize leaves. What should I do?"
3. AI Layer identifies crop, symptom, and location context.
4. Advisory content is retrieved from ICAR-KVK sources for maize pest and disease management in Karnataka.
5. Diagnosis, recommended action, and input guidance are displayed.
6. Ramesh can also access a related advisory video or document from the same interface.

### 3.2 Value Add
* Delivers timely pest and disease guidance before crop losses escalate
* Reduces incorrect or excessive pesticide use
* Extends the reach of KVK and institutional advisory beyond physical extension visits
* Supports document and video-based learning for farmers with varying literacy levels

### 3.3 API Calls & Schema

`search` → `on_search`
*(Used for crop advisory, pest and disease queries, package of practices, document search, and video search workflows.)*

**search (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "search",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>",
    "location": {
      "country": {
        "code": "IND"
      },
      "state": {
        "code": "KA"
      }
    }
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "code": "crop-advisory"
        }
      },
      "item": {
        "descriptor": {
          "name": "maize white leaf patch"
        }
      },
      "fulfillment": {
        "stops": [
          {
            "location": {
              "gps": "13.0122,76.1003"
            }
          }
        ]
      }
    }
  }
}
```

**on_search (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_search",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "agri-advisory-provider",
          "descriptor": {
            "name": "Agricultural Advisory Services"
          },
          "items": [
            {
              "id": "maize-turcicum-blight-advisory",
              "descriptor": {
                "name": "Maize Turcicum Leaf Blight Advisory",
                "short_desc": "Fungal disease identified — early intervention recommended"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "diagnosis"
                  },
                  "value": "Turcicum leaf blight caused by Exserohilum turcicum"
                },
                {
                  "descriptor": {
                    "code": "symptoms"
                  },
                  "value": "Elongated grey-green lesions on lower leaves progressing upward"
                },
                {
                  "descriptor": {
                    "code": "recommended_action"
                  },
                  "value": "Apply Mancozeb 75% WP at 2g per litre; avoid overhead irrigation"
                },
                {
                  "descriptor": {
                    "code": "source"
                  },
                  "value": "ICAR-KVK Hassan"
                },
                {
                  "descriptor": {
                    "code": "related_content"
                  },
                  "value": "Video: Maize disease management — ICAR extension series"
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

## 4. Scheme Information
The Scheme Information use case enables users to discover central and state government agricultural schemes, understand eligibility criteria, benefits, and application processes through conversational interaction across web, mobile app (text and voice), and IVR.

**Problem:** Government scheme information is fragmented across multiple ministry portals, circulars, and PDFs. Farmers and intermediaries often struggle to identify relevant schemes, leading to low uptake and dependence on informal agents.

**Solution:** BharatVISTAAR consolidates verified scheme information into a single conversational access layer. The AI Layer interprets user needs, maps them to relevant schemes (central and state), and retrieves structured scheme details from authoritative sources, presented in simple, localised language.

### 4.1 User Journey
**Persona:** Kavita, an FPO Programme Manager supporting smallholder farmers (non-farmer persona)
**Channel:** Web (text query)

**Steps:**
1. Kavita opens the BharatVISTAAR website.
2. Searches for "central schemes for drip irrigation".
3. AI Layer identifies scheme category and target beneficiaries.
4. Scheme information is retrieved from verified government sources.
5. Structured scheme details are displayed on the website.

### 4.2 Value Add
* Improves scheme discoverability and awareness
* Enables intermediaries (FPOs, NGOs) to support farmers at scale
* Reduces misinformation and reliance on agents
* Improves inclusion of small and marginal farmers

### 4.3 API Calls & Schema

`search` → `on_search`

**search (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "search",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>",
    "ttl": "PT10M",
    "location": {
      "country": {
        "code": "IND"
      },
      "state": {
        "code": "KA"
      }
    }
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "code": "schemes-agri"
        }
      },
      "item": {
        "descriptor": {
          "name": "drip irrigation"
        }
      },
      "tags": [
        {
          "descriptor": {
            "code": "scheme_scope"
          },
          "value": "central"
        }
      ]
    }
  }
}
```

**on_search (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_search",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "agri-schemes-provider",
          "descriptor": {
            "name": "Government Agricultural Schemes"
          },
          "items": [
            {
              "id": "pmksy-drip",
              "descriptor": {
                "name": "PMKSY – Per Drop More Crop",
                "short_desc": "Financial assistance for drip irrigation systems"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "eligibility"
                  },
                  "value": "Small and marginal farmers"
                },
                {
                  "descriptor": {
                    "code": "benefit"
                  },
                  "value": "Subsidy up to 55%"
                },
                {
                  "descriptor": {
                    "code": "application_process"
                  },
                  "value": "Apply through state agriculture department portal"
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

## 5. PM-KISAN Scheme Status Check
The PM-KISAN Scheme Status Check use case enables beneficiaries to verify beneficiary details, payment status, and installment information through OTP-authenticated conversational interaction.

**Problem:** Farmers are often uncertain about payment release timelines, beneficiary status, or installment failures, leading to misinformation and repeated visits to local offices.

**Solution:** BharatVISTAAR integrates PM-KISAN systems into a conversational interface, enabling beneficiaries to securely retrieve payment and beneficiary status using registration details and OTP-based verification.

### 5.1 User Journey
**Persona:** Suresh, a millet farmer from Koppal district, Karnataka
**Channel:** IVR

**Steps:**
1. Suresh calls the BharatVISTAAR IVR number.
2. Requests PM-KISAN scheme status.
3. Provides PM-KISAN registration number.
4. AI Layer initiates OTP verification.
5. Suresh enters OTP received on registered mobile number.
6. PM-KISAN status information is retrieved and communicated via voice.

### 5.2 Value Add
* Improves payment transparency
* Reduces misinformation and dependency on agents
* Strengthens trust in income support schemes
* Enhances financial planning for farmers

### 5.3 API Calls & Schema

`init` → `on_init`  
`status` → `on_status`

**init (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "init",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>",
    "location": {
      "country": {
        "code": "IND"
      },
      "city": {
        "code": "*"
      }
    }
  },
  "message": {
    "order": {
      "provider": {
        "id": ""
      },
      "items": [
        {
          "id": ""
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "person": {
              "name": "Customer Name",
              "tags": [
                {
                  "display": true,
                  "descriptor": {
                    "name": "Registration Details",
                    "code": "reg-details"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "name": "Registration Number",
                        "code": "reg-number"
                      },
                      "value": "BR295454592",
                      "display": true
                    }
                  ]
                }
              ]
            }
          }
        }
      ]
    }
  }
}
```

**on_init (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_init",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "ack": {
      "status": "ACK"
    },
    "order": {
      "id": "473138",
      "state": "OTP_SENT"
    }
  }
}
```

**status (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "status",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order_id": "473138",
    "registration_number": "BR295454592"
  }
}
```

**on_status (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_status",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order": {
      "state": "ACTIVE",
      "fulfillments": [
        {
          "state": {
            "descriptor": {
              "name": "Installment Credited",
              "short_desc": "₹2000 credited successfully"
            }
          }
        }
      ]
    }
  }
}
```

## 6. PM-KISAN Grievance Check
The PM-KISAN Grievance Check use case enables beneficiaries to register and track grievances related to PM-KISAN payments, beneficiary records, or data mismatches through conversational interaction.

**Problem:** Beneficiaries who raise grievances related to payment failures or data mismatches often lack visibility into grievance status and next steps, leading to repeated visits to local offices.

**Solution:** BharatVISTAAR integrates grievance registration and tracking systems into a conversational interface, enabling users to create grievances and check status updates using simple identifiers.

### 6.1 User Journey
**Persona:** Mahesh, a wheat and mustard farmer from Morena district, Madhya Pradesh
**Channel:** IVR

**Steps:**
1. Mahesh calls the BharatVISTAAR IVR number.
2. Selects PM-KISAN grievance option.
3. Provides registration number and grievance details.
4. Grievance is registered through BharatVISTAAR.
5. Mahesh later checks grievance status through conversational interaction.
6. Status updates are communicated via voice.

### 6.2 Value Add
* Improves transparency in grievance redressal
* Reduces repeated office visits
* Builds trust in scheme delivery mechanisms
* Saves time and cost for beneficiaries

### 6.3 API Calls & Schema

`init` → `on_init` (for grievance registration)  
`search` → `on_search` (for grievance tracking)

**init (request you will receive for Grievance Registration)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "init",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order": {
      "provider": {
        "id": "pmkisan-grievance"
      },
      "items": [
        {
          "id": "pmkisan-grievance"
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "person": {
              "tags": [
                {
                  "descriptor": {
                    "code": "reg-number"
                  },
                  "value": "R1234567890"
                },
                {
                  "descriptor": {
                    "code": "grievance-type"
                  },
                  "value": "G001"
                },
                {
                  "descriptor": {
                    "code": "grievance-description"
                  },
                  "value": "Payment not received"
                }
              ]
            },
            "contact": {
              "phone": "9876543210"
            }
          }
        }
      ]
    }
  }
}
```

**on_init (response you will send for Grievance Registration)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_init",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "ack": {
      "status": "ACK"
    },
    "order": {
      "id": "GRV202600123",
      "state": "GRIEVANCE_REGISTERED"
    }
  }
}
```

**search (request you will receive for Grievance Tracking)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "search",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "intent": {
      "category": {
        "descriptor": {
          "name": "grievance-agri",
          "code": "grievance"
        }
      },
      "order": {
        "fulfillments": [
          {
            "customer": {
              "person": {
                "tags": [
                  {
                    "descriptor": {
                      "code": "reg-number"
                    },
                    "value": "R1234567890"
                  }
                ]
              }
            }
          }
        ]
      }
    }
  }
}
```

**on_search (response you will send for Grievance Tracking)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_search",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "pmkisan-grievance",
          "items": [
            {
              "id": "grievance-status",
              "descriptor": {
                "name": "PM-KISAN Grievance Status"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "status"
                  },
                  "value": "Under Review"
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

## 7. PMFBY Status Check
The PMFBY Status Check use case enables insured farmers to check crop insurance application, claim, and settlement status through conversational channels.

**Problem:** Crop insurance status information is often delayed or difficult to access, resulting in uncertainty and mistrust, especially during crop loss events.

**Solution:** BharatVISTAAR integrates PMFBY systems to allow farmers to query insurance status using simple prompts, ensuring verified, real-time information delivery in an accessible format.

### 7.1 User Journey
**Persona:** Raju, a groundnut farmer from Anantapur district, Andhra Pradesh
**Channel:** Mobile App (voice query)

**Steps:**
1. Raju opens the BharatVISTAAR mobile app.
2. Asks via voice: "What is my crop insurance status?"
3. AI Layer identifies scheme and user context.
4. Raju provides his registered mobile number.
5. AI Layer initiates OTP verification.
6. Raju enters the OTP received on his registered mobile number.
7. PMFBY system retrieves policy, application, claim, and settlement status.
8. Status is displayed on the app and read out via audio.

### 7.2 Value Add
* Improves transparency in crop insurance processes
* Reduces anxiety during crop loss events
* Strengthens confidence in risk mitigation schemes
* Encourages higher insurance adoption

### 7.3 API Calls & Schema

`init` → `on_init`  
`status` → `on_status`

**init (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "init",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order": {
      "provider": {
        "id": "pmfby-agri"
      },
      "items": [
        {
          "id": "pmfby"
        }
      ],
      "fulfillments": [
        {
          "customer": {
            "contact": {
              "phone": "9876543210"
            }
          }
        }
      ]
    }
  }
}
```

**on_init (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_init",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "ack": {
      "status": "ACK"
    },
    "order": {
      "id": "6256",
      "state": "OTP_SENT"
    }
  }
}
```

**status (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "status",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order_id": "6256"
  }
}
```

**on_status (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_status",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order": {
      "state": "ACTIVE",
      "items": [
        {
          "id": "pmfby"
        }
      ],
      "fulfillments": [
        {
          "state": {
            "descriptor": {
              "name": "Claim Approved",
              "short_desc": "Claim settlement in progress"
            }
          }
        }
      ]
    }
  }
}
```

## 8. Soil Health Card (SHC) Status Check
The SHC Status Check use case enables users to verify Soil Health Card generation, test results availability, and recommendations through conversational interaction.

**Problem:** Farmers often submit soil samples but are unaware of report availability or how to interpret results, limiting the usefulness of the Soil Health Card programme.

**Solution:** BharatVISTAAR integrates SHC systems to allow users to track card status and receive simplified explanations of soil results and recommendations.

### 8.1 User Journey
**Persona:** Lakshmi, a paddy farmer from East Godavari district, Andhra Pradesh
**Channel:** Mobile App (text query)

**Steps:**
1. Lakshmi opens the BharatVISTAAR app.
2. Searches for "Soil Health Card status".
3. AI Layer validates farmer profile and sample details.
4. SHC system retrieves card status.
5. Status and next steps are displayed.

### 8.2 Value Add
* Improves utilisation of soil testing infrastructure
* Encourages balanced fertiliser use
* Reduces input costs
* Improves soil health awareness

### 8.3 API Calls & Schema

`init` → `on_init`

**init (request you will receive)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "init",
    "version": "1.1.0",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
  },
  "message": {
    "order": {
      "provider": {
        "id": "shc-discovery"
      },
      "items": [
        {
          "id": "soil-health-card"
        }
      ]
    }
  }
}
```

**on_init (response you will send)**
```json
{
  "context": {
    "domain": "schemes:vistaar",
    "action": "on_init",
    "transaction_id": "<transaction_id>"
  },
  "message": {
    "order": {
      "state": "REPORT_AVAILABLE"
    }
  }
}
```
