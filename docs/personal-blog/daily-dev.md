# Daily Dev

[cpm-cms daily-dev - cpm-cms/daily-dev]()

I refer to cpm-cms all the time, because it is public. currently personal-blog is a private git repository. I want documentation available to the public. I refer to primary document, and express the changes needed for personal-blog in this type of secondary document. 

## Changes specific to personal-blog:

- Add ssh keys to ssh agent 
- Start Virtual Machine
  - VM Name: vmblog
- Development sub domain name: devblogserver
- coolify-server-name: devblogserver
- Domain name: my-domain.com 
- Check /etc/hosts of local machine for the IP of VM
  - Make changes in /etc/hosts according to my setup
- ssh to Virtual Machine
- Update apt packages
- Open Coolify Web Interface
- Development steps
  - payload-blog-source-dir: < workspace-full-path >/payload-blog
    - git repository: https://github.com/rgucluer/personal-blog
  - payload-blog-docs-dir: < workspace-full-path >/payload-blog-docs
- Run on local machine, checked http://localhost:3000
  - Did not login to admin dashboard, continue to git commit and following steps .
- Redeploy on VM
