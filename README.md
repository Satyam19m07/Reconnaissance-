# Reconnaissance Cheatsheet(Kali Linux)

## **Introduction**

Reconnaissance (Recon) is the first phase of ethical hacking and penetration testing. It means collecting information about a target (domains, IPs, subdomains, services, people, and technologies) to build a clear picture of the attack surface. Recon can be **passive** (OSINT) or **active** (light probes / scans) — always get permission before running active tests.

---

## Quick legal reminder

**Do not** scan systems you do not own or do not have explicit permission to test. Use `scanme.nmap.org`, your lab VMs, or platform labs (TryHackMe / HackTheBox) for practice.

---

## **Prerequisite — Install core tools (run once before starting)**

Run this to make sure core reconnaissance tools are available on your Kali VM:

sudo apt update && sudo apt install -y whois dnsutils nmap curl jq git

### 01) Ping — Connectivity check

ping -c 4 scanme.nmap.org

**Short:** Check if the host is up and measure latency (round-trip time).

---

### 02) Traceroute — Network path & hops

traceroute scanme.nmap.org

**Short:** Show the intermediate routers (hops) between you and the target.

---

### 03) Whois — Domain registration info (OSINT)

whois example.com

**Short:** Retrieve domain registration metadata (registrar, creation/expiry, name servers).

---

### 04) Dig — DNS lookup (A record)

dig example.com A +short

**Short:** Get IPv4 addresses (A records) for a domain. Multiple IPs often indicate CDN or load balancing.

---

### 05) theHarvester (bing) — Passive OSINT

theHarvester -d example.com -b bing

**Short:** Collect public emails, hosts, and subdomains from search engines (bing). (Google engine deprecated.)

---

### 06) subfinder — Fast passive subdomain enumeration

subfinder -d example.com -silent > subs.txt

**Short:** Discover subdomains from multiple public sources and save to a file for later use.

---

### 07) crt.sh via curl — Certificate-based subdomain discovery

curl -s "[https://crt.sh/?q=%25.example.com&output=json](https://crt.sh/?q=%25.example.com\&output=json)" | jq '.[].name_value' | sort -u

**Short:** Find subdomains recorded in public TLS certificate logs (crt.sh).

---

### 08) Nmap — Basic TCP SYN scan (authorized targets only)

sudo nmap -sS -p 1-1000 -T4 scanme.nmap.org

**Short:** Discover open ports and running services. `-sS` is a SYN scan, `-T4` speeds up scanning.

---

### 09) Curl — Fetch HTTP headers (fingerprinting)

curl -I [http://example.com](http://example.com)

**Short:** Show HTTP response headers to learn server type and basic configuration.

---

## Extra: Red Hawk — Quick web reconnaissance

Red Hawk is a simple all-in-one web reconnaissance tool that gathers site info (IP, headers, CMS detection, robots.txt, subdomains, etc.). **Install & run**

git clone [https://github.com/Tuhinshubhra/RED_HAWK.git](https://github.com/Tuhinshubhra/RED_HAWK.git) cd RED_HAWK # make sure php is installed: sudo apt install php-cli -y php rhawk.php -i example.com

**Short:** One-shot web recon to quickly view headers, geoIP, possible CMS and robots/sitemap info. Verify and manually validate all findings.

---

## Quick glossary

- **Recon:** The initial information-gathering phase.
- **OSINT:** Open-Source Intelligence — public data collection.
- **Footprint:** The target's digital map (domains, subdomains, IPs).
- **Subdomain:** e.g., `api.example.com` — separate service/area of a domain.
