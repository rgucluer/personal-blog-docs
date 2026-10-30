# Set e-mail with Resend

- Configure Resend to send emails
  - resend.com/emails
    - Domains  
      - Add domain
        - Name: contact.< domain-name >
        - Region: Choose nearest to you
        - Add domain
          - Read from Resend page, then go to your DNS management page, and set your DNS records according to the document. It can change according to DNS provider.
            - DKIM
          - Enable Sending (Check)
            - SPF
            - DMARC

- Copy API Key from Resend
  - Resend -> API Keys:
    - Delete if any unused exiting API keys
    - Create API Key
      - Name: resendsend
      - Permission: Sending access
      - Domain: Select your domain ( contact.< domain-name > )
      - Add
        - View API Key
          - Click copy icon, and store the API key in a safe place
          - After you store the API key to a safe place Click Done

- Coolify Settings -> Email
  - From Name: John Doe
  - From Address: info@contact.< domain-name >
  - Resend
    - API Key: Enter the API Key we created at the previous step
    - Resend delivery: Enabled
  - Save changes
  - Resend
    - Click `Send Test` button
      - Enter Recipient address and hit Send Email
        - "Success , Test Email sent." notification appears.

- Coolify UI -> Manage -> Notifications -> Email
  - Email delivery
    - Email service: Use system-wide settings
  - Notification Events:
    - Check notifications you want. 
      - (Don't forget!; Resend (free version) has limited resources.)

---

For Development Installation:

### Continue with : Create a snapshot of the virtual machine [coolify/create-snapshot.md](../install-personal-blog-dev.md#create-a-snapshot-of-the-virtual-machine-coolifycreate-snapshot)

---

For Production Installation:

### Create a Payload CMS Application from a private git repository [payload/publish-payload-cms-vps](../payload/publish-payload-cms-vps.md#)

---

### References
- https://resend.com/docs/dashboard/domains/manage-domains
- https://resend.com/docs/dashboard/domains/manage-domains#what-are-spf-records
- https://resend.com/docs/knowledge-base/hetzner

---

### Troubleshooting

#### Error: Undefined array key "name"
- Be sure the domain name definitions are correct and point to the same subdomain in Resend.
  - API Key domain
  - Domains Domain
