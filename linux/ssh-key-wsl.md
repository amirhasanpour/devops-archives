# Set up **SSH key-based authentication** using WSL 

---

## **Step 1: Generate an SSH Key Pair on Your WSL Host**
First, check if you already have an SSH key:
```bash
ls -l ~/.ssh/id_rsa.pub
```
- If you see `id_rsa.pub`, you already have a key and can use it.
- If not, generate a new key:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- When prompted for a file to save the key, press **Enter** (this saves it as `~/.ssh/id_rsa`).
- **Do not enter a passphrase** unless required by your organization.

---

## **Step 2: Copy the Public Key to the Linux Server**
You need to add your WSL public key (`id_rsa.pub`) to the Linux server's `~/.ssh/authorized_keys` file.

Run this command from your WSL terminal (replace `your_user` and `server_ip` with actual values):

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub your_user@server_ip
```
- If `ssh-copy-id` is not installed, manually copy the key:
```bash
  cat ~/.ssh/id_rsa.pub
```
  - Copy the output.
  - SSH into the server:
```bash
    ssh your_user@server_ip
```
  - On the **server**, append the key to `authorized_keys`:
```bash
    mkdir -p ~/.ssh
    echo "PASTE_YOUR_KEY_HERE" >> ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys
    chmod 700 ~/.ssh
```

---

## **Step 3: Ensure SSH Server Allows Key Authentication**
On the **Linux server**, check the SSH config file:

```bash
sudo nano /etc/ssh/sshd_config
```
Ensure these settings are enabled:
```text
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no  # Optional: Disables password logins for security
```
Restart SSH service to apply changes:
```bash
sudo systemctl restart ssh
```

---

## **Step 4: Test SSH Connection**
From your **WSL host**, try logging in:
```bash
ssh your_user@server_ip
```
- If it logs in without a password, **key authentication is working**.
- If prompted for a password, ensure the key was correctly added to `~/.ssh/authorized_keys`.

---

## **Step 5: Secure Your SSH Access (Optional)**
Since you have root access, consider improving security:
1. **Disable root login via SSH** *(on the server)*
```bash
   sudo nano /etc/ssh/sshd_config
```
   Set:
```text
   PermitRootLogin no
```
   Restart SSH:
```bash
   sudo systemctl restart ssh
```
2. **Use a Firewall**
```bash
   sudo ufw allow OpenSSH
   sudo ufw enable
```

---

## **Step 6: Use SSH Agent for Convenience (Optional)**
To avoid entering your SSH passphrase each time, add your key to the SSH agent:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

---