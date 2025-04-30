# Oracle-Cloud-N8N-Setup

Setting up the Oracle Cloud often fails from the very start as you are not able to create an instance as given their generous always free tier limits people use it for private servers more often than for business related services. 

Sign up on cloud.oracle.com (bare in mind you have to create an account name and email and password... if you are using a password manager the account name later wont be saved so remeber that one as you are going to need it for your login). 
There is 2 major things you can do in order to increase the chance of even getting a free VM under their generous terms.
1. Choose the location wisely (I tried Chicago Central US and wasnt able to create an instance for weeks). Bare in mind that the location you are choosing here can later not be changed (at least not regarding this account).
2. Upgrade your account to a pay as you go account ... it will still be eligable to use the always free assets. So effectively it woulndt change but it increases your chances of getting a instance. For Frankfurt, Germany (closest to me) it worked wihtout setting the account up for a pay as you go so it mostly depends on the instance's location.
![Screenshot 2025-04-30 111210](https://github.com/user-attachments/assets/c620bafd-4bd5-4808-b8e6-05863b1d88f5)
![image](https://github.com/user-attachments/assets/f58fec3f-99e1-41c7-9d33-419279a495a0)


## 1. Setting up your VM

After signing up your welcome screen should look somewhat like this: 
![image](https://github.com/user-attachments/assets/eee9688f-eb67-4f61-9650-db0a171cde94)

Create your VM based on the configs you need (for always free tier use RM1 processor with 4 cores and 24GB of RAM and leave the boot network to default) 

Make sure you download your public and private ssh key... 

## 2. Establishing a connection via SSH

open your terminal and enter the path to your private ssh key file... in my case thats in the downlaods folder and connect it with your public IP 
You can get your public IP by clicking on instances --> your instance and networking 
![image](https://github.com/user-attachments/assets/fce460e8-6b9a-4c83-9b4d-4c64e0d07a74)

Enter that in your terminal locally:

ssh -i C:\Users\ME\Downloads\ssh-key-2025-04-29.key ubuntu@YOURPUBLICIP

![image](https://github.com/user-attachments/assets/0956a199-d477-4e33-92fd-653a16a74866)

## 3. Make IP static and add A-level record

Go to your Domain and its DNS settigns add a A level record enter a name of your subdomain and add the public ipv4 
![image](https://github.com/user-attachments/assets/8876b8eb-e8eb-4c9b-8d46-778efb358d13)



From here on out you can esta
