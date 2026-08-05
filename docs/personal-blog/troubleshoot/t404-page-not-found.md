# 404 page not found

## Do initial checks
- Virtual Machine is running
  - No: Start VM
    - ```bash
      multipass start < vm-name >
      ```
    - List the VMs
      ```bash
      multipass list
      ```
- Database service is running
  - Cooliy Web UI -> Projects -> < project-name > -> Databases -> < database-name > -> Running (healthy)
- Payload service is running
  - Cooliy Web UI -> Projects -> < project-name > -> Applications -> < coolify-application-name > -> Running (healthy)
  - Logs
- Check Domains for payload in Application General Settings
  - Coolify Web UI -> Projects -> < project-name > -> Applications -> < coolify-application-name > -> 
    - Configuration -> General -> Domains -> Domains for payload
      - Check the domain names
        - https://www.< dev-domain-name >,https://< dev-domain-name >
        - https://www.devblogserver.my-domain.com,https://devblogserver.my-domain.com
        - Top Right -> Advanced -> Force Deploy
        - Wait 4 minutes after deployment for the service to settle.
  - Coolify Web UI -> Servers -> `< vm-server-name >` -> Proxy -> Restart Proxy ... 

- Check checkbox : Build Preserve Repository During Deployment 
  - Coolify UI -> Projects -> < project-name > -> < coolify-application-name > -> Configuration -> General
    - Build
      - Check : Preserve Repository During Deployment
      - Save
      - Stop if service is running
      - Advanced -> Force deploy (without cache)
  - Coolify UI -> Servers -> < vm-server-name >
    - Restart Proxy
    

## Check Coolify Payload CMS Deployment log, even it is green. 
  - Look for error messages
  - Look for post deployment messages.
    - Deployment failed: App\Jobs\ApplicationDeploymentJob has timed out.
      - It was about the structure of Dockerfile, fixed after some changes.

## Check DNS settings are properly set.
Go to you service provider panel and check/fix settings

## Check if /etc/hosts file is set up to point the VM
  - List VM with `multipass list` command, edit /etc/hosts file and compare the two IP values
- Traefik is set, and running
  - Check Traefik status
    - Browse to : https://traefik.< dev-domain-name >/ping
      - It should return OK
    - Browse to : https://traefik.< dev-domain-name >/dashboard/
      - Look for errors in HTTP Routers
    - Coolify Web UI -> Servers -> < vm-server-name > -> Proxy -> Logs
  - Cooliy Web UI -> Servers -> < vm-server-name > -> Proxy -> Dynamic Configurations
    - File: payload-app.yaml -> Edit
      - http:
        - routers:
          - payload-https:
            - service: 
            - Check service name 
      
    - Values in the file are changed according to your setup
    - No Tabs, only spaces


