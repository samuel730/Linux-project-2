## 2. Lab Environment

- Host OS: Windows

- Virtualization Tool: VirtualBox

- VM Management Tool: Vagrant

- Guest OS: Ubuntu
## 3. Procedure and Implementation
### Step 1: Creating the Project Directory

Commands used:
```
mkdir pistis
cd pistis
```
This created a working directory named pistis.

<img width="1004" height="64" alt="Screenshot 2026-02-27 153428" src="https://github.com/user-attachments/assets/1faca25b-a7ac-441a-a8d5-7a2df6dcfe15" />
### Step 2: Creating Sample File and Directory
``` 
touch example.txt
mkdir exampledir
ls -l
```
Initial permissions observed:
```
-rw-rw-r-- example.txt
drwxrwxr-x exampledir
```
<img width="1184" height="235" alt="Screenshot 2026-02-27 153738" src="https://github.com/user-attachments/assets/5844c3f0-3fb6-4436-985a-626de4dfed29" />
### Step 3: Modifying File Permissions
```
chmod u+rw example.txt
ls -l
```
The owner was granted read and write permissions.

<img width="1230" height="192" alt="Screenshot 2026-02-27 153952" src="https://github.com/user-attachments/assets/82377df4-23b9-4a6b-b12c-7b36dbab9e88" />
### Step 4: Modifying Directory Permissions
```
chmod u+rwx exampledir
ls -ld exampledir
```
The owner was granted full permissions (read, write, execute).

<img width="1230" height="121" alt="Screenshot 2026-02-27 154449" src="https://github.com/user-attachments/assets/b35f15c8-53f6-43c2-9a12-b9b478c9fb69" />
### Step 5: Creating a New Group
```
sudo groupadd pistisgroup
```
verification: 
```
cat /etc/group | grep pistisgroup
```
<img width="1297" height="125" alt="Screenshot 2026-02-27 154850" src="https://github.com/user-attachments/assets/10b29cf5-1809-4e69-9002-4dfe4f168ff4" />
### Step 6: Creating a New User
```
sudo useradd -m -G pistisgroup pistisuser
sudo passwd pistisuser
```
This created a new user and added them to pistisgroup.

<img width="1640" height="184" alt="Screenshot 2026-02-27 155246" src="https://github.com/user-attachments/assets/b0bbbe19-aa96-4962-818f-a3be5010b564" />
### Step 7: Changing File and Directory Ownership
```
sudo chown -R pistisuser:pistisgroup /home/pistisuser/pistis
ls -l
```
Ownership successfully changed to:
```
pistisuser pistisgroup
```
<img width="1830" height="409" alt="Screenshot 2026-02-27 155630" src="https://github.com/user-attachments/assets/b83c46c1-e147-49c8-ba84-50c7184a9e7d" />
### Step 8: Testing Access as New User
```
sudo su - pistisuser
echo "Permission test successful" >> example.txt
cat example.txt
```
This confirmed that the new owner had write access.

<img width="1317" height="298" alt="Screenshot 2026-02-27 155756" src="https://github.com/user-attachments/assets/8e756a68-f813-4987-9ea1-acb42390a905" />
### Step 9: Applying Special Permissions
### Setuid
```
sudo chmod u+s example.txt
ls -l example.txt
```
Result:
```
-rwSrw-r--
```
<img width="1633" height="154" alt="Screenshot 2026-02-27 160317" src="https://github.com/user-attachments/assets/521d1b46-4f12-4987-8e0e-a41256f6c7c4" />
### Setgid
```
sudo chmod g+s exampledir
ls -ld exampledir
```
Result:
```
drwxrwsr-x
```
<img width="1479" height="114" alt="Screenshot 2026-02-27 160551" src="https://github.com/user-attachments/assets/e2877c47-3e12-4392-91b3-9a0e996f601b" />

### Sticky Bit
```
sudo chmod +t exampledir
ls -ld exampledir
```
Result:
```
drwxrwsr-t
```
<img width="1685" height="114" alt="Screenshot 2026-02-27 160741" src="https://github.com/user-attachments/assets/efc4b0a7-e0c6-4996-bda4-4dfc4998bc2f" />
### Step 10: Recursive Permission Change
```
sudo chmod -R 755 exampledir
ls -ld exampledir
```
Result:
```
drwxr-sr-x
```
<img width="1533" height="116" alt="Screenshot 2026-02-27 161048" src="https://github.com/user-attachments/assets/d7080f8b-8fbb-440e-9bf0-b3c16ad48d37" />
### Step 11: Checking and Modifying umask

Check default umask:
```
umask
```
Output:
```
0002
```
Change umask and create a new file:
```
umask 0027
touch newfile
ls -l newfile
```
Result:
```
-rw-r-----
```
This demonstrated how umask controls default file permissions.

<img width="1550" height="1120" alt="Screenshot 2026-02-27 161323" src="https://github.com/user-attachments/assets/e990a5b9-ed2f-4391-8e4c-57e4d4d02e2e" />
### 4. Challenges Encountered

During the lab, the following issues were encountered:

- Permission denied errors when logged in as a non-owner.

- User not in sudoers file error.

- Directory access errors due to incorrect path.

- Write restriction due to directory permission settings.

These issues were resolved by:

- Switching to the correct user.

- Using sudo from the administrative account.

- Correcting directory paths.

- Understanding ownership and permission hierarchy.
### 5. Conclusion

This lab provided practical experience in Linux file system security and permission management. Through this exercise, I learned how to:

- Modify file and directory permissions using chmod.

- Change file ownership using chown.

- Manage group ownership using chgrp.

- Create and manage users and groups.

- Apply special permissions (Setuid, Setgid, Sticky Bit).

- Control default permissions using umask.

- Troubleshoot permission-related errors.

The lab successfully demonstrated how Linux enforces access control and how system administrators manage file security in multi-user environments.

### 6. References

Canonical. (n.d.). Ubuntu documentation. https://help.ubuntu.com

Shotts, W. E. (2019). The Linux command line: A complete introduction. No Starch Press.

