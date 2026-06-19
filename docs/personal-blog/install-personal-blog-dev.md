# Personal Blog
This is a blog made with Payload CMS. Cloned https://github.com/rgucluer/cpm-cms , will modify as needed.

## Clone cpm-cms
```bash
cd < workspace-full-path >
```
```bash
git clone https://github.com/rgucluer/cpm-cms.git personal-blog
```

## Install Personal Blog on a Virtual Machine

- Open two document side by side:
	- Document One: [cpm-cms/development.md](../cpm-cms/development.md)
	- Document Two is this one
- Document one cpm-cms/development.md is the primary one, this document expresses differences.
	- Set swap
	- Install Multipass and Create a Virtual Machine
		- Create & Add ssh keys
			- vm-payload-user
			- vm-payload-user-key: SSH key for < vm-payload-user >
			- vm-coolify-rootkey: SSH key for VM root user
		- Install Canonical Multipass
			- Edit multipass/cloud-init/cloud-config.yaml file.
				- hostname: devblogserver
				- fqdn: devblogserver.my-server.com 
				- keyboard:
						layout: us	
		- Create the Virtual Machine
			- vm-name: Virtual Machine Name
				- vmblog
			```bash
			cd < payload-blog-docs-dir >/multipass/cloud-init
			```
			Set disk, and memory according to your setup (Recommended Min: 4GB Ram, 40GB Disk)
			```bash
			multipass launch 24.04 --name vmblog --cpus 2 --disk 40G --memory 8G --cloud-init cloud-config.yaml
			```

			```bash
			multipass list
			```

			```bash
			Name                    State             IPv4             Image
			coolvm                  Stopped           --               Ubuntu 24.04 LTS
			vmblog                  Running           10.124.32.69     Ubuntu 24.04 LTS
			```
	- Install & run Coolify on a local Virtual Machine
	- Configure Coolify - Setup Wildcard SSL Certificates with Traefik
		- Set Coolify Domain
			- Coolify Web UI -> Settings -> Configuration -> General
				- URL: https://coolify.devblogserver.my-domain.com
				- Name: coolify-dev
		- Setup General Settings for the Virtual Machine
			- Name: devblogserver
			- Wildcard Domain: https://devblogserver.my-domain.com
		- Servers -> devblogserver -> Sentinel
			- Coolify URL http://coolify.devblogserver.my-domain.com
			- Save
			- Restart
		- Servers -> devblogserver -> Proxy -> Configuration -> Advanced
			- Override default request handler: Unchecked
			- Save
  		- Restart Proxy
		- Set Timezone
		- Prepare username & password for Traefik Basic Authentication
		- Edit Traefik Configuration on VM (Check full text in docs/cpm-cms/development.md)
			- Servers -> devblogserver -> Proxy -> Configuration -> Traefik (Coolify Proxy)
			- The variables below must be filled in.
			```yaml
				services:
					traefik:
						.....
						environment:
							- 'EMAIL=< lego-email >'
							- 'DNS=< lego-service-provider >'
							- '< lego-service-provider-env-var >=< lego-service-provider-api-token >'
						.....
						healthcheck:
							test: 'wget -qO- https://traefik.< dev-domain-name >/ping || exit 1'
						.....
						command:
							.....
							- '--certificatesresolvers.letsencrypt.acme.dnschallenge.provider=< lego-service-provider >'
							.....
							- '--certificatesresolvers.letsencrypt.acme.email=< lego-email >'
							.....
			```
		- Add Traefik Dynamic Configuration
			- Servers -> < servername > -> Proxy -> Dynamic Configurations
				- +Add
					- Filename: traefik-dashboard.yml
					```yaml
					http:
						middlewares:
							authentication:
								basicAuth:
									realm: traefik-dashboard
									users:
										- '< traefik-user >:< traefik-enc-password >'
						.....
						routers:
							ping-http:
								.....
								rule: 'Host(`traefik.< dev-domain-name >`)&&PathPrefix(`/ping`)'
								service: noop@internal
							ping-https:
							  .....
								rule: 'Host(`traefik.< dev-domain-name >`)&&PathPrefix(`/ping`)'
								service: ping@internal
								.....
							traefik-dashboard-http:
								.....
								rule: 'Host(`traefik.< dev-domain-name >`)&&(PathPrefix(`/api`)||PathPrefix(`/dashboard`))'
								service: noop@internal
							traefik-dashboard-https:
								.....	
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

		- Set DNS Servers
			- DNS Settings
				- DNS Validation: Uncheck
			- Custom DNS Servers:
				- Enter IPs of your domain's name servers
			- API Settingss
				- Allowed IPs for API Access
					- `< vps-ip >`
					- `< virtual-m-ip >`
					- seperated with a comma
				- API Access: Check
			- Save
			- Settings -> Configuration -> Advanced
				- DNS Settings
					- DNS Validation: Check
				- Save
			- Settings -> Configuration -> Advanced -> UI Settings
				- SPA Navigation: Uncheck
				- Save
			- Start Proxy or Restart Proxy
			- Check https://coolify.devblogserver.< domain-name >
  - Set e-mail with Resend
    - Store the API key in a safe place
	- Create a snapshot of the virtual machine
	- Create a Payload CMS Application from a private git repository
		- Project Name: personal-blog
    - Description: Personal Blog made with Payload CMS
    - Create a MongoDB using Coolify
      - Name: mongodb-payload  
    - Install nvm, and Node
      - v24.16.0
    - Create a Payload CMS Application          
      - Method Two : [Clone an existing git repository](../cpm-cms/payload/create-payload-cms-with-git-clone.md) .
        - Git repository: https://github.com/rgucluer/cpm-cms.git
        - Rename directory as: personal-blog
        - Edit .env file
    - Make some changes in source code
      - These changes are for changing the pnpx installation
      - Following is sufficient for git cloned install
        - Install/update pnpm. 
          ```bash
          cd < payload-blog-source-dir >
          ```
          ```bash
          corepack use pnpm@latest-10
          ```
    - Install npm packages & build the project
    - Create a new private respository on GitHub
      - Repository name: < github-username >/personal-blog
      - Visibility: Private      
    - Add a new remote for payload-blog
      ```bash
      cd < payload-blog-source-dir >
      ```
      - Use the previously created git repository as new remote
      ```bash
      git remote set-url origin git@github.com:< github-username >/personal-blog.git
      ```
    - git push code to git repository
      - https://github.com/rgucluer/personal-blog
    - git push to dev branch
    - Coolify - Add a new Source
      - Visit https://coolify.devblogserver.< domain-name >
      - github-app-name-vm: app-personal-blog-vm
      - Selected Endpoint: https://coolify.devblogserver.< domain-name >
        - Uncheck Preview Deployments 
        - Register Now
        - Create Github App for < github-username >
        - Click Install Repositories on GitHub
      - Authorization Request for GitHub - Install `< github-app-name-vm >`
        - Only select repositories
          - < github-username >/payload-blog
        - Click Install
      - We are back in Coolify UI Sources - GitHub App Form -> 
        - App Name: app-personal-blog-vm
        - Save
    - Add Source to the Project
      - Coolify Web UI -> Projects -> personal-blog (production)
        - Click Resources +New
          - Applications -> Git Based
            - Click Private Repository ( with GitHub App )
              - Select a GitHub App: Click app-personal-blog-vm button
                - Repository: personal-blog
                - Click Load Repository
                  - Branch: dev
                  - Build Pack: Docker Compose
                  - Change Docker Compose Location to:
                    - /docker-compose.yml
                    - ATTENTION : File extension is .yml not .yaml. Please check & edit.
                  - Continue
                  - If successful, this loads docker-compose.yml, opens configuration menu.
                  - Save
    - Configure the Payload Project (docs/cpm-cms/coolify/configure-payload-dev.md)
      - Coolify Web UI -> Projects -> personal-blog ( production )
        - Applications -> personal-blog
          - Name : payload
          - Domains for payload: https://www.devblogserver.my-domain.com,https://devblogserver.my-domain.com
      - Configuration -> Environment Variables
        - NODE_OPTIONS=--no-deprecation --max-old-space-size=2048
        - HOSTNAME, DATABASE_URL, PAYLOAD_SECRET, CRON_SECRET, PREVIEW_SECRET
        - NEXT_PUBLIC_SERVER_URL : https://www.devblogserver.< domain-name >
      - Configuration -> Git Source
        - Repository: < github-username >/personal-blog
        - Branch: dev
      - Configuration -> Resource Limits
      - Deploy / Redeploy Payload Application
    - Set Traefik for the new application (docs/cpm-cms/coolify/set-traefik-for-new-app.md)
      - Get container name information
        - Example : payload-abcdefghijklmabcdefghi-012345678901
        - string to enter : payload-abcdefghijklmabcdefghi@docker
      - Filename: personal-blog.yaml
      - Enter devblogserver instead of devserver1
      - Enter your domain name instead of my-domain.com
      - service: payload-abcdefghijklmabcdefghi@docker
      - Save
      - Restart Proxy
    - Check the Application
	  - https://www.devblogserver.< domain-name >
---

Notes:
- Use the following in Payload Blog 
  - Use devblogserver instead of devserver1
  - Use vmblog instead of coolvm
