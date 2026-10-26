
recon-ng is a framework.
> Its preinstalled with kali

## Installation
```bash
sudo apt install recon-ng
```

## Launch
```bash
recon-ng
```


## Creating a workspace

```bash
workspaces create lunipod
```

## Entering into a workspace

```bash
workspaces load lunipod
```

## Adding a domain in domains table

```bash
db insert domains
```

## To see the table

```bash
show domains
```

## To see the list of modules

```bash
marketplace search
```

## To read the info/details about a particular module

```bash
marketplace info recon/domains-hosts/certificate_transparency
```

## To install module

```bash
marketplace install recon/domains-hosts/certificate_transparency
```


## Load modules

```bash
modules load recon/domains-hosts/certificate_transparency
```

Any newly discovered hostnames can be stored in Recon-ng's hosts table.


## Run test with domains table
```bash
options set SOURCE default
run
```

default means workspace default domains
