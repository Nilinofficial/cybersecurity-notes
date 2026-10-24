# Subdomain Enumeration with Subfinder and httpx

A beginner-friendly, hands-on guide to discovering subdomains, checking DNS resolution, and identifying responsive web services using **Subfinder** and **ProjectDiscovery httpx** on Kali Linux.

> **Scope:** Only test domains you own or have explicit permission to assess. The examples use `lunipod.com`, my authorized practice domain. Check third-party provider rules before testing services they operate.

## 1. Install Subfinder

Update package information and install Subfinder:

```bash
sudo apt update
sudo apt install subfinder
```

Optional: Install Go if you need it for other Go-based tools or source installations. It is **not required** for the Kali package installation above.

```bash
sudo apt install golang-go
```

Verify the installation:

```bash
subfinder -version
```

## 2. Discover subdomains

```bash
subfinder -d lunipod.com -silent
```

| Flag | Meaning |
| --- | --- |
| `-d` | Target domain to enumerate |
| `-silent` | Show discovered hostnames without extra banners or status messages |

Subfinder primarily uses passive sources to discover subdomains. **A discovered hostname is not necessarily DNS-resolvable or a working website.**

### Display and save the results

```bash
subfinder -d lunipod.com -silent | tee lunipod-subdomains.txt
```

- `|` (pipe) sends the **standard output** of `subfinder` to the standard input of `tee`.
- `tee` prints that output to the terminal **and** saves it to `lunipod-subdomains.txt`.
- The file can be reused with other tools.

Alternatively, use Subfinder's output option:

```bash
subfinder -d lunipod.com -silent -o lunipod-subdomains.txt
```

## 3. Check DNS resolution

```bash
subfinder -d lunipod.com -nW -silent
```

| Flag | Meaning |
| --- | --- |
| `-nW` | Remove discovered hostnames that do not resolve in DNS (active resolution filtering) |

**Important:** DNS resolution means a hostname has a resolvable DNS answer. It does **not** prove that an HTTP server is responding or that an application works.

To inspect a particular DNS record:

```bash
dig v0.lunipod.com CNAME +short
```

This queries the hostname's CNAME record. A CNAME points to another hostname; it does not establish that the associated website is available.

## 4. Check HTTP responsiveness with ProjectDiscovery httpx

On Kali Linux, the ProjectDiscovery tool may be packaged as `httpx-toolkit` (distinct from another tool named `httpx`).

Install and verify:

```bash
sudo apt install httpx-toolkit
httpx-toolkit -version
```

### Test one website

```bash
echo "https://lunipod.com" | httpx-toolkit -silent -status-code -title
```

Example output from my authorized test:

```text
https://lunipod.com [200] [Lunipod - iOS & macOS Apps by Anaz]
```

| Part | Meaning |
| --- | --- |
| `echo "https://lunipod.com"` | Prints the URL so it can be piped to the next command |
| `-silent` | Suppresses extra output |
| `-status-code` | Displays the HTTP response status code |
| `-title` | Displays the HTML page title, when available |
| `200` | The HTTP request succeeded; it does not prove every application feature works |

### Test hostnames from a file

```bash
httpx-toolkit -l lunipod-subdomains.txt -silent -status-code -title
```

- `-l` reads targets from the specified file.
- `-status-code` helps identify successful responses, redirects, and errors.
- `-title` helps identify which web page responded.

Common HTTP status codes:

| Status | Typical meaning |
| --- | --- |
| `200` | Request succeeded |
| `301` / `302` | Redirect |
| `403` | Server understood but refused the request |
| `404` | Requested resource not found |
| `500` | Internal server error |
| `502` / `503` | Gateway error or service unavailable |

A hostname missing from the results is **not automatically dead**. It might have DNS, TLS, network, timeout, or other connectivity issues. Investigate individually before drawing conclusions.



```bash
# Discover subdomains
subfinder -d lunipod.com -silent

# Discover and save
subfinder -d lunipod.com -silent | tee lunipod-subdomains.txt

# Filter for DNS-resolving hostnames
subfinder -d lunipod.com -nW -silent

# Inspect a CNAME record
dig v0.lunipod.com CNAME +short

# Probe a single URL
echo "https://lunipod.com" | httpx-toolkit -silent -status-code -title

# Probe a saved list of hostnames
httpx-toolkit -l lunipod-subdomains.txt -silent -status-code -title
```
