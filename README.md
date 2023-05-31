### Elastic File System

Amazon Elastic File System (EFS) is a scalable and fully managed file storage service provided by Amazon Web Services (AWS). It is designed to provide shared file storage for multiple Amazon Elastic Compute Cloud (EC2) instances. Amazon Elastic File System (EFS) automatically grows and shrinks as you add and remove files with no need for management or provisioning.

EFS is built using the Network File System version 4 (NFSv4) protocol, which allows it to provide a simple and familiar file system interface to EC2 instances. It supports the standard file system operations such as create, read, write, and delete, and allows multiple EC2 instances to concurrently access the same file system.

<p align="center">
  <img src="https://github.com/arshadrebin/efs/assets/116037443/7c3d33e9-e5ae-40a7-a530-cc616bb48f40"></p>
  
  This representation shows the Amazon EFS file system as a box at the top, and below it are the EC2 instances represented by boxes. 
  
  Below steps are used to create a simple EFS in AWS.
  
1. Go to the AWS Management Console and open the Amazon EFS console.
2. Click on "Create file system."
3. Select the desired options for your file system, such as the VPC, availability zones, and performance mode.
Configure the network settings, security groups, and encryption options.
Optionally, enable file system backup and configure your preferred backup settings.

<img width="935" alt="Screenshot 2023-05-31 094827" src="https://github.com/arshadrebin/efs/assets/116037443/1ec28978-a377-4045-914c-79317fef0fa3">
<img width="731" alt="Screenshot 2023-05-31 094934" src="https://github.com/arshadrebin/efs/assets/116037443/f619de77-628b-41ba-aa7f-68dc85e60284">
<img width="734" alt="Screenshot 2023-05-31 095100" src="https://github.com/arshadrebin/efs/assets/116037443/12bc6164-c1ce-4b84-ab71-1f5f90bdab8c">

Please make sure to add a inbound rule allowing NFS traffic (TCP port 2049) from the security group associated with your EFS mount target.

4. Click on "Create file system" to create your EFS file system


Then we would need to mount this EFS into the EC2 instance. 

1. Connect to your EC2 instance using SSH or any other remote access method.
2. Run the following command to install the amazon-efs-utils client package (if not already installed)

```
[ec2-user@ip-172-31-46-3 ~]$  sudo yum install amazon-efs-utils -y
```



                            
