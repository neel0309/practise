When I connected my Private instance to NAT Gateway ans SSHd to private instance
through Bastion I was able to talk to S3 Bucket. But when I removed the NAT Gateway
and used VPC Endpoint I was not able to talk to S3.

The reason behind this was my S3 buckets were in different region so I had to create 
the buckets in the same region which is asia-south1 in my case and it worked.

