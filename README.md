# SQLMap Usage Cheat Sheet

This document provides a **practical overview of commonly used SQLMap techniques** during **Web Application Security Testing (VAPT)**.

---

## ⚠️ Disclaimer

This guide is intended strictly for **educational and authorized security testing purposes**.  
Use SQLMap **only on applications you own or have explicit permission to test**.

---

## 🧪 Prerequisites

- SQLMap installed
- Captured HTTP request saved as a `.txt` file (e.g., `demo.txt`)
- Basic understanding of SQL Injection concepts

---

## 1️⃣ Test All Parameters in a Request

SQLMap can test **every parameter present in the HTTP request body, headers, and URL** using a raw request file.

```bash
sqlmap -r demo.txt --dbs --batch --dbms=mysql --tables --dump --threads=3
```

### What this does:
- Automatically tests all parameters found in `demo.txt`
- Detects SQL Injection
- Enumerates available databases if vulnerable

---

## 2️⃣ Escalate SQL Injection to RCE (OS Shell)

If the backend database supports **file read/write operations**, SQLMap can attempt to escalate SQL Injection into **Remote Command Execution (RCE)**.

```bash
sqlmap -r demo.txt --os-shell
```
Now you have to select backend language used
You can pass any commands

### Flow:
- SQLMap detects injectable parameter
- Prompts for backend DB type and OS
- Attempts to upload a payload
- Provides an interactive OS shell

> ⚠️ RCE depends on database privileges and server configuration.

---

## 3️⃣ Bypass WAF Using Tamper Scripts

SQLMap includes **tamper scripts** to bypass poorly configured WAFs and filters.

### List all available tamper scripts:
```bash
sqlmap --list-tampers
```

### Use a specific tamper script:
```bash
sqlmap -r demo.txt --tamper="charencode.py" --dbs
```

### Purpose:
- Obfuscates SQL payloads
- Bypasses basic pattern-based filters
- Helps evade WAF misconfigurations

---

## 4️⃣ Handle Interconnected / Multiple Requests

In some applications, SQL Injection may involve **multiple interconnected requests**.

### Scenario:
- First request triggers SQL Injection
- Second request processes or reflects the result

### Steps:
1. Save the **first request** in `demo.txt`
2. Copy the URL of the **second request**

```bash
sqlmap -r demo.txt --dbs --second-url="URL_OF_SECOND_REQUEST"
```

### Use case:
- Applications with chained requests
- Token-based workflows
- Multi-step data processing

---

## 🧠 Notes

- Always test in **non-production environments**
- Use verbosity flags (`-v`) for deeper debugging
- Combine with manual validation for accuracy
- Not all SQL injections are exploitable via SQLMap

---

## 📚 References

- SQLMap Official Documentation  
  https://github.com/sqlmapproject/sqlmap
- OWASP SQL Injection  
- CWE-89: SQL Injection

---

## 📜 License

This content is intended for **educational and authorized security testing purposes only**
