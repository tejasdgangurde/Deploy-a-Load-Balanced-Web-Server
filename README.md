# Deploy-a-Load-Balanced-Web-Server •Configure a new server and set up a Network Load Balancer (NLB)- Website is accessible through the load balancer
Step 1: Launch Two or More EC2 Instances
Go to AWS Console → Open EC2 Dashboard.
Click Launch Instance and configure:
Choose Amazon Linux 2 or Ubuntu.
Select an instance type (e.g., t2.micro for free tier).
Create or select an existing key pair.
Ensure port 80 (HTTP) is open in the security group.
Click Launch.
Repeat this process to launch at least two instances.

Step 2: Install a Web Server on Each EC2 Instance
Connect to each EC2 instance using SSH:
sh
Copy
Edit
ssh -i your-key.pem ec2-user@your-ec2-public-ip
Install Apache (for Amazon Linux):
sh
Copy
Edit
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
(For Ubuntu, use sudo apt update && sudo apt install -y apache2)
Add a simple index.html file:
sh
Copy
Edit
echo "<h1>Server 1 - Load Balancer Test</h1>" | sudo tee /var/www/html/index.html
(For the second server, change the text to "Server 2" to differentiate it)
Restart the web server:
sh
Copy
Edit
sudo systemctl restart httpd
✅ Now, both EC2 instances serve a webpage.

Step 3: Create a Network Load Balancer (NLB)
Go to AWS Console → Open EC2 Dashboard.
Click Load Balancers → Click Create Load Balancer.
Choose Network Load Balancer and click Create.
Configure:
Name: MyNLB
Scheme: Internet-facing
IP Address Type: IPv4
VPC & Subnets: Select your existing VPC and at least two subnets.
Listener: Ensure Protocol: TCP, Port: 80.
Click Next: Configure Routing.

Step 4: Create a Target Group
Choose Target Group Type: Instances.
Protocol: TCP, Port: 80.
Target Type: Instance.
Name: MyTargetGroup.
Click Next: Register Targets.
Select your EC2 instances and click Include as pending.
Click Create Target Group.

Step 5: Attach the Target Group to the Load Balancer
Go back to the Load Balancer setup.
Select the Target Group (MyTargetGroup).
Click Create Load Balancer.

Step 6: Test the Load Balancer
Go to EC2 Dashboard → Load Balancers.
Copy the DNS name of your NLB.
Open a browser and visit:
cpp
Copy
Edit
http://<NLB-DNS-Name>
Refresh multiple times to see the requests being distributed across Server 1 and Server 2.
✅ Congratulations! Your website is now accessible through a Load Balancer. 🚀
