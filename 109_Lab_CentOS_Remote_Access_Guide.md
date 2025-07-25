# 109 Lab CentOS Remote Access Guide

This guide provides instructions on how to connect to, monitor, and transfer files to and from the 109 Lab CentOS workstations using a remote computer.

---

## Prerequisites

Before you begin, you will need two things:

* **Your Workstation's static IP Address**: The unique address of your workstation on the network.
* **GlobalProtect VPN**: The VPN client must be installed on the local machine you are connecting from. Installation instructions can be found here: [https://und.teamdynamix.com/TDClient/2048/IT/KB/ArticleDet?ID=145487](url)

---

## Step 1: Find Your Workstation IP Address

If you don't know your workstation's static IP address, you can find it by opening a terminal on the lab workstation and running the following command:

```bash
ifconfig
```

The static IP address is the number located next to the `inet` keyword (it will be a number of the form: 10.226.47.**). 

---

## Step 2: Connect to the VPN

<img width="295" height="417" alt="VPN" src="https://github.com/user-attachments/assets/0eb266fa-593e-4ba0-b9e1-1b52170be849" />

1.  Open the **GlobalProtect** application on your local machine.
2.  In the **Portal** field, enter `vpn.und.edu` and click **Connect**.
3.  When the NDUS window appears, log in with your UND email and password.

---

## Step 3: Connect to Your Workstation via SSH

1.  Once the VPN connection is active, open a terminal on your local machine. For Windows users, the **Windows PowerShell** application works well.
2.  Use the `ssh` command to securely connect. Replace `yourUserName` with your workstation username and `yourWorkstationIP` with its static IP address. 
    ```bash
    ssh yourUserName@yourWorkstationIP
    ```

3.  You will be prompted to enter your password. Type it in and press Enter to connect. You are now remotely connected to your workstation's command line.

<img width="871" alt="terminal_ssh" src="https://github.com/user-attachments/assets/896641ee-77e9-42ac-adf2-2c31f7edcf23" />

---

## Step 4: Manage Jobs Remotely

### ✅ Running a Job Persistently

To ensure your job continues running even if you disconnect or turn off your local machine, you **must** launch it with the `nohup` command. Without it, the job will terminate as soon as you close the remote session.

1.  Navigate to your case directory.
    ```bash
    cd /path/to/your/case/
    ```
2.  Launch your script using `nohup` and place an ampersand (`&`) at the end to run it as a background process.
    ```bash
    nohup ./your_script_name.sh &
    ```

### 👀 Monitoring a Job

You can monitor the real-time progress of a running job by "tailing" its log file.

```bash
tail -f name_of_your_logfile.log
```

This command will continuously display new lines as they are added to the log file. Press `Ctrl+C` to stop tailing the file.

---

## Step 5: Transfer Files with SCP
To transfer files, open a **new, separate** terminal on your local computer and use the `scp` (secure copy) command.
### 📥 Downloading from the Workstation
To copy a file or directory from the remote workstation to your local machine, use this format:
```bash
# For a single file
scp yourUserName@yourWorkstationIP:/home/yourUserName/remote/path/to/file.tgz /local/path/to/download/

# For a directory (use -r flag)
scp -r yourUserName@yourWorkstationIP:/home/yourUserName/remote/path/to/directory/ /local/path/to/download/
```
* **Source:** The file on the remote workstation.
* **Destination:** The directory on your local machine.

**Note:** Replace `yourWorkstationIP` with the actual IP address you found earlier using the `ifconfig` command.
### 📤 Uploading to the Workstation
To copy a file from your local machine to the remote workstation, use this format:
```bash
# For a single file
scp /local/path/to/your/file.tgz yourUserName@yourWorkstationIP:/home/yourUserName/remote/path/

# For a directory (use -r flag)
scp -r /local/path/to/your/directory/ yourUserName@yourWorkstationIP:/home/yourUserName/remote/path/
```
* **Source:** The file on your local machine.
* **Destination:** The directory on the remote workstation.
