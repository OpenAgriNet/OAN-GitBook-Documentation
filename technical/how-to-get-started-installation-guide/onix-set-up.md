# ONIX: Set Up

## Beckn ONIX Installation Guide (Sandbox / Local Testing)

This guide describes deploying Beckn ONIX components (Registry, Gateway and ONIX Adapter) on a single Ubuntu server for sandbox, proof-of-concept and local testing purposes. It is not intended for production deployments.\
<br>

### Infrastructure Requirements

* Ubuntu OS
* 8 vCPU, 16 GB RAM
* 100 GB free disk space
* Public IP
* Ports 22, 80 and 443 open
* Docker Engine & Docker Compose
* Git, Curl, jq, Nginx, OpenSSL and Certbot

### DNS Configuration

| Host         | Domain               | Port |
| ------------ | -------------------- | ---- |
| Registry     | registry.example.com | 3030 |
| Gateway      | gateway.example.com  | 4030 |
| ONIX Adapter | adapter.example.com  | 8081 |

Create A records for all three subdomains pointing to the same public IP.

### SSL Certificates

You can generate SSL via below steps or you can use your existing/alternatives

\`\`\`\
sudo certbot --nginx \\\
-d registry.example.com \\\
-d gateway.example.com \\\
-d adapter.example.com\
\
sudo certbot renew --dry-run\
\`\`\`

### Nginx Reverse Proxy

Configure three independent virtual hosts:

* registry.example.com  ->  http://127.0.0.1:3030
* gateway.example.com  ->  http://127.0.0.1:4030
* adapter.example.com  ->  http://127.0.0.1:8081

Use a dedicated server block per subdomain instead of path-based proxying.

### Installing Beckn ONIX

Fork the beckn-onix repository to your GitHub account and clone your forked repository on the server

Repository reference: https://github.com/beckn/beckn-onix/

\`\`\`\
git clone https://github.com/\<your-github-user>/beckn-onix.git\
cd beckn-onix/install\
\
chmod +x setup.sh\
./beckn-onix.sh\
\`\`\`\
\
Select:\
\
Set up a network on local machine with local registry and gateway (without Beckn One)\
<br>

The installer creates Registry, Gateway and ONIX Adapter.

### Container Verification

\`\`\`\
docker ps\
\
docker logs \<container-name>\
\`\`\`

### End-to-End Validation

* Open https://registry.example.com in a browser.
* Verify the Registry UI loads successfully.
* Login using username: root and password: root.
* Open https://gateway.example.com in a browser.
* Verify the Gateway UI loads successfully.
* Login using username: root and password: root.
* Verify ONIX Adapter is reachable at https://adapter.example.com.

### Deployment Checklist

☐ Ubuntu server provisioned

☐ Docker and Docker Compose installed

☐ Git, Nginx and Certbot installed

☐ DNS records created

☐ SSL certificates generated

☐ Nginx configured for three subdomains

☐ Repository cloned

☐ ONIX installation completed

☐ Registry container running

☐ Gateway container running

☐ ONIX Adapter container running

☐ Registry UI accessible

☐ Gateway UI accessible

☐ Logged into Registry using root/root

☐ Logged into Gateway using root/root

<br>
