# 🔴 GTFOBins Privilege Escalation (find)

## 📌 Objective
The aim of this lab is to identify a misconfigured sudo permission and exploit it using GTFOBins to escalate privileges and gain root access.

---

## 🧰 Tools Used
- Kali Linux Terminal
- `sudo`
- GTFOBins

---

## 🔍 Steps Taken

### 1. Check Sudo Permissions
Ran the following command:

```bash
sudo -l
```

This command lists which commands the current user is allowed to run with `sudo`.

---

### 2. Review the Output

```bash
(ALL) NOPASSWD: /usr/bin/find
```

### Breakdown of the Output
- `(ALL)` means the command can be run as any user, including `root`
- `NOPASSWD` means no password is required
- `/usr/bin/find` is the binary that is allowed

This means the `find` binary can be executed with root privileges without authentication, which creates a privilege escalation opportunity.

---

### 3. Identify the Exploitation Method
Searched for `find` on GTFOBins and identified that it can be abused to execute a shell when run with `sudo`.

---

### 4. Execute the Exploit

```bash
sudo find . -exec /bin/bash \; -quit
```

### Breakdown of the Exploit Command

#### `sudo`
Runs the command with elevated privileges. In this case, it runs as `root`.

#### `find .`
Tells `find` to search in the current directory.  
The dot (`.`) means "start searching here".

#### `-exec /bin/bash`
The `-exec` option tells `find` to execute a command.  
Here, the command being executed is `/bin/bash`, which starts a Bash shell.

#### `\;`
This marks the end of the `-exec` command.  
The backslash is used so the shell does not interpret the semicolon before `find` does.

#### `-quit`
Tells `find` to stop after the first match and execution.  
This makes the exploit faster and cleaner because only one shell is needed.

---

### 5. What the Exploit Does
This command abuses the `find` binary to execute `/bin/bash` as `root`.

That results in a **root shell**, which means full administrative access to the system.

---

### 6. Verify Root Access

```bash
whoami
```

Expected output:

```bash
root
```

This confirms that privilege escalation was successful.

---

## 🚨 Findings
- The `sudo -l` output showed that `/usr/bin/find` could be run as `root` without a password
- This created a privilege escalation vulnerability
- The `find` binary was successfully abused to spawn a root shell
- Full administrative access to the system was obtained

---

## 🧠 Key Takeaways
- Misconfigured `sudo` permissions are a major security risk
- `NOPASSWD` allows commands to run without requiring authentication
- GTFOBins is a valuable resource for identifying known privilege escalation techniques
- Legitimate binaries such as `find` can be dangerous if they are given excessive privileges

---

## 📷 Evidence
Add your screenshot here:

```markdown
![sudo -l output showing find as NOPASSWD](images/gtfobins-find.png)
```

---

## ✅ Conclusion
This lab demonstrated how a misconfigured `sudo` rule can be exploited for privilege escalation. Because the `find` binary was allowed to run as `root` without a password, it was possible to abuse its `-exec` functionality to launch `/bin/bash` and gain a root shell. This highlights the importance of restricting `sudo` permissions and regularly reviewing privileged binaries.

---

## 💡 Commands Used

```bash
sudo -l
sudo find . -exec /bin/bash \; -quit
whoami
```
