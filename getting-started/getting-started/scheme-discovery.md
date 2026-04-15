# Scheme Discovery

**Version 0.1**

Version History

<table><thead><tr><th width="179">Date</th><th>Version</th><th>Description</th></tr></thead><tbody><tr><td>08-03-2025</td><td>0.1</td><td>Initial Version</td></tr><tr><td>08-03-2025</td><td>0.1</td><td>Current Version</td></tr></tbody></table>

This section will provide all the details to implement and integrate the Scheme discovery use case on to the network



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
{
  "context": {
    "domain": "scheme:oan",
    "action": "search",
    "location": {
      "country": {
        "code": "IND"
      }
    },
    "version": "1.1.0",
    "bap_id": "seeker-bap-example.seekerdomain.com",
    "bap_uri": "https://seeker-bap-example.seekerdomain.com",
    "transaction_id": "d28ec57e-8c8f-4db0-a5aa-73d6563942e1",
    "message_id": "6c8b36e8-7886-4cc8-b3a6-8a3d464fcd6c",
    "timestamp": "2025-03-05T09:15:30Z"
  },
  "message": {
    "intent": {
      "descriptor": {
        "name": "agricultural scheme"
      },
      "item": {
        "tags": [
          {
            "descriptor": {
              "name": "languages"
            },
            "list": [
              {
                "value": "mr"
              },
              {
                "value": "en"
              }
            ]
          }
        ]
      },
      "fulfillment": {
        "stops": [
          {
            "type": "applicable-region",
            "location": {
              "state": {
                "name": "Maharshtra"
              },
              "country": {
                "name": "India"
              }
            }
          }
        ]
      }
    }
  }
}
```



**on\_search**

**on\_search with catalog of results**

* The catalog that comes back has a list of providers.
* Each provider has a list of items.
* Each item is the catalog listing for a resource.
* The name, short\_desc and long\_desc fields contain the name and description of the resource.
* Further, if the resource is a video or a pdf, its mimetype and url are specified in the media field. And what is stored in the media field is the actual link of the resources

BPP can send the scheme details in the catalog. if a BPP wants to collect more detail about the applicant, it can send an xinput in the catalog.

If a BAP receives an xinput in the catalog, optionally, the BAP can render the xinput form on UI. Once the applicant fills and submits the form, BPP can send back relevant schemes to the applicant.

```
{
  "context": {
    "domain": "scheme:oan",
    "action": "on_search",
    "location": {
      "country": {
        "code": "IND"
      }
    },
    "version": "1.1.0",
    "bap_id": "seeker-bap-example.seekerdomain.com",
    "bap_uri": "https://seeker-bap-example.seekerdomain.com",
    "transaction_id": "d28ec57e-8c8f-4db0-a5aa-73d6563942e1",
    "message_id": "6c8b36e8-7886-4cc8-b3a6-8a3d464fcd6c",
    "timestamp": "2025-03-05T09:15:30Z"
  },
  "message": {
    "catalog": {
      "providers": [
        {
          "id": "p1",
          "descriptor": {
            "name": "SchemeFinder",
            "short_desc": "A Scheme Discovery and Application Service helps users discover",
            "long_desc": "The provider has ....",
            "images": [
              {
                "url": "https://image_url"
              }
            ]
          },
          "categories": [
            {
              "id": "c1",
              "descriptor": {
                "code": "Financial-Assistance",
                "name": "Financial Assistance"
              }
            },
            {
              "id": "c2",
              "descriptor": {
                "code": "Housing-Infrastructure",
                "name": "Housing Infrastructure"
              }
            },
            {
              "id": "c3",
              "descriptor": {
                "code": "Agriculture-Rural-Development",
                "name": "Agriculture Rural Development"
              }
            },
            {
              "id": "c4",
              "descriptor": {
                "code": "Education-Skill-Development",
                "name": "Education & Skill Development"
              }
            }
          ],
          "fulfillments": [
            {
              "id": "f1",
              "type": "Direct Benefit Transfer (DBT)" // Cash or subsidy is transferred directly to the beneficiary's bank account.
            },
            {
              "id": "f2",
              "type": "Subsidized Services" // Discounts on specific services such as housing
            },
            {
              "id": "f3",
              "type": "Employment & Placement" //Job guarantees, apprenticeships, or employment-linked benefits.
            }
          ],
          "locations": [
            {
              "id": "l1",
              "city": {
                "name": "Nashik",
                "code": "Nashik"
              }
            },
            {
              "id": "l2",
              "city": {
                "name": "Aurangabad",
                "code": "Aurangabad"
              }
            }
          ],
          "items": [
            {
              "id": "i1",
              "descriptor": {
                "name": "Ayushman Bharat Yojana",
                "short_desc": "Pradhan Mantri Jan Arogya Yojana (PM-JAY) is implemented to reduce the financial burden on poor and vulnerable groups arising out of catastrophic hospital episodes and ensure their access to quality health services.",
                "long_desc": "1) The scheme will be cashless & paperless at public hospitals and empanelled private hospitals. 2) The beneficiaries will not be required to pay any charges for the hospitalization expenses. \n3) The benefit also includes pre and post-hospitalization expenses. \n4) Medical and hospitalization expenses for almost all secondary care and most of the tertiary care procedures \n5) a benefit cover of ₹ 500,000 per family per year (on a family floater basis). Note: Benefit cover of ₹ 5 lakhs per family per year (on a family floater basis)"
              },
              "tags": [
                {
                  "display": true,
                  "descriptor": {
                    "code": "target-beneficiary-type",
                    "name": "Target beneficiary type"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "Ind",
                        "name": "Individual"
                      },
                      "display": true
                    }
                  ]
                },
                {
                  "display": true,
                  "descriptor": {
                    "code": "funding-model",
                    "name": "Funding model"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "PM000024",
                        "name": "Central Sector Scheme"
                      },
                      "display": true
                    }
                  ]
                },
                {
                  "display": true,
                  "descriptor": {
                    "code": "required-docs",
                    "name": "Required documents"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "POR",
                        "name": "Proof of Residence"
                      },
                      "display": true
                    },
                    {
                      "descriptor": {
                        "code": "POI",
                        "name": "Proof of Identity"
                      },
                      "value": "Aadhaar Card",
                      "display": true
                    },
                    {
                      "descriptor": {
                        "code": "POC",
                        "name": "Proof of caste",
                        "short_description": "optional"
                      },
                      "display": true
                    }
                  ]
                },
                {
                  "display": true,
                  "descriptor": {
                    "code": "demographic-eligibility",
                    "name": "Demographic eligibility"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "SOCR",
                        "name": "State of Current Residence"
                      },
                      "value": "Maharashtra, Karnataka",
                      "display": true
                    }
                  ]
                },
                {
                  "display": true,
                  "descriptor": {
                    "code": "additional-eligibility",
                    "name": "Additional eligibility"
                  },
                  "list": [
                    {
                      "descriptor": {
                        "code": "CT0001TU",
                        "name": "Has SECC Registration Number"
                      },
                      "value": "Yes",
                      "display": true
                    }
                  ]
                }
              ],
              "category_ids": [
                "c1"
              ],
              "location_ids": [
                "l1"
              ],
              "fulfillment_ids": [
                "f2"
              ],
              "xinput": {
                  "required": false,
                  "head": {
                      "descriptor": {
                          "name": "Details Form"
                      },
                      "index": {
                          "min": 0,
                          "cur": 0,
                          "max": 3
                      },
                      "headings": [
                          "Personal Details",
                          "Educational Details",
                          "Financial Details",
                          "Review & Submit"
                      ]
                  },
                  "form": {
                      "mime_type": "text/html",
                      "url": "https://6vs8xnx5i7.scheme-finder.co.in/schems/xinput/formid/a23f2fdfbbb8ac402bfd54f",
                      "resubmit": false,
                      "auth": {
                          "descriptor": {
                              "code": "jwt"
                          },
                          "value": "eyJhbGciOiJIUzI.eyJzdWIiOiIxMjM0NTY3O.SflKxwRJSMeKKF2QT4"
                      }
                  }
              }
            }
          ]
        }
      ]
    }
  }
}
```
