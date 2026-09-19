# Set Hetzner API Token if you use Hetzner

- Hetzner Console:
  - Create an API token in the Hetzner Console → choose Project → Security → API Tokens.
    - Generate API Token
      - Description: HETZNER_API_TOKEN
      - Permissions: Read&Write
      - Generate API Token
      - Copy and securely store the value somewhere

- Coolify Web User Interface:
  - Keys & Tokens -> Cloud Tokens
    - New Token
      - Provider Hetzner: 
        - Token Name: HETZNER_API_TOKEN
        - API Token: < lego-service-provider-api-key >
        - Validate & Add
        - Cloud provider token added successfully.

---

### Continue with: Configure Coolify - Setup Wildcard SSL Certificates with Traefik [coolify/configure-coolify-traefik-vps](../install-personal-blog-prod.md#configure-coolify---setup-wildcard-ssl-certificates-with-traefik-coolifyconfigure-coolify-traefik-vps)  
