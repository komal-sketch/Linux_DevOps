                                                                    # Linux Commands Reference

This file is a categorized reference of common Linux commands, user management, file permissions, package management, and file operations.

---

## 1. File and Directory Management

- rm -rf dir_name : Remove a directory and all its contents recursively and forcefully. **Be very careful with this command!**

- cat /etc/passwd : Display user account information stored in the system.

- zip -r <name>.zip <directory_name> :  Compress a directory into a zip file recursively.

- gzip <file> : Compress a file using gzip.

- gunzip <file.gz> : Decompress a gzip file.

- tar -cvzf <file_name>.tar.gz <dir_name> : Create a compressed tarball of a directory.

- tar -xvzf <file_name>.tar.gz : Extract a compressed tarball.

---

## 2. Package Management

- **APT (Debian/Ubuntu)**:
  - `apt`  
  - `apt-get`  
  - `sudo apt remove <name>`  

- **RedHat/CentOS/Fedora**:
  - `yum`  
  - `dnf`  

- **Other package managers**:
  - `packman`  
  - `portage`  

---

## 3. User and Group Management:

- Add a new user:  
                  sudo useradd -m Sameer
                  sudo passwd Sameer

- Delete a user: sudo userdel Jack
- Add a new group: sudo groupadd devops and check groups: cat /etc/group
- Add user to group: sudo gpasswd -a Sameer devops
- Set multiple users in a group: sudo gpasswd -M Sameer,Jack,Ryan,Rehana devops
- Delete a group: sudo groupdel devops


## 4. File Permissions and Ownership:

sudo chmod 400 file_name
sudo chown new_owner_name file_name
sudo chgrp grp_name file_name

umask : Display default file creation permissions.
cat .bashrc : View user shell configuration file.


## 5. Remote File Transfer:

SCP (Secure Copy Protocol): Copy files from local to remote or vice versa.

scp -i "/Path_of_pem_file/name_key.pem" -r file_to_copy.txt ubuntu@ec2-1.1.1.1.us-east-2.compute.amazonaws.com:/path_of_remote_server_directory

rsync : Synchronize files and directories between local and remote systems efficiently.


## 6. Shell Shortcuts:

Ctrl + r : Reverse search previous commands in the terminal.

