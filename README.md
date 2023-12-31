
# EC2 Memory Disk Monitoring

Monitor EC2 Memory and Disk Utilisation using CloudWatch | AWS EC2 Monitoring with Cloudwatch | Monitor Memory and Disk Utilisation using CloudWatch | AWS CloudWatch Demo






## Steps

- Step 1: Create an AWS EC2 IAM Role and add CloudWatch and SSM Full Access. - Role Name: EC2-CloudWatch-Role.

- Step 2: Create a Parameter in Systems Manger with the name "/alarm/AWS-CWAgentLinConfig" and store the value.

- Step 3: Create an EC2 Instance, Attach the role created in Step 1 and Add the commands in the Userdata Section.


## Commands that needs to be added in Userdata Section:

use this below shell Command while running EC2

```bash
#!/bin/bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
unzip AmazonCloudWatchAgent.zip
sudo ./install.sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:/alarm/AWS-CWAgentLinConfig -s
```

## Check if EC2 Instance has CWAgent Installed or not:

use this to check the if cloudwatch is Installed or running on EC2 connect

```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

## Value for the SSM Parameter (/alarm/AWS-CWAgentLinConfig):

```bach
{
	"metrics": {
		"append_dimensions": {
			"InstanceId": "${aws:InstanceId}"
		},
		"metrics_collected": {
			"mem": {
				"measurement": [
					"mem_used_percent"
				],
				"metrics_collection_interval": 60
			},
            "disk": {
				"measurement": [
                     "disk_used_percent"
				],
				"metrics_collection_interval": 60
			}
		}
	}
}
```

## Reference/Additional Reading:

1 . https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent-New-Instances-CloudFormation.html

2 . https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html

 


## Complete Guide

 - First we have created IAM role with 
 SSM FullAccess , EC2 FullAccess ,  CloudWatch FullAccess

 - Then we have created Parameter in SSM name : /alarm/AWS-CWAgentLinConfig and in values we have pasted code of mentioned above Value for the SSM Parameter (/alarm/AWS-CWAgentLinConfig):
 - Then we Created Ec2 Instance and  
 assigned IAM instance profile  Info name and select the role created earlier after that paste the Userdata mentioned above.

 - Then Connect the ec2 and Check if EC2 Instance has CWAgent Installed or not :  sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status 

 - if this running and configured it means agent is running 

 - now go to CloudWatch create alarm and select matric  select CWAgent for memory select instanceid and for disk select instance, disk,etc. give 80 persent threshold next it will show you memory adn disk utilazation 

 - MISSION SUCCESSFUL :)
 



## Support

For support, follow https://github.com/roshanwaghmare

 


