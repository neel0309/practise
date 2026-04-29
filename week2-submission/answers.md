# What is the relationship cardinality between an IGW and a VPC? Why?
````
The IGW is a public facing interface to the VPC. There is 1:1 relationship between the IGW and the VPC.
````

# The route table already had one route before you added 0.0.0.0/0. What was it, what was its target, and why does it exist by default?
````
It enables communnication between the resources in the same VPC.
With this one resouce will not be able to communnicate /  connect to 
other resources in the same VPC.
````

# Open the role and look at the Trust relationships tab. What does the JSON say? Explain in your own words what "trust policy" means and how it differs from "permissions policy."
````
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "ec2.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
The above json states that ec2 AWS service can assume the role.
The trust policy states that the role can be assumed by other AWS services wheras
the permission policy states what actions the role can take.
````

# What is the difference between an AWS managed policy, a customer managed policy, and an inline policy? When would you use each?

````
AWS managed policy is a policy that is created and managed by AWS.
Customer managed policy is a policy that is created and managed by the customer.
Inline policy is a policy that is embedded in the role.
````

# What's the difference between a Gateway Endpoint and an Interface Endpoint? Which AWS services support Gateway Endpoints? Why might you still pay for an Interface Endpoint despite the cost?

````
Gateway endpoint is free to use and it connects resources withing the VPC
whereas Interface endpoints is a paid resouce and it is used when we try to
connect to resouces in on-prem or in a different VPC.

The services which supports Gateway Endpoints are:
Amazon S3
Amazon DynamoDB

We might pay for interface because if we have on-prem which connects 
via Direct Connect or VPN and try to access S3 then gateway wont work.
Also if there are two VPCs and use VPC Peering then Gateway endpoint
wont work. we need Interface endpoint so that two VPCs can talk to each other.
````