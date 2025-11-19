Sharing a folder between a Windows host and an Ubuntu guest is a standard task in VirtualBox, but it requires three distinct steps to work correctly: **Host Configuration**, **Guest Additions Installation**, and **User Permissions**.

Here is the step-by-step guide to setting this up.

### Phase 1: Configure the Shared Folder (Windows Side)

Before starting the Virtual Machine (VM), set up the link in VirtualBox.

1.  **Create a folder** on your Windows Desktop (or anywhere else) that you want to share (e.g., name it `ShareFolder`).
2.  Open **VirtualBox** and select your Ubuntu VM.
3.  Click **Settings** $\rightarrow$ **Shared Folders**.
4.  Click the **Add Folder icon** (blue folder with a green +) on the right.
5.  **Folder Path:** Select the folder you created in Step 1.
6.  **Folder Name:** Give it a simple name (e.g., `myshare`). *Note this name for later.*
7.  **Check these boxes:**
      * [x] **Auto-mount:** This ensures the folder appears automatically every time you boot.
      * [x] **Make Permanent:** This saves the setting for future sessions.
8.  Click **OK** to save.

-----

### Phase 2: Install Guest Additions (Ubuntu Side)

The shared folder feature relies on software called "Guest Additions." If you haven't installed this inside Ubuntu yet, the sharing will not work.

1.  Start your Ubuntu VM and log in.
2.  In the top menu bar of the VirtualBox window, go to **Devices** $\rightarrow$ **Insert Guest Additions CD image...**
3.  Ubuntu should detect the "CD" and ask if you want to run the software. Click **Run**.
      * *If it doesn't auto-run:* Open the "CD" in your file manager, right-click inside the folder, choose "Open in Terminal", and type: `sudo ./VBoxLinuxAdditions.run`
4.  Let the installation finish (it requires your password).
5.  **Reboot the Ubuntu VM** explicitly for the drivers to load.

-----

### Phase 3: Grant Permissions (The "Access Denied" Fix)

This is the step most people miss. By default, VirtualBox locks the shared folder so only the `root` user can open it. You need to add your user to the allowed group.

1.  Open a terminal in Ubuntu (`Ctrl`+`Alt`+`T`).
2.  Run the following command to add your current user to the `vboxsf` group:
    ```bash
    sudo usermod -aG vboxsf $USER
    ```
    *(Note: `$USER` automatically fills in your current username, so you can copy-paste this exactly.)*
3.  **Log out and log back in** (or restart the VM again) for this group change to take effect.

-----

### Phase 4: Access Your Files

Your folder is now ready. You can find it in two places:

1.  **File Manager:** Open "Files" in Ubuntu. You should see `sf_myshare` (or whatever you named it, prefixed with `sf_`) in the left sidebar or under `/media/`.
2.  **Terminal:** You can navigate to it using:
    ```bash
    cd /media/sf_myshare
    ls
    ```

### Troubleshooting

If the folder does not appear automatically, you may need to mount it manually.

1.  Create a folder to act as the mount point:
    ```bash
    mkdir ~/Desktop/WindowsShare
    ```
2.  Mount the folder (replace `myshare` with the name you used in Phase 1):
    ```bash
    sudo mount -t vboxsf myshare ~/Desktop/WindowsShare
    ```

[How to create a shared folder in VirtualBox (Windows host, Ubuntu guest)](https://www.youtube.com/watch?v=h_adwDledYM)
I selected this video because it provides a clear visual walkthrough of the exact process described above, specifically addressing the common "permission denied" error and how to fix it using the terminal.

http://googleusercontent.com/youtube_content/0
