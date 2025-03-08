# Notes

## Failure binding to port 53

May need to turn off the `systemd-resolved` to successfully bind to port 53.

Edit `/etc/systemd/resolved.conf`:

```ini
[Resolve]
DNS=1.1.1.1 # Any external DNS server is fine. This one is for Cloudflare.
DNSStubListener=no
```

Then symlink systemd's `resolv.conf` to use the same configuration as the system:

```sh
# -s is for symbolic link, -f is to override existing files/symlinks
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

Docker should now be able to bind to port 53. If you're still running into issues, diagnose with `sudo lsof -i :53`.
