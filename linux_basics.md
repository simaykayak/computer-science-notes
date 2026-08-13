# Linux Basics

## 1. What is Linux?

Linux is an open-source, Unix-based family of operating systems. It is widely used in servers, software development environments, cloud systems, and embedded systems.

---

## 2. Basic Terminal Commands

In Linux, various commands are used to work with files and directories through the terminal.

| Command | Function |
|---|---|
| `pwd` | Shows the current directory |
| `ls` | Lists the contents of a directory |
| `cd` | Changes the current directory |
| `mkdir` | Creates a new directory |
| `touch` | Creates a new empty file |
| `cp` | Copies a file or directory |
| `mv` | Moves or renames a file |
| `rm` | Removes a file or directory |
| `cat` | Displays the contents of a file |
| `grep` | Searches for text |

### Example

```bash
mkdir linux-practice
cd linux-practice
touch notes.txt
ls
```

These commands create a directory called `linux-practice`, move into that directory, create an empty file called `notes.txt`, and list the contents of the directory.

---

## 3. Grep and Pipe Usage

In Linux, commands can be combined to work together.

### Pipe `|`

A pipe passes the output of one command as the input to another command.

```bash
ls | grep ".txt"
```

This command filters the output of `ls` and displays only the results containing `.txt`.

### Output Redirection

```bash
echo "Linux Notes" > notes.txt
```

The `>` operator redirects the output to a file. If the file already contains data, its existing content is overwritten.

```bash
echo "New line" >> notes.txt
```

The `>>` operator appends the output to the end of the file without deleting its existing content.

---

## 4. File Permissions

In Linux, access permissions for files and directories are defined by three basic permissions:

| Permission | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |

Permissions are defined separately for the **owner, group, and others**.

For example:

```text
rwxr-xr-x
```

can be divided as:

```text
rwx | r-x | r-x
Owner | Group | Others
```

### chmod

The `chmod` command is used to change file permissions.

```bash
chmod +x script.sh
```

This command gives execute permission to `script.sh`.

Permissions can also be represented numerically:

```text
r = 4
w = 2
x = 1
```

For example:

```bash
chmod 755 script.sh
```

sets the permissions to:

```text
rwxr-xr-x
```

---

## 5. Package Management

Linux distributions use package managers to install, update, and manage software packages.

| Distribution | Package Manager |
|---|---|
| Ubuntu / Debian | `apt` |
| Arch Linux | `pacman` |
| Fedora | `dnf` |

Ubuntu/Debian example:

```bash
sudo apt update
sudo apt install git
```

Arch Linux:

```bash
sudo pacman -S git
```

Fedora:

```bash
sudo dnf install git
```

---

## 6. Process Management

A running program is managed by the operating system as a **process**.

To display running processes:

```bash
ps
```

or:

```bash
top
```

To find a specific process:

```bash
ps aux | grep program
```

Each process has its own **PID (Process ID)**.

To terminate a process:

```bash
kill PID
```

---

## 7. Service Management – systemctl

Many system services running in the background on Linux are managed by `systemd`.

To check the status of a service:

```bash
systemctl status ssh
```

To start a service:

```bash
sudo systemctl start ssh
```

To stop a service:

```bash
sudo systemctl stop ssh
```

To restart a service:

```bash
sudo systemctl restart ssh
```

To enable a service to start automatically when the system boots:

```bash
sudo systemctl enable ssh
```

---

## 8. Important Linux Directories

The Linux file system starts from the `/` directory, which is called the **root directory**.

| Directory | Function |
|---|---|
| `/home` | Contains users' personal files |
| `/etc` | Contains system configuration files |
| `/var` | Contains logs and variable system data |
| `/tmp` | Contains temporary files |
| `/usr` | Contains programs and system resources |
| `/dev` | Contains representations of hardware devices |