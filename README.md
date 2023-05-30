### Elastic File System

Elastic File System (EFS) is a scalable, fully managed file storage service provided by Amazon Web Services (AWS). It is designed to provide highly available and durable file storage that can be accessed concurrently by multiple EC2 instances or on-premises servers. Amazon EFS is commonly used for a variety of use cases, including content management, web hosting, development and testing environments, database backups, analytics, and machine learning. Its ability to scale automatically and provide shared access to files makes it a versatile and convenient storage solution within the AWS ecosystem. Overall, Amazon EFS provides a highly scalable and shared file storage solution for your AWS infrastructure. It eliminates the complexity of managing file servers and allows you to easily share data across multiple instances and services.

Here are some key aspects and features of Amazon EFS:

1. Scalability: Amazon EFS automatically scales its capacity as you add or remove files, eliminating the need for manual capacity planning. It can grow to petabyte-scale, and you only pay for the storage you use.
2. Shared File System: Multiple EC2 instances running in the same Amazon VPC (Virtual Private Cloud) can access a single Amazon EFS file system concurrently. This enables collaboration and shared data access across instances.
3. Durability and Availability: Amazon EFS is built to be highly durable and available. It automatically replicates data across multiple Availability Zones within a region, providing durability and protecting against the failure of a single Availability Zone.
4. Security: Amazon EFS supports encryption of data at rest using AWS Key Management Service (KMS). It also integrates with AWS Identity and Access Management (IAM) for fine-grained access control and supports VPC security groups for network-level security.
5. Integration: Amazon EFS integrates with various AWS services, including Amazon EC2, AWS Lambda, Amazon ECS, Amazon EKS, AWS Batch, and more. This allows you to easily use Amazon EFS as a shared storage solution for your applications.
6. Performance: Amazon EFS is designed to provide low-latency, high-throughput performance. It can support thousands of concurrent connections and is optimized for a wide range of workloads.


In order to create EFS via AWS console, you would need to follow the below steps.

1. Sign in to the AWS Management Console (https://console.aws.amazon.com/).
2. Open the Amazon EFS console by searching for "EFS" in the AWS service search bar or by navigating to the "Storage" category and selecting "Elastic File System."
3. Click on the "Create file system" button.
4. In the "Create file system" wizard, configure the following settings:

Enter the name and select the VPC. Click on the Customized option and make the changes as below.

<img width="948" alt="Screenshot 2023-05-30 111328" src="https://github.com/arshadrebin/efs/assets/116037443/a7a526e0-6fff-46f2-8de8-8b0513880566">
   
<img width="730" alt="Screenshot 2023-05-30 111439" src="https://github.com/arshadrebin/efs/assets/116037443/d2196477-730b-40f2-a6ad-95cbc517e20e">

Please make sure that the port 2049 should be opened on the security group.

The creation process may take a few moments. Once the file system is created, you can start using it by mounting it on your EC2 instances or integrating it with other AWS services.

#### Mounting of the EFS to the instance.
---





