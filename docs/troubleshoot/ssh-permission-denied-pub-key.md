# Permission denied (publickey)

During ssh to the VM, or VPS we can get this error:
```bash
Permission denied (publickey).
```

## Add ssh keys to ssh agent and try again

```bash
ssh-add ~/.ssh/< vm-root-key >
```
```bash
ssh-add ~/.ssh/< vm-user-key >
```

Try again
```bash
ssh root@< virtual-m-ip >
```
```bash
ssh vm2user@< virtual-m-ip >
```

