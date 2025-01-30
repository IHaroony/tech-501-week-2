## **Documenting the Deployment and Image Creation Process for Tech501 App**



#### **Step 1: Access the VM Using SSH**

To access your virtual machine (VM), you'll use the **SSH (Secure Shell)** protocol. This allows you to securely connect to your VM from your local machine.

##### **Breaking down the command:**



`ssh -i ~/.ssh/tech501-haroon-az-key adminuser@<your-vm-ip>`

- **`ssh`**: The command to initiate an SSH connection.
- **`-i ~/.ssh/tech501-haroon-az-key`**: This specifies the path to your private SSH key.
    - The `tech501-haroon-az-key` is the private key that was generated and saved when the VM was created. This key ensures secure, password-less access.
- **`adminuser`**: The username for the VM. This was set up during the VM creation process.
- **`@<your-vm-ip>`**: Replace `<your-vm-ip>` with the public IP address of your VM, which can be found in the Azure portal.

##### **Example:**


`ssh -i ~/.ssh/tech501-haroon-az-key adminuser@20.68.240.97`

This command connects you to the VM securely. Once connected, you'll have access to the VM's terminal and can begin configuring it.


        
- **Step 2: Install Required Dependencies (Node.js, Nginx)**
    - Install **Node.js** version 20 and **Nginx** on the VM:
        
      
        
        `sudo apt update sudo apt install curl curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - sudo apt install -y nodejs sudo apt install -y nginx`
        

### **2. App Setup and Files**

- **Step 3: Clone or Upload the App**
    
    - **Option 1**: Clone the app from GitHub (after pushing to your own repository):
        
        
        
        `git clone https://github.com/IHaroony/tech-501-sparta-app-deployment.git cd tech-501-sparta-app-deployment`
        


        
    - **Option 2**: Upload the app using **SCP** or **GitHub** and then clone it onto your VM.







- **Step 4: Test App Manually**
    
    - After installing dependencies and moving into your app folder, run:
        
        
        
        `npm start`
        
    - Ensure your app is running and accessible at port **3000**.

### **4. Preparing the VM for Imaging**

#### **Step 6: Move App Folder to Root Directory**

To organize the app and prepare the VM for imaging, move the app folder to the root directory (`/repo`):

```
sudo mkdir /repo
sudo mv /home/adminuser/tech-501-sparta-app-deployment /repo/app
```

- `**sudo mkdir /repo**`: Creates a new folder named `repo` in the root directory.
    
- `**sudo mv**`: Moves the app folder into `/repo` and renames it to `app`.
    

Verify the move:

```
ls /repo/app
```

You should see your app files listed inside `/repo/app`.

#### **Step 7: Deprovision the VM**

Clean up the VM and remove user-specific data to make it ready for imaging:

```
sudo waagent -deprovision+user
```

- `**sudo waagent -deprovision+user**`: Removes user accounts and sensitive data.
    

Type `y` when prompted to confirm. **Note:** After this, you will be logged out and cannot log back into the VM.

---

### **5. Creating the Image in Azure**

#### **Step 8: Create the Custom Image**

1. In the Azure portal, go to your VM.
    
2. Click **Capture**.
    
3. Fill in the details:
    
    - **Name**: `tech501-haroon-sparta-app-ready-to-run-img`
        
    - **Generalized**: Select this option.
        
    - **Delete the VM after image creation**: Check this box.
        
4. Click **Create**.
    

The image creation process may take **10-30 minutes**. Once completed, the image will be available in your Azure Compute Gallery.

#### **Step 9: Verify the Image**

1. Create a new VM using the custom image:
    
    - Go to **Create a Resource** > **Virtual Machine**.
        
    - Select your custom image under **Image**.
        
2. Test the app:
    
    - SSH into the new VM.
        
    - Start the app using `npm start`.
        
    - Visit the VM's IP address on port 3000 to confirm the app is working.
        

---

### **Conclusion**

This document outlines all the steps from setting up your VM to creating a reusable image for future deployments