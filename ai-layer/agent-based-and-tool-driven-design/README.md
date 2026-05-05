# Agent-Based and Tool-Driven Design

Rather than relying on a single monolithic model, the AI module uses an agent-based architecture. Different agents are responsible for:

* Moderating and validating user queries
* Understanding intent and orchestrating tool calls
* Generating follow-up suggestions where applicable

All meaningful actions, such as fetching weather data, searching advisory content, or checking scheme status, are performed via explicit tools and APIs. The language model’s role is limited to reasoning, routing, and response composition.

This design improves safety and predictability, enables easier auditing and monitoring and allows independent evolution of tools and models

The AI module uses capability-based tools rather than use case–specific logic. Each user query is first classified by intent, and the AgriNet Agent then invokes the appropriate tool with structured parameters. The same tools are reused across deployments like Maha VISTAAR and Bharat VISTAAR, with differences only in which tools are enabled.

#### <mark style="color:green;">Maha VISTAAR</mark>&#x20;

| Use Case                   | Tool Chain / Key Tools                                                    | Purpose of Tool(s)                                                                                                                                              | Typical Parameters                                    |
| -------------------------- | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| Advisory                   | search\_terms → search\_documents → get\_advisory                         | Identifies standard agricultural terminology, retrieves relevant advisory documents, and generates grounded pest, disease, and farming practice recommendations | crop, pest/disease symptoms, crop stage, district     |
| Mandi Prices               | fetch\_agristack\_data OR mandi\_prices                                   | Retrieves real-time commodity prices across mandis using farmer location or user-provided district and commodity                                                | commodity, district / market, date                    |
| Scheme Information         | get\_scheme\_codes → get\_scheme\_info OR get\_multiple\_schemes\_info    | Retrieves verified scheme benefits, eligibility criteria, and application process details; supports multiple scheme retrieval based on farmer need              | scheme category, farmer category, eligibility context |
| Weather Forecast           | fetch\_agristack\_data OR forward\_geocode → get\_weather                 | Retrieves real-time weather forecast using farmer coordinates or location from query                                                                            | location OR latitude/longitude, forecast date         |
| Warehouses (Agri Services) | fetch\_agristack\_data OR forward\_geocode → agri\_services               | Identifies warehouse and storage infrastructure availability based on location and commodity                                                                    | district OR coordinates, commodity, service type      |
| AgriAssistant Contact      | fetch\_agristack\_data OR forward\_geocode → contact\_agricultural\_staff | Retrieves verified contact details of local agricultural extension officers and support staff                                                                   | district OR coordinates, service type                 |
| Maha DBT Status            | get\_scheme\_status                                                       | Retrieves application and benefit disbursement status for MahaDBT schemes (requires authentication context)                                                     | farmer ID / beneficiary ID (auto if authenticated)    |
| Weather Historical         | fetch\_agristack\_data OR forward\_geocode → get\_historical\_weather     | Retrieves historical weather and rainfall trends for planning and analysis                                                                                      | location OR coordinates, date range                   |

#### <mark style="color:green;">Bharat VISTAAR</mark>

| Use Case                   | Tool Chain / Key Tools                                    | Purpose of Tool(s)                                                                                                                              | Typical Parameters                                   |
| -------------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Scheme Information         | search\_terms → search\_documents → scheme\_info          | Identifies scheme-related terminology, retrieves verified scheme documents, and provides scheme benefits, eligibility, and application guidance | scheme name, scheme category, farmer category, state |
| PM-Kisan Grievance Check   | pmkisan\_grievance\_status                                | Retrieves grievance status from PM-Kisan grievance systems and returns current resolution stage                                                 | grievance ID, registered mobile number               |
| PMFBY Status Check         | pmfby\_scheme\_status (OTP validation → status retrieval) | Retrieves crop insurance application and claim status from PMFBY systems with OTP-based authentication validation                               | policy ID, farmer ID, OTP (if required)              |
| SHC Status Check           | shc\_scheme\_status                                       | Retrieves Soil Health Card test processing and report generation status from SHC systems                                                        | SHC ID, registered mobile number                     |
| PM-Kisan Instalment Status | pmkisan\_scheme\_status                                   | Retrieves PM-Kisan instalment disbursement and payment status from official programme systems                                                   | beneficiary ID, registered mobile number             |
| Advisory                   | search\_terms → search\_documents → get\_advisory         | Identifies standard crop and issue terminology, retrieves advisory documents, and provides grounded agricultural recommendations                | crop, pest/disease issue, crop stage, district       |
|                            |                                                           |                                                                                                                                                 |                                                      |

####
