# Steps to Install a BPP (Beckn Provider Platform)

### Step 1: Confirm Prerequisites Are in Place
Before installation, the server environment must meet the infrastructure requirements (OS, Docker, public domain, TLS certificate, reverse proxy, accessible ports) covered separately in the BharatVISTAAR prerequisites.

### Step 2: Install Beckn-ONIX
Clone the Beckn-ONIX repository and run the installation script:
```bash
git clone https://github.com/beckn/beckn-onix.git
cd beckn-onix/install
./beckn-onix.sh
```

### Step 3: Provide Installer Configuration Values
When prompted by the installer, supply the following:

| Prompt | Value to Provide |
| :--- | :--- |
| Participant Type | BPP |
| Subscriber ID | Public domain/subdomain (e.g., bpp.yourorg.in) |
| Subscriber URI | HTTPS endpoint (e.g., https://bpp.yourorg.in) |
| Registry URL | Provided by the BharatVISTAAR onboarding team after sandbox access is granted |

During this step, the installer also generates an Ed25519 key pair for message signing. The public key must be submitted to the BharatVISTAAR registry when requesting onboarding (Step 6 below); the private key must be stored securely and never shared or committed to version control.

### Step 4: Configure the NGINX Reverse Proxy
Route HTTPS traffic to the BPP adapter. Sample configuration:
```nginx
server {
    listen 443 ssl;
    server_name bpp.yourorg.in;

    ssl_certificate     /etc/letsencrypt/live/bpp.yourorg.in/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/bpp.yourorg.in/privkey.pem;

    location / {
        proxy_pass         http://localhost:8080/;
        proxy_set_header   Host $host;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
    }
}
```

### Step 5: Select a Deployment Mode

| Mode | Config Path | Use When |
| :--- | :--- | :--- |
| BPP-Only Mode | `config/onix-bpp/` | Deploying only a Provider Platform |
| Combined BAP+BPP | `config/onix/` | Single instance serving both Seeker and Provider roles |
| Local Development | `config/local-simple.yaml` | Testing locally without cloud infrastructure |

BPP-Only mode is recommended for production; Combined mode is acceptable for development and testing.

### Step 6: Submit Onboarding Request
Email oansocial@coss.org.in with the subject line "Request for Sandbox Access – [Organisation Name]," including organization name, contact person, email, phone number, purpose, address, participant type, FQDN, and the Ed25519 public key generated in Step 3.

### Step 7: Receive Sandbox Credentials
After review, the OAN onboarding team will register the subscriber ID and public key in the staging registry, then share the sandbox registry URL, gateway URL, and test credentials by email.

### Step 8: Install Layer 2 Configuration
Each use case domain (e.g., agriculture:schemes, agriculture:advisory) requires its own Layer 2 config file. Install using:
```bash
cd beckn-onix/layer2
./download_layer_2_config_bpp.sh
```

When prompted, provide the URL of the relevant Layer 2 config file shared by the BharatVISTAAR team. The Layer 2 config must be downloaded and installed for every use case domain the provider plans to support, before transacting on the network.

### Step 9: Configure the Core Adapter
Update the adapter configuration (typically `config/onix-bpp/config.yaml`) to reflect the provider's subscriber ID and Layer 2 schema location:

```yaml
appName: "bharatvistaar-bpp"
log:
  level: info
http:
  port: 8080
pluginManager:
  root: ./plugins
modules:
  - name: bppTxnReceiver
    path: /bpp/receiver/
    handler:
      type: std
      role: bpp
      subscriberId: bpp.yourorg.in    # Must match registry entry
    plugins:
      cache:
        id: cache
        config:
          addr: localhost:6379
      schemaValidator:
        id: schemav2validator
        config:
          type: url
          location: <layer2-config-url>  # URL from Step 8
    steps:
      - validateSign
      - validateSchema
      - forwardToBackend
```

### Step 10: Configure Backend Routing
Point the adapter to the internal backend webhook that will handle incoming search requests:
```yaml
# config/routing.yaml (BPP routing config)
routes:
  - action: search
    url: https://internal.yourorg.in/beckn/search
    method: POST
    timeout: 5000    # milliseconds
```

The backend webhook must respond within the configured timeout. For use cases requiring asynchronous data fetch (e.g., PMFBY claim status), implement the Beckn callback pattern and return an acknowledgment immediately rather than waiting on the full result.

### Step 11: Implement and Test the Backend Integration
With routing configured, implement the backend logic to:
* Receive and parse the structured Beckn search request
* Map request parameters to internal service/database queries
* Transform results into a valid `on_search` response conforming to the installed Layer 2 schema
* Return the response synchronously, or via callback for async flows

### Step 12: Validate Against the Sandbox
Using the sandbox credentials from Step 7:
* Run end-to-end flow tests using the test BAP provided by the BharatVISTAAR team
* Confirm `on_search` responses validate against the Layer 2 schema
* Confirm signature verification passes on all responses
* Address any schema or latency issues flagged during testing
