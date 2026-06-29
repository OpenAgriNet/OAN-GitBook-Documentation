# BharatVISTAAR Use Cases

## 1. Scheme Information

The Scheme Information use case enables users to discover central and state government agricultural schemes, understand eligibility criteria, benefits, and application processes through conversational interaction across web, mobile app (text and voice), and IVR.

**Problem:** Government scheme information is fragmented across multiple ministry portals, circulars, and PDFs. Farmers and intermediaries often struggle to identify relevant schemes, leading to low uptake and dependence on informal agents.

**Solution:** BharatVISTAAR consolidates verified scheme information into a single conversational access layer. The AI Layer interprets user needs, maps them to relevant schemes (central and state), and retrieves structured scheme details from authoritative sources, presented in simple, localised language.

### 1.1 User Journey

**Persona:** Kavita, an FPO Programme Manager supporting smallholder farmers (non-farmer persona)
**Channel:** Web (text query)

**Steps:**
1. Kavita opens the BharatVISTAAR website.
2. Searches for "central schemes for drip irrigation".
3. AI Layer identifies scheme category and target beneficiaries.
4. Scheme information is retrieved from verified government sources.
5. Structured scheme details are displayed on the website.

### 1.2 Value Add
* Improves scheme discoverability and awareness
* Enables intermediaries (FPOs, NGOs) to support farmers at scale
* Reduces misinformation and reliance on agents
* Improves inclusion of small and marginal farmers

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

## 2. PM-KISAN Scheme Status Check

The PM-KISAN Scheme Status Check use case enables beneficiaries to verify beneficiary details, payment status, and installment information through OTP-authenticated conversational interaction.

**Problem:** Farmers are often uncertain about payment release timelines, beneficiary status, or installment failures, leading to misinformation and repeated visits to local offices.

**Solution:** BharatVISTAAR integrates PM-KISAN systems into a conversational interface, enabling beneficiaries to securely retrieve payment and beneficiary status using registration details and OTP-based verification.

### 2.1 User Journey

**Persona:** Suresh, a millet farmer from Koppal district, Karnataka
**Channel:** IVR

**Steps:**
1. Suresh calls the BharatVISTAAR IVR number.
2. Requests PM-KISAN scheme status.
3. Provides PM-KISAN registration number.
4. AI Layer initiates OTP verification.
5. Suresh enters OTP received on registered mobile number.
6. PM-KISAN status information is retrieved and communicated via voice.

### 2.2 Value Add
* Improves payment transparency
* Reduces misinformation and dependency on agents
* Strengthens trust in income support schemes
* Enhances financial planning for farmers

### 2.3 API Calls & Schema

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

## 3. PM-KISAN Grievance Check

The PM-KISAN Grievance Check use case enables beneficiaries to register and track grievances related to PM-KISAN payments, beneficiary records, or data mismatches through conversational interaction.

**Problem:** Beneficiaries who raise grievances related to payment failures or data mismatches often lack visibility into grievance status and next steps, leading to repeated visits to local offices.

**Solution:** BharatVISTAAR integrates grievance registration and tracking systems into a conversational interface, enabling users to create grievances and check status updates using simple identifiers.

### 3.1 User Journey

**Persona:** Mahesh, a wheat and mustard farmer from Morena district, Madhya Pradesh
**Channel:** IVR

**Steps:**
1. Mahesh calls the BharatVISTAAR IVR number.
2. Selects PM-KISAN grievance option.
3. Provides registration number and grievance details.
4. Grievance is registered through BharatVISTAAR.
5. Mahesh later checks grievance status through conversational interaction.
6. Status updates are communicated via voice.

### 3.2 Value Add
* Improves transparency in grievance redressal
* Reduces repeated office visits
* Builds trust in scheme delivery mechanisms
* Saves time and cost for beneficiaries

### 3.3 API Calls & Schema

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

## 4. PMFBY Status Check

The PMFBY Status Check use case enables insured farmers to check crop insurance application, claim, and settlement status through conversational channels.

**Problem:** Crop insurance status information is often delayed or difficult to access, resulting in uncertainty and mistrust, especially during crop loss events.

**Solution:** BharatVISTAAR integrates PMFBY systems to allow farmers to query insurance status using simple prompts, ensuring verified, real-time information delivery in an accessible format.

### 4.1 User Journey

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

### 4.2 Value Add
* Improves transparency in crop insurance processes
* Reduces anxiety during crop loss events
* Strengthens confidence in risk mitigation schemes
* Encourages higher insurance adoption

### 4.3 API Calls & Schema

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

## 5. Soil Health Card (SHC) Status Check

The SHC Status Check use case enables users to verify Soil Health Card generation, test results availability, and recommendations through conversational interaction.

**Problem:** Farmers often submit soil samples but are unaware of report availability or how to interpret results, limiting the usefulness of the Soil Health Card programme.

**Solution:** BharatVISTAAR integrates SHC systems to allow users to track card status and receive simplified explanations of soil results and recommendations.

### 5.1 User Journey

**Persona:** Lakshmi, a paddy farmer from East Godavari district, Andhra Pradesh
**Channel:** Mobile App (text query)

**Steps:**
1. Lakshmi opens the BharatVISTAAR app.
2. Searches for "Soil Health Card status".
3. AI Layer validates farmer profile and sample details.
4. SHC system retrieves card status.
5. Status and next steps are displayed.

### 5.2 Value Add
* Improves utilisation of soil testing infrastructure
* Encourages balanced fertiliser use
* Reduces input costs
* Improves soil health awareness

### 5.3 API Calls & Schema

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

## 6. Advisory

The Advisory use case enables users to access validated agricultural guidance and decision-support services related to crop practices, pest and disease management, weather forecasts, mandi prices, crop registries, and crop recommendations through conversational interaction.

**Problem:** Scientific advisory content is fragmented across institutions and often fails to reach farmers and intermediaries in a timely and understandable manner.

**Solution:** BharatVISTAAR aggregates verified advisory and decision-support services from institutional and ecosystem sources. The AI Layer contextualises queries and delivers actionable guidance in simple language.

### 6.1 User Journey

**Persona:** Ajay, a Block Agriculture Officer supporting multiple villages (non-farmer persona)
**Channel:** Web (text query)

**Steps:**
1. Ajay accesses BharatVISTAAR on the web.
2. Searches for pest advisory for maize.
3. AI Layer identifies crop and issue context.
4. Advisory content is retrieved from institutional sources.
5. Recommendations are displayed for dissemination to farmers.

### 6.2 Value Add
* Strengthens public extension delivery
* Improves the consistency of advisory dissemination
* Reduces incorrect input usage
* Supports data-driven decision-making

### 6.3 API Calls & Schema

`search` → `on_search`  
*(Used for weather forecasts, mandi prices, crop registries, crop recommendations, and advisory discovery workflows.)*

**search (request you will receive)**
```json
{
  "context": {
    "domain": "advisory:mh-vistaar",
    "action": "search",
    "version": "1.1.0",
    "bap_id": "bharatvistaar.gov.in",
    "bap_uri": "https://bharatvistaar.gov.in",
    "transaction_id": "<transaction_id>",
    "message_id": "<message_id>",
    "timestamp": "<timestamp>"
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
          "name": "maize pest advisory"
        }
      },
      "fulfillment": {
        "stops": [
          {
            "location": {
              "gps": "23.2599,77.4126"
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
    "domain": "advisory:mh-vistaar",
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
              "id": "fall-armyworm-advisory",
              "descriptor": {
                "name": "Maize Fall Armyworm Advisory"
              },
              "tags": [
                {
                  "descriptor": {
                    "code": "symptoms"
                  },
                  "value": "Leaf damage and caterpillar infestation"
                },
                {
                  "descriptor": {
                    "code": "recommended_action"
                  },
                  "value": "Apply recommended bio-pesticide within 48 hours"
                },
                {
                  "descriptor": {
                    "code": "source"
                  },
                  "value": "ICAR-KVK"
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
