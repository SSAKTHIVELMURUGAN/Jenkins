 1 
 
# Jenkins. 
 4 
 
 5 
 
## Installation on EC2 Instance
 6 
 
 7 
 
YouTube Video ->
 8 
 
https://www.youtube.com/watch?v=zZfhAXfBvVA&list=RDCMUCnnQ3ybuyFdzvgv2Ky5jnAA&index=1
 9 
 
 10 
 
 11 
 
![Screenshot 2023-02-01 at 5 46 14 PM](https://user-images.githubusercontent.com/43399466/216040281-6c8b89c3-8c22-4620-ad1c-8edd78eb31ae.png)
 12 
 
 13 
 
Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.
 14 
 
 15 
 
## AWS EC2 Instance
 16 
 
 17 
 
- Go to AWS Console
 18 
 
- Instances(running)
 19 
 
- Launch instances
 20 
 
 21 
 
<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">
 22 
 
 23 
 
### Install Jenkins.
 24 
 
 25 
 
Pre-Requisites:
 26 
 
 - Java (JDK)
 27 
 
 28 
 
### Run the below commands to install Java and Jenkins
 29 
 
 30 
 
Install Java
 31 
 
 32 
 
```
 33 
 
sudo apt update
 34 
 
sudo apt install openjdk-11-jre
 35 
 
```
 36 
 
 37 
 
Verify Java is Installed
 38 
 
 39 
 
```
 40 
 
java -version
 41 
 
```
 42 
 
 43 
 
Now, you can proceed with installing Jenkins
 44 
 
 45 
 
```
 46 
 
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
 47 
 
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
 48 
 
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
 49 
 
  https://pkg.jenkins.io/debian binary/ | sudo tee \
 50 
 
  /etc/apt/sources.list.d/jenkins.list > /dev/null
 51 
 
sudo apt-get update
 52 
 
sudo apt-get install jenkins
 53 
 
```
 54 
 
 55 
 
**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.
 56 
 
 57 
 
- EC2 > Instances > Click on <Instance-ID>
 58 
 
- In the bottom tabs -> Click on Security
 59 
 
- Security groups
 60 
 
- Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).
 61 
 
 62 
 
<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">
 63 
 
 64 
 
 65 
 
### Login to Jenkins using the below URL:
 66 
 
 67 
 
http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]
 68 
 
 69 
 
Note: If you are not interested in allowing `All Traffic` to your EC2 instance
 70 
 
      1. Delete the inbound traffic rule for your instance
 71 
 
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
 72 
 
  
 73 
 
After you login to Jenkins, 
 74 
 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
 75 
 
      - Enter the Administrator password
 76 
 
      
 77 
 
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">
 78 
 
 79 
 
### Click on Install suggested plugins
 80 
 
 81 
 
<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">
 82 
 
 83 
 
Wait for the Jenkins to Install suggested plugins
 84 
 
 85 
 
<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">
 86 
 
 87 
 
Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]
 88 
 
 89 
 
<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">
 90 
 
 91 
 
Jenkins Installation is Successful. You can now starting using the Jenkins 
 92 
 
 93 
 
<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">
 94 
 
 95 
 
## Install the Docker Pipeline plugin in Jenkins:
 96 
 
 97 
 
   - Log in to Jenkins.
 98 
 
   - Go to Manage Jenkins > Manage Plugins.
 99 
 
   - In the Available tab, search for "Docker Pipeline".
 100 
 
   - Select the plugin and click the Install button.
 101 
 
   - Restart Jenkins after the plugin is installed.
 102 
 
   
 103 
 
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">
 104 
 
 105 
 
Wait for the Jenkins to be restarted.
 106 
 
 107 
 
 108 
 
## Docker Slave Configuration
 109 
 
 110 
 
Run the below command to Install Docker
 111 
 
 112 
 
```
 113 
 
sudo apt update
 114 
 
sudo apt install docker.io
 115 
 
```
 116 
 
 
 117 
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.
 118 
 
 119 
 
```
 120 
 
sudo su - 
 121 
 
usermod -aG docker jenkins
 122 
 
usermod -aG docker ubuntu
 123 
 
systemctl restart docker
 124 
 
```
 125 
 
 126 
 
Once you are done with the above steps, it is better to restart Jenkins.
 127 
 
 128 
 
```
 129 
 
http://<ec2-instance-public-ip>:8080/restart
 130 
 
```
 131 
 
 132 
 
The docker agent configuration is now successful.
 133 
 
 134 
 
 135 
 
 136 
 
 137 

 
