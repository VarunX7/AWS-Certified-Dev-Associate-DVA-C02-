# ASG: Auto Scaling Group

In real-life, the load on your websites and applications can change. You can create and get rid of servers very quickly

The goal of an Auto Scaling Group (ASG) is to:
* Scale out (add EC2 Instances) to match an increased load
* Scale in (remove EC2 Instances) to match a decreased load
* Ensure we have a minimum and a maximum number of machines running
* Automatically register new instances to a load balancer

ASGs can be scaled in/out based on Cloudwatch alarms.
You can set an alarm based on any metric in cloudwatch including your custom metrics.

#### ASGs have the following attributes
* A launch Template ("Launch configurations" is deprecated)
    * AMI + Instance Type
    * EC2 User Data
    * EBS Volumes
    * Security Groups
    * SSH Key Pair
    * IAM roles for your EC2 instances
    * Network + Subnets Information
    * Load Balancer Information
* Scaling Policies
* Min Size / Max Size / Initial Capacity

#### Dynamic Scaling Policies
* **Target tracking Scaling** : For eg- You want your avg ASG CPU to stay around 40%
* **Simple/ Step Scaling** : When a cloudwatch alarm is triggered then add or remove x no. of instances
* **Scheduled Actions** : Anticipate a scaling based on known usage patterns (eg- increase min capacity to 10 at 5pm on friday)
* **Predictive Scaling** : continuously forcast load and schedule scaling by analysing previous load data

#### Good metrics to scale on
* CPU utilization
* Request count per target
* Average network in/out
* Any application specific custom metric that you push to cloudwatch

#### Scaling Cooldown
* After a scaling activity happens, you enter a cooldown period (default 300s)
* During cooldown ASG will not launch or terminate additional EC2 instances(to allow metrics to stabilize)
* You should use ready to use AMI to reduce configuration time in order to serve requests faster and reduce cooldown period

#### ASG Instance Refresh
* Sometimes you may want to launch a new version of our current EC2 instances without stopping all of them at once
* For this you use the native feature of 'instance refresh' 
* you set a minimum number of instances that need to keep running  
* Rest of the instances would be terminated and new instances would be launched from the updated AMI
* This happens over and over again until all the previous instances are terminated and new ones are launched
You can specify warm-up time( how long before the new EC2 instance is ready to accept traffic)

#### ASG Summary
* Scaling policies can be on CPU, Network… and can even be on custom metrics or based on a schedule (if you know your visitors patterns)
* ASGs use Launch Templates, and you update an ASG by providing a new launch template
* IAM roles attached to an ASG will get assigned to EC2 instances
* ASG are free. You pay for the underlying resources being launched
* Having instances under an ASG means that if they get terminated for whatever reason, the ASG will restart them. Extra safety
* ASG can terminate instances marked as unhealthy by an LB (and hence replace them)