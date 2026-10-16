# Lab 5: Issues Found and Fixed (electricity_bill.py)

Line numbers refer to the original file.

| Issue | Tool / Code | Severity | Line(s) | Description | Fix Approach |
|---|---|---|---|---|---|
| Shell injection | Bandit B605 | High | 63 | `os.system("curl http://" + meterIp ...)` runs through a shell, so a crafted IP could execute arbitrary commands | Validate with `ipaddress.ip_address()`, call `subprocess.run(["curl", url], check=True)` with no shell |
| Unsafe XML parsing | Bandit B314 | Medium | 18 | `ET.fromstring` on XML is vulnerable to XML attacks such as entity expansion | Use `defusedxml.ElementTree` |
| Insecure XML import | Bandit B405 | Low | 3 | `xml.etree.ElementTree` imported for parsing untrusted XML | Import `defusedxml.ElementTree` instead |
| Hardcoded password | Bandit B105 | Low | 13 | `CLERK_PASSWORD = "bill-desk-demo"` stored in source | Read from `os.environ.get("CLERK_PASSWORD")` |
| Mutable default argument | Pylint W0102 | Bug | 42 | `extras={}` is created once and shared across calls | Default to `None`, then `extras = extras or {}` |
| Unused argument | Pylint W0613 | Warning | 50 | `currency` in `averageBill` is never used | Removed the parameter and updated the caller |
| Missing file encoding | Pylint W1514 | Warning | 57 | `open()` without `encoding` depends on the OS default | `open(file_name, "w", encoding="utf-8")` |
| Missing docstrings | Pylint C0114, C0116 | Convention | 1, 17, 22, 29, 42, 50, 56, 61, 66 | Module and 8 functions undocumented | Added docstrings |
| camelCase names | Pylint C0103 | Convention | 17-67 | Functions and variables not in snake_case (e.g. `readMeters`, `meterId`) | Renamed all to snake_case and updated every call |
| Missing blank line | Flake8 E302 | Style | 22 | One blank line before a top-level function | Two blank lines |
| Space before colon | Flake8 E203 | Style | 44 | `"meter" :` | `"meter":` |
| Line too long | Flake8 E501 | Style | 72 | `print(...)` is 86 characters (limit 79) | Wrapped across two lines |
| Missing operator spacing | Flake8 E225 | Style | 76 | `total=sum(...)` | `total = sum(...)` |

## Results

| Tool | Before | After |
|---|---|---|
| Pylint | 5.44/10 | 10.00/10 |
| Bandit | 4 issues | 0 issues |
| Flake8 | 4 warnings | 0 warnings |

Program output was verified identical before and after the changes.
