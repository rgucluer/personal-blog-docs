# Variables for Personal Blog
When you see these variables through the document , enter the values valid for your setup. Do not enter the angle brackets ( < > ).

- Host Machine (Developer PC, Controller PC)
  - local-user-name: System user name
  - workspace: workspace
    - The directory to store software source code.
  - workspace-full-path: /home/< local-user-name >/< workspace >
  - dev-pc-local-ip: Developer PC local IP
  - dev-pc-global-ip: Global IP address that Developer PC connects to internet.

- Domain information
  - domain-name: my-domain.com
    - The domain name registered to you. 
    - fqdn.tld
  - dev-domain-name: devblogserver.my-domain.com
  - prod-domain-name: my-domain.com
    - You can use a subdomain: blog.my-domain.com
  - servername: 
      vm: devblogserver
      vps: server1

- Payload App
  - payload-app-directory: personal-blog
  - payload-app-full-path: < workspace-full-path >/< payload-app-directory >
    - example: /home/jack/workspace/cpm-cms

- Virtual Machine
  - vm-name: vmblog
  - vm-user-name: vm2user
  - vm-user-key: vm2user-ssh-key
    - SSH key for < vm-user-name >
  - vm-root-key: Virtual Machine ssh key for root user
  - vm-ssh-port: 22
  - virtual-m-ip: IP of Virtual Machine. Learn from the result of multipass list command.
  - vm-server-name: devblogserver

- Virtual Private Server
  - vps-ip-address:
  - vps-server-name : server1 
  - vps-root-key: Virtual Private Server ssh key for root user
  - vps-ssh-port: 22
  - vps-user-name
  - vps-user-key: vps-user-ssh-key

- Coolify
  - coolify-project-name:
    - personal-blog
  - coolify-application-name
    - personal-blog
  - coolify-server-name
    - < vm-server-name >
    - < vps-server-name >
    - Coolify Payload App Service Name
      - TODO: Enter how to find Payload App Service Name
  - Databases
    - database-name: mongodb-payload
  - Traefik ( Coolify Proxy )
    - traefik-user : traefikuser
    - traefik-password
    - traefik-encrypted-password
    - lego-email : E-mail registered for letsencrypt
    - lego-service-provider-api-token
      - Read your DNS Provider's documentation.
    - lego-service-provider-env-var : HETZNER_API_TOKEN
      - Look up for your DNS Provider's settings in https://go-acme.github.io/lego/dns/#dns-providers
    - lego-service-provider-dns : hetzner
      - Look up for your DNS Provider's settings in https://go-acme.github.io/lego/dns/#dns-providers
    - traefik-ping-address : https://traefik.my-server.com/ping

- GitHub
  - github-username: githubuser
  - github-user-key: github-ssh-key
  - github-name-surname: John Doe
  - github-user-email
  - github-repo-name: personal-blog
  - git-repo-url : https://github.com/< github-username >/personal-blog
  - github-app-name-vm: 
    - personal-blog-for-vm ( Used in Virtual Machine)
  - github-app-name-vps: 
    - personal-blog-vps ( Used in Virtual Private Server)



