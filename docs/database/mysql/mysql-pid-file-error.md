# Fixing MySQL "PID file could not be found" Error

When MySQL reports the following error:

```text
ERROR! MySQL is running but PID file could not be found
```

it usually means that the server is running, but the **PID file** (which stores the process ID of the MySQL server) is
missing or has been deleted. Without this file, MySQL cannot track or manage its own process correctly.

This issue often occurs on macOS systems where MySQL was installed via **Homebrew** or other package managers.

---

## Steps to Resolve

### 1. Check MySQL Status

Run the following command to verify the issue:

```bash
mysql.server status
```

If you see:

```text
ERROR! MySQL is running but PID file could not be found
```

then proceed to the next steps.

---

### 2. List All Running MySQL Processes

Find out which MySQL processes are currently running:

```bash
ps aux | grep mysql
```

This will list all processes related to MySQL (including the `grep` command itself).

---

### 3. Kill MySQL Processes

Stop all MySQL processes by killing them manually:

```bash
sudo kill <pid1> <pid2> <pid3> ...
```

Replace `<pid1>`, `<pid2>`, etc., with the process IDs you found in the previous step.

---

### 4. Restart MySQL

Now try starting MySQL again:

```bash
mysql.server start
```

If successful, you should see:

```text
Starting MySQL . SUCCESS!
```

---

## Permanent Fix (Optional)

If you frequently encounter this problem, you may need to define a **PID file location** in your MySQL configuration.

- Locate your PID file (if it exists):

```bash
/usr/local/var/mysql/{username}.pid
```

- Locate your MySQL configuration file (installed with Homebrew):

```bash
/usr/local/Cellar/mysql/<version>/my.cnf
```

- Edit the configuration file:

```bash
subl ~/.my.cnf
```

- Add the following line to the bottom of the file (replace `{username}` with your macOS username):

```bash
pid-file = /usr/local/var/mysql/{username}.pid
```

- Save the file and restart MySQL.

---

## Summary

- The error occurs because the **PID file** is missing.
- Kill all existing MySQL processes.
- Restart the MySQL server.
- (Optional) Configure a fixed **PID file path** in `my.cnf` to prevent the issue from reoccurring.

---

## References

- [StackOverflow: Error - MySQL manager or server PID file could not be found](https://stackoverflow.com/questions/17424863/error-mysql-manager-or-server-pid-file-could-not-be-found-qnap)
