# AWS Elastic Beanstalk


![1](https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/e754ae04-deb2-4c83-8da9-831780f7fcb7)


Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications.


## Features and Benefits

+ Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

+ Elastic Beanstalk supports applications developed in Go, Java, .NET, Node.js, PHP, Python, and Ruby.
  
+ When you deploy your application, Elastic Beanstalk builds the selected supported platform version and provisions one or more AWS resources, such as Amazon EC2 instances, to run your application.

+ You can interact with Elastic Beanstalk by using the Elastic Beanstalk console, the AWS Command Line Interface (AWS CLI), or eb, a high-level CLI designed specifically for Elastic Beanstalk.

+ You can also perform most deployment tasks, such as changing the size of your fleet of Amazon EC2 instances or monitoring your application, directly from the Elastic Beanstalk web interface (console). 


## Beanstalk Work Flow

+ To use Elastic Beanstalk, you create an application, upload an application version in the form of an application source bundle (for example, a Java .war file) to Elastic Beanstalk, and then provide some information about the application.
  
+ Elastic Beanstalk automatically launches an environment and creates and configures the AWS resources needed to run your code. After your environment is launched, you can then manage your environment and deploy new application versions.



![Bs2](https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/061dba03-73b5-4a8d-8466-0bb90c249cd7)



## Elastic Beanstalk Concepts

AWS Elastic Beanstalk enables you to manage all of the resources that run your application as environments. 

+ Here are some key Elastic Beanstalk concepts.


## Application

+ An Elastic Beanstalk application is a logical collection of Elastic Beanstalk components, including environments, versions, and environment configurations. 

+ In Elastic Beanstalk an application is conceptually similar to a folder.


## Application Version

+ In Elastic Beanstalk, an application version refers to a specific, labeled iteration of deployable code for a web application.

+ An application version points to an Amazon Simple Storage Service (Amazon S3) object that contains the deployable code, such as a Java WAR file.

+ An application version is part of an application. Applications can have many versions and each application version is unique.

+ In a running environment, you can deploy any application version you already uploaded to the application, or you can upload and immediately deploy a new application version.

+ You might upload multiple application versions to test differences between one version of your web application and another.


## Environment

+ An environment is a collection of AWS resources running an application version.

+ Each environment runs only one application version at a time, however, you can run the same application version or different application versions in many environments simultaneously.

+ When you create an environment, Elastic Beanstalk provisions the resources needed to run the application version you specified.


## Environment Tier

+ When you launch an Elastic Beanstalk environment, you first choose an environment tier.

+ The environment tier designates the type of application that the environment runs, and determines what resources Elastic Beanstalk provisions to support it.

+ An application that serves HTTP requests runs in a web server environment tier.


## Environment Configuration

+ An environment configuration identifies a collection of parameters and settings that define how an environment and its associated resources behave.

+ When you update an environment’s configuration settings, Elastic Beanstalk automatically applies the changes to existing resources or deletes and deploys new resources (depending on the type of change).


## Saved Configuration

+ A saved configuration is a template that you can use as a starting point for creating unique environment configurations.

+ You can create and modify saved configurations, and apply them to environments, using the Elastic Beanstalk console, EB CLI, AWS CLI, or API. The API and the AWS CLI refer to saved configurations as configuration templates.


## Platform

+ A platform is a combination of an operating system, programming language runtime, web server, application server, and Elastic Beanstalk components. You design and target your web application to a platform.

+ Elastic Beanstalk provides a variety of platforms on which you can build your applications.


## Elastic Beanstalk architecture for a web server environment tier

+ The environment is the heart of the application. In the diagram, the environment is shown within the top-level solid line.

+ When you create an environment, Elastic Beanstalk provisions the resources required to run your application.

+ AWS resources created for an environment include one elastic load balancer (ELB in the diagram), an Auto Scaling group, and one or more Amazon Elastic Compute Cloud (Amazon EC2) instances.



![aeb-architecture2](https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/57a5c8e3-7d0a-4e97-b29f-c2b1ffebd799)


+ The load balancer sits in front of the Amazon EC2 instances, which are part of an Auto Scaling group. 

+ Amazon EC2 Auto Scaling automatically starts additional Amazon EC2 instances to accommodate increasing load on your application. 

+ If the load on your application decreases, Amazon EC2 Auto Scaling stops instances, but always leaves at least one instance running.

+ Amazon Route 53 is a highly available and scalable Domain Name System (DNS) web service. It provides secure and reliable routing to your infrastructure.

+ The software stack running on the Amazon EC2 instances is dependent on the container type. A container type defines the infrastructure topology and software stack to be used for that environment.

+ The Amazon EC2 instances shown in the diagram are part of one security group. A security group defines the firewall rules for your instances.

+ By default, Elastic Beanstalk defines a security group, which allows everyone to connect using port 80 (HTTP). You can define more than one security group.
  

### Example

+ Elastic Beanstalk environment with an Apache Tomcat container uses the Amazon Linux operating system, Apache web server, and Apache Tomcat software.
  
+ Each Amazon EC2 instance that runs your application uses one of these container types.


## Host Manager

a software component called the host manager (HM) runs on each Amazon EC2 instance. 

The host manager is responsible for the following:

1. Deploying the application

2. Aggregating events and metrics for retrieval via the console, the API, or the command line

3. Generating instance-level events

4. Monitoring the application log files for critical errors

5. Monitoring the application server

6. Patching instance components

7. Rotating your application's log files and publishing them to Amazon S3


## Presets

+ Each preset includes recommend values for several configuration options.

+ The Single instance presets are primarily recommended for development use cases and will save costs.

+ The High availability presets are recommended for production environments. They include a load balancer and scale with multiple instances in response to load.

+ If Custom configurations is selected, Elastic Beanstalk will provide the standard default values. Choose this option if you are deploying a source bundle with configuration files.


## Elastic Beanstalk architecture for a Worker environment tier

+ AWS resources created for a worker environment tier include an Auto Scaling group, one or more Amazon EC2 instances, and an IAM role.

+ For the worker environment tier, Elastic Beanstalk also creates and provisions an Amazon SQS queue if you don’t already have one.

+ When you launch a worker environment, Elastic Beanstalk installs the necessary support files for your programming language of choice and a daemon on each EC2 instance in the Auto Scaling group.

+ The daemon reads messages from an Amazon SQS queue.

+ The daemon sends data from each message that it reads to the web application running in the worker environment for processing.

+ If you have multiple instances in your worker environment, each instance has its own daemon, but they all read from the same Amazon SQS queue.

+ Amazon CloudWatch is used for alarms and health monitoring



![aeb-architecture_worker](https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/d305d359-e8a4-4a0c-974d-6209ac68593e)



## Amazon SQS

+ Amazon Simple Queue Service (Amazon SQS) offers a secure, durable, and available hosted queue that lets you integrate and decouple distributed software systems and components.

+ It exchanges and stores messages between software components. 
  
+ It provides a generic web services API that you can access using any programming language that the AWS SDK supports.


# Design Consideration

Because applications deployed using AWS Elastic Beanstalk run on AWS Cloud resources, you should keep several configuration factors in mind to optimize your applications: 

1. Scalability

2. Security

3. Persistent storage

4. Fault tolerance

5. Content delivery

6. Software updates and patching

7. Connectivity.

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.concepts.design.html >> Use this link to know more about design consideration.


## Permissions

When you create an environment, AWS Elastic Beanstalk prompts you to provide the following AWS Identity and Access Management (IAM) roles:


**Servicerole**
      
+ Elastic Beanstalk assumes a service role to use other AWS services on your behalf.

Example : example, Elastic Beanstalk uses a service role when it calls Amazon Elastic Compute Cloud (Amazon EC2), Elastic Load Balancing, and Amazon EC2 Auto Scaling APIs to gather information. The service role that Elastic Beanstalk uses is the one that you specified when you create the Elastic Beanstalk environment.


**Instance profile**

+ Elastic Beanstalk applies instances profile to the instances in your environment.An instance profile is a container for an IAM role that you can use to pass role information to an EC2 instance when the instance starts.

+ If your web application requires access to other additional AWS services, add statements or managed policies to the instance profile that allow access to those services.


**User policies**  

+ Applying user policies allows the users to create and manage Elastic Beanstalk applications and environments. Elastic Beanstalk also provides managed policies for full access and read-only access.

+ Elastic Beanstalk requires permissions not only for its own API actions, but also for several other AWS services. Elastic Beanstalk uses user permissions to launch resources in an environment. 

+ These resources include EC2 instances, an Elastic Load Balancing load balancer, and an Auto Scaling group.


## Platforms

+ AWS Elastic Beanstalk provides a variety of platforms on which you can build your applications. You design your web application to one of these platforms, and Elastic Beanstalk deploys your code to the platform version you selected to create an active application environment.

+ Elastic Beanstalk provides platforms for different programming languages, application servers, and Docker containers. Some platforms have multiple concurrently-supported versions.

AWS Elastic Beanstalk provides a variety of platforms on which you can build your applications.

1. Linux

2. Docker

3. Go

4. Java

5. .NET and .NET core

6. Node.js

7. PHP

8. Python

9. Ruby

Use this link to know more about platforms >> https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts-all-platforms.html


## How to create Elastic Beanstalk




### **Step 1: Login to the AWS management console**



### **Step 2: Click on the Elastic Beanstalk service under the services dropdown**



![bs4](https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/efdf2ff5-8689-4bc1-9e05-d81afb1e818d)




### **Step 3: Click on Get Started on the opening page and then create a Web Application by providing the required details.**



### Configure Environment



+ ### **In Environment tier we can choose beanstalk environment and give name for your application**




<img width="679" alt="Createbean" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/767a596a-0c73-489c-9d0c-58353627a7c9">





+ ### **Key details you provide here:**


+ Environment name


+ Domain – A subdomain for accessing your web application.



<img width="485" alt="create1" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/62361acf-bb1e-40f3-b463-cb9eac181408">




+ ### **Choose platform type and choose platform, branch and version  (example: python, docker, go, etc...)**




<img width="485" alt="c2" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/ba8ee064-f4e9-4f0d-ba74-9921c839f93a">




+ ### **In Application code choose upload your code. we can choose local code Zip file or provide s3 URL.**  




<img width="486" alt="c3a" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/a982ef3b-6cdc-419a-bcb3-d45da1297f37">




+ ### **Select a preset configuration that matches your use case. Each preset includes recommend values for several configuration options.**




<img width="487" alt="c4" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/8b8ed996-5160-423e-95b9-98f835d9854e">




### Configure service access



### **Step 4: In this step we need to provide service role, key pair, instance profile details and click next** 





<img width="685" alt="c5" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/ff065bb0-ffc2-4906-9896-3a0457c9184e">





### Set up networking, database, and tags 



### **Step 5:This steps are optional when we create application. values for this fields are  assigned automatically.**


+ ### **Select our VPC**




<img width="489" alt="c6" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/1ad5e876-78ff-4394-8f1d-e25dffe991b4">




+ ### **Add public IP and Instance subnetes**



<img width="486" alt="c7" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/903ed621-685a-4e22-96af-a73b8be26a7c">




+ ### **when we need to integrate our database to our application provide all information.** 






<img width="493" alt="c8" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/3ae96322-36e6-49c7-b1cb-8f9b7f3c9866">





+**use default values for this**





<img width="484" alt="c9" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/dba0fa9a-bc0d-498f-b7b1-9b4f796737c5">







+ **Use tags to group and filter resources.**




<img width="490" alt="c10" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/6f20087f-2a02-4680-9b00-5a1c6c89336e">






### Configure instance traffic and scaling



### **Step 6: This steps are optional when we create application. values for this fields are  assigned automatically.**




<img width="304" alt="c11" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/b14f59a7-ea6e-4661-898a-cba486c04935">




+**check security groups and capacity**




<img width="317" alt="c12" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/8b75d253-29d3-4bc1-8025-6cd1f1a56595">





+**Select t2.micro instance for our application**




<img width="305" alt="c13" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/a329ba07-846d-4060-a9cf-845c7f6c67df">



### Configure updates, monitoring, and logging





### **Step 7: This steps are optional when we create application. Values for this fields are  assigned automatically.**




<img width="410" alt="c14" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/a7824b6a-11af-470e-b590-711ab689d926">




+**uncheck Managed updates**





<img width="410" alt="c15" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/6cd9057f-770d-475a-9b4d-b31616ca9f75">






+**Give Email id if need email notification**






<img width="407" alt="c16" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/3e53372a-ace8-4038-b32f-46999327b8e4">





+**Choose updates as fixed or percentage**






<img width="308" alt="c17" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/d26251b5-a08e-41a4-a039-30572eac883d">





+**If your project contains env variables you need add in env property**








<img width="308" alt="c18" src="https://github.com/zen-class/zen-class-devops-documentation/assets/77039703/2c45ff93-e215-4790-9225-ff6c44e29eb8">







### Review


**Step 8: Review our Configuration and Submit to create our application using beanstalk**


## Pricing

There is no additional charge for Elastic Beanstalk. You pay only for the underlying AWS resources that your application consumes.


## Tutorials and samples

Language and framework specific tutorials are spread throughout the AWS Elastic Beanstalk Developer Guide.

Use this link to know more about beanstalk deployment samples in all platforms
     https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/tutorials.html





















