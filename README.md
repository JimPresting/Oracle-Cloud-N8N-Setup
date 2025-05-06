**Summary of Steps**  
1. Register on Oracle Cloud and optimize your account  
2. Create an Always-Free VM with SSH keys  
3. SSH into your instance  
4. Reserve a static IP and configure an A-record  
5. Install n8n (per the GCP guide)  
6. Open HTTP/HTTPS & enable Load Balancing  

---

# Oracle-Cloud-N8N-Setup

Setting up an Oracle Cloud VM can be tricky due to Always-Free tier restrictions. By default, free-tier capacity is limited, so provisioning may fail unless you optimize your region choice or account type.

---

## Prerequisites

- A valid Oracle Cloud account at cloud.oracle.com  
- A remembered **Account Name** (password managers often omit this)  
- An SSH key-pair (public & private)  

---

## 1. Register & Optimize Your Account

1. **Sign up** at cloud.oracle.com  
   - Provide Account Name, Email, Password  
   - **Remember** the Account Name for future logins  

2. **Boost provisioning success**  
   - **Select the right region**  
     - Some regions (e.g. Chicago – US-Central) may throttle Always-Free VMs  
     - Choose a region with lighter demand (e.g. Frankfurt, Germany)  
   - **Optionally upgrade** to **Pay As You Go**  
     - Retains eligibility for Always-Free resources  
     - Improves chances of successful VM creation  

> **Note:** Region selection is permanent for that tenancy.

---

## 2. Create Your Always-Free VM

1. From the Oracle Cloud **Dashboard**, click **Create Instance**.  
2. Under **Shape**, choose an Always-Free configuration:  
   - **RM1** processor  
   - **4 OCPUs** (cores)  
   - **24 GB RAM**  
3. Leave **Boot Volume** and **Network** settings at default.  
4. **Generate & Download** your SSH key-pair:  
   - Keep the **private key** secure (e.g. in Downloads)  
   - Note the location of your **public key**  

![VM Creation](https://github.com/user-attachments/assets/eee9688f-eb67-4f61-9650-db0a171cde94)

---

## 3. SSH Access

1. Locate your VM's **Public IPv4**:  
   - **Compute → Instances → Your Instance → Networking**  
   - Copy the Public IP  

   ![Networking Panel](https://github.com/user-attachments/assets/fce460e8-6b9a-4c83-9b4d-4c64e0d07a74)

2. From your local terminal, run:  
   ```bash
   ssh -i ~/Downloads/ssh-key-2025-04-29.key ubuntu@YOUR_PUBLIC_IP
   ```  
   - Replace the key path and IP as needed  

   ![SSH Login](https://github.com/user-attachments/assets/0956a199-d477-4e33-92fd-653a16a74866)

---

## 4. Reserve a Static IP & Configure DNS

1. In Oracle Cloud, go to **Network → Virtual Cloud Networks → Public IPs**  
   - **Assign** a reserved (static) Public IPv4 to your instance  
   - For detailed instructions, watch this video: https://www.youtube.com/watch?v=-IVG9hTwN_Q
   - If you already created an instance before, you can also later configure this by clicking on your Instance → Networking and then on your VNIC

2. In your domain registrar's **DNS settings**, add an **A-record**:  

| Host (subdomain)  | Type | Value (IPv4)         |
|-------------------|:----:|----------------------|
| `your-subdomain`  | A    | `<Your Static IPv4>` |

![DNS A-Record](https://github.com/user-attachments/assets/8876b8eb-e8eb-4c9b-8d46-778efb358d13)

---

## 5. Install n8n (Follow GCP Guide)

From here, the n8n installation on Oracle Cloud mirrors the Google Cloud steps:

1. Follow the self-hosted n8n guide for GCP.  
2. Adjust firewall/security settings as below.  

🔗 [Continue with the n8n on GCP guide »](https://github.com/JimPresting/n8n-gcp-selfhost)

---

## 5.1 Open HTTP/HTTPS & Enable Load Balancing

1. In the Oracle Cloud Console, navigate to **Networking → Virtual Cloud Networks**.  
2. Select the VCN associated with your project (only one if new account).  
3. Click **Security** in the left sidebar, then choose the **Default Security List** for that VCN.  
4. Switch to the **Security Rules** tab, then click **Add Ingress Rules**.  
5. Add these rules:  

| Source CIDR   | IP Protocol | Port Range | Description         |
|-------------: |------------:|-----------:|---------------------|
| `0.0.0.0/0`   | TCP         | `80`       | Allow HTTP traffic  |
| `0.0.0.0/0`   | TCP         | `443`      | Allow HTTPS traffic |

![Screenshot 2025-04-30 125503](https://github.com/user-attachments/assets/4354a2e2-79d3-42fa-aa31-d40d3a8f1e3e)
![image](https://github.com/user-attachments/assets/d1c3dd0e-02ae-4f1d-be58-511cd9b76824)



6. Click **Save**.  

Your Oracle Cloud VM now has HTTP/HTTPS open and is ready for load-balanced n8n traffic!
