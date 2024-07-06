Project: Deploy and Configure Azure Firewall (Premium)
-------------------------------------------------------------------------------------
A high-level summary acivities of this project:

- Created 3 subnets within a single resource group and region (Australia East) under NahidHomeLab subscription.
- Deployed Azure Firewall with premium SKU in the NahidHomeLab virtual network spanning all availability zones.
- Configured a route table to direct network traffic.
- Associated the route table with the NahidHomeLab subnet to manage traffic flow.
- Established application rules allowing outgoing HTTP/HTTPS traffic from Azure network.
- Implemented network firewall rules permitting DNS traffic from Azure network to external sites.
- Set up a DNAT rule for RDP access.
- Enabled Intrusion Detection and Prevention System (IDPS) as a premium feature to monitor inbound and outbound traffic.
---------------------------------------------------------------------------------------------------------------------------

First, I’ll create 3 subnets that I’ll need later stage to configure, deploy and manage my firewall.
To keep things smooth, I'll keep all resouces in the same resource group and regions under my NahidHomeLab subscription.


![image](https://github.com/nahid7474/Firewall/assets/170605912/a694e2a2-6616-49f7-a649-d081169983ae)

Will then create 3 subnets within this V-Net

![image](https://github.com/nahid7474/Firewall/assets/170605912/3fdcef74-1bed-43f9-933f-c673a84c6178)

On the Azure Portal, search for firewall and select the Fiirewalls to create one.

![image](https://github.com/nahid7474/Firewall/assets/170605912/8e887373-599d-4b75-a819-f72ff0f65779)


Choose Azure subscription 1, HomeLab resource group, Australia East region, select all the evailability zone, premium SKU, Homelab virual nwtwork with the address space of 176.16.0.0/24.
Pass the validation, Review and then create.

![image](https://github.com/nahid7474/Firewall/assets/170605912/7afc97c6-1ed3-4069-890b-d3819f73a4cf)

Deployment is successful.

![image](https://github.com/nahid7474/Firewall/assets/170605912/5fe75186-c5b3-4955-bfca-abc394982bc8)

Will keep a note of firewall's public and private IPs that will be required at the later stage.

Private IP: 176.16.0.4
Public IP: 20.248.209.8

![image](https://github.com/nahid7474/Firewall/assets/170605912/5e6ce08b-0a81-47bc-9c78-ce9ea711b23f)

Next, will create a route table.
Purpose of this is to directs network traffic within a virtual network by specifying the next hop for different destination IP addresses. 
Will keep this resource under the same subscription and resource group.

![image](https://github.com/nahid7474/Firewall/assets/170605912/06e5ef0c-0d68-41c7-a486-bf1ca3169694)


Deployment sucessful.

![image](https://github.com/nahid7474/Firewall/assets/170605912/be76ee28-f37d-444c-b3e5-090d5d2b5129)


Next, I’ll associate the route table to the workload subnet called NahidHomeLab-allworkload (where my VMs live in) from the Subnets Settings pane on left side. 

![image](https://github.com/nahid7474/Firewall/assets/170605912/63ed787f-fdd2-4b18-9ef9-492996d4315c)


Next, go to Routes pane, add route, give it a name (FW-HomeLab-DR), Put 0.0.0.0 as the destination IP address, it is the default route here. 
Add firewall’s private IP address as the next hop address. CLick Add

This route tells the v-net to send any traffic that doesn’t have a route for shoud go to the firewall. 

![image](https://github.com/nahid7474/Firewall/assets/170605912/8b286781-47eb-43a7-b8a7-1a8145597f6a)


Now I'll start creating my firewall rules, starting with application rule. 
On the Application rules menu, click Add a rule collection to create one. 

![image](https://github.com/nahid7474/Firewall/assets/170605912/8fe76a85-c45a-4cf6-b0ce-f127ba17b26a)

By default, firewall blcoks everything.
Just to test, I'll create a rule below that will allow all my outgoing traffic from my azure nwtwork to any website via port http and https.
THis is not great for sucurity, but for now I'll keep it like this. 

![image](https://github.com/nahid7474/Firewall/assets/170605912/5184ec6b-3b82-4290-b8c1-fdea76b7489a)

Will create a similar network firewall rule allowing all DNS traffic to outside world to all sites from all my network via port 53 using UDP protocol.

![image](https://github.com/nahid7474/Firewall/assets/170605912/a854b9bd-aabd-4552-bae0-d18b9d25688f)


Next, will add a rule for DNAT to provide/allow RDP (Remote Desktop) access for users via TCP port 3389 via our Firewall's public IP.

![image](https://github.com/nahid7474/Firewall/assets/170605912/0c5751eb-ad60-4165-807a-338ba942e4d4)



Finally, as a premium feature, I will enable Intrusion Detection and Prevention System to monitor all my inboud and outbound traffic.


![image](https://github.com/nahid7474/Firewall/assets/170605912/22b17d4b-a4e9-45d0-891e-5d310f46af1c)



This concludes my Azure Premium Firewall Deployment project!!
