# Linux & Git Command Line Fundamentals

Welcome to the **Linux & Git Command Line Fundamentals** repository! This guide will help you learn important linux commands which you'll be using regularly in near future in your Cybersecurity Journey.

## Table of Contents

1. [File & Directory Management](#1-file--directory-management)

2. [Viewing, Editing & Inspecting Files](#2-viewing-editing--inspecting-files)

3. [Text Processing, Encoding & Comparison](#3-text-processing-encoding--comparison)

4. [Piping & Redirection (Streams)](#4-piping--redirection-streams)

5. [Search & Archiving](#5-search--archiving)

6. [Users, Permissions & Privilege](#6-users-permissions--privilege)

7. [Networking & Remote Access](#7-networking--remote-access)

8. [Git Version Control](#9-git-version-control)

## 1. File & Directory Management

Navigating the filesystem and managing files are the building blocks of using the command line.

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `pwd` | **P**rint **W**orking **D**irectory. Shows your current folder path. | `pwd` | 
| `ls` | **L**i**s**ts files and directories. Use `-la` to see hidden files and details. | `ls -la` | 
| `cd` | **C**hange **D**irectory. Move between folders. | `cd /var/log` or `cd ..` (go back) | 
| `touch` | Creates an empty file or updates the timestamp of an existing file. | `touch notes.txt` | 
| `cp` | **C**o**p**ies files or directories (use `-r` for recursive directory copying). | `cp source.txt backup.txt` or `cp -r dir1/ dir2/` | 
| `mv` | **M**o**v**es or renames files and directories. | `mv oldname.txt newname.txt` or `mv file.txt /tmp/` | 
| `rm` | **R**e**m**oves (deletes) files or directories (with `-r`). | `rm file.txt` or `rm -rf folder/` | 
| `rmdir` | **R**e**m**oves an *empty* **dir**ectory. | `rmdir empty_folder/` | 
| `shred` | Overwrites a file to hide its contents, optionally deleting it (`-u`), making recovery very difficult. | `shred -u -v secret.txt` | 

## 2. Viewing, Editing & Inspecting Files

Quickly inspect file contents, metadata, or edit configurations directly inside the terminal.

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `cat` | Con**cat**enates and displays the entire contents of a file on the screen. | `cat /etc/os-release` | 
| `head` | Outputs the first part of a file (defaults to the first 10 lines; use `-n` to specify). | `head -n 5 access.log` | 
| `tail` | Outputs the last part of a file (use `-n` for line count, or `-f` to follow live updates). | `tail -n 20 error.log` or `tail -f syslog` | 
| `file` | Determines the actual file type by inspecting its header/magic bytes, ignoring the file extension. | `file mystery_data` | 
| `stat` | Displays detailed status and metadata about a file or filesystem (size, inodes, permissions, timestamps). | `stat index.html` | 
| `echo` | Prints text to the terminal window or redirects it into a file. | `echo "Hello World"` | 
| `nano` | A beginner-friendly, straightforward command-line text editor. (Save with `Ctrl+O`, Exit with `Ctrl+X`). | `nano config.json` | 
| `vi` | A powerful, ubiquitous modal text editor found on almost every UNIX system. (Press `i` to insert text, `Esc` then `:wq` to save and quit). | `vi script.sh` | 

## 3. Text Processing, Encoding & Comparison

Essential tools for slicing, sorting, counting, encoding, and comparing text streams or binaries (heavily used in log analysis and security challenges like OverTheWire Bandit).

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `grep` | Searches for a specific pattern or regular expression inside files or input streams. | `grep -i "error" logfile.txt` | 
| `cut` | Removes/extracts sections from each line of input using a delimiter (`-d`) and field number (`-f`) or character position (`-c`). | `cut -d ':' -f 1 /etc/passwd` | 
| `sort` | Sorts lines of text files alphabetically or numerically (`-n`), or in reverse (`-r`). | `sort -n numbers.txt` | 
| `uniq` | Reports or filters out repeated *adjacent* lines in a file (almost always paired after `sort`). Use `-u` for unique only, `-c` to count occurrences. | `sort data.txt | uniq -c` | 
| `wc` | **W**ord **C**ount. Prints newline (`-l`), word (`-w`), and byte (`-c`) counts for files or input streams. | `wc -l /etc/passwd` | 
| `tr` | **Tr**anslates or deletes characters from standard input (useful for case conversion or ROT13 ciphers). | `echo "hello" | tr 'a-z' 'A-Z'` | 
| `rev` | **Rev**erses lines characterwise. | `echo "ressed" | rev` (Outputs: `dessert`) | 
| `base64` | Encodes or decodes data into Base64 format. Use `-d` to decode. | `echo "SGVsbG8=" | base64 -d` | 
| `strings` | Extracts and prints readable text strings from binary or non-text files. | `strings program.bin` | 
| `cmp` | **C**o**mp**ares two files byte by byte and reports the first difference. | `cmp file1.bin file2.bin` | 
| `diff` | Compares files line by line and shows what needs to change to make them identical. | `diff -u old_code.py new_code.py` | 

## 4. Piping & Redirection (Streams)

In Linux, almost everything is treated as a stream of text. Every command interacts with three standard data streams:

* **`stdin` (0)**: Standard Input (defaults to your keyboard).

* **`stdout` (1)**: Standard Output (defaults to your terminal screen).

* **`stderr` (2)**: Standard Error (error messages, defaults to your terminal screen).

You can chain commands together or redirect where these streams go using **pipes** and **redirection operators**:

| Operator | Name | Description | Example Usage | 
| ----- | ----- | ----- | ----- | 
| `\|` | **Pipe** | Sends the `stdout` of the command on the left directly into the `stdin` of the command on the right. | `cat access.log \| grep "404" | wc -l` | 
| `>` | **Redirect Output (Overwrite)** | Captures `stdout` and writes it to a file, **overwriting** the file if it already exists. | `echo "First line" > notes.txt` | 
| `>>` | **Redirect Output (Append)** | Captures `stdout` and **appends** it to the bottom of an existing file without erasing its contents. | `echo "Second line" >> notes.txt` | 
| `<` | **Redirect Input** | Feeds the contents of a file directly into a command's `stdin`. | `sort < unsorted_list.txt` | 
| `2>` | **Redirect Error** | Captures `stderr` (error messages) and sends them to a file. (Often sent to `/dev/null` to hide permission errors). | `find / -name "flag.txt" 2> /dev/null` | 
| `&>` or `2>&1` | **Redirect Both** | Combines both normal output (`stdout`) and errors (`stderr`) into the same file. | `./script.sh > full_output.log 2>&1` | 

## 5. Search & Archiving

Locate deeply nested files or bundle directories together for storage and transfer.

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `find` | Searches for files and directories in a directory hierarchy based on name, size, permissions, etc. | `find /home -name "*.txt" -size +1M` | 
| `zip` | Compresses and packages files into a `.zip` archive. | `zip -r archive.zip project_dir/` | 
| `unzip` | Extracts files from a `.zip` archive. | `unzip archive.zip -d destination/` | 

## 6. Users, Permissions & Privilege

Linux is a multi-user system. These commands manage who you are and who can access what.

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `whoami` | Displays the username of the current effective user. | `whoami` | 
| `passwd` | Changes the password for a user account. | `passwd` | 
| `sudo` | **S**uper**u**ser **Do**. Executes a command with administrative/root privileges. | `sudo apt update` | 
| `chmod` | **Ch**anges the **mod**e (read, write, execute permissions) of a file or directory. | `chmod +x script.sh` or `chmod 755 file` | 

## 7. Networking & Remote Access

Test connectivity, inspect network paths, and connect to remote servers or services.

| Command | Description | Example Usage | 
| ----- | ----- | ----- | 
| `ping` | Sends ICMP ECHO_REQUEST packets to network hosts to test reachability and latency. | `ping -c 4 google.com` | 
| `traceroute` | Prints the route (hops) that packets take to reach a network host. | `traceroute github.com` | 
| `curl` | **C**lient **URL**. Transfers data to or from a server using HTTP/HTTPS, FTP, and more. Great for testing APIs, inspecting headers (`-I`), or downloading files (`-O`). | `curl -I https://example.com` or `curl -s http://ipinfo.io/ip` | 
| `ssh` | **S**ecure **Sh**ell. Securely connects to a remote machine over an encrypted network connection. | `ssh user@192.168.1.50 -p 22` | 
| `nc` | **N**et**c**at. The "Swiss-army knife" of networking. Reads and writes data across network connections using TCP or UDP. | `nc -nv 10.0.0.1 8080` or `nc -lvnp 4444` | 


## 8. Git Version Control

Git is the industry-standard distributed version control system for tracking changes in source code.

### Setup & Initialization

* **`git config`**: Sets configuration values for your username, email, and preferences.

  ```
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

* **`git clone`**: Downloads an existing remote repository to your local machine.

  ```
  git clone https://github.com/username/repository.git
  ```

### Staging & Saving Changes

* **`git add`**: Adds modified or new files in your working directory to the staging area.

  ```
  git add index.html        # Stage a specific file
  git add .                 # Stage all changes
  ```

* **`git commit`**: Captures a snapshot of the currently staged changes and saves it to the project history.

  ```
  git commit -m "Add user authentication feature"
  ```

* **`git push`**: Uploads your local repository commits to a remote repository (like GitHub).

  ```
  git push origin main
  ```

### Branching & Navigation

* **`git branch`**: Lists, creates, or deletes branches.

  ```
  git branch                # List local branches
  git branch feature-login  # Create a new branch
  git branch -d old-branch  # Delete a branch
  ```

* **`git switch`**: A modern, dedicated command to switch between branches (introduced in Git 2.23).

  ```
  git switch feature-login      # Switch to existing branch
  git switch -c new-feature     # Create and switch to a new branch
  ```

* **`git checkout`**: The classic versatile command used to switch branches, inspect old commits, or restore working tree files.

  ```
  git checkout main             # Switch branch
  git checkout -b hotfix        # Create and switch to branch
  git checkout -- broken.txt    # Discard local changes in a file
  ```

### Inspecting History & Recovery

* **`git log`**: Shows the chronological commit history for the currently active branch.

  ```
  git log                       # Detailed commit log
  git log --oneline --graph     # Compact visual commit tree
  ```

* **`git show`**: Displays detailed metadata and the exact content diff of a specific commit, tag, or object.

  ```
  git show                      # Inspect the latest commit
  git show a1b2c3d              # Inspect a specific commit hash
  ```

* **`git reflog`**: **Ref**erence **log**s. Tracks every movement of `HEAD` (commits, branch switches, resets). It is your ultimate safety net for recovering "lost" commits or deleted branches!

  ```
  git reflog
  # Find the hash before a mistake (e.g., HEAD@{2}) and restore it:
  git checkout a1b2c3d
  ```

---
