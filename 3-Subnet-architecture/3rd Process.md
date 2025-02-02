# Creating an NVA Virtual Machine (NVA-VM) in Azure

## Step 1: Creating a New Virtual Machine

1. Navigate to the **Azure Portal**.
    
2. Click **Create a new VM** (Do not use an existing image).
    

### **Virtual Machine Basics**

- **Virtual Machine Name**: `tech501-haroon-3-subnet-sparta-nva`
    
- **Zone Option**: `2`
    
- **Security Type**: `Standard Image` (**DO NOT choose the other one**)
    
- **Image**: `Ubuntu Server 22.04 LTS - X64 Gen2`
    
- **Size**: `Standard B1s`
    
- **Username**: `Adminuser`
    
- **SSH Key Public Source**: Use an **existing key** in Azure
    
- **Shared Keys**: Choose your **own key**
    
- **Inbound Ports**: `SSH`
    

## Step 2: Configure Disks

1. Go to the **Disks** page.
    
2. Set **OS Disk Type**: `Standard SSD`
    
3. Enable **Delete with VM**: `Yes`
    

## Step 3: Configure Networking

1. Navigate to the **Networking** tab.
    
2. Configure the following settings:
    
    - **Virtual Network**: `tech501-haroon-3-subnet-vnet` (Choose your own VNet if different)
        
    - **Subnet**: `dmz-subnet (10.0.3.0/24)`
        
    - **Public IP**: `tech501-haroon-haroon-in-3-sparta-app-nva-ip`
        

## Step 4: Tags, Review & Create

1. Use **your name** for tags.
    
2. Click **Review + Create**.
    
3. Validate settings and click **Create**.



## Next Steps: Creating a Route Table

### 1. **Create the Route Table**:

- Go to **Route Tables** in the Azure portal.
- Click **Create**.

Set the following configurations:

- **Name**: `tech501-haroon-to-private-subnet-rt`
- **Region**: UK South
- **Tags**: `Name-owner: Haroon`

After filling in the details, click **Review + Create** and then click **Create**.

### 2. **Configure Routes in the Route Table**:

- Once the route table is created, go to the **Route Table** resource.
- Under **Settings**, click on **Routes** and then click **Add**.

Add a new route with the following settings:

- **Route Name**: `to-private-subnet-route`
- **Destination Type**: `IP addresses`
- **CIDR Block**: `10.0.4.0/24` (Private subnet)
- **Next Hop Type**: `Virtual appliance`
- **Next Hop Address**: `10.0.3.4` (NVA private IP)

### 3. **Associate the Route Table with the Subnet**:

- Navigate to **Subnet** and associate the route table with the appropriate subnet.
- Choose your **Virtual Network**.
- **Associate it to the public subnet** where the traffic will originate.

---

### **About Route Tables**:

In Azure, a **Route Table** defines how network traffic is routed between subnets, networks, and external destinations. When you associate a route table with a subnet, the routes defined in the table control the flow of traffic for resources within that subnet. This helps ensure that traffic flows correctly between different parts of your network, such as private subnets, public subnets, and virtual appliances.

You can add custom routes to control traffic to specific destinations or to forward traffic to a Network Virtual Appliance (NVA), as demonstrated in this setup.

---

This should help clarify how to configure and associate the route table for proper network traffic management.