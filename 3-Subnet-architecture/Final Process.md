
# IP Forwarding and Creating Iptables Rules 




## Enabling IP Forwarding in Azure

1. Navigate to your **NVA VM** in the Azure portal.
    
2. Click on **Network Settings**.
![[Screenshot 2025-02-02 204658.png]]


1. Under **IP Configuration**, locate the option **Enable IP Forwarding** and check the box.
    
2. Apply the changes.

At this point, IP forwarding is enabled at the Azure level, but it must also be enabled within Linux.
## Enabling IP Forwarding in Linux

1. SSH into the NVA VM:
    
    ```
    ssh -i ~/.ssh/tech501-haroon-az-key adminuser@20.26.120.162
    ```
    
2. Check if IP forwarding is enabled:
    
    ```
    sysctl net.ipv4.ip_forward
    ```
    
    - If the output is `0`, IP forwarding is disabled.
        
3. Enable IP forwarding:
    
    ```
    sudo nano /etc/sysctl.conf
    ```
    
    - Locate the line: `#net.ipv4.ip_forward=1`
        
    - Uncomment it by removing `#` so it reads:
        
        ```
        net.ipv4.ip_forward=1
        ```
        
    - Save and exit using **CTRL+O**, then **CTRL+X**.
        
4. Reload the configuration:
    
    ```
    sudo sysctl -p
    ```
    
5. Verify the change:
    
    ```
    sysctl net.ipv4.ip_forward
    ```

The output should now be `1`.

Also when editing the file this how the updated file should look like.


![[Pasted image 20250202204421.png]]




## Creating IPtables Rules



#### Be Cautious When Setting IPTables Rules

- **Setting IPTables rules in the wrong order can lock you out of your VM**. Make sure you understand the rules before applying them.

#### Checking IPTables Installation

1. SSH into your **NVA VM**.
2. Run the following command to check if `iptables` is installed:
    
    bash
    
    CopyEdit
    
    `sudo iptables --help`
    
    - This will display the available options and confirm that `iptables` is present.

#### Purpose of These Rules

- These rules **restrict traffic** so that only the required traffic from the App VM reaches the Database VM.
- Again, **be cautious**—misconfiguring the rules can block access to your VM

Creating a bash script to automate this:

- `nano config-ip-tables.sh`

```
#!/bin/bash
 
# configure iptables
 
echo "Configuring iptables..."
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A OUTPUT -m state --state ESTABLISHED -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -m state --state INVALID -j DROP
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
sudo iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
 
# uncomment the following lines if want allow SSH into NVA only through the public subnet (app VM as a jumpbox)
# this must be done once the NVA's public IP address is removed
#sudo iptables -A INPUT -p tcp -s 10.0.2.0/24 --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
#sudo iptables -A OUTPUT -p tcp --sport 22 -m state --state ESTABLISHED -j ACCEPT
 
# uncomment the following lines if want allow SSH to other servers using the NVA as a jumpbox
# if need to make outgoing SSH connections with other servers from NVA
#sudo iptables -A OUTPUT -p tcp --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
#sudo iptables -A INPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A FORWARD -p tcp -s 10.0.2.0/24 -d 10.0.4.0/24 --destination-port 27017 -m tcp -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -A FORWARD -p icmp -s 10.0.2.0/24 -d 10.0.4.0/24 -m state --state NEW,ESTABLISHED -j ACCEPT
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -P INPUT DROP
 
# ADD COMMENT ABOUT WHAT THE FOLLOWING COMMAND(S) DO
sudo iptables -P FORWARD DROP
 
echo "Done!"
echo ""
 
# make iptables rules persistent
# it will ask for user input by default
 
echo "Make iptables rules persistent..."
sudo DEBIAN_FRONTEND=noninteractive apt install iptables-persistent -y
echo "Done!"
echo ""
```

1. **Save and Exit the Editor:**
    
    - Press `Ctrl + O` to save the file.
    - Press `Enter` to confirm the filename.
    - Press `Ctrl + X` to exit the editor.
2. **Change Permissions of the Script:**
    
    - Run the command to give the script execution permissions:
    

        `chmod +x config-ip-tables.sh`
        
    - To verify, use the `ls` command:
        

    - If the script has been granted execution permissions, it will appear in green.

3. **Run the Script:**
    
    - Now, execute the script by running:
    - 
        `./config-ip-tables.sh`
        

This will ensure that the script is executable and properly run on your system!

## Editing the Database Network Security Group 


**Navigate to the Database VM**:

- Go to the Network Security Group (NSG) settings for your database VM.
![[Screenshot 1a.png]]
**Access Inbound Security Rules**:

- On the left-hand panel, look for the list of settings.
- Click the **Settings** tab, then select **Inbound security rules**

![[Pasted image 20250202214023.png]]

Here is where all the port and where will setting mongodb as port

![[Pasted image 20250202214059.png]]

**Configure MongoDB Port**:

- Click **Add** to create a new inbound rule.
- Set the following values:
    - **Source IP Addresses**: `10.0.2.0/24`
    - **Service**: `MongoDB`
- This allows MongoDB traffic to flow to your database VM

![[Pasted image 20250202214324.png]]


--

   
**Deny All Other Traffic**:

- Change the **Destination port ranges** to `*` to block all other traffic.
- Set the **Priority** to `1000` so it can be easily managed above any allow rules.
- Add to Apply Rule 
![[Pasted image 20250202214734.png]]

**Allow Ping (Optional)**:

- If you want to allow ping (ICMP), create a new rule above the "Deny everything else" rule.
- Ensure that this rule permits ICMP traffic to allow the ping from the app VM to reach the database VM.


#done 