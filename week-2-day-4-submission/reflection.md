#   Why does the bootcamp deliberately do this with the console before Terraform? What would change if you did it with Terraform from day one?
````
Doing through console helps us to understand the concepts of each resource and also help us to map 
a resource to another mentally. Doing it through terraform at the beginning will create more confusion
as we were not aware of which options to select in a resource.
````
#   In your own words, what is the difference between the EC2 health check and the ELB health check on an ASG? Which one would have replaced an instance with a hung gunicorn process?
````
The main difference between EC2 Health check and ELB health check is that EC2 HC is at instance level and it answers whether the instance is live or not but ELB health check it is at the
loadbalancer level and it answers whether the application is live or not.

In this case ELB will be able to identify wether the application is live or not
````
#   The ALB security group allows 0.0.0.0/0 on 443, but the ASG security group only allows 8000 from the ALB security group. Walk through what this restriction actually protects against.
````
Here ALB will accept request from internet on 443 (HTTPS) and ASG will accept request only from load balancer which protects the EC2 instance from outside
world. The attack surface is less.
````
#   A teammate proposes putting the database in a private subnet of the same route table as the app tier (skipping the dedicated rds-1/rds-2 subnets). What's lost? Reference NACLs in your answer.
````
If DB and App shares the same subnet then the NACL also gets shared which reduce the privacy of the DB. DB should allow only 5432 and app is more
open which makes the DB more open to attacks
````