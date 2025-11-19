# ✅ **Workaround for Experiment No. 9

Launch a Virtual Machine using DevStack (Local OpenStack Demo Environment)**

This replaces TryStack with **DevStack** — the official lightweight OpenStack environment meant for learning, testing, and launching VMs.
It gives the **same dashboard (Horizon)** and **same steps** as TryStack.

---

# **Prerequisites**

1. **A machine or VM running Ubuntu 22.04 or 20.04**
   (Minimum 8 GB RAM, 4 vCPUs recommended)
2. **Internet connection**
3. **Git installed**:

   ```bash
   sudo apt update
   sudo apt install -y git
   ```
4. **A non-root user with sudo privileges**, e.g., `stack`.

---

# **Simplest Working Procedure (DevStack)**

## **Step 1 — Create a non-root user**

```bash
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo su - stack
```

---

## **Step 2 — Download DevStack**

```bash
git clone https://opendev.org/openstack/devstack
cd devstack
```

---

## **Step 3 — Create a simple config file**

```bash
nano local.conf
```

Paste this minimal working configuration:

```
[[local|localrc]]
ADMIN_PASSWORD=admin
DATABASE_PASSWORD=admin
RABBIT_PASSWORD=admin
SERVICE_PASSWORD=admin
```

Save & exit.

---

## **Step 4 — Start OpenStack installation**

```bash
./stack.sh
```

⏳ Takes 10–20 minutes.

If installation completes, you will see:

```
Horizon is now available at http://localhost/dashboard
```

---

## **Step 5 — Log in to Horizon Dashboard**

Open browser:

```
http://<your VM IP>/dashboard
```

Login:

```
Username: admin
Password: admin
```

(Exactly same UI as TryStack.)

---

## **Step 6 — Upload/Create a Key Pair**

Navigate:

```
Compute → Key Pairs → Create Key Pair
```

Download the `.pem` file and secure it:

```bash
chmod 600 mykey.pem
```

---

## **Step 7 — Launch a Virtual Machine**

1. Go to **Compute → Instances**
2. Click **Launch Instance**
3. Configure:

   * **Instance Name:** MyTestVM
   * **Image:** Choose “cirros” (default small test OS)
   * **Flavor:** m1.tiny
   * **Network:** private
   * **Key Pair:** select your key
4. Click **Launch Instance**

Instance state changes: **BUILD → ACTIVE**

---

## **Step 8 — Assign a Floating IP**

(OpenStack always requires it for SSH)
Go to:

```
Instances → > → Associate Floating IP
```

Assign the IP.

---

## **Step 9 — Access the VM through SSH**

Example:

```bash
ssh -i mykey.pem cirros@<floating-ip>
```

If you see:

```
$ uname -a
$ ls
```

Your VM is launched and accessible successfully.

---

# **Output**

* Horizon dashboard screenshots
* Instance status: ACTIVE
* Floating IP
* SSH login terminal output

---

# **Conclusion**

Using DevStack (local OpenStack demo environment), a virtual machine instance was successfully launched, configured, and accessed.
This replicates the full TryStack workflow and provides hands-on experience with:

* Image selection
* Flavor selection
* Networking
* Key pair authentication
* Instance launch & verification