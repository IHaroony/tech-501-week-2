
# Creating Machine (DB-VM) in Azure

## Step 1: Selecting the Image and Creating the VM

1. Navigate to the **Azure Portal**.
    
2. Locate your **desired image** (use your own working image).
    
3. Click on the **image**.
    
4. Click on **Create VM**.


![[Pasted image 20250201004542.png]]
    

## Step 2: Configure Basic Settings

### **Virtual Machine Basics**

- **Virtual Machine Name**: `tech501-haroon-3-subnet-sparta-db`
    
- **Zone Options**: `3`
    
- **Image**: Select your **own image** or a **working image**
    
- **Size**: `Standard B1s`
    
- **Username**: `Adminuser`
    
- **SSH Key Public Source**: Use an **existing key** in Azure
    
- **Shared Keys**: Choose your **own key**
    
- **Inbound Ports**: `SSH`
    
- **License Type**: `Other`
    

## Step 3: Configure Disks

1. Go to the **Disks** page.
    
2. Set **OS Disk Type**: `Standard SSD`
    
3. Enable **Delete with VM**: `Yes`
    

## Step 4: Configure Networking

1. Navigate to the **Networking** tab.
    
2. Configure the following settings:
    
    - **Virtual Network**: `tech501-haroon-3-subnet-vnet` (Choose your own VNet if different)
        
    - **Subnet**: `Private Subnet`
        
    - **Public IP**: `None`
    - ![[Pasted image 20250201005356.png]]
    - 
        

## Step 5: Tags, Review & Create

1. Optionally, set **tags** to organize resources.
    
2. Click **Review + Create**.
    
3. Validate settings and click **Create**.
    

### **Additional Notes**

- **Private IP**: The VM will use a private IP from the selected subnet.
    
- **Security**: Ensure NSG rules allow necessary connections for database communication.
    

---

This document provides a clear, step-by-step guide for creating a **DB-VM** with appropriate networking and security settings.


tech501-haroon-3-subnet-sparta-db-ip