## Set Traefik for the new application

- After deploying payload service,
- Get Payload application service name information
  - Coolify UI -> Projects : payload-project -> Applications: < coolify-application-name >
    - Configuration -> General
    - Click Details
      - Service Name : Resource Name + "-" + Resource UUID
      - Service Name : personal-blog-some25charsuuidstring1234

Use personal-blog-some25charsuuidstring1234@docker in payload-app.yaml in Dynamic Configurations below

- Coolify UI on VPS (https://coolify.my-domain.com)
  - Servers -> < vps-server-name > -> Proxy -> Dynamic Configurations
    - +Add/edit
      - Filename: personal-blog.yaml
      - Configuration: 
```yaml
http:
  middlewares:
    root-to-www:
      redirectRegex:
        regex: '^((https?:\/\/)|).my-domain.com'
        replacement: 'https://www.my-domain.com'
        permanent: true
  routers:
    redirect-root:
      rule: Host(`my-domain.com`)
      entryPoints:
        - http
        - https
      middlewares:
        - root-to-www
      tls:
        certResolver: letsencrypt
        domains:
          main: my-domain.com
      service: noop@internal
    payload-https:
      rule: Host(`www.my-domain.com`)
      entryPoints:
        - https
      tls:
        certResolver: letsencrypt
        domains:
          main: my-domain.com
          sans:
            - '*.my-domain.com'
      service: personal-blog-some25charsuuidstring1234@docker
```
      - Check spaces, do not use tabs, use only spaces. Indentation is important.
      - Save
      - Close Form
      - Restart Proxy
        - Wait a few minutes than close the "Proxy Startup Logs" form
          - A white page can appear, click background on edges of white form.
        - Still unresponsive, browse to https://coolify.<domain-name>


## Continue with : Check the Application [check-the-application](../payload/publish-payload-cms-vps.md#check-the-application)
