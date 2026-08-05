When you need to pnpm to a new version

```bash
$ rm -rf ./node_modules/ ./.next pnpm-lock.yaml
```

```bash
$ pnpm store prune
```

```bash
$ corepack enable pnpm
```

```bash
$ corepack use pnpm@latest-11
```