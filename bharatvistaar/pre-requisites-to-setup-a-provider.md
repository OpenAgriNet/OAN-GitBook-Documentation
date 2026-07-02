# Prerequisites to Set Up a Provider on BharatVISTAAR

## Infrastructure Prerequisites

| Requirement | Details |
| :--- | :--- |
| Operating System | Ubuntu 22.04 LTS (recommended), Debian, macOS, or WSL2 on Windows |
| Infrastructure | Cloud VM with SSH access; minimum 2 GB RAM recommended |
| Docker | Latest stable version of Docker and Docker Compose |
| Domain | A public FQDN or subdomain pointing to the server (e.g., bpp.yourorg.in) |
| SSL / HTTPS | Valid TLS certificate (Let's Encrypt or equivalent); HTTPS mandatory |
| Reverse Proxy | NGINX or Caddy configured to route traffic to the adapter ports |
| Ports | Port 8080 (or configured equivalent) accessible; firewall rules updated accordingly |

## Software Prerequisites

* **Beckn-ONIX Protocol Server**: the reference middleware adapter that connects internal systems to the BharatVISTAAR network, handling protocol compliance, message signing, and schema validation. Installed via the Beckn-ONIX repository and installation script.
* **Ed25519 key pair**: generated automatically during installation, used to sign every outgoing message. The public key must later be submitted to the BharatVISTAAR registry; the private key must be stored securely and never shared or committed to version control.

### Configuration Values Needed During Installation

| Value | Description |
| :--- | :--- |
| Participant Type | BPP |
| Subscriber ID | The organization's public domain/subdomain |
| Subscriber URI | The HTTPS endpoint corresponding to the subscriber ID |
| Registry URL | Provided by the BharatVISTAAR onboarding team after sandbox access is granted |

## Backend Readiness

Before integration testing can begin, the provider's internal systems must be ready to:
* Expose an HTTPS webhook the Beckn-ONIX adapter can call for incoming search requests
* Parse the structured Beckn request context and message body
* Map incoming parameters to internal service or database queries
* Return responses as a valid on_search structure conforming to the relevant Layer 2 schema

## Organizational Information for Onboarding Request

Once the protocol server is running, the following must be submitted by email to oansocial@coss.org.in to request sandbox access:

| Field | Description |
| :--- | :--- |
| Organization Name | Legal entity name as registered |
| Contact Person | Full name of primary technical contact |
| Email | Official organizational email address |
| Phone Number | Contact number for the technical POC |
| Purpose | Brief description of intended use case(s) |
| Address | Registered address of the organisation |
| Participant Type | Seeker (BAP), Provider (BPP), or both |
| FQDN | Publicly accessible domain of the instance |
| Public Key | Ed25519 public key generated during Beckn-ONIX installation |

## Security and Compliance Baseline

These are not setup steps per se, but standing requirements that must be in place before going live:
* All endpoints must be HTTPS — HTTP endpoints are rejected by the network
* Every response must be signed with the provider's Ed25519 private key (handled automatically by Beckn-ONIX)
* Responses must conform to the Beckn core schema and the applicable Layer 2 config
* No Aadhaar numbers, bank details, or other PII in response fields unless explicitly required by the use case schema
* Backend webhook access must be restricted to requests from the provider's own Beckn-ONIX adapter only
