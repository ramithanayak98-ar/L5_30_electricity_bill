# Lab 5 Reflection

## 1. Which issues were easiest and hardest to fix?
The easiest were the Flake8 style problems, because `autopep8` fixed
the spacing and blank lines automatically. The Pylint naming issues were
repetitive but simple. The hardest were the security fixes: the shell
injection in `sync_meter` needed a real change (validate the IP with
`ipaddress`, then use `subprocess.run` with a list and no shell), and I
had to understand why the original was dangerous, not just silence the
warning.

## 2. Did the tools report any false positives or low-value warnings?
Mostly no. Bandit's B105 (hardcoded password) was low severity, but it
pointed at a real bad habit. After switching to `subprocess`, Bandit
raised low-severity notes (B404, B603, B607) that I suppressed with
`# nosec` because the input is validated and no shell is used. A
suppression should always have a reason, not just hide a warning.

## 3. How do the three tools differ?
- **Bandit** found the security problems: shell injection (High),
  unsafe XML parsing (Medium) and a hardcoded password (Low). The other
  two tools missed all of these.
- **Pylint** found real bugs and quality problems: the mutable default
  argument, the unused `currency` parameter, the missing file encoding,
  missing docstrings and naming. Score went from 5.44 to 10.00.
- **Flake8** only checks style: spacing, blank lines and line length.

## 4. How would static analysis fit into a real workflow?
I would run all three tools automatically on every commit or pull
request (a pre-commit hook or CI job) so problems are caught before
review. They do not replace testing: I compared the program output
before and after each change to confirm behavior did not change. Static
analysis finds a class of problems early and cheaply, but it cannot
tell whether the billing logic is correct.
