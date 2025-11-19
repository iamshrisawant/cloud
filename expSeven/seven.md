
# **Experiment No. 7 — Simplest Working Procedure**

## **Title**

Transfer files from one virtual machine to another virtual machine.

---

## **Prerequisites**

1. **Both VMs must run Linux (Ubuntu recommended).**
2. **Both VMs must be on the same network** (Host-Only, NAT-Network, or Bridged).

   * Check IP addresses using:

     ```bash
     ip addr
     ```
   * Both IPs should be reachable:

     ```bash
     ping <destination-ip>
     ```
3. **SSH server installed on both VMs.**
4. **You know the username and password of the destination VM.**

---

# **Procedure (Shortest Working Version)**

## **Step 1 — Install SSH on both VMs**

Run on **both VM-A and VM-B**:

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

---

## **Step 2 — Find the destination VM's IP address**

On **VM-B (destination)**:

```bash
ip addr
```

Example IP: `192.168.56.102`

---

## **Step 3 — Check connectivity**

On **VM-A (source)**:

```bash
ping -c 3 192.168.56.102
```

If ping replies → connection is OK.

---

## **Step 4 — Transfer a file using SCP**

On **VM-A (source)**:

```bash
scp file.txt user@192.168.56.102:/home/user/
```

* Replace `user` with the actual username on VM-B.
* Enter VM-B's password when asked.

For a folder:

```bash
scp -r myfolder user@192.168.56.102:/home/user/
```

---

## **Step 5 — Verify the transfer**

On **VM-B (destination)**:

```bash
ls -l /home/user/
```

You should see:

```
file.txt
myfolder/
```
