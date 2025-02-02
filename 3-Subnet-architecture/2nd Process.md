# Creating an Application Virtual Machine (App-VM) in Azure


## Step 1: Selecting the Image and Creating the VM

1. Navigate to the **Azure Portal**.
    
2. Locate your **desired app image** (use your own or a tested and working image).
    
3. Click on the **image**.
    
4. Click **Create VM** inside the Image Page.
    

## Step 2: Configure Basic Settings

### **Virtual Machine Basics**

- **Virtual Machine Name**: `tech501-haroon-3-subnet-sparta-app-vm`
    
- **Zone Option**: `1`
    
- **Image**: Select your **own image** or a **working image**
    
- **Size**: `Standard B1s`
    
- **Username**: `Adminuser`
    
- **SSH Key Public Source**: Use an **existing key** in Azure
    
- **Shared Keys**: Choose your **own key**
    
- **Inbound Ports**: `SSH, HTTP, Port 22, Port 80`
    
- **License Type**: `Other`
    

## Step 3: Configure Disks

1. Go to the **Disks** page.
    
2. Set **OS Disk Type**: `Standard SSD`
    
3. Enable **Delete with VM**: `Yes`
    

## Step 4: Configure Networking

1. Navigate to the **Networking** tab.
    
2. Configure the following settings:
    
    - **Virtual Network**: `tech501-haroon-3-subnet-vnet` (Choose your own VNet if different)
        
    - **Subnet**: `Public Subnet (10.0.2.0/24)`
        
    - **Public IP**: `tech501-haroon-3-subnet-sparta-db-ip`
        

## Step 5: Configure Advanced Settings

1. Navigate to the **Advanced** tab.
    
2. Enable **User Data** and enter the following script:
    

```
#!/bin/bash

cd /repo/app
export DB_HOST=mongodb://10.0.4.4:27017/post
pm2 start app.js
```

_Note: Do not use quotation marks, as they prevent the app from running correctly._

## Final Step: Tags, Review & Create

1. Optionally, set **tags** to organize resources.
    
2. Click **Review + Create**.
    
3. Validate settings and click **Create**.
    

### **Post-Creation Configuration**

- **Edit Private IP**: Add the private IP of the **DB VM** created earlier.
    

---



# Next Steps


## Check App/Posts Page:

- Check if you can connect to the vm and db: `http://20.26.120.162/posts to show the app and the posts page are showing


## SSH into the App VM:
- `ssh -i ~/.ssh/tech501-haroon-az-key adminuser@20.26.120.162

## Ping the db VM through the App VM:


- `ping 10.0.4.4`


![[Pasted image 20250201011708.png]]

This is too check the connection between the app and the database VM