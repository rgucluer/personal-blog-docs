# Set e-mail with Resend

- Configure Resend to send emails
  - resend.com Site 
    - Domains  
      - Add domain
        - Name: < domain-name >
        - Region: Choose nearest to you
        - Add domain
          - Read Fill in your DNS Records section, and set your DNS records according to the document.
            - DKIM
          - Enable Sending (Green Enabled)
            - SPF
            - DMARC
          - If you enable receiving, also set the mx record for it.

- Copy API Key from Resend
  - Resend -> API Keys:
    - Delete if any unused exiting API keys
    - Create API Key
      - Name: resendsend
      - Permission: Sending access
      - Domain: Select your domain ( < domain-name > )
      - Add
        - View API Key
          - Click copy icon, and store the API key in a safe place
          - After you store the API key to a safe place Click Done

- Coolify Settings -> Instance -> Email
  - From Name: John Doe
  - From Address: info@< domain-name >
  - Resend
    - API Key: Enter the API Key we created at the previous step
    - Check Enabled

- Click Send Test
  - Enter a recipient e-mail address
  - Click Send Test
    - Error: Undefined array key "name"

- Coolify UI -> Manage -> Notifications -> Email
  - Email delivery
    - Email service: Use system-wide settings
  - Check: Use system wide (transactional) email settings
  - Notification Events:
    - Check notifications you want. 
      - (Don't forget!; Resend (free version) has limited resources.)

---

### Continue with : Create a snapshot of the virtual machine [coolify/create-snapshot.md](../install-personal-blog-dev.md#create-a-snapshot-of-the-virtual-machine-coolifycreate-snapshot)
