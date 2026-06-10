**Task 4: Passive and Active Reconnaissance Report**

---

**1. Passive vs. Active Reconnaissance**

Understanding the boundary between passive and active information gathering is critical to staying undetected and operating within legal boundaries.

| Feature | Passive Reconnaissance | Active Reconnaissance |
| :--- | :--- | :--- |
| **Direct Interaction** | None. Data is gathered from third-party aggregators and public records. | Yes. Packets are sent directly to the target infrastructure. |
| **Risk of Detection** | Zero. The target’s defensive systems see nothing. | High. Generates logs, triggers alerts, and exposes the attacker's IP address. |
| **Common Tools** | WHOIS, Google Dorking, Shodan, DNSDumpster, OSINT frameworks. | Nmap, Ping, Traceroute, Dirbuster, Nikto, Port Scanners. |
| **Primary Goal** | To build a broad blueprint of the target without alerting them. | To map out live hosts, open ports, OS versions, and active services. |

---

**2. WHOIS Lookup**

The WHOIS database stores registered users or assignees of an Internet resource, such as a domain name or an IP address block.

**Command Execution:**
whois example.com

<img width="1023" height="715" alt="image" src="https://github.com/user-attachments/assets/de4d25b8-e472-4762-ab06-02ff49de669b" />


**Key Findings:** The domain is managed directly by IANA (Internet Assigned Numbers Authority). It features active registrar locks (clientTransferProhibited) and reveals the authoritative name servers handling its DNS records.

---

**3. DNS Information Gathering (nslookup & dig)**

DNS enumeration maps a domain name to its corresponding IP infrastructure and mail servers.

**Exercise A: Utilizing nslookup**
* Querying the A Record (IP Address):
  nslookup example.com
* Querying Mail Servers (MX Records):
  nslookup -type=mx example.com

**Exercise B: Utilizing dig**
* Querying Any Available Records:
  dig example.com ANY

<img width="336" height="249" alt="image" src="https://github.com/user-attachments/assets/5c7ecaa4-482f-4ee7-a5aa-232af416987a" />


**Key Findings:** The target resolves to the IPv4 address 93.184.215.14. The SOA (Start of Authority) record reveals the primary master DNS server (ns.icann.org) and operational timers for zone transfers.

---

**4. Google Dorking Techniques**

Advanced search operators allow operators to filter through Google’s massive index to find exposed data or misconfigurations without making contact with the target.

* **Dork 1: Exposed Configuration Files**
  Query: site:example.com filetype:env OR filetype:conf
  Concept: Filters indexing to look exclusively inside the target domain for configuration or environment files that might contain database credentials or API keys.

* **Dork 2: Exposed Directory Listings**
  Query: site:example.com intitle:"index of /"
  Concept: Searches for web servers that have directory listing enabled, potentially exposing backup folders, source code, or unlinked assets.

* **Dork 3: Unprotected Administrative Entry Points**
  Query: site:example.com inurl:admin OR inurl:login
  Concept: Narrows down search results to paths containing access portals, mapping out the administrative surface area of the application.

* **Dork 4: Publicly Indexed Sensitive Documentation**
  Query: site:example.com filetype:pdf "confidential" OR "internal use only"
  Concept: Locates uploaded PDF documents containing sensitive strings that should have been restricted behind an authentication wall.

* **Dork 5: Backup Files and Database Dumps**
  Query: site:example.com filetype:sql OR filetype:bkp OR filetype:bak
  Concept: Scans for forgotten database exports or structural backups left in public web root directories during maintenance.

<img width="382" height="163" alt="image" src="https://github.com/user-attachments/assets/7b4abde9-f810-45eb-8326-58aff6bc1273" />


---

**5. Shodan Search Concepts**

Shodan scans the entire internet and indexes the "banners" returned by devices (routers, servers, IoT devices) rather than public web content.

**Core Architecture Filters:**
* product:"Product Name" (Targets specific software builds)
* port:"Number" (Limits scope to devices running specific services)
* country:"Code" (Pinpoints infrastructure geolocation)

**Applied Search Concepts:**
1. Finding Exposed Web Servers Globally: product:"nginx" port:80
2. Identifying Vulnerable Legacy Protocols Geolocated: port:23 country:"US"
3. Locating Industrial Control Systems: port:502

<img width="757" height="141" alt="image" src="https://github.com/user-attachments/assets/d9cd3034-0326-483a-8949-a58020419c73" />


