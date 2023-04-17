# CloudFormation--Creating-DynamoDB-Tables-EC2-Instances-and-IAM-Policies

AWS CloudFormation is a service that helps you automate the process of creating and managing AWS resources. It allows you to use a template to provision and manage resources in your AWS account. The template is written in JSON or YAML and specifies the resources that you want to create, as well as the properties for those resources.

With CloudFormation, you can create and manage resources in a safe and predictable manner, and you can use version control to track changes to your infrastructure. It also makes it easier to automate the provisioning and management of resources, as you can use the same template to create and manage resources in multiple regions or accounts.

In this step-by-step guide, I will walk you through using a Cloud Formation template to launch:

An EC2 Instance within a public subnet

Create a DynamoDB Table

Create a DynamoDBReadOnly IAM Policy for our EC2 Instance

Let’s get started!

Prerequisites

A computer with internet access
AWS account with admin IAM access permissions
AWS EC2 instance configuration knowledge
Comfort with AWS Cloud Formation
General knowledge and understanding of YAML code structure


Step 1: Understanding the YAML code of CloudFormation Template

Below you will find my CloudFormation template which is going to create the stack of our infrastructure of resources. In short, this is what is being created

Creating a VPC

Creating an Internet Gateway & attaching it to our VPC

Editing the Route Table of our IG to allow traffic out of our VPC

Creating a Public Subnet

Creating a Security Group for our VPC and allowing traffic on ports 80, 22, and 443


You will notice that I am dictating the order that CloudFormation is launching the resources by using the “DependsOn” command. Without these, my stack will fail when dependent resources are not present.

Once all those resources are provisioned, my CloudFormation template will continue to build my DynamoDB Table, IAM Policy, EC2 Instance, and attach the IAM Policy to our EC2 Instance.


Here is our CloudFormation template in YAML all together before we upload to AWS:


        
Step 2: Launching our Template in CloudFormation

From the AWS Console, we navigate to the CloudFormation dashboard, upload our .YAML CloudFormation template and launch our new stack.

On Step 1 of the CloudFormation create stack, I am going to upload our .yaml file and click the orange “Next” button.


On Step 2, I give our stack a name then click the orange “Next” button.


On Step 3, I am going to ensure that CloudFormation ‘rolls back’ any resources created if our template encounters an error. I click the orange “Next” button.


Finally, since I am creating an IAM policy, I need to acknowledge this CloudFormation disclaimer. I check the box and click the orange “Submit” to run my CloudFormation template.


CloudFormation stacks can take several minutes to provision all the resources depending on the size of your stack.

It took my stack about 7 minutes to complete.


What a CloudFormation Stack looks like while in process via the stacks dashboard
Our CloudFormation has provisioned our stack successfully.


Once our stack is completed, we can verify our results in the AWS Console.

Step 3: Test our IAM Policy using EC2 Connect into our EC2 Instance
After our Stack completes, we can move over to our EC2 Dashboard to confirm that our instance is running and that our IAM Policy was attached to our instance


Let’s SSH into our EC2 Instance to verify our IAMPolicy is working as designed

Since my CLI was already configured with an AWS account with admin access permissions. For our testing, I chose to use the EC2 Instance Connect to ensure that my aws configure access via my CLI wasn’t circumventing the IAM policy for our EC2 instance.

This code is attempting to write to our DynamoDB Table. If our IAM policy was written as intended, we should receive a “permission denied” error message returned to our CLI:


Presto! Our CloudFormation template was designed and implemented correctly.
