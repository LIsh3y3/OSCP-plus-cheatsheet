## Passive (=OSINT)

- [OSINT](../Cheatsheet/Common/OSINT.md)

---
---
## Active

- DNS Enumeration: [53 - DNS](../Cheatsheet/Ports%20-%20Service/53%20-%20DNS.md)

- PortScan：[Port Scan & Vuln Scan](../Cheatsheet/Common/Port%20Scan%20&%20Vuln%20Scan.md)

- SMB Enumeration: [139,445 -NetBIOS, SMB](../Cheatsheet/Ports%20-%20Service/139,445%20-NetBIOS,%20SMB.md)

- SMTP Enumeration: [25,465,587 - SMTP](../Cheatsheet/Ports%20-%20Service/25,465,587%20-%20SMTP.md)

- SNMP Enumeration: [161,162 - SNMP](../Cheatsheet/Ports%20-%20Service/161,162%20-%20SNMP.md)

- Exiftool: [Module 12：Client-side Attacks](Module%2012：Client-side%20Attacks.md#Exiftool)

###### 🤖LLM

- サブドメイン列挙のwordlist作成プロンプト
```txt
Using public data from [domain]'s website and any information that can be inferred about its organizational structure, products, or services, generate a comprehensive list of potential subdomain names.
	•	Incorporate common patterns used for subdomains, such as:
	•	Infrastructure-related terms (e.g., "api", "dev", "test", "staging").
	•	Service-specific terms (e.g., "mail", "auth", "cdn", "status").
	•	Departmental or functional terms (e.g., "hr", "sales", "support").
	•	Regional or country-specific terms (e.g., "us", "eu", "asia").
	•	Factor in industry norms and frequently used terms relevant to [domain]'s sector.

Finally, compile the generated terms into a structured wordlist of 1000  words, optimized for subdomain brute-forcing against [domain]

Ensure the output is in a clean, lowercase format with no duplicates, no bulletpoints and ready to be copied and pasted.
Make sure the list contains 1000 unique entries.
```


