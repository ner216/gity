# Gity

`gity` is a lightweight Python automation tool designed for developers who manage multiple Git repositories across their local machine. Instead of navigating into every directory individually to check for uncommitted changes, pull updates, or fetch remote branches, `gity` scans your project directories and executes Git commands concurrently across all of them at once.

It also includes a built-in "jump" utility to instantly teleport your shell to any repository directory by typing a short command.

---

## Features & Use Cases

* **Bulk Git Status:** Instantly see which repositories have uncommitted changes or untracked files.
* **Bulk Fetch & Pull:** Keep all your local project forks and branches up-to-date with a single command.
* **Smart Repository Search:** Searches standard developer directories (`~/Git`, `~/dev`, `~/projects`, etc.) up to 3 levels deep by default, or scans your entire home directory on demand.
* **The "Jump" Feature (`jp`):** Forget deep nesting paths. Quickly jump directly into any repository just by typing `jp <repo-name>`.

---

## Installation & Setup

To make `gity` available as a global command on your system, follow these steps:

### 1. Copy the Script

Save the script as `gity` (no `.py` extension) in a directory that is in your system's `$PATH` (e.g., `/usr/local/bin` for system-wide access, or `~/.local/bin` for your user only).

### 2. Make it Executable

Open your terminal and run:

```bash
chmod +x /usr/local/bin/gity

```

*(If saving to `/usr/local/bin`, you may need to prepend `sudo`).*

### 3. Verify Installation

Ensure it runs properly by checking the help menu:

```bash
gity --help

```

---

## CLI Options & Usage

```text
usage: gity [-h] [-f] [-p] [-l] [--show-default] [-a] [-g GET_PATH] [-j]

```

### Command Flags

| Flag | Long Flag | Description |
| --- | --- | --- |
| `-h` | `--help` | Show the help message and exit. |
| `-f` | `--fetch` | Run `git fetch` on all discovered repositories. |
| `-p` | `--pull` | Run `git pull` on all discovered repositories. |
| `-l` | `--list` | List the absolute paths of all repositories found by Gity. |
|  | `--show-default` | Display the default directories scanned by Gity. |
| `-a` | `--all` | Recursively search your entire home directory (`~/`) instead of the defaults. |
| `-g` | `--get-path` | Prints the absolute path of a specified repository name (primarily used internally by `jp`). |
| `-j` | `--jump-setup` | Automates the setup for the `jp` (jump) terminal feature. |

### Examples

**Check status of all default repositories:**

```bash
gity

```

**Pull updates for all repositories across your entire home directory:**

```bash
gity -a -p

```

---

## Setting Up the 'Jump' (`jp`) Feature

The "Jump" feature allows you to type `jp <repository-name>` from anywhere in your terminal to instantly change your current directory (`cd`) to that repository and run a `git status` on it.

### Automatic Setup

1. Run the setup flag built into the script:
```bash
gity -j

```


2. Reload your shell configuration:
```bash
source ~/.bashrc

```



### How It Works Behind the Scenes

Because a standalone Python script executes in a subshell, it cannot change the directory of your *parent* terminal session directly. The `-j` flag appends a custom shell function to your `~/.bashrc` file:

```bash
jp() { cd "$(gity -g "$@")" && git status; }

```

When you type `jp my-repo`, the shell function queries `gity -g my-repo` to find the exact absolute path (checking the `.git/config` remote URL first, then falling back to folder names) and passes that path directly to the shell's native `cd` command.
