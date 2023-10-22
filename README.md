## Automation of AWS FIS experiment templates using AWS CloudFormation

This repository contains CloudFormation templates for AWS Fault Injection Simulator (FIS). AWS FIS is a service that helps you perform fault injection experiments on your applications and infrastructure to test their resiliency. By using these templates, you can easily provision and configure FIS resources in your AWS account.

Experiment Templates for the following Experiments are included in the CFN:

* Stopping an EC2 instance
* Reboot an EC2 instance
* Stop and Start an EC2 instance
* Kernel Crash - intentionally causing a kernel crash at the operating system level
* Network Disruption (Subnet) - Causing a disruption in network connectivity at the subnet level by blocking incoming and outgoing traffic to and from the subnet.
* EBS IO Stuck - This will temporarily halt the input/output (IO) activity on all the underlying EBS volumes attached to the database server including root volumes.

It also has an optional section with step by step instructions to change ebs volumes from gp2 to gp3 at scale.

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
    User Input: 1) Experiment Name 2) User should select instance from the list 3) User should enter duration for which experiment should run (numeric)

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

## Changing EBS volumes from GP2 to GP3 at scale (Optional - in case you have a requirement)
When you modify an EBS volume from one type to another e.g. gp2 to gp3, or modify IOPS and throughput to an existing gp3 or io2 volume types, the volume and instance will continue to work, you do not need to restart the instance or detach the volume.

Manually changing volumes is a repetitive process that can be prone to human error: Providing a script to automate the GP2 to GP3 volume conversion at scale. We can use AWS Cloudshell which is a browser-based shell with AWS CLI access from the AWS Management Console and execute the automation provided.

* Download the script from the this link: modify_ebs_gp2_to_gp3 to a folder on your PC/laptop.
* In AWS console, enter "cloudshell" in the search bar and select Cloudshell.
* You should now see a dialog on AWS CloudShell, click on Close.
* Click on Actions and click Upload File. 
* Select the file downloaded from the folder
* The file should get uploaded in the folder /home/cloudshell-user
* Change the permissions of the file to run it as a program by executing command: chmod +x modify_ebs_gp2_to_gp3
* Execute now the automation provided, the file ./modify_ebs_gp2_to_gp3 and have a look at the argument that are required to run it. You will see that the following arguments are required: --tag-key, --tag-value, --from-volume-type, --to-volume-type
* Execute, **./modify_ebs_gp2_to_gp3 --tag-key Name --tag-value HANA-Data --from-volume-type gp2 --to-volume-type gp3**
* and type yes. This will execute the automation script provided by the automation team and will target all gp2 volumes that have a tag Name=HANA-Data configured. Moreover, the automation script is checking the quota of EBS before starting the conversion to gp3. A disclaimer will appear, make sure you read it and write yes and press enter to accept it.
* After you execute the automation script, you will see the output about the quota check and a json block for each volume that is being migrated. You can now go back to the Volumes list in the console and you will see that the HANA-data volumes have now migrated to gp3. Make sure you refresh the page if you do not see the volume on GP3 yet.

Note: 
When doing such migrations is very important to test it first in the dev environment, doing it a larger scale you will have to take into account the service quotas for modification of GP2 volumes, and for the total storage allocated for GP3. In any case, you do have the flexibility to go back to GP2 if needed, or you could stay with GP2 and ask the automation team to keep that into account in their automation.


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

