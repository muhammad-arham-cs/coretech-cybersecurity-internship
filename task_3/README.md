Linux Commands for Cybersecurity 
================================

This document contains 22 essential Linux commands every cybersecurity professional must know. Each command includes a short explanation and a screenshot from a real terminal.

1. File System Navigation & File Viewing
----------------------------------------

ls – List directory contents
Shows files and folders in the current directory. Use ls -la to see hidden files and permissions.
<img width="568" height="547" alt="ls-command" src="https://github.com/user-attachments/assets/3895d13b-644b-4ad0-850a-bb98c75ed524" />


cd – Change directory
Moves you into a folder. Example: cd /var/log
<img width="193" height="55" alt="Screenshot 2026-06-06 173643" src="https://github.com/user-attachments/assets/c8394899-bb30-475d-b96b-3c38e94c9e5b" />


pwd – Print working directory
Displays the full path of your current location.
<img width="197" height="58" alt="Screenshot 2026-06-06 173714" src="https://github.com/user-attachments/assets/8d125a6d-6385-414c-b5cc-546438b5fcd5" />


mkdir – Make a new directory
Creates a folder. Example: mkdir myfolder
<img width="1497" height="157" alt="Screenshot 2026-06-06 174037" src="https://github.com/user-attachments/assets/d8720866-d0ad-4e3f-aefe-0491b96db504" />


rm – Remove files or directories
Deletes files. Use rm -r for folders. Be careful – there is no undo.
<img width="1477" height="161" alt="Screenshot 2026-06-06 174107" src="https://github.com/user-attachments/assets/67e2611f-d36e-493b-9559-781326999dc7" />


cat – View file contents
Prints the entire contents of a file to the terminal. Useful for reading logs or config files.
<img width="212" height="67" alt="Screenshot 2026-06-06 174142" src="https://github.com/user-attachments/assets/33d0c2b4-8e6e-48f4-b75b-09facdf23dd2" />



2. File Permissions & Ownership
-------------------------------

chmod – Change file permissions
Modifies read, write, and execute permissions. Example: chmod 755 script.sh
<img width="498" height="69" alt="Screenshot 2026-06-06 174205" src="https://github.com/user-attachments/assets/713533fe-01ba-4d51-bfd3-b89151c4d934" />


chown – Change file owner
Transfers ownership of a file or folder. Example: chown user:group file.txt
<img width="437" height="75" alt="Screenshot 2026-06-06 174231" src="https://github.com/user-attachments/assets/e3ad58b1-cacf-4761-a80f-8cb895dbf175" />



3. User Management
------------------

adduser – Add a new user
Creates a new user account and home directory. Requires root privileges.
<img width="452" height="226" alt="Screenshot 2026-06-06 174326" src="https://github.com/user-attachments/assets/fc26bec6-e1c2-4a70-8243-e1dd22a9b2bb" />


passwd – Change a user’s password
Sets or updates the password for a user account. Example: passwd username
<img width="316" height="115" alt="Screenshot 2026-06-06 174434" src="https://github.com/user-attachments/assets/899f7daf-553a-4fb9-af08-6ddbf3c14deb" />



4. Networking Commands
----------------------

ifconfig or ip a – Show network interfaces
Displays IP addresses, MAC addresses, and interface status. Modern systems prefer ip a.
<img width="658" height="335" alt="Screenshot 2026-06-06 174457" src="https://github.com/user-attachments/assets/13972855-32a7-435b-b9d6-4cd86bdadba3" />


netstat or ss – Show network connections
<img width="830" height="85" alt="Screenshot 2026-06-06 174514" src="https://github.com/user-attachments/assets/f9f07791-5400-4fb7-906e-8203de079884" />


nmap – Network scanner
Scans open ports on a target. Example: nmap -sV 192.168.1.1
<img width="660" height="147" alt="Screenshot 2026-06-06 174752" src="https://github.com/user-attachments/assets/93e3a2bb-4700-4524-9bcd-5e2cfc1f428e" />



5. Process Management
---------------------

ps – List running processes
Shows processes for the current user. ps aux shows every process on the system.
<img width="920" height="725" alt="Screenshot 2026-06-06 174858" src="https://github.com/user-attachments/assets/88d656d2-8053-4478-89b4-5509fedcae38" />


kill – Terminate a process
Sends a signal to stop a process. kill -9 PID forcefully kills it.
<img width="262" height="137" alt="Screenshot 2026-06-06 174939" src="https://github.com/user-attachments/assets/5627edd7-a53a-41bf-afde-2d2521ae0ee3" />


top or htop – Monitor system resources
Shows real-time CPU, memory usage, and running processes. Press q to quit.
<img width="1185" height="728" alt="Screenshot 2026-06-06 175010" src="https://github.com/user-attachments/assets/e37b0ac8-e60a-4029-a159-261f76361bac" />



6. Text Searching & File Finding
--------------------------------

grep – Search inside files
Finds lines that match a pattern. Example: grep "error" /var/log/syslog
<img width="698" height="83" alt="Screenshot 2026-06-06 175033" src="https://github.com/user-attachments/assets/5a47be12-ec3f-4c9a-8232-efcfa7dde74b" />


find – Locate files and directories
Searches for files by name, type, or size. Example: find / -name "passwd" 2>/dev/null
<img width="382" height="63" alt="Screenshot 2026-06-06 175058" src="https://github.com/user-attachments/assets/1388e0a2-8c1e-4835-b818-4c9ba5f62eb5" />



7. Package Management & Sudo
----------------------------

apt-get update – Refresh package lists
Downloads the latest package information from repositories.
<img width="668" height="124" alt="Screenshot 2026-06-06 175143" src="https://github.com/user-attachments/assets/b574b2fb-5cf6-486c-bd32-86f32dfd21bb" />


apt-get install – Install a package
Example: sudo apt-get install wireshark
<img width="352" height="133" alt="Screenshot 2026-06-06 175224" src="https://github.com/user-attachments/assets/cb6ac235-2e79-4c93-b2a0-9953d398c122" />


sudo – Run a command as superuser
Grants temporary root privileges. Almost every system‑wide change needs sudo.
<img width="190" height="58" alt="Screenshot 2026-06-06 175242" src="https://github.com/user-attachments/assets/c0ab7360-4c89-4cef-aa25-84432cc5396d" />
