# Setup Wildcard SSL Certificates with Traefik

https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs

## Requirements
- You need to have a domain name and a DNS provider that supports wildcard subdomains.
- You need to use [dnsChallenge](https://doc.traefik.io/traefik/https/acme/#dnschallenge) in Traefik to get wildcard certificates from Let's Encrypt.
- You need to use one of the supported DNS providers.
  - Each provider needs environment variables to be set in the Traefik configuration.
  - You can find the required variables in the [official Traefik documentation](https://doc.traefik.io/traefik/https/acme/#providers) .

---

## Configuration

### Set Coolify Domain

- Coolify Web UI -> Settings -> Configuration -> General
  - URL: https://coolify.< dev-domain-name >
  - Name: coolify-dev
  - Redirect HTTP to HTTPS: Enabled
  - Instance Timezone: UTC
    - Select your timezone
  - Do not change Instance public IPv4
  - Do not change Instance public IPv6
  - May give a DNS validation error, continue
  - Your IPv4 may not be reachable from public internet because of network structure.

### Set DNS Servers

- Settings -> Configuration -> Advanced
  - DNS Validation
    - DNS Validation: Disabled
  - Custom DNS Servers:
    - Enter IPs of your domain's name servers
  - API and MCP
    - Allowed IPs for API Access
      - `< vps-ip-address >`
      - `< virtual-m-ip >`
      - seperated with a comma
    - API Access: Enabled
  - DNS Validation
    - DNS Validation: Enabled
  - Interface and telemetry
    - Navigation: SPA navigation

---

### Setup General Settings for the Virtual Machine
- Coolify Web User Interface:
  - dev-domain-name: devblogserver.my-domain.com
  - vm-server-name: devblogserver
  - Servers -> < vm-server-name > -> Settings -> General -> Connection
    - Name: < vm-server-name > (initial value: localhost)
    - Wildcard Domain: https://< dev-domain-name >
    - IP Address/Domain: host.docker.internal
    - SSH user: root
    - Port: < vm-ssh-port > ( default: 22 )
    - Server timezone: Select your timezone
    - Save changes
  - Restart Proxy
    - Actions -> Start/Restart Proxy
  - Wait a few minutes, and close the "Proxy Startup Logs" form.
  - Refresh Page

### Setup Sentinel for the Virtual Machine
- Servers -> < servername > -> Sentinel
  - Coolify URL
    - http://coolify.< dev-domain-name >
    - Sentinel Enabled
    - Click Sync button if Sentinel is out of Sync
  - Save Changes

---

#### Prepare username & password for Traefik Basic Authentication

https://coolify.io/docs/knowledge-base/proxy/traefik/basic-auth#how-to-generate-user-password

On Developer PC
```bash
cd ~
```

```bash
sudo apt install apache2-utils
```

```bash
htpasswd -c users.txt traefikuser
```

Enter a password you choose twice. This will save your traefik user name and encrypted password to user.txt file. Open the file and copy the row you see. We will enter it below as  `Traefik Dynamic Configuration` value for `traefik.http.middlewares.traefik-basic-auth.basicauth.users` in single quotes .

### Edit Traefik Configuration on VM
- dev-domain-name: devblogserver.my-domain.com
- vm-server-name: devblogserver
- Coolify Web User Interface:
  - Servers -> < vm-server-name > -> Platform -> Proxy -> 
    - Routing behavior
      - Generated labels: Labels for the active proxy only
      - Unknown requests: Return the default 503 response
    - Traefik configuration
      - Add or modify the following sections: 
        - ( ..... represents the ommited sections )
        - < variable-inside > Enter the value of the variable suits to your setup without the angle brackets .
        - Remove section networks if exists
        - Remove section services.traefik.networks if exists
        - Get values from your DNS Provider for
          - For Hetzner
            - DNS: hetzner
            - EMAIL: the e-mail used in DNS provider registration
            - HETZNER_API_TOKEN
          - For detailed information please read
            - Lego Dns Provider list: https://go-acme.github.io/lego/dns/

```yaml
name: coolify-proxy

services:
  traefik:
    container_name: coolify-proxy
    image: 'traefik:v3.7'
    restart: unless-stopped
    environment:
      - 'TZ=Universal'
      - 'EMAIL=< lego-email >'
      - 'DNS=< lego-service-provider-dns >'
      - '< lego-service-provider-env-var >=< lego-service-provider-api-token >'
    extra_hosts:
      - 'host.docker.internal:host-gateway'
    security_opt:
      - 'no-new-privileges=true'
    healthcheck:
      test: 'wget -qO- https://traefik.< dev-domain-name >/ping || exit 1'
      interval: 10s
      timeout: 4s
      retries: 5
      start_period: 6s
    ports:
      - '80:80'
      - '443:443'
      - '443:443/udp'
      - '8080:8080'
      - '3000:3000'
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock:ro'
      - '/data/coolify/proxy/:/traefik'
    command:
      - '--global.checkNewVersion=true'
      - '--ping=true'
      - '--ping.entryPoint=https'
      - '--ping.terminatingStatusCode=204'
      - '--api.dashboard=true'
      - '--api.insecure=false'
      - '--entrypoints.http.address=:80'
      - '--entrypoints.https.address=:443'
      - '--entrypoints.http.http.encodequerysemicolons=true'
      - '--entrypoints.https.http.encodequerysemicolons=true'
      - '--entrypoints.http.http2.maxConcurrentStreams=250'
      - '--entrypoints.https.http2.maxConcurrentStreams=250'
      - '--entrypoints.https.http3'
      - '--providers.docker=true'
      - '--providers.docker.exposedbydefault=false'
      - '--providers.docker.endpoint=unix:///var/run/docker.sock'
      - '--providers.docker.network=coolify'
      - '--providers.file.directory=/traefik/dynamic/'
      - '--providers.file.watch=true'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge=true'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.provider=hetzner'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.delaybeforecheck=60'
      - '--certificatesresolvers.letsencrypt.acme.storage=/traefik/acme.json'
      - '--certificatesresolvers.letsencrypt.acme.email=< lego-email >'
      - '--certificatesresolvers.letsencrypt.acme.certificatesDuration=1080'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.disablePropagationCheck=false'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[0]=213.133.100.102:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[1]=213.239.204.242:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[2]=193.47.99.4:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[3]=1.1.1.1:53'
      - '--certificatesresolvers.letsencrypt.acme.dnschallenge.resolvers[4]=8.8.8.8:53'
      - '--accesslog.format=json'
      - '--global.sendAnonymousUsage=false'
    labels:
      - traefik.enable=true
      - coolify.managed=true
      - coolify.proxy=true
```  

Save Changes

Lego Dns Provider list: https://go-acme.github.io/lego/dns/

https://doc.traefik.io/traefik/expose/docker/

### Add Traefik Dynamic Configuration

- Coolify UI -> Servers -> < vm-server-name > -> Platform -> Proxy -> Dynamic Configurations
  - +Add
    - Filename: traefik-dashboard.yml
```yaml
http:
  middlewares:
    authentication:
      basicAuth:
        realm: traefik-dashboard
        users:
          - '< traefik-user >:< traefik-encrypted-password >'
    content-type:
      contenttype: true
  routers:
    ping-https:
      entryPoints:
        - https
      middlewares:
        - content-type
      rule: 'Host(`traefik.< dev-domain-name >`)&&PathPrefix(`/ping`)'
      service: ping@internal
      tls:
        certResolver: letsencrypt
    traefik-dashboard-https:
      entryPoints:
        - https
      middlewares:
        - authentication
        - content-type
        - gzip
      rule: 'Host(`traefik.< dev-domain-name >`)&&(PathPrefix(`/api`)||PathPrefix(`/dashboard`))'
      service: api@internal
      tls:
        certResolver: letsencrypt
        domains:
          -
            main: < dev-domain-name >
            sans:
              - '*.< dev-domain-name >'
```    

- Save
- Close Form
- Actions -> Restart Proxy
  - Confirm ... -> Restart Proxy
  - Wait for the process to start ...
    - "Successfully started coolify-proxy."
    - Close Proxy Startup Logs
    - Refresh the page

---

### Check https://coolify.< dev-domain-name >

- Login
- Change your password. 

Check https://traefik.< dev-domain-name >/ping

If it returns "OK", Traefik ping is OK .

https://traefik.< dev-domain-name >/dashboard/

Please enter the last `/` (forward-slash) to the URL, it is important.

Asks for username and password. Enter username (< traefik-user >) & password you created during "Traefik Basic Authentication" step.

- Close the tab that is using the IP to connect Coolify.

- You can close ports 8000, 6001, 6002 on firewall after a successful login via domain name.

---

Later , after adding a App (Resource), read https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs#normal for setting up Traefik settings for your application.

---

### Continue with : Send e-mail with Resend [coolify/coolify-email-resend.md](../install-personal-blog-dev.md#send-e-mail-with-resend-coolifycoolify-email-resend)

---


## Troubleshooting

### Error: ssh: connect to host < servername >.< domain-name > port 22: Connection refused

- Coolify Web User Interface:
  - Servers -> < servername > -> Configuration -> General -> General
    - IP Address/Domain: Change value as below
      - host.docker.internal
      - Save
### Deploy command ends in error. Can not connect to MongoDB with Mongo URL (Internal)
  - If Dockerfile includes Node build commands, it wants to connect to the database, but it can not connect during build time. So moving build steps after image deployment gets rid of the error.
  - As a second option, you can enable Mongo URL (Public), and use it as DATABASE_URL
  - This project's current setup moved build step after deployment, runs in container.
