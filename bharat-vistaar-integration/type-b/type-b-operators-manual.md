# Type B - Operator's Manual

_A step-by-step, screenshot-guided walkthrough for semi-technical teams_

Covers all five stages: Infrastructure · Beckn Provider · Network · API Wrapper · AI Layer

Installation reference: Beckn-ONIX v0.6.0 User Guide ([github.com/beckn/beckn-onix](https://github.com/beckn/beckn-onix))

### Before You Begin <a href="#before-you-begin" id="before-you-begin"></a>

This manual is written for a semi-technical operator: someone comfortable typing commands into a Linux terminal and editing a text file, but who does not need to be a protocol expert. Each stage tells you the goal, the exact commands to run, what a successful screen looks like, and what to check if it doesn't.

The screenshots in this manual are representative — they show what your terminal or editor should look like at each point. Names like `beckn.agri.state.gov.in` and the IP `203.0.113.42` are placeholders; substitute your State's actual domain and server details.

#### What you need in hand <a href="#what-you-need-in-hand" id="what-you-need-in-hand"></a>

1. A Linux server (Ubuntu 22.04 recommended) with at least 2 CPU cores and 8 GB RAM, and SSH access to it.
2. A public domain name for your node (request it from your NIC/DNS administrator).
3. Authority to raise firewall requests and to correspond with the BharatVistaar team.
4. Roughly half a day for stages 1–3. Stage 4 is a development task and takes longer.

{% hint style="info" %}
Keep a plain notebook (or a shared doc) where you record: your domain, server IP, the subscriber ID you choose, and who approved what on which date. Every later stage asks for these.
{% endhint %}

### Stage 1 — Infrastructure Readiness <a href="#stage-1--infrastructure-readiness" id="stage-1--infrastructure-readiness"></a>

**Goal of this step:** a server that the outside world can reach at a fixed, named, secure address.

#### 1.1 Verify the server meets the minimum <a href="#id-11-verify-the-server-meets-the-minimum" id="id-11-verify-the-server-meets-the-minimum"></a>

Log in to the server over SSH and run the three commands below. You are confirming: at least 2 CPUs, roughly 8 GB of memory, and comfortable free disk space.

<figure><img src="../../.gitbook/assets/Picture 1.png" alt=""><figcaption></figcaption></figure>

_Figure 1: Checking CPU, memory, and disk on the provisioned server_

#### 1.2 Point TWO subdomain names at the server <a href="#id-12-point-two-subdomain-names-at-the-server" id="id-12-point-two-subdomain-names-at-the-server"></a>

The Beckn Provider exposes two web endpoints, so it needs two names. Ask your DNS administrator to create two A records pointing at the same server IP: a network endpoint (e.g. `bpp.agri.state.gov.in`) — the address the BharatVistaar network calls — and a client endpoint (e.g. `bpp-client.agri.state.gov.in`) — the address your own wrapper software calls. Once they confirm, verify each from any machine:

<figure><img src="../../.gitbook/assets/picture 2.png" alt=""><figcaption></figcaption></figure>

_Figure 2: nslookup must return your server's public IP against each subdomain_

{% hint style="info" %}
Watch out ! DNS changes can take up to a few hours to spread. If `nslookup` shows nothing or an old IP, wait and retry before raising it back with the DNS team. Check BOTH names.
{% endhint %}

#### 1.3 Install SSL certificates and the Nginx reverse proxy <a href="#id-13-install-ssl-certificates-and-the-nginx-reverse-proxy" id="id-13-install-ssl-certificates-and-the-nginx-reverse-proxy"></a>

Your security/hosting team installs CA-issued certificates for both subdomains (Let's Encrypt via Certbot, or an NIC-issued certificate — both are fine; self-signed is not), and configures Nginx as a reverse proxy: the network endpoint forwards to local port 6002, the client endpoint to port 6001. These are the ports the BPP software will listen on in Stage 2:

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

_Figure 3: Nginx forwards each https subdomain to the right local port (6002 network, 6001 client)_

Then confirm the certificate on the network endpoint is trusted:

<figure><img src="../../.gitbook/assets/Picture 4.png" alt=""><figcaption></figcaption></figure>

_Figure 4: "SSL certificate verify ok" confirms the lock is trusted_

#### 1.4 Open the firewall <a href="#id-14-open-the-firewall" id="id-14-open-the-firewall"></a>

1. Inbound: allow HTTPS (port 443) from the internet, and SSH (22) from your office network only.
2. Outbound: allow HTTPS to the BharatVistaar Registry and Gateway addresses (the BharatVistaar team will share these).
3. Record the change-request number in your notebook.

Stage 1 is complete when

Both subdomains resolve to your server, both load over `https://` with a valid padlock, Nginx forwards them to ports 6002 and 6001, and the firewall request is approved.

### Stage 2 — Beckn Provider Installation <a href="#stage-2--beckn-provider-installation" id="stage-2--beckn-provider-installation"></a>

**Goal of this step:** the Beckn Provider software (your "receptionist") installed with the official Beckn-ONIX installer and registered on the network.

We follow the Beckn-ONIX v0.6.0 User Guide (`github.com/beckn/beckn-onix`, `docs/user_guide.md`). Its installer script asks a handful of questions and then sets up everything as Docker containers. Have your notebook from Stage 1 ready — the installer will ask for the names you created there.

#### 2.1 Install Docker and fix permissions (one-time) <a href="#id-21-install-docker-and-fix-permissions-one-time" id="id-21-install-docker-and-fix-permissions-one-time"></a>

<figure><img src="../../.gitbook/assets/Pciture 5.png" alt=""><figcaption></figcaption></figure>

_Figure 5: Docker installed; the groupadd/usermod lines prevent the most common installer failure_

{% hint style="info" %}
Watch out ! After the usermod command you MUST log out and back in (or open a new shell). Skipping this causes a docker permission denied error when the installer runs — the single most common failure at this stage.
{% endhint %}

#### 2.2 Download Beckn-ONIX <a href="#id-22-download-beckn-onix" id="id-22-download-beckn-onix"></a>

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

_Figure 6: Cloning the beckn-onix repository and entering the install folder_

#### 2.3 Run the installer and answer its questions <a href="#id-23-run-the-installer-and-answer-its-questions" id="id-23-run-the-installer-and-answer-its-questions"></a>

The installer is interactive. You are JOINING an existing network (BharatVistaar), and installing the BPP component. Answer with the two subdomains from Stage 1, the webhook address where your wrapper (Stage 4) will listen, and the registry subscription endpoint the BharatVistaar team gives you — note it ends in `/subscribers`:

<figure><img src="../../.gitbook/assets/Pciture 6.png" alt=""><figcaption></figcaption></figure>

_Figure 7: The five answers that matter. Substitute your own names; the registry endpoint comes from the BharatVistaar team_

{% hint style="info" %}
Subscriber ID convention: use your network-endpoint URL without the `https://` part — e.g. `bpp.agri.state.gov.in`. The registry may enforce uniqueness, so agree it with the BharatVistaar team beforehand. The installer generates your signing key pair and registers you with the registry automatically.
{% endhint %}

#### 2.4 Confirm the adapter is running <a href="#id-24-confirm-the-adapter-is-running" id="id-24-confirm-the-adapter-is-running"></a>

<figure><img src="../../.gitbook/assets/Pciture 7.png" alt=""><figcaption></figcaption></figure>

_Figure 8: bpp-client (6001) and bpp-network (6002) both Up — matching the Nginx ports from Stage 1_

#### 2.5 Install the Layer 2 domain configuration <a href="#id-25-install-the-layer-2-domain-configuration" id="id-25-install-the-layer-2-domain-configuration"></a>

The core install alone cannot transact. Each Beckn domain has a Layer 2 configuration file — the rulebook agreed for that domain by the network. Ask the BharatVistaar team for the URL of the agriculture-domain file, then run the download script:

<figure><img src="../../.gitbook/assets/Figure 6.png" alt=""><figcaption></figcaption></figure>

_Figure 9: download\_layer\_2\_config\_bpp.sh fetches the domain rulebook into the BPP container_

{% hint style="info" %}
Watch out! Without the Layer 2 config, transactions are rejected even though the containers look healthy. If test transactions in Stage 3 fail mysteriously, check this first.
{% endhint %}

Stage 2 is complete when

The installer finished without errors, `docker ps` shows the bpp-client and bpp-network containers Up, and the Layer 2 config for your domain is installed.

### Stage 3 — Network Enablement <a href="#stage-3--network-enablement" id="stage-3--network-enablement"></a>

**Goal of this step:** your node approved, listed in the network's official Registry, and proven working end-to-end.

#### 3.1 Know your identity details <a href="#id-31-know-your-identity-details" id="id-31-know-your-identity-details"></a>

The ONIX installer already generated your key pair and registered your details with the registry in Stage 2. Locate and record them — the BharatVistaar team will verify these during approval. They live in the BPP configuration (enter the container with `docker exec -it bpp-client sh` if needed):

<figure><img src="../../.gitbook/assets/Screenshot 2026-07-23 at 10.50.38 AM.png" alt=""><figcaption></figcaption></figure>

_Figure 10: Your identity and keys in the BPP config — share the public key only, never the private key_

{% hint style="info" %}
The private key is the shop's stamp. Anyone who has it can impersonate your State on the network. Keep it on the server only; share nothing but the `signingPublicKey`.
{% endhint %}

#### 3.2 Apply for approval — and get flipped to Subscribed <a href="#id-32-apply-for-approval--and-get-flipped-to-subscribed" id="id-32-apply-for-approval--and-get-flipped-to-subscribed"></a>

1. Write to the Centre/BharatVistaar team requesting onboarding, quoting your department and services.
2. Share three details for whitelisting: your subscriber ID (e.g. `bpp.agri.state.gov.in`), your callback URL (`https://<your domain>/bpp/receiver`), and your public signing key.
3. Registration itself already happened in Stage 2 — but you start in an unsubscribed state. The network facilitator (BharatVistaar team) reviews your details and manually changes your registry status to Subscribed. Only then can you transact.

#### 3.3 Verify your Registry entry <a href="#id-33-verify-your-registry-entry" id="id-33-verify-your-registry-entry"></a>

Once they confirm, look yourself up. The status field is what matters:

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

_Figure 11: Registry lookup returning your entry with status SUBSCRIBED — you are live_

#### 3.4 Run the end-to-end test <a href="#id-34-run-the-end-to-end-test" id="id-34-run-the-end-to-end-test"></a>

With the BharatVistaar team on a call, fire one test transaction (they will typically trigger a discover/search aimed at your node). Watch your logs — a successful round trip looks like this:

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

_Figure 12: A search arrives, goes to your webhook, and the signed on\_search goes back out_

Stage 3 is complete when

Your Registry entry shows SUBSCRIBED and a witnessed test transaction completes with valid signatures both ways.

### Stage 4 — API Wrapper Development <a href="#stage-4--api-wrapper-development" id="stage-4--api-wrapper-development"></a>

**Goal of this step:** a small translation service (the "adapter plug") that lets your existing State APIs answer Beckn questions without being modified.

This is the main development stage, done by your developers. The wrapper is a small web service that sits between the Beckn Provider (Stage 2) and your existing backend. Its whole job fits in five moves, visible in the code below: acknowledge, translate in, call your API, translate out, reply.

#### 4.1 Understand the shape of a wrapper <a href="#id-41-understand-the-shape-of-a-wrapper" id="id-41-understand-the-shape-of-a-wrapper"></a>

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

_Figure 13: A minimal wrapper: \~20 lines per service. Your existing API (STATE\_API) is called as-is_

{% hint style="info" %}
One wrapper endpoint per Beckn action you support. Start with just discover/search — that alone makes your service findable and answerable on the network. Add select, init, confirm later only if the service takes bookings.
{% endhint %}

#### 4.2 The three rules the wrapper must follow <a href="#id-42-the-three-rules-the-wrapper-must-follow" id="id-42-the-three-rules-the-wrapper-must-follow"></a>

1. Reply ACK immediately, then do the work. Beckn is asynchronous — the answer goes back separately as an `on_...` message.
2. Answer within the network's time limit (TTL). If your backend is slow, cache common answers.
3. Never change the backend. All translation, field mapping and error handling lives in the wrapper.

#### 4.3 Test the wrapper locally <a href="#id-43-test-the-wrapper-locally" id="id-43-test-the-wrapper-locally"></a>

Feed the wrapper a sample Beckn request (sample Postman collections are available at `github.com/beckn/beckn-sandbox` under artefacts) and confirm your real service's answer comes back through, converted:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

_Figure 14: The wrapper ACKs instantly, then the adapter logs show the converted catalog going out_

Stage 4 is complete when

For each exposed service, a sample Beckn request produces a correct, Beckn-formatted answer within the time limit — using your real backend data.

### Stage 5 — AI Layer Enablement <a href="#stage-5--ai-layer-enablement" id="stage-5--ai-layer-enablement"></a>

**Goal of this step:** the BharatVistaar AI assistant reliably recognises when a farmer's question is meant for your services.

#### 5.1 Write one briefing sheet per service <a href="#id-51-write-one-briefing-sheet-per-service" id="id-51-write-one-briefing-sheet-per-service"></a>

This stage is documentation, not coding — but its quality decides how often farmers actually reach your service. For each service, prepare a descriptor answering: what it does, what it covers, sample questions, and what it does not cover:

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

_Figure 15: A service descriptor: plain, specific, with real sample questions in farmers' words_

{% hint style="info" %}
The sample\_questions matter most. Collect 10–15 real questions from your call-centre or extension staff, in the languages farmers actually use. Real phrasing beats formal descriptions.
{% endhint %}

#### 5.2 Hand over and tune together <a href="#id-52-hand-over-and-tune-together" id="id-52-hand-over-and-tune-together"></a>

1. Send the descriptors to the BharatVistaar AI team.
2. They load these into the AI's service catalog and routing prompts.
3. Joint testing: they fire representative farmer questions; you confirm the answers your service returns are the right ones.
4. Iterate — usually 2–3 rounds — until routing accuracy is acceptable to both sides.

Stage 5 is complete when

Test questions in each supported language consistently reach the correct service and come back with correct answers.

### Quick Reference — One Page <a href="#quick-reference--one-page" id="quick-reference--one-page"></a>

| Stage | You are done when…                                    | Key command / artefact                            |
| ----- | ----------------------------------------------------- | ------------------------------------------------- |
| 1     | Domain resolves, padlock valid, firewall open         | `curl -vI https://<your-domain>`                  |
| 2     | bpp-client + bpp-network Up; Layer 2 config installed | `./beckn-onix.sh` (join network → BPP)            |
| 3     | Registry shows status SUBSCRIBED; test txn passes     | registry lookup + witnessed discover/on\_discover |
| 4     | Sample Beckn request answered from real backend       | wrapper service (ACK → translate → call → reply)  |
| 5     | Farmer-style test questions route correctly           | descriptor files + joint tuning rounds            |

Where to get help: the Beckn-ONIX v0.6.0 User Guide (`github.com/beckn/beckn-onix` → `docs/user_guide.md`) covers deployment diagrams, Nginx samples (Appendix B), subdomain setup (Appendix A), and upgrade steps. For registry and whitelisting issues, contact the BharatVistaar onboarding team.
