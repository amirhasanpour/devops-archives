# SCP Command Guide

The `scp` (Secure Copy) command is used in Linux and Unix-based systems to securely transfer files and directories between local and remote systems over SSH (Secure Shell). It ensures encrypted file transfers, making it a secure alternative to `cp` when working with remote machines.

## **Basic Syntax**
```sh
scp [options] source destination
```

## **Common Use Cases**

### **1. Copy a file from local to a remote server:**
```sh
scp file.txt user@remote_host:/path/to/destination/
```
- `file.txt` → The file to be copied.
- `user@remote_host` → SSH username and remote server address.
- `/path/to/destination/` → Destination directory on the remote machine.

### **2. Copy a file from remote to local machine:**
```sh
scp user@remote_host:/path/to/file.txt /local/destination/
```

### **3. Copy a directory recursively (-r option):**
```sh
scp -r my_folder user@remote_host:/path/to/destination/
```

### **4. Specify SSH port (-P option):**
```sh
scp -P 2222 file.txt user@remote_host:/path/to/destination/
```
- Useful if the SSH server runs on a non-default port (default is `22`).

### **5. Use a specific private key (-i option):**
```sh
scp -i ~/.ssh/custom_key.pem file.txt user@remote_host:/path/to/destination/
```
- Used when SSH authentication requires a specific private key.

### **6. Limit bandwidth usage (-l option, in Kbit/s):**
```sh
scp -l 500 file.txt user@remote_host:/path/to/destination/
```
- Limits transfer speed to 500 Kbit/s.

### **7. Copy multiple files:**
```sh
scp file1.txt file2.txt user@remote_host:/path/to/destination/
```
