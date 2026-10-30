# Setup Wildcard SSL Certificates with Traefik

https://coolify.io/docs/knowledge-base/proxy/traefik/wildcard-certs

## Requirements
- Domain name and a DNS provider that supports wildcard subdomains.
- Use [dnsChallenge](https://doc.traefik.io/traefik/https/acme/#dnschallenge) in Traefik to get wildcard certificates from Let's Encrypt.
- Use one of the supported DNS providers.
  - Each provider needs environment variables to be set in the Traefik configuration.
  - You can find the required variables in the [official Traefik documentation](https://doc.traefik.io/traefik/https/acme/#providers) .

---

## Configuration

### Set Coolify Domain

- Open Coolify Web UI
  - URL: http://< vps-ip-address >:8000
    - Security Warning
      - It gives error, because we did not finish setting up https
      - Click `Continue to HTTP Site`

- Coolify Web UI 
  - Settings -> General
    - URL: https://coolify.< prod-domain-name >
    - Name: coolify-vps
    - Redirect HTTP to HTTPS: Enabled
    - Instance Timezone: UTC
      - Select your timezone
    - Network addresses
      - Do not change Instance public IPv4
      - Do not change Instance public IPv6

### Set DNS Servers

- Settings -> Advanced
  - DNS Validation
    - DNS Validation: Disabled
  - Custom DNS Servers:
    - Enter IPs of your domain's name servers
    - Save changes
  - API and MCP
    - Allowed IPs for API Access
      - `< vps-ip-address >`
      - seperated with a comma
    - API Access: Enabled
    - MCP Server: Disabled
  - DNS Validation
    - DNS Validation: Enabled
  - Interface and telemetry
    - Navigation: SPA navigation
    - Choose the others as you wish
---

### Setup General Settings for the Virtual Private Server
- Coolify Web User Interface:
  - Servers -> < vps-server-name > -> General -> Connection
    - Name: < vps-server-name >  (initial value: localhost)
      - server1
    - IP Address/Domain: host.docker.internal
    - SSH user: root
    - Port: < vps-ssh-port > ( default: 22 )
    - Server timezone: Select your timezone
    - Wildcard Domain: `https://< prod-domain-name >`
    - Save changes
  - Actions -> Start/Restart Proxy
    - Confirm ... Restart Proxy
  - Wait a few minutes, and close the "Proxy Startup Logs" form.
  - Refresh Page

### Setup Sentinel for the Virtual Private Server
- Servers -> < servername > -> -> Sentinel
  - Connection -> Coolify URL
    - https://coolify.< prod-domain-name >
    - Click Sync button if Sentinel is out of Sync

---

#### Prepare username & password for Traefik Basic Authentication

https://coolify.io/docs/knowledge-base/proxy/traefik/basic-auth#how-to-generate-user-password

On Developer PC
```bash
$ cd ~
```

```bash
$ sudo apt install apache2-utils
```

```bash
$ htpasswd -c users.txt traefikuser
```

Enter a password you choose twice. This will save your traefik user name and encrypted password to user.txt file. Open the file and copy the row you see. We will enter it below as `Traefik Dynamic Configuration` value for `traefik.http.middlewares.traefik-basic-auth.basicauth.users` in single quotes .

### Edit Traefik Configuration on VPS
- Coolify Web User Interface:
  - Servers -> < servername > -> -> Proxy -> Configuration
    - Routing behavior
      - Generated labels: Labels for the active proxy only
      - Unknown requests: Return the default 503 response
      - Certificate resolver: letsencrypt
    - Traefik configuration
      - Add or modify the following sections: 
        - Remove section networks if exists
        - Remove section services.traefik.networks if exists
        - `.....` represents the ommited sections 
        - < variable-inside > Enter the value of the variable without the angle brackets .
        - Get values from your DNS Provider
          - For detailed information please read
            - Lego Dns Provider list: https://go-acme.github.io/lego/dns/
          - For Hetzner
            - DNS: hetzner
            - EMAIL: the e-mail used in DNS provider registration
            - HETZNER_API_TOKEN

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
      test: 'wget -qO- https://traefik.< prod-domain-name >/ping || exit 1'
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
      - '--ping.entrypoint=https'
      - '--ping.terminatingStatusCode=204'
      - '--api.dashboard=true'
      - '--api.insecure=false'
      - '--entrypoints.http.address=:80'
      - '--entrypoints.https.address=:443'
      - '--entrypoints.http.http.encodequerysemicolons=true'
      - '--entrypoints.http.http2.maxConcurrentStreams=250'
      - '--entrypoints.https.http.encodequerysemicolons=true'
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
      .....
    labels:
      - traefik.enable=true
      - coolify.managed=true
      - coolify.proxy=true
    traefik-logrotate:
    .....
```  

Save Changes

Lego Dns Provider list: https://go-acme.github.io/lego/dns/

https://doc.traefik.io/traefik/expose/docker/

### Add Traefik Dynamic Configuration

- Coolify UI -> Servers -> < servername > -> Proxy -> Dynamic Configurations
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
      rule: 'Host(`traefik.< prod-domain-name >`)&&PathPrefix(`/ping`)'
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
      rule: 'Host(`traefik.< prod-domain-name >`)&&(PathPrefix(`/api`)||PathPrefix(`/dashboard`))'
      service: api@internal
      tls:
        certResolver: letsencrypt
        domains:
          -
            main: < prod-domain-name >
            sans:
              - '*.< prod-domain-name >'
```

- Change basicAuth.users value with your traefik user and encrypted password
- Change < prod-domain-name > with yours
- Save
- Close Form
- Actions -> Restart Proxy
  - Confirm ... -> Restart Proxy
  - Wait for the process to start ...
    - "Successfully started coolify-proxy."
    - Close Proxy Startup Logs
    - Refresh the page

---

### Check https://coolify.< prod-domain-name >

- Login
- Change your password

Check https://traefik.< prod-domain-name >/ping

If it returns "OK", Traefik ping is OK .

https://traefik.< prod-domain-name >/dashboard/

Please enter the last `/` (forward-slash) to the URL, it is important.

Asks for username and password. Enter username & password you created during "Traefik Basic Authentication" step.

- Close the tab that is using the IP to connect Coolify.

- You can close ports 8000, 6001, 6002 on firewall after a successful login via domain name.

---

### Continue with : Send e-mail with Resend [coolify/coolify-email-resend](../install-personal-blog-prod.md#send-e-mail-with-resend-coolifycoolify-email-resend)

## References
- https://coolify.io/docs/knowledge-base/server/firewall

## Troubleshooting

### After changing Traefik settings get error:
- "ERR_CONNECTION_REFUSED"
  - Open port 8000 on VM/VPS
    - Browse to http://<vps-ip-address>:8000
      - Continue setup
      - Change password after a successsfull login via domain name.
  - Start Proxy
- After successful login via domain name, you can close the ports
  - 8000
  - 6001
  - 6002

### Error: ssh: connect to host < servername >.< prod-domain-name > port 22: Connection refused
- Coolify Web User Interface:
  - Servers -> < servername > -> Configuration -> General -> General
    - IP Address/Domain: Change value as below
      - host.docker.internal
      - Save

### Deploy command ends in error. Can not connect to MongoDB with Mongo URL (Internal)
  - If Dockerfile includes Node build commands, it wants to connect to the database, but it can not connect during build time. So moving build steps after image deployment gets rid of the error.
  - As a second option, you can enable Mongo URL (Public), and use it as DATABASE_URL
  - This project's current setup moved build step after deployment, runs in container.
