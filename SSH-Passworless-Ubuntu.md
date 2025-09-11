SSH Setup - Passwordless access to SSH. 

✅ Step-by-Step Instructions:

### 🔍 1. **Check SSH Service on the Ubuntu Machine**

Make sure SSH server is installed and running.

**Log in physically or via another working connection (e.g., local terminal or remote desktop) and run:**

```bash
sudo systemctl status ssh
```

* If it's **inactive** or **not found**, do the following:

```bash#
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

### 🌐 2. **Check Network and IP Address**

On the Ubuntu machine:

```bash#
ip a
```

Make sure you're using the **correct IP address** when connecting from MobaXterm. It should look something like `192.168.x.x` or `10.x.x.x` (for local networks).

Also, **ensure your Windows PC and Ubuntu machine are on the same network**, especially if using Wi-Fi or LAN.

---

### 🔥 3. **Check Firewall (UFW)**

Ubuntu might be blocking incoming SSH connections.

```bash#
sudo ufw status
```

* If it's active and blocking SSH, allow it:

```bash#
sudo ufw allow ssh
sudo ufw reload
```

---

### 🚪 4. **Verify SSH Port**

By default, SSH runs on **port 22**.

* If you're using a non-standard port, make sure MobaXterm is set to that port in the session settings.
* You can check the port used by SSH with:

```bash#
sudo nano /etc/ssh/sshd_config
```

Look for this line:

```
Port 22
```

If it's something else, use that in MobaXterm.

---

### 🧪 5. **Test SSH Locally**

Try connecting from the Ubuntu machine to itself to test if SSH is working:

```bash#
ssh localhost
```

If this fails, there's an issue with the SSH service itself.

---

### 🖥️ 6. **Ping the Ubuntu Machine from Windows**

From **Windows Command Prompt or MobaXterm terminal**, run:

```bash#
ping <ubuntu_ip>
```
---
### ✅ Final Checklist

| Item                             | Should Be |
| -------------------------------- | --------- |
| SSH server installed and running | ✅        |
| Correct IP and port              | ✅        |
| Ubuntu firewall allows SSH       | ✅        |
| MobaXterm points to private key  | ✅        |
| Machines are on same network     | ✅        |
---
Great! Since SSH is now working, let's set up **auto-connection without a password** using **SSH key-based authentication** in **MobaXterm**.

---

## ✅ Step-by-Step: Passwordless SSH Login from MobaXterm

### 🔹 1. **Generate SSH Key in MobaXterm**

1. Open **MobaXterm**.
2. Go to: **Tools → MobaKeyGen (SSH key generator)**.
3. In MobaKeyGen:

   * Choose key type: **RSA**.
   * Click **Generate**, and move the mouse around to complete key generation.
   * Save the **private key** (e.g., `id_rsa.ppk`).
   * Save the **public key** (e.g., `id_rsa.pub`).
   * **Copy** the entire contents of the box labeled “Public key for pasting into OpenSSH authorized\_keys file”.

---

### 🔹 2. **Add Your Public Key to the Ubuntu Server**

1. Connect to the Ubuntu server (with password) using MobaXterm.
2. On the Ubuntu terminal, run:

```bash#
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

3. Paste the copied **public key** content from MobaKeyGen into the file.
4. Save and exit (`Ctrl+O`, then `Enter`, then `Ctrl+X`).
5. Set correct permissions:

```bash#
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

### 🔹 3. **Configure MobaXterm to Use the Private Key**

1. On the **MobaXterm home screen**, click **Session** → **SSH**.
2. Enter:

   * **Remote host**: your Ubuntu IP.
   * **Specify username**: your Ubuntu username.
3. Go to the **"Advanced SSH settings"** tab:

   * Check **"Use private key"**.
   * Browse and select the `.ppk` file you saved earlier.
4. Click **OK** and connect.

---

### ✅ Test the Connection

You should now connect **without being prompted for a password**.

---

### 💡 Optional: Convert PPK to OpenSSH Format (if needed)

If you want to use OpenSSH-style private keys (e.g., for scripting or use with Linux/macOS tools), you can convert `.ppk` to `id_rsa` using **PuTTYgen**:

1. Open **PuTTYgen** (also part of MobaXterm Tools).
2. Load your `.ppk` file.
3. Go to **Conversions → Export OpenSSH key**.
4. Save as `id_rsa` and use it with native `ssh`.

---

