+++
title = "Deploying a Wordpress blog with AWS Lightsail"
date =  2020-06-13T05:37:54+08:00
weight = 1
+++

### Deploying a Wordpress Blog with AWS Lightsail

Let's get started with deploying a simple Wordpress application with AWS Lightsail

#### What is AWS Lightsail?

Amazon Lightsail is the easiest way to get started with AWS for developers, small businesses, students, and other users who need a simple virtual private server (VPS) solution. Lightsail provides developers compute, storage, and networking capacity and capabilities to deploy and manage websites and web applications in the cloud. Lightsail includes everything you need to launch your project quickly – a virtual machine, SSD-based storage, data transfer, DNS management, and a static IP – for a low, predictable monthly price.

You can watch the [video] (https://www.youtube.com/watch?v=taMlabDBO58) below to have a quick overview of Lightsail

{{< youtube id="taMlabDBO58" >}}

Lightsail provides the following resources:
* Lightsail instances - These are burstable performance instances that provide a baseline level of CPU performance with the additional ability to burst above the baseline. This design enables you to get the performance you need, when you need it, while protecting you from the variable performance or other common side effects that you might typically experience from over-subscription in other environments.
* Load balancers - A Lightsail load balancer distributes incoming web traffic among multiple Lightsail instances, in multiple Availability Zones. Load balancing increases the availability and fault tolerance of the application on your instances. You can attach Lightsail instances to your load balancer, and then you can configure HTTPS with a validated SSL/TLS certificate. Lightsail also performs health checks on your instances so that you can monitor error codes, response time, and more.
* Block storage - Every Lightsail server comes with high-performing, persistent SSD-based block storage. Your system disk is automatically replicated within its Availability Zone to protect you from component failure. This offers you high availability and durability. You can also create additional block storage disks and attach them to your Lightsail instances.
* Databases - You can create a MySQL or PostgreSQL managed database in Amazon Lightsail with a few steps. Lightsail makes database administration more efficient by managing your common maintenance and security tasks. 
* Public IP and Private addresses - A public IP address is the address that is assigned to a Lightsail instance to allow direct access over the Internet. Web servers, email servers, or any server devices that can be accessed directly from the Internet are all candidates for a public IP address. A public IP address is globally unique, and can only be assigned to a unique device. A private IP address is the address space allocated by InterNIC to allow organizations to create their own private networks.
* Static IP addresses - A static IP is a fixed, public IP address that you can assign and reassign to an instance or other resource. If you haven't set up a static IP address, each time you stop or restart your instance, Lightsail assigns a new public IP address.
* Snapshots - You can create point-in-time snapshots of instances, databases, and block storage disks in Amazon Lightsail, and use them as baselines to create new resources or for data backup. A snapshot contains all of the data that is needed to restore your resource (from the moment when the snapshot was taken). You will be billed a snapshot storage fee for snapshots on your Lightsail account.
* CDN - Lightsail content delivery network (CDN) distributions make it easy for you to accelerate the delivery of content hosted on your Lightsail resources by storing and serving it on Amazon’s global delivery network, powered by Amazon CloudFront. Distributions also help you enable your website to support HTTPS traffic by providing simple SSL certificate creation and hosting.

#### Example of Lightsail deployments
* Wordpress blogs
* Drupal/Joomla and other Content Management Systems
* E-commerce platforms like Magento
* Project management applications like Redmine
* Development stacks like LAMP, Node.js, MEAN stack, Django
* Version control services like GitLab

#### Who uses AWS Lightsail?


#### Step-by-Step Guide

##### Step 1: Log in to the AWS console
[Sign up for AWS] (https://console.aws.amazon.com/console/home), or [sign in to AWS] (https://console.aws.amazon.com/console/home) if you already have an account.

##### Step 2: Create a WordPress instance in Lightsail
1. Sign in to the [Lightsail console] (https://lightsail.aws.amazon.com/).
2. On the **Instances** tab of the Lightsail home page, choose **Create instance**.
3. Choose the AWS Region and Availability Zone for your instance.
4. Choose your instance image.
    1. Choose Linux/Unix as the platform.
    2. Choose **WordPress** as the blueprint.
    3. Choose an instance plan.
    A plan includes a low, predictable cost, machine configuration (RAM, SSD, vCPU), and data transfer allowance. You can try the $3.50 USD Lightsail plan without charge for one month (up to 750 hours). AWS credits one free month to your account.
    4. Enter a name for your instance. Resource names:
        * Must be unique within each AWS Region in your Lightsail account.
        * Must contain 2 to 255 characters.
        * Must start and end with an alphanumeric character or number.
        * Can include alphanumeric characters, numbers, periods, dashes, and underscores.
    5. Choose **Create instance**.

##### Step 3: Connect to your instance via SSH and get the password for your WordPress website
The default password to sign in to the administration dashboard of your WordPress website is stored on the instance.

Complete the following steps to connect to your instance using the browser-based SSH client in the Lightsail console, and get the password for the administration dashboard.

1. On the **Instances** tab of the Lightsail home page, choose the SSH quick-connect icon for your WordPress instance.
2. After the browser-based SSH client window opens, enter the following command to retrieve the default application password:
```
cat $HOME/bitnami_application_password
```
3. Make note of the password displayed on the screen. You use it later to sign in to the administration dashboard of your WordPress website.

##### Step 4: Sign in to the administration dashboard of your WordPress website
1. In a browser window, go to:
```
http://PublicIpAddress/wp-login.php
```
In the address, replace **PublicIpAddress** with the public IP address of your WordPress instance. You can get your instance's public IP address from the Lightsail console.

2. In the **Username or Email Address** box, enter ```user```.
3. In the **Password** box, enter the default password obtained earlier in this tutorial.
4. Choose *Log in*.

##### Step 5: Create a Lightsail static IP address and attach it to your WordPress instance
The default public IP for your WordPress instance changes if you stop and start your instance. A static IP address, attached to an instance, stays the same even if you stop and start your instance.

1. On the **Instances** tab of the [Lightsail home page] (https://lightsail.aws.amazon.com/), choose your running WordPress instance.
2. Choose the **Networking** tab, then choose **Create static IP**.
3. The static IP location, and attached instance are pre-selected based on the instance that you chose earlier in this tutorial.
4. Name your static IP, then choose **Create**.

##### Step 6: Create a Lightsail DNS zone and map a domain to your WordPress instance
Transfer management of your domain's DNS records to Lightsail. This allows you to more easily map a domain to your WordPress instance, and manage more of your website’s resources using the Lightsail console.

1. On the **Networking** tab of the Lightsail home page, choose **Create DNS zone**.
2. Enter your domain, then choose **Create DNS zone**.
3. Make note of the name server addresses listed on the page. You add these name server addresses to your domain name’s registrar to transfer management of your domain’s DNS records to Lightsail.
4. After management of your domain’s DNS records are transferred to Lightsail, add an A record to point the apex of your domain to your WordPress instance, as follows:
    1. In the DNS zone for your domain, choose **Add record**.
    2. In the **Subdomain** box, enter an @ symbol to map the apex of your domain (such as example.com) to your instance. The @ symbol explicitly symbolizes that you’re adding an apex record. It is not added as a subdomain.
    3. In the **Maps to** box, choose the static IP that you attached to the WordPress instance in the previous step of this tutorial.
    4. Choose the save icon.
    
    Allow time for the change to propagate through the internet's DNS before your domain begins routing traffic to your WordPress instance.

#### Resources

[Launch and configure a WordPress instance with Amazon Lightsail in 10 minutes] (https://aws.amazon.com/getting-started/hands-on/launch-a-wordpress-website/)

[Launch a Linux Virtual Machine with Amazon Lightsail in 10 minutes] (https://aws.amazon.com/getting-started/hands-on/launch-a-virtual-machine/)

[Creating a database in Amazon Lightsail] (https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-creating-a-database)

[Connecting to your Lightsail instance, database] (https://lightsail.aws.amazon.com/ls/docs/en_us/search?s=Connect&c=how-to)

[Creating a snapshot of your Linux or Unix instance in Amazon Lightsail] (https://lightsail.aws.amazon.com/ls/docs/en_us/articles/lightsail-how-to-create-a-snapshot-of-your-instance)

[Enabling or disabling automatic snapshots for instances or disks in Amazon Lightsail] (https://lightsail.aws.amazon.com/ls/docs/en_us/articles/amazon-lightsail-configuring-automatic-snapshots)

[Creating and attaching additional block storage disks to your Linux-based Lightsail instances] (https://lightsail.aws.amazon.com/ls/docs/en_us/articles/create-and-attach-additional-block-storage-disks-linux-unix)

[How do I shut down my Amazon Lightsail resources?] (https://aws.amazon.com/premiumsupport/knowledge-center/shut-down-lightsail/)