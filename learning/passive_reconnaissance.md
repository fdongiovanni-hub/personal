# Passive reconnaissance

Gather information never directly attacking the target, only using public registers and OSINT instruments.

## learn how to use codes

Alternatives to man:

- --help or -help: shows instructions directly in the terminal.
	- ```command_name --help```
- tldr: community-made, shows the 5 most used commands.
	- ```tld command_name```
- cheat.sh: cheat sheet online to consult without installation.
	- ```curl cheat.sh/command_name```

## Network and DNS research commands


| Record Type | What it Contains | Hacker / Defender Utility |
| :--- | :--- | :--- |
| **A** | IPv4 Address (the server's numbers) | Finding the physical machine hosting the website. |
| **AAAA** | IPv6 Address (modern version of IP) | Same utility as the A record, but targeting modern networks. |
| **MX** | Email Management Servers | Identifying the email platform used (e.g., seeing outlook.com reveals Office 365 usage, which is perfect for phishing targeting). |
| **TXT** | Free Text (notes and configurations) | Often used in CTF challenges/labs to hide secret Flags. |

### DNS commands

- dig (searches for records in a clean way):
	- ```dig domain.com RECORD_TYPE```
	- specific server (es. Cloudflare): ```dig @1.1.1.1 tryhackme.com A```
	- add ```+ short``` at the end for a shorter answer
- nslookup (older):
	- ```nslookup -type=RECORD_TYPE domain.com```
	- (es. E-mail): nslookup -type=MX disney.com
	- ask directly official NS: ```nslookup -type=A disney.com ns5.twdcns.co.uk```

## Data Analysis and Filtering (JSON & WHOIS)

WHOIS text protocol (port 43) is migrating to RDAP (port 443/HTTPS), which outputs in JSON format.

- nc (Netcat): Raw text tool used for manual checks or quick port probing.
	- Port scan: ```nc -zv domain.com PORT```
	- Manual WHOIS: ```nc whois.verisign-grs.com 43``` (type domain and press enter)
- curl: CLI tool to interact with web protocols and HTTPS encryption.
	- Syntax: ```curl -s URL``` (add `-s` to hide progress bar)
- jq: JSON processor to filter and format raw data structures using paths (`.`).
	- List main keys: ```curl -s URL | jq 'keys'```
	- Extract exact key: ```curl -s URL | jq '.key_name'```
	- Iterate through arrays: ```curl -s URL | jq '.array_name[].subkey'```

## External OSINT Platforms

- crt.sh / ctlogs.dev: Certificate Transparency logs used to discover historical or hidden subdomains.
- DNSDumpster: Mapping tool for subdomains, servers, and technology fingerprinting (Banners).
- Shodan.io: Search engine for internet-connected devices; features faceted search filtering (e.g., `product:"nginx"`).

## Traffic Privacy & Protection

- DoH / DoT: DNS over HTTPS/TLS; encrypts DNS queries (e.g., using `1.1.1.1`) to prevent local network logging.
- VPN / Tor: Full network encryption; masks both DNS queries and target IP addresses from local network administrators.
