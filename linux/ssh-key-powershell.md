# Set up **SSH key-based authentication** using PowerShell

---

## **Step 1: Check for Existing SSH Keys**
On your **PowerShell host**, open PowerShell and check if you already have SSH keys:
```powershell
ls ~/.ssh/
```
Look for files named:
- `id_rsa` and `id_rsa.pub` (RSA keys)  
- `id_ed25519` and `id_ed25519.pub` (Ed25519 keys)  

If you don’t have them, generate a new key pair.

---

## **Step 2: Generate a New SSH Key (If Needed)**
If no keys exist, generate a new SSH key pair using PowerShell:
```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Press **Enter** to save it in `~/.ssh/` (default location).  
- If asked, you can **set a passphrase** (optional for extra security).

After this, you should have:
- **`id_ed25519`** (private key)  
- **`id_ed25519.pub`** (public key)

---

## **Step 3: Copy the Public Key to the Linux Server**
Now, copy your public key to the server. Run:
```powershell
scp ~/.ssh/id_ed25519.pub user@your_server_ip:/tmp/
```
Replace:
- `user` → Your Linux server username  
- `your_server_ip` → Server’s IP address  

Alternatively, you can use:
```powershell
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@your_server_ip
```
If `ssh-copy-id` is not available, use the manual method (Step 4).

---

## **Step 4: Configure the Linux Server**
Log in to the Linux server manually using a password:
```powershell
ssh user@your_server_ip
```
Once inside the Linux server:
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cat /tmp/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
rm /tmp/id_ed25519.pub
```
This:
- Creates the `.ssh` folder if it doesn’t exist.
- Adds your public key to `authorized_keys`.
- Ensures correct permissions.

---

## **Step 5: Test SSH Key Login**
On your PowerShell host, try SSH again:
```powershell
ssh user@your_server_ip
```
If it logs in **without asking for a password**, the setup is correct.

---

## **Step 6: Disable Password Authentication (Optional)**
For better security, edit the SSH config on the server:
```bash
sudo nano /etc/ssh/sshd_config
```
Find and modify:
```bash
PasswordAuthentication no
PubkeyAuthentication yes
```
Then restart SSH:
```bash
sudo systemctl restart ssh
```
This ensures **only SSH keys** can be used to log in.

---