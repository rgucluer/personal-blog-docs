# Variables for Personal Blog
When you see these variables through the document , enter the values valid for your setup. Do not enter the angle brackets ( < > ).

- Host Machine (Developer PC, Controller PC)
  - local-user-name: System user name
  - workspace: workspace
    - The directory to store software source code.
  - workspace-full-path: /home/< local-user-name >/< workspace >
  - dev-pc-local-ip: Developer PC local IP
  - dev-pc-global-ip: Global IP address that Developer PC connects to internet.

- Payload App
  - app-dir-name: personal-blog
  - personal-blog-source: < workspace-full-path >/< app-dir-name >
  - payload-app-full-path: < workspace-full-path >/< app-dir-name >
  - personal-blog-docs-full-path: < workspace-full-path >/personal-blog-docs

- Virtual Machine
  - vm-name: vmblog
  - vm-user-name : vm2user
  - vm-user-key: vm2user-ssh-key
    - SSH key for < vm-user-name >
  - vm-ssh-port: 22
  - virtual-m-ip: IP of Virtual Machine. Learn from the result of multipass list command.
  - vm-server-name : 
    - devblogserver

- Virtual Private Server
  - vps-ip-address:
  - vps-server-name : server1 
  - vps-root-key: root-ssh-key

- domain-name: my-domain.com
- dev-domain-name: devblogserver.my-domain.com
- prod-domain-name: my-domain.com
- servername: 
    vm: devblogserver
    vps: server1

- Coolify
  - vm-coolify-rootkey: SSH key for VM root user
    - my-coolify-root-key
  - coolify-project-name:
    - personal-blog
  - coolify-application-name:
    - personal-blog
  - coolify-server-name
    - < servername >
    - < vm-server-name >
    - < vps-server-name >
  - Coolify Payload Service Name
    - Combine Payload Coolify Resource Name , and Payload Coolify Resource UUID with a hyphen
      - Click Details button in Configuration -> General section
        - Conbine values under Resource
        - < name >-< UUID >
      - payload-< payload-container-uuid >

  - Databases
    - database-name: mongodb-payload
  - Traefik ( Coolify Proxy )
    - traefik-user : traefikuser
    - traefik-password
    - traefik-encrypted-password

- GitHub
  - github-username: githubuser
  - github-repo-name: cpm-cms
  - github-user-key: github-ssh-key
  - github-name-surname: John Doe
  - github-user-email
  - git-repo-url : https://github.com/< github-username >/cpm-cms
  - github-app-name-vm: 
    - cpm-cms ( Used in Virtual Machine)
  - github-app-name-vps: 
    - cpm-cms-vps ( Used in Virtual Private Server)
    - payload-app-vps ( Used in Virtual Private Server)

TODO: List all possible places for domain name entry


