### Prerequisites**

1. **SSH Private Key**: Ensure you have your SSH private key (e.g., `tech501-haroon-az-key`) ready.
    
2. **Azure VM Details**:
    
    - **Username**: Your VM’s username (e.g., `adminuser`).
    - **IP Address**: The public IP address of your Azure VM (e.g., `20.117.122.166`).
3. **Local Path to Folder**: The folder you want to copy is located on your local machine at:
    
    
    
    `C:\Users\ixHar\nodejs20-sparta-test-app`
    
4. **Tools**:
    
    - Make sure you have **Git Bash** or a terminal that supports `scp` (Secure Copy Protocol) installed on your local machine.
    - Ensure `scp` is available in your terminal. If you're using Git Bash or WSL, this should be available by default.

---

### **Step-by-Step Instructions**

#### **Step 1: Prepare Your Command**

- Convert the local folder path to a Linux-style path for `scp`. On Windows, this should be:
    
    
    
    `/c/Users/ixHar/nodejs20-sparta-test-app`
    
- Here is the full `scp` command to copy the folder to your Azure VM:
    

    
    `scp -i ~/.ssh/tech501-haroon-az-key -r /c/Users/ixHar/nodejs20-sparta-test-app adminuser@20.117.122.166:/home/adminuser/`
    
    - **Explanation of the Command**:
        - `scp`: Secure Copy Protocol to transfer files over SSH.
        - `-i ~/.ssh/tech501-haroon-az-key`: Specifies the path to your SSH private key.
        - `-r`: Recursively copies the entire directory (required for folders).
        - `/c/Users/ixHar/nodejs20-sparta-test-app`: Local folder path in Windows-style.
        - `adminuser@20.117.122.166`: Your VM’s username and IP address.
        - `/home/adminuser/`: Destination directory on the VM.

#### **Step 2: Execute the Command**

1. Open **Git Bash** (or any terminal that supports `scp`).
2. Run the following command:
    
 
    
    `scp -i ~/.ssh/tech501-haroon-az-key -r /c/Users/ixHar/nodejs20-sparta-test-app adminuser@20.117.122.166:/home/adminuser/`
    

#### **Step 3: Verify File Transfer**

After the transfer is complete:

1. Log into your Azure VM using SSH:
    

    
    `ssh -i ~/.ssh/tech501-haroon-az-key adminuser@20.117.122.166`
    
2. Once logged in, check the destination directory to ensure the folder was copied successfully:
    
 
    
    `ls /home/adminuser/`
    
    - You should see `nodejs20-sparta-test-app` listed there.

---

### **Troubleshooting Tips**

- **Permission Denied**: If you receive a `Permission Denied` error:
    - Ensure the private key file (`tech501-haroon-az-key`) has the correct permissions:
        

        
        `chmod 600 ~/.ssh/tech501-haroon-az-key`
        
- **File Not Found**: Ensure the path to the folder is correct (`/c/Users/ixHar/nodejs20-sparta-test-app`).
- **Command Not Found**: If `scp` is not recognized, ensure you're using a terminal that supports it, like **Git Bash** or **WSL**.


### **To delete it**


### **1. Delete the Folder from the Azure VM**

To delete the `nodejs20-sparta-test-app` folder from the `/home/adminuser/` directory on your Azure VM, follow these steps:

1. **Log in to your Azure VM**:
    

    
    `ssh -i ~/.ssh/tech501-haroon-az-key adminuser@20.117.122.166`
    
2. **Navigate to the directory** where the folder is located:
    

    
    `cd /home/adminuser/`
    
3. **Delete the folder** using the `rm -r` command (be very careful with `rm -r`, as it will remove the folder and all its contents):
    
    
    
    `rm -r nodejs20-sparta-test-app`
rm -rf nodejs20-sparta-test-app

    
    This will remove the entire folder. If the folder is successfully deleted, you won't see it listed when you run `ls` in `/home/adminuser/`:
    

    
    `ls /home/adminuser/`