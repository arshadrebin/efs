### Elastic File System

Amazon Elastic File System (EFS) is a scalable and fully managed file storage service provided by Amazon Web Services (AWS). It is designed to provide shared file storage for multiple Amazon Elastic Compute Cloud (EC2) instances. Amazon Elastic File System (EFS) automatically grows and shrinks as you add and remove files with no need for management or provisioning.

EFS is built using the Network File System version 4 (NFSv4) protocol, which allows it to provide a simple and familiar file system interface to EC2 instances. It supports the standard file system operations such as create, read, write, and delete, and allows multiple EC2 instances to concurrently access the same file system.


![241981412-e2302d19-e20f-49f8-aaee-af012523e2c1](https://github.com/arshadrebin/efs/assets/116037443/67468091-4220-4833-8c6a-b428c2e21e6d)


EFS offers a simple and scalable file storage solution that can be accessed by multiple instances within the same AWS region. It provides a file system interface, which means you can mount EFS to your instances using standard file system commands. This makes it easy to migrate existing applications that rely on traditional file systems to the cloud without modifying your application code.


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

For the testing purpose, we would need to put some files to the document root of the server. So I am installing the apache and PHP to the master instance.

```
[ec2-user@ip-172-31-46-3 ~]$ sudo yum install httpd php -y
[ec2-user@ip-172-31-46-3 ~]$ sudo systemctl restart httpd php-fpm
[ec2-user@ip-172-31-46-3 ~]$ sudo systemctl enable  httpd php-fpm
```


After this we need to mount the elastic file system to the instance.

```
[ec2-user@ip-172-31-46-3 ~]$ sudo vim /etc/fstab
```

Add the below entry to the ftsab file. Need to change the fs-id accordingly.

<img width="766" alt="Screenshot 2023-05-31 103136" src="https://github.com/arshadrebin/efs/assets/116037443/bab92bd4-b793-4292-8fe2-2c06934b3802">

Then mount the file system and verify.

```
[ec2-user@ip-172-31-46-3 ~]$ sudo mount -a
[ec2-user@ip-172-31-46-3 ~]$ df -h
Filesystem                                           Size  Used Avail Use% Mounted on
devtmpfs                                             4.0M     0  4.0M   0% /dev
tmpfs                                                475M     0  475M   0% /dev/shm
tmpfs                                                190M  2.8M  188M   2% /run
/dev/xvda1                                           8.0G  1.6G  6.4G  20% /
tmpfs                                                475M     0  475M   0% /tmp
tmpfs                                                 95M     0   95M   0% /run/user/1000
/dev/xvda128                                          10M  1.3M  8.7M  13% /boot/efi
fs-072b061b3cd9022c7.efs.ap-south-1.amazonaws.com:/  8.0E     0  8.0E   0% /var/www/html
````

Now we would need to upload or clone some website contents to the apache document root.

```
[ec2-user@ip-172-31-46-3 ~]$ sudo git clone https://github.com/arshadrebin/AWS-ELB-Site.git  /var/website/
[ec2-user@ip-172-31-46-3 ~]$ sudo cp -r /var/website/*  /var/www/html/
[ec2-user@ip-172-31-46-3 ~]$ sudo chown -R apache:apache /var/www/html/*
```

### Mounting of the EFS on the EC2 instance which is created by Auto Scaling Group.

![241981412-e2302d19-e20f-49f8-aaee-af012523e2c1](https://github.com/arshadrebin/efs/assets/116037443/75d086cc-1841-49ee-a723-a0857cb3e76f)

Here we are going to see how we can mount the above EFS to an ASG created Instances.

For this we need to create a Launch Configuration.



```
#!/bin/bash

yum install amazon-efs-utils httpd php  -y
echo "fs-072b061b3cd9022c7:/    /var/www/html/  efs  defaults,_netdev  0  0"  >> /etc/fstab
mount -a
systemctl restart httpd php-fpm
systemctl enable httpd php-fpm
```

Then create an Auto Scaling Group with the LC which we created above. Now the newly deployed instance will have the same website document root and its contents of the master instance.




                            
