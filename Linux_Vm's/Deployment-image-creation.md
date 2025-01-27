## **Documenting the Deployment and Image Creation Process for Tech501 App**

### **1. Initial Setup and VM Access**

- **Step 1: Access the VM**
    - Use **SSH** to connect to your VM:
        
    
        
        `ssh -i ~/.ssh/tech501-haroon-az-key adminuser@your-vm-ip`
        
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
        
        
        
        `npm install npm start`
        
    - Ensure your app is running and accessible at port **3000**.

### **3. Preparing the VM for Imaging**

- **Step 5: Move App Folder to Root Directory**
    - Move the app code to the root directory (`/repo`) for organization:
        
        
        
        `sudo mv /home/adminuser/tech-501-sparta-app-deployment /repo`
        
- **Step 6: Deprovision the VM**
    - Run the following command to clean up the VM and prepare it for imaging:
        
        
        `sudo waagent -deprovision+user`
        

### **4. Image Creation**

- **Step 7: Create a Custom Image in Azure**
    - In the Azure portal, go to your VM and click **Capture**.
    - Fill in the details (name the image `tech501-haroon-sparta-app-ready-to-run-img`).
    - Choose **Generalized** and proceed with creating the image.
- **Step 8: Create and Replicate the Image in Azure Gallery**
    - If prompted to create a gallery, do so by naming it appropriately and selecting your region.
    - Wait for the image to be created and replicated. The process may take **10-30 minutes**.

### **5. Final Verification**

- **Step 9: Test the Image (Optional)**
    - If successful, create a new VM from the image and test the app by accessing it via **port 3000**.

---

### **Conclusion**

This document outlines all the steps from setting up your VM to creating a reusable image for future deployments