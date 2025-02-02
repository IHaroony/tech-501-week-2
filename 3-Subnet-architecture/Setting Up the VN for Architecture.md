
## **Creating the Virtual Network in Azure**

### **Step 1: Create a Virtual Network**

1. Navigate to **Azure Portal**.
2. Click **Virtual Networks** and select **Create**.
3. Choose the **correct Resource Group**.
4. Enter the following details:
    - **Virtual Network Name**: `tech502-haroon-3-subnet-vnet`
    - **Region**: `UK South`

---

### **Step 2: Configure IP Addresses and Adding Subnets**  

1. Go to the **IP Addresses** tab.

#### **2.1 Add Public Subnet**

- **Subnet Name**: `Public Subnet`
- **IPv4 Address Range**: `10.0.0.0/16`
- **Starting Address**: `10.0.2.0`

![[Pasted image 20250201003227.png]]
#### **2.2 Add DMZ Subnet**

- **Subnet Name**: `dmz-subnet`
- **IPv4 Address Range**: `10.0.0.0/16`
- **Starting Address**: `10.0.3.0`

#### **2.3 Add Private Subnet**

- **Subnet Name**: `private-subnet`
- **IPv4 Address Range**: `10.0.0.0/16`
- **Starting Address**: `10.0.4.0`
- **Private Subnet (No Default Outbound Access)**: ✅ **Enable this option**

### **Step 3: Tags, Review & Create**

1. **Tag your work** – use your **name** as a tag.
2. Click **Review + Create**.
3. Validate your settings and click **Create**.
Done