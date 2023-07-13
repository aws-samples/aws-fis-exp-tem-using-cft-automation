## Automation of AWS FIS experiment templates using AWS CloudFormation

This repository contains CloudFormation templates for AWS Fault Injection Simulator (FIS). AWS FIS is a service that helps you perform fault injection experiments on your applications and infrastructure to test their resiliency. By using these templates, you can easily provision and configure FIS resources in your AWS account.

Experiment Templates for the following Experiments are included in the CFN:

* Stopping an EC2 instance
* Reboot an EC2 instance
* Stop and Start an EC2 instance
* Kernel Crash - intentionally causing a kernel crash at the operating system level
* Network Disruption (Subnet) - Causing a disruption in network connectivity at the subnet level by blocking incoming and outgoing traffic to and from the subnet.
* EBS IO Stuck - This will temporarily halt the input/output (IO) activity on all the underlying EBS volumes attached to the database server including root volumes.


## Prerequisites
Before using these templates, make sure you have the following pre-requisites:

* An AWS account with the necessary permissions to provision FIS resources.
* AWS Console access Or AWS Command Line Interface (CLI) installed and configured with your AWS credentials.
* Basic knowledge of CloudFormation and FIS concepts.

## Getting Started
To get started, follow these steps:

* Clone or download this repository to your local machine
* Navigate to the repository’s directory: $ cd aws-fis-exp-tem-using-cft-automation
* Ensure you have the AWS CLI installed and properly configured with your AWS account credentials: $ aws configure
* Modify the template parameters and configurations as needed for your specific use case.

## Templates
CFT Name: awsfis_experiment template_stopec2.yaml

    FIS Action: aws:ec2:stop-instances
    Target Type: aws:ec2:instance
    User Input: 1) Experiment Name 2) User should select instance from the list

CFT Name: awsfis_experiment template_rebootec2.yaml

    FIS Action: aws:ec2:reboot-instances
    Target Type: aws:ec2:instance
    User Input: 1) Experiment Name 2) User should select instance from the list

CFT Name: awsfis_experiment template_stopstartec2.yaml

    FIS Action: aws:ec2:stop-instances
    Target Type: aws:ec2:instance
    User Input: 1) Experiment Name 2) User should select instance from the list

CFT Name: awsfis_experiment template_networkdisrupt.yaml

    FIS Action: aws:network:disrupt-connectivity
    Target Type: aws:ec2:subnet
    User Input: 1) Experiment Name 2) User should enter duration for which experiment should run (numeric) 3) User should select subnet from the list

CFT Name: awsfis_experiment template_ebspausevolume.yaml

    FIS Action: aws:ebs:pause-volume-io
    Target Type: aws:ec2:ebs-volume
    User Input: 1) Experiment Name 2) User should enter duration for which experiment should run (numeric) 3) User should select ebs volume from the list

CFT Name: awsfis_experiment template_ssmsendcmdec2.yaml

    FIS Action: aws:ssm:send-command
    Target Type: aws:ec2:instance
    User Input: 1) Experiment Name 2) User should select instance from the list
    Remark: 1) SSM document is in CFT itself 2) Document arn for the created SSM document will be auto used in the experiment

## Console Approach
Open the AWS CloudFormation 

*  link in a new tab and log in to your AWS account.
* Choose Create stack (With new resources (Standard) from the top-right side of the page.
* In Prepare template, choose Template is ready.
* In Template source, choose Upload a template file.
* Choose the Choose file button and navigate to your workshop directory.
* Select the appropriate template file .yaml 
* Choose Next.
* Provide a Stack name. For example cfn-fis-exptemplate-stopec2. 
* The Stack name identifies the stack. Use a name to help you distinguish the purpose of this stack.
* Choose Next.
* Choose to accept default values for Configure stack options; choose Next.
* On the Review <stack_name> page, scroll to the bottom and choose Submit.
* Use the refresh button to update the page as needed, until you see the stack has the CREATE_COMPLETE status.

Note:

* Monitor the CloudFormation stack creation process to ensure successful deployment
* Once the stack creation is complete, you can verify the  resources created by navigating to AWS Management Console or using AWS CLI commands



## CLI Approach
* Choose the appropriate template based on the resources you want to create.
* Execute CloudFormation ‘create-stack’ command to create the stack, specifying the template file and any required parameter

Note:

* Monitor the CloudFormation stack creation process to ensure successful deployment
* Once the stack creation is complete, you can verify the  resources created by navigating to AWS Management Console or using AWS CLI commands

## Clean Up
Follow these steps to clean up created resources:

In the CloudFormation console 

* Select the stacks you have created using the above cloudformation templates. 
* In the top right corner, select Delete.
* In the pop-up window, select Delete.
* Wait for the stack to reach the DELETE_COMPLETE status. You need to periodically select Refresh to see the latest stack status.



## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

