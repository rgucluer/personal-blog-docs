# Setup e-mail provider

## Install @payloadcms/email-resend package
```bash
$ cd < payload-app-full-path >
```

```bash
pnpm update payload --latest
```

```bash
pnpm update @payloadcms/* --latest  
```

```bash
pnpm add @payloadcms/email-resend
```

## Add environment variables for Resend

- Add RESEND_API_KEY, RESEND_FROM_ADDRESS, RESEND_FROM_NAME to Payload CMS as environment variables
  - Add environment variables to Coolify Payload application
  - Add environment variables to .env file


## Edit payload.config.js
```bash
// payload.config.js
import { resendAdapter } from '@payloadcms/email-resend'

export default buildConfig({
  email: resendAdapter({
    defaultFromAddress: 'info@< dev-domain-name >',
    defaultFromName: 'Payload CMS',
    apiKey: process.env.RESEND_API_KEY || '',
  }),
})
```

Apply [troubleshoot/frozen-lockfile](../troubleshoot/frozen-lockfile.md)

---

## References:
- https://www.npmjs.com/package/@payloadcms/email-resend

---
## Troubleshooting:

