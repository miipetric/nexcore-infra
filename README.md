# NexCore Ansible Infrastructure Automation

Pokretanje iz direktorija projekta:

```bash
ansible-galaxy collection install -r requirements.yml
ansible-playbook -i inventory.ini site.yml -k -K
```

Napomene:

- `users` se izvršava prije `automount`.
- AutoFS koristi direct map i mounta samo `~/Company_Share`, ne cijeli `/home/<user>`.
- DNS konfiguracija uključuje forwardere za vanjske domene.
- SSH hardening se izvršava zadnji kako ne bi prekinuo playbook dok se koristi password SSH login.

## DNS note for Red Hat lab
The DNS role uses `dnsmasq` on `servera` for `nexcore.local` and forwards external DNS queries to the Red Hat/lab DNS servers. Forwarders are auto-detected from the current resolver before `/etc/resolv.conf` is rewritten. If auto-detection fails, set them explicitly in `group_vars/all.yml`:

```yaml
dns_forwarders:
  - 192.168.222.1
```

Because the lab may mark `/etc/resolv.conf` immutable, the resolver role removes the immutable flag with `chattr -i`, writes the resolver configuration, and restores it with `chattr +i`.

## MediaWiki

The web role installs a real MediaWiki release non-interactively. It downloads the official MediaWiki archive, extracts it under `/var/www/mediawiki-<version>`, points `/var/www/wiki` to that installation, and runs `maintenance/install.php` against the MariaDB database on `serverb`.

Default local MediaWiki admin credentials for lab validation:

- User: `Admin`
- Password: `ChangeMeWikiAdmin123!`

