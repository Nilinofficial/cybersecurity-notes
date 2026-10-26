## Chaos by ProjectDiscovery.

Chaos is different from Subfinder and Assetfinder. It provides access to ProjectDiscovery's collected DNS and subdomain datasets, rather than relying only on live enumeration sources.


```bash
go install github.com/projectdiscovery/chaos-client/cmd/chaos@latest

read -rsp "Enter Chaos API key: " CHAOS_KEY
echo
export CHAOS_KEY

export PDCP_API_KEY="$CHAOS_KEY"
```
Add API Key



One security consideration: Typing the API key directly into a command may save it in your Bash history. The hidden-input method avoids putting the key in the command itself.

> something is wrong, API key is not updating.