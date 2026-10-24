# Subdomain Enumeration with Subfinder

## 1. Install it on Kali Linux

```bash
sudo apt update
sudo apt install assetfinder
```


## To find sub domains

```bash
assetfinder --subs-only lunipod.com
```

assetfinder — discovers domains and subdomains from public sources.
--subs-only — limits results to subdomains of lunipod.com, rather than including related domains.
lunipod.com — your target domain.

```bash
assetfinder --subs-only lunipod.com | sort -u > assetfinder-subdomains.txt
```

sort — sorts the output alphabetically.
-u — removes duplicate lines.
>  — writes the output into a file, replacing existing contents.