
# Linux Command Line

## 1. Introduction to the Linux Command Line

### Why Learn Linux?

*   **It Powers the Internet!**
  Leading web servers (Google, Amazon, Facebook) run Linux—making it the backbone of the web.

*   **Dominates the Cloud & Servers.**
  Most cloud infrastructure (AWS, Azure, Google Cloud) and supercomputers rely on Linux.

*  **Runs on Most Smartphones.**
  Android, built on the Linux kernel, powers the majority of smartphones worldwide—meaning billions of devices use it daily.

* **Everywhere from Smart Devices to Supercomputers.**
  Linux is also found in industries like cybersecurity, embedded systems (e.g., routers and Raspberry Pi), high-performance computing, and IoT.

* **A Must‑Have Skill.**
  Whether you’re a software engineer, DevOps professional, system administrator, or cybersecurity expert, knowing how to use Linux is essential—it’s literally what runs the world’s infrastructure.

**In short:** 
  Linux isn’t just *another* operating system—it’s the foundation of almost everything online, from websites to smart gadgets. Learning it gives you the keys to work with servers, cloud systems, and devices at scale.

**Linux at SESAME?**  

Linux is the **core operating system** for controlling synchrotron light sources (like SESAME) because:  

1.  **Reliability & Stability**:  
    Essential for 24/7 operation of accelerators and sensitive experiments.  
2.  **Open & Customizable**:  
    Engineers adapt Linux to interface with unique hardware (e.g., beamline detectors, motors, power supplies, and beam position sensors).  
3.  **Industry Standard**:  
    Synchrotrons like SESAME rely on it for scalability and scientific tool integration.  


### Why Command Line?

* **Automation & Scripting:** Save time with shell scripts.
* **Power & Flexibility:** Configure and troubleshoot beyond GUIs.
* **Efficiency:** One command replaces many clicks.
* **Remote Access:** SSH into servers.
* **Lightweight & Consistent:** Same tools across distros; minimal resources.

### GUI Familiarity Map

| Windows         | Linux (e.g. Ubuntu/Rocky) |
| --------------- | -------------------------- |
| File Explorer   | Files (Dolphin)   |
| Start Menu      | Activities/Menu            |
| Taskbar         | Panel or Dock              |
| Control Panel   | Settings                   |
| Microsoft Store | Software Center            |


### Exercise (on Windows 11) - CLI Efficiency:

1. **Create Demo Folder**  
   Inside your **Downloads** folder, create a directory named "CLI_Demo" (you can do this via GUI or PowerShell)


2. **Open PowerShell**  
   Open **Windows PowerShell** (no admin rights needed - regular user session is safer for file operations)

3. **Navigate to CLI_Demo**  
   ```powershell
   cd "$env:USERPROFILE\Downloads\CLI_Demo"
   ```

4. **Create Test Files**  
   ```powershell
   1..100 | ForEach-Object {
       $types = 'pdf','jpg','txt'
       $months = 'Jan','Feb','Mar','Apr','May','Jun'
       $t = $types[($_ - 1) % 3]
       $m = $months[($_ - 1) % 6]
       New-Item "File${_}_$m.$t" -ItemType File
   }
   ```

5. **Manual Sorting Challenge (GUI Method)**
- **Task:** Organize files into type/month folders using File Explorer
- **Steps:**
  1. Open CLI_Demo in File Explorer
  2. Create folders: pdf, jpg, txt
  3. Move all PDFs to pdf folder (repeat for jpg/txt)
  4. Open pdf folder
  5. Create Jan-Jun subfolders
  6. Move files containing "Jan" to Jan folder (repeat for all months)
  7. Repeat steps 4-6 for jpg and txt folders
- **Timing:**
  - Note start time
  - Perform all steps
  - Note end time and calculate duration.

6. **Reset Environment**

```powershell
# Delete all organized content
Remove-Item -Recurse -Force .\*  # Careful! Only in CLI_Demo
# Regenerate test files (same as Step 2)
1..100 | ForEach-Object {
    $types = 'pdf','jpg','txt'
    $months = 'Jan','Feb','Mar','Apr','May','Jun'
    $t = $types[($_ - 1) % 3]
    $m = $months[($_ - 1) % 6]
    New-Item "File${_}_$m.$t" -ItemType File
}
```

7. **Run Sorting Command**  
   ```powershell
   foreach ($t in @('pdf','jpg','txt')) {
       mkdir $t -ErrorAction Ignore
       Move-Item "*.$t" $t -ErrorAction SilentlyContinue
       Push-Location $t
       foreach ($m in @('Jan','Feb','Mar','Apr','May','Jun')) {
           mkdir $m -ErrorAction Ignore
           Get-ChildItem -File -Filter "*$m*" | Move-Item -Destination $m
       }
       Pop-Location
   }
   ```
**Key Points:**
- **Manual task:** Notice how many steps this involves
- **Automated task:** This same power is amplified in Linux with tools like grep, find, and others
- Now imagine doing this for 10,000 files instead of 100...

---

### Terminal

* **Open** the “Terminal” app.
* Prompt:

  ```
  [user@host ~]$
  ```

  * `user` = your account
  * `host` = computer name
  * `~` = home directory
  * `$` = regular user (use `#` for root)


### First 5 Commands 
Master these essentials—they're the foundation of everything:  

1. **`ls`** → *"What's here?"*  
   ```bash
   ls  # Lists files in current folder
   ```  
2. **`cd`** → *"Navigate to folder"*  
   ```bash
   cd Documents  # Enters your Documents folder
   ```  
3. **`mkdir`** → *"Create folder"*  
   ```bash
   mkdir project_alpha  # Makes new folder
   ```  
4. **`mv`** → *"Move/rename files"*  
   ```bash
   mv draft.txt final_report.txt  # Renames file
   ```  
5. **Install Software** → *"Linux's App Store"*  
   ```bash
   sudo dnf install htop  # Installs system monitor (Fedora/Rocky)
   ```  
   *Note:* `sudo` grants temporary admin rights  


### Quick 10-Minute Quest

1. **Folder Practice**

   ```bash
   cd
   ls
   cd Documents
   mkdir linux_lab
   cd linux_lab
   ```
2. **Install & Play**

   ```bash
   sudo dnf install ninvaders    # Install ninvaders
   ninvaders                     # Arrow keys to play; Ctrl+C to exit
   ```




## 2. Structure of a Command

Every shell command follows:

```bash
command [options] [arguments]
```

* **command**
  The program you run (e.g. `ls`, `cp`, `grep`, `zip`).

* **options**
  Short flags (`-l`, `-r`, `v`, `-p`) that tweak behavior.

* **arguments**
  What you act on—files, directories, patterns, etc.


### Example

```bash
grep -rin "TODO" ~/projects
```

* `grep` → the program
* `-r` → recursive
* `-i` → ignore case
* `-n` → show line numbers
* `"TODO"` → search term
* `~/projects` → where to look

**Sample output**:

```bash
/home/ahmad/projects/main.c:42:// TODO: handle edge case when input is zero
```

## 3. Opening a Terminal and the Prompt

A *terminal emulator* (often just called “Terminal”) is the software you use today to access the CLI. It simulates a physical console. Common terminal emulators include **Terminal** (GNOME), **Konsole** (KDE), and **xterm** (lightweight).

When you open a terminal emulator, you’ll see a *prompt* indicating the system is ready for your input. It usually looks like:

```
user@hostname:~/current/path$
```

* **user:** Your current username.
* **hostname:** The name of your computer.
* **\~:** A shortcut for your home directory (e.g., `/home/yourusername`).
* **/current/path:** Your current working directory. If you’re in your home directory, this will just be `~`.
* **\$:** Indicates you’re a regular user. If it were `#`, you’d be the root (administrative) user.

You can customize this prompt’s appearance and information by editing the `PS1` variable in your `~/.bashrc` file.

## 4. Getting Help

When you need help with a command, the CLI provides several tools:

* `man <command>`: Detailed manual (full documentation).
* `<command> --help`: Quick usage summary (common options).
* `info <command>`: Hyperlinked documentation pages (often more detailed than `man`).
* `apropos <keyword>`: Search man-page descriptions for a keyword.
* `whatis <command>`: One-line summary of what a command does.

**⚡ Bonus Tool: tldr (Too Long; Didn't Read)** – A simplified help resource.

* `tldr <command>`: Practical examples and common use cases for a command. (Note: `tldr` is a separate tool and might need installation.)

### Exercise: Getting Help
```bash
# Task 1: View man page for ls
man ls | head -n5

# Task 2: Get quick help for mkdir
mkdir --help | grep -i "create"

# Task 3: Install tldr & get examples for chmod
sudo dnf install -y tldr
tldr chmod
```

## Homework:
Investigate the `grep` command using these help methods:  

1. **Quick Reference**  
```bash
grep --help | head -5
```  
*What to note*: First usage line and one option description  

2. **Manual Summary**  
```bash
whatis grep
man grep | head -3
```  
*What to note*: One-line summary and brief description  

3. **Practical Examples**  
```bash
tldr grep 2>/dev/null || echo "Example: grep 'error' /var/log/syslog"
```  
*What to note*: One example usage  


**Email submission**:  
- **Subject**: `HW4 – Help Tools – [Your Name]`  
- **Body**:  
  1. Key points from `--help`  
  2. Description from `whatis` and `man`  
  3. One practical example


## 5. Package Management with DNF

Rocky Linux (a Red Hat-based distribution) uses the RPM package ecosystem. The core components are:

* **RPM (Red Hat Package Manager):** The low-level package format (`.rpm` files) containing software and metadata.
* **DNF (Dandified YUM):** The next-generation package manager you’ll interact with. It replaced the older `yum`.
* **Repositories:** Online servers (or local directories) that store collections of RPM packages. DNF automatically fetches packages and their dependencies from repos.

DNF handles software installation, updates, and removal while resolving dependencies automatically. Most DNF commands require `sudo`. When you install software, DNF ensures all required dependencies are also installed.

**Essential DNF Commands:**

* **Install:** `sudo dnf install <package_name(s)>`
  Example: `sudo dnf install htop alacritty`.
* **Remove:** `sudo dnf remove <package_name(s)>`
  Example: `sudo dnf remove htop ulauncher`.
* **Search:** `dnf search <keyword>` (No sudo needed.)
  Example: `dnf search apache` (often `httpd` for the Apache web server).
* **List Installed:** `dnf list installed` (No sudo needed.)
* **Upgrade All (System Update):** `sudo dnf upgrade`
  *Crucial for security; run regularly.* (You can also use `sudo dnf update`.)
* **Upgrade Specific:** `sudo dnf upgrade <package_name>`
  Example: `sudo dnf upgrade firefox`.
* **Reinstall:** `sudo dnf reinstall <package_name>`.
* **Remove Unused Dependencies:** `sudo dnf autoremove`.
* **Clean Caches:** `sudo dnf clean all`.

## Homework:
Perform these tasks **in your terminal** and report outputs:  

1. **Search for Packages**  
   ```bash
   dnf search nano
   ```  
   *What to note*: First package name containing "nano"  

2. **Simulate Installation**  
   ```bash
   sudo dnf install --assumeno nano
   ```  
   *What to note*: Number of packages marked for installation  

3. **Verify & Install**  
   ```bash
   dnf list installed nano || sudo dnf install -y nano
   nano --version
   ```  
   *What to note*: Nano version number  

4. **Simulate Removal**  
   ```bash
   sudo dnf remove --assumeno nano
   ```  
   *What to note*: Number of packages marked for removal  

**Email submission**:  
- **Subject**: `HW5 – Package Practice – [Your Name]`  
- **Body**:  
  1. First matching package from Task 1  
  2. Dependency count from Task 2  
  3. Version output from Task 3  
  4. Removal count from Task 4
 



## 6. Basic Navigation and File Management  
Master these essential commands to navigate and organize your Linux system like a pro:  

#### Core Navigation  
```bash  
pwd                     # Show your current directory (Where am I?)  
ls -lh                  # List files (human-readable sizes)  
cd ~/Documents          # Jump to Documents  
cd ..                   # Move UP one level  
cd -                    # Return to previous directory  
cd                      # Go home (~ shortcut)  
```

#### Directory Operations  
```bash  
mkdir project-alpha      # Create single folder  
mkdir -p lab/{data,code} # Create nested folders in one command  
```

#### File Operations  
```bash  
touch experiment.txt    # Create empty file OR update timestamp  
cp -a template.cfg backup/ # Copy with permissions/timestamps  
mv draft.txt final_report.txt # Rename file  
mv data/ ~/archive/     # Move folder  
```

#### ⚠️ Careful With These!  
```bash  
rm -i temp.log          # Interactive delete (asks confirmation)  
rmdir empty_folder      # Remove EMPTY directory (safe)  
rm -rf old_builds/      # ⚠️ DANGER! Force-deletes folder + contents  
```  
> **Critical Safety Note:**  
> `rm -rf` is irreversible. Double-check paths before running!  
> *(Tip: Use `ls` before `rm` to verify targets)*  

#### ⚡ Bonus Tool: **Ranger**  
A visual file manager for your terminal:  
```bash  
sudo dnf install ranger  # Install  
ranger                   # Launch  
```  
Navigate with **arrow keys**, preview files with **space**, quit with **Q**  
*(Like Windows Explorer but keyboard-driven!)* 

### Exercise: File Operations
```bash
# Task 1: Create project structure
mkdir -p myproject/{docs,src}
touch myproject/README.txt

# Task 2: Copy & rename
cp myproject/README.txt myproject/docs/
mv myproject/docs/README.txt myproject/docs/INSTRUCTIONS.txt

# Task 3: Cleanup
rm myproject/docs/INSTRUCTIONS.txt
```


## Homework:

Perform these tasks **in your terminal** and report outputs:  

1. **Home Directory Check**  
   ```bash
   pwd
   ```
   *What to note*: Your starting directory path  

2. **Create Archive Structure**  
   ```bash
   mkdir -p ~/Documents/conf_2025/slides
   ```

3. **Generate Sample Files**  
   ```bash
   touch ~/Documents/conf_2025/slides/slide{1..3}.pdf
   ```

4. **Copy & Rename Practice**  
   ```bash
   cp -v ~/Documents/conf_2025/slides/slide1.pdf ~/Documents/conf_2025/slides/opening_keynote.pdf
   ```

5. **Create Notes File**  
   ```bash
   touch ~/Documents/conf_2025/qa_notes.txt
   ```

6. **Verify Results**  
   ```bash
   ls -l ~/Documents/conf_2025
   ls ~/Documents/conf_2025/slides
   ```

**Email submission**:  
- **Subject**: `HW6 – File Practice – [Your Name]`  
- **Body**:  
  1. Output of `pwd` (Task 1)  
  2. Output of first `ls` (Task 6) showing `conf_2025` contents  
  3. Output of second `ls` (Task 6) showing `slides` contents 


## 7. Working with File Content  

#### Viewing Files Safely  
```bash  
cat short.txt          # Show SMALL files (⚠️ avoid on large files!)  
less /var/log/messages # Scroll through files safely (↑/↓ arrows, Q to quit)  
```  
> **Pro Tip:** Always use `less` for large files - it won't flood your terminal!

#### Inspecting File Headers & Footers  
```bash  
head -n 10 data.csv    # Show first 10 lines  
tail -n 5 debug.log    # Show last 5 lines  
```

#### Real-Time Monitoring  
```bash  
tail -f /var/log/auth.log  # Watch new entries LIVE (Ctrl+C to stop)  
```  
Perfect for tracking server logs or live application output.

#### Powerful Text Search  
```bash  
grep -i "critical" ~/logs/*.txt  # Find "critical" (case-insensitive) in .txt files  
grep -r "404" /var/www/          # Recursively search directories  
```  
**Key Flags:**  
- `-i`: Case-insensitive  
- `-r`/`-R`: Recursive search  
- `-n`: Show line numbers  
- `-v`: Invert match (exclude pattern)  

#### ⚠️ Critical Safety Notes  
1. **Never** `cat` large files (>100 lines) - it can freeze your terminal  
2. Use `less +F /path/to/log` for safer live viewing than `tail -f`  
   (Press Ctrl+C → Q to exit cleanly)  
3. Avoid `grep -r` on system directories like `/` - target specific folders  


### Exercise: File Content
```bash
# Task 1: Create sample file
echo -e "Line 1\nLine 2\nLine 3" > text.txt

# Task 2: View first 2 lines
head -n2 text.txt

# Task 3: Find "Line 2"
grep "Line 2" text.txt
```

## Homework:

Perform these tasks in your terminal and report outputs:  

1. **Create & View Sample File**  
```bash
echo -e "App log: Info entry\nApp log: ERROR: File missing\nApp log: Warning: Low disk\nApp log: Info: Backup complete" > app.log
head -3 app.log
```  
*What to note*: First 3 lines  

2. **Search for Errors**  
```bash
grep -i "error" app.log
```  
*What to note*: The matching line  

3. **Monitor File Changes**  
```bash
echo "New log entry at $(date)" >> app.log
tail -n1 app.log
```  
*What to note*: The new last line  

**Email submission**:  
- **Subject**: `HW7 – File Practice – [Your Name]`  
- **Body**:  
  1. First 3 lines from Task 1  
  2. Error line from Task 2  
  3. New entry from Task 3  
  4. Why `less` is safer than `cat` for large files
 


## 8. Permissions and Ownership

Permissions and ownership control access to files and directories.

**Viewing Permissions:** Use `ls -l` to see permissions. Example output:

```
drwxr-xr--  2 alice developers 4096 Mar  5 10:00 mydir
```

* The first character shows the file type: `d` (directory), `-` (file), `l` (symbolic link).
* The next nine characters are permissions, divided into three groups of three: **user/owner**, **group**, and **others**.

  * `r` (read), `w` (write), `x` (execute). A `-` means the permission is not granted.

For example, `drwxr-xr--` means: it's a directory (`d`); owner has read/write/execute (`rwx`); group has read/execute (`r-x`); others have only read (`r--`).

### Changing Permissions (`chmod`)

Modify permissions with `chmod`:

* **Symbolic Mode:** Use `u`, `g`, `o`, `a` (user, group, others, all) with `+` (add), `-` (remove), or `=` (set exactly).

  * Example: `chmod u+x script.sh` (add execute for owner).
  * Example: `chmod go-w secret.txt` (remove write for group and others).

* **Numeric (Octal) Mode:** Use three octal digits (`chmod XYZ file`), each digit 0–7: read=4, write=2, execute=1. Add values for each class.

| Sum | Permissions |
| :-: | ----------- |
|  0  | ---         |
|  1  | --x         |
|  2  | -w-         |
|  3  | -wx         |
|  4  | r--         |
|  5  | r-x         |
|  6  | rw-         |
|  7  | rwx         |

* Example: `chmod 755 script.sh` sets `rwxr-xr-x`. (Common for scripts/directories.)
* Example: `chmod 644 config.yaml` sets `rw-r--r--`. (Common for config files.)
* Example: `chmod -R 770 my_project/` sets `rwxrwx---` for the project directory and all its contents (recursive).

### Changing Ownership (`chown`)

Change file/directory owner with `chown` (requires `sudo`):

* `sudo chown <new_owner> <file/dir>`: Change the user owner only.
* `sudo chown :<new_group> <file/dir>`: Change the group owner only.
* `sudo chown <new_owner>:<new_group> <file/dir>`: Change both owner and group.

  * Tip: If you use `sudo chown new_owner:`, the group will default to the new owner's primary group.
* Use `-R` to apply recursively to directories and their contents.

Examples:

```bash
sudo chown alice:developers project/   # owner=alice, group=developers
sudo chown -R bob: project_data/       # owner=bob for project_data/ and all inside; group=bob's group
```

## Homework:
  
#### Task 1: Permission Interpretation  
Interpret these file permissions:  
`-rwxr--r-- 1 ahmad analysts 2048 Jul 10 09:15 secret_notes.txt`  

*Answer these in your email*:  
1. File type: Regular file, Directory, or Symbolic link?  
2. Can ahmad:  
   - Read the file?  
   - Modify the file?  
   - Execute the file?  
3. Can group members:  
   - Read the file?  
   - Modify the file?  
   - Execute the file?  
4. Can others:  
   - Read the file?  
   - Modify the file?  
   - Execute the file?  

#### Task 2: Hands-On Practice  
Follow these steps in your terminal:  

1. **Create test file**  
```bash
touch ~/test_file.txt
```

2. **Check default permissions**  
```bash
ls -l ~/test_file.txt
```

3. **Add execute permission**  
```bash
chmod u+x ~/test_file.txt
```

4. **Verify change**  
```bash
ls -l ~/test_file.txt
```

5. **Remove write permission**  
```bash
chmod go-w ~/test_file.txt
```

6. **Final verification**  
```bash
ls -l ~/test_file.txt
```

**Email submission**:  
- **Subject**: `HW8 – Permissions – [Your Name]`  
- **Body**:  
  1. Task 1 answers  
  2. Output from Step 2  
  3. Output from Step 4  
  4. Output from Step 6  
  5. Why execute permission is dangerous for regular files


## 9. Process Management

Process management involves monitoring and controlling programs running on your Linux system. Understanding processes is crucial for troubleshooting and seeing what your computer is doing.

### The `ps` Command: Viewing Processes

Use `ps` (process status) to see running processes.

```bash
ps
# Example Output:
#    PID TTY          TIME CMD
#   2925 pts/0    00:00:00 bash
#  12968 pts/0    00:00:00 ps
```

* **PID:** Process ID (unique number for each running program).
* **TTY:** Terminal; `?` means not tied to a specific terminal.
* **TIME:** CPU time used by the process (not wall-clock time).
* **CMD:** The command that started the process.

#### Viewing All Your Processes

To see all processes started by your user (even those not tied to the current terminal):

```bash
ps x
```

This shows more processes (e.g., graphical apps). You might see a `STAT` column, indicating the process state (e.g., `S` = sleeping, `R` = running).

#### Viewing All Processes on the System

For all processes on the system (regardless of user):

```bash
ps aux
```

* **USER:** Owner of the process (name, easier than numeric UID).
* **%CPU:** Percent of CPU used by the process.
* **%MEM:** Percent of memory (RAM) used by the process.
* **START:** Time or date when the process began.

Think of `ps aux` as your primary tool for a quick overview of system activity.

### Beyond `ps` (Basic Process Control for Beginners)

* **`top`:** An interactive, real-time process viewer (like a terminal task manager). Displays processes sorted by CPU usage. Press `q` to exit `top`.
* **`kill <PID>`:** Stop a frozen or misbehaving process by its PID. First find the PID (with `ps aux` or `top`), then:

  ```bash
  kill 12345
  ```

  Example: To stop a process with PID 12345, run `kill 12345`.

⚡ **Bonus Tool: htop (Interactive Process Viewer)**
`htop` is an enhanced, user-friendly alternative to `top`. It shows a colorful display of processes and resource meters, with easy keyboard shortcuts to sort or kill processes. Install it on Rocky Linux with:

```bash
sudo dnf install htop
```

Then run `htop`.



## Homework:  
Perform these tasks in your terminal:  

#### Task 1: View Your Processes  
```bash
ps x
```  
*What to note*:  
- PID of your terminal (look for `bash` or your shell name)  
- One background process if available  

#### Task 2: System Process Overview  
```bash
ps aux | head -5
```  
*What to note*:  
- First two processes listed  
- Their USER and %CPU columns  

#### Task 3: Safe Process Handling  
```bash
# Start a harmless sleep process
sleep 60 &

# Find its PID
sleep_pid=$!

# View process details
ps -p $sleep_pid

# Cancel it safely
kill $sleep_pid

# Verify it's gone
ps -p $sleep_pid || echo "Process terminated"
```  
*What to note*:  
- PID of the sleep process  
- Output after killing it  

**Email submission**:  
- **Subject**: `HW9 – Processes – [Your Name]`  
- **Body**:  
  1. Your terminal's PID  
  2. First two processes from `ps aux`  
  3. Sleep process PID  
  4. Termination verification message  
  5. Why we avoid `kill -9`
 



## 10. Useful Utilities

Here are some handy commands and utilities:

```bash
find . -type f -name "*.log" -exec gzip {} \;
tar -cJvf archive.tar.xz /path/to/dir
tar -xzvf archive.tar.xz
sudo <command>
sudo -i                # interactive root shell
```

### Vim (Text Editor)

Vim is a powerful terminal-based text editor, available by default on most Unix-like systems.

**Basic Vim Workflow:**

1. **Start Vim:**

   ```bash
   vim filename.txt
   ```

2. **Vim Modes:**

   * **Normal Mode (default):** Navigate, delete, copy, paste.
   * **Insert Mode:** Type text. Press `i` to enter insert mode.
   * **Command Mode:** Save, quit, and more. Press `:` to enter command mode.

3. **Essential Commands (in Normal Mode):**

   * `i` → Enter insert mode (to start typing).
   * `<Esc>` → Return to normal mode.
   * `:w` → Save (write) the file.
   * `:q` → Quit Vim.
   * `:wq` → Save and quit.
   * `:q!` → Quit without saving.
   * `dd` → Delete (cut) the current line.
   * `yy` → Copy (yank) the current line.
   * `p` → Paste after the current line.
   * `/word` → Search for “word” in the file.
   * `n` → Jump to the next search result.

4. **Exit Vim:** Press `<Esc>`, type `:wq`, and press Enter (save and quit).

**Quick Tip:** If you accidentally open Vim and don’t know how to exit: press `<Esc>`, then type `:q!` and press Enter (force quit without saving).

### Exercise: Find & Archive
```bash
# Task 1: Create files
touch file1.log file2.conf

# Task 2: Find .log files
find . -name "*.log"

# Task 3: Create archive
tar -czf archive.tar.gz file1.log
```

## Homework:  

#### Task 1: Create Practice Files  
```bash
mkdir -p ~/util_practice  
touch ~/util_practice/file{1..3}.tmp  
echo "Original content" > ~/util_practice/notes.txt  
```  

#### Task 2: Compress Files  
```bash  
gzip -v ~/util_practice/*.tmp  
```  
*What to note*: Names of compressed files  

#### Task 3: Create Archive  
```bash  
tar -czvf util_backup.tar.gz ~/util_practice  
```  
*What to note*: First file listed in tar output  

#### Task 4: Vim Editing Practice  
```bash  
vim ~/util_practice/notes.txt  
```  
*Follow these steps in Vim*:  
1. Press `gg` (go to top)  
2. Press `i` (enter insert mode)  
3. Type `# Backup Completed`  
4. Press `Esc`  
5. Type `:wq` + `Enter` (save and quit)  

#### Task 5: Verify Results  
```bash  
head -1 ~/util_practice/notes.txt  
ls ~/util_practice  
```  

**Email submission**:  
- **Subject**: `HW10 – Utilities – [Your Name]`  
- **Body**:  
  1. Compressed file from Task 2  
  2. First file in tar output (Task 3)  
  3. First line of notes.txt (Task 5)


## 11. Basic Networking Commands

### SSH (Secure Shell): Remote Server Management

SSH is a secure way to connect to and manage Linux servers remotely. You can execute commands on a distant server as if you were sitting in front of it.

* **Basic Connection:** Connect by specifying your username and the server’s address:

  ```bash
  ssh user@remote-host
  ```

  * `user`: Your username on the remote server.
  * `remote-host`: The server’s IP address or domain name.
* **First-Time Connection:** The first time you connect, SSH will ask you to confirm the server’s authenticity. Type `yes` and press Enter.
* **Password Prompt:** After confirming, you’ll be prompted for the remote user’s password.
* **Default Username:** If your local username matches your remote username, you can omit `user@`:

  ```bash
  ssh remote-host  # if local_user == remote_user
  ```
* **Specify a Different Port:** If the server uses a non-standard SSH port (not 22), use `-p`:

  ```bash
  ssh -p 2222 user@remote-host
  ```
* **Exiting an SSH Session:** To disconnect, type:

  ```bash
  exit
  ```

### Other Network Commands

* **Test Connectivity (`ping`):** Check if a host is reachable. Example:

  ```bash
  ping -c 4 example.com  # send 4 packets to example.com
  ```
* **Secure Copy (`scp`):** Copy files between local and remote hosts using SSH. Examples:

  ```bash
  scp file.txt user@remote-host:/path/to/dest   # copy local to remote
  scp -r mydir/ user@remote-host:/backup/      # recursively copy directory
  ```
* **Network Statistics (`netstat` / `ss`):** List active connections and ports. `netstat` is older; `ss` is a modern, faster alternative. Examples:

  ```bash
  netstat -tuln   # list listening TCP/UDP ports (numeric)
  ss -tulwn       # same, more verbose
  ```

## Homework:  
#### Task 1: Connectivity Test  
```bash  
ping -c 3 localhost  
```  
*What to note*:  
- Round-trip times  
- Packet loss percentage  

#### Task 2: Network Status Check  
```bash  
ss -tun | head -5  
```  
*What to note*:  
- One listening service  
- Its port number  

#### Task 3: Remote Command Simulation  
```bash  
echo "Simulated remote hostname: $(hostname)"  
```  
*What to note*:  
- Your system's hostname  

**Email submission**:  
- **Subject**: `HW11 – Networking – [Your Name]`  
- **Body**:  
  1. Ping results summary  
  2. One service from `ss` output  
  3. Your hostname

---

### EPICS Installation Task  
**Objective**: Install EPICS Base on Rocky Linux. Document your process and verify success.  

---

### Task Outline  
1. **Identify Prerequisites**  
   Research: What development tools and libraries are required to build EPICS Base?  
   *Hint*: Focus on C/C++ compilers, build tools, and essential libraries.  

2. **Install Dependencies**  
   Using `dnf`, install the required packages.  
   *Document*: List all packages you installed.  

3. **Obtain EPICS Source**  
   Research: Where is the official EPICS source repository?  
   *Action*: Download the latest stable release (not git master).  
   *Document*: Source URL and download command used.  

4. **Build EPICS**  
   Research: What is the standard build command?  
   *Action*: Compile EPICS Base in its directory.  
   *Document*: Full build command and any fixes for errors.  

5. **Configure Environment**  
   Research: What environment variables must be set?  
   *Action*:  
   - Set `EPICS_BASE`  
   - Set `EPICS_HOST_ARCH`  
   - Add binaries to `PATH`  
   *Document*: How you made these changes permanent.  

6. **Verify Installation**  
   *Action*:  
   - Run `softIoc`  
   - Check for `epics>` prompt  
   *Document*: Screenshot of successful prompt.  

---

### Submission Requirements  
**Email Subject**: `HW12 – EPICS Research Install – [Your Name]`  
**Body**:  
1. Prerequisite packages list  
2. EPICS source URL  
3. Build command used  
4. Environment configuration method  
5. `EPICS_HOST_ARCH` value  
6. Verification screenshot  

---

### Research Hints  
| Step | Key Questions | Verification Tips |
|------|---------------|-------------------|
| **1** | What packages provide: <br>- C/C++ compilers? <br>- Make tools? <br>- Perl? <br>- Readline? | `gcc --version` <br>`make --version` |
| **2** | What `dnf group` installs development tools? | `sudo dnf group list hidden` |
| **3** | Where are stable releases on epics-controls.org? | Look for "base-R*" directories |
| **4** | How to start EPICS build? | Run `make` in base directory |
| **5** | What is `EPICS_HOST_ARCH` format? | Match subdirectory in `$EPICS_BASE/bin/` |

---
