## Set Traefik for the new application

- After deploying payload service,
- Get Payload application service name information
  - Coolify UI -> Projects : personal-blog -> Applications: personal-blog
    - Configuration -> General
    - Click Details
      - Service Name : Resource Name + "-" + Resource UUID
      - Service Name : personal-blog-some25charsuuidstring1234


Use personal-blog-some25charsuuidstring1234@docker in personal-blog.yaml in Dynamic Configurations below

- Coolify UI on VM (https://coolify.< dev-domain-name >)
  - Servers -> < vm-server-name > -> Proxy -> Dynamic Configurations
    - +Add/Edit
      - Filename: personal-blog.yaml
      - Configuration: 
```yaml
http:
  middlewares:
    root-to-www:
      redirectRegex:
        regex: '^((https?:\/\/)|).devblogserver.my-domain.com'
        replacement: 'https://www.devblogserver.my-domain.com'
        permanent: true
  routers:
    redirect-root:
      rule: Host(`devblogserver.my-domain.com`)
      entryPoints:
        - http
        - https
      middlewares:
        - root-to-www
      tls:
        certResolver: letsencrypt
        domains:
          main: devblogserver.my-domain.com
      service: noop@internal
    personal-blog-https:
      rule: Host(`www.devblogserver.my-domain.com`)
      entryPoints:
        - https
      tls:
        certResolver: letsencrypt
        domains:
          main: devblogserver.my-domain.com
          sans:
            - '*.devblogserver.my-domain.com'
      service: personal-blog-some25charsuuidstring1234@docker
```
      - Check spaces, do not use tabs, use only spaces. Indentation is important.
      - Save
      - Close Form
      - Restart Proxy
        - Wait a few minutes than close the "Proxy Startup Logs" form
          - A white page can appear, click background on edges of white form.
        - Still unresponsive, browse to https://coolify.<domain-name>

## Continue with : Check the Application [check-the-application](../payload/publish-payload-app.md#check-the-application)

