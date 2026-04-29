# In Task 2.1 you opened SSH from 0.0.0.0/0. In a real company, what would you restrict it to instead, and how?

````
1. We can use your company IP or VPN IP. In source we will
put the company IP or VPN IP.
2.  We can use a Bastion Host/Jump server
3. We can use SSM. No SSH is required also no port is added.
````
# You copied a private key onto the bastion in Task 3.2. Name two reasons this is a bad practice and one alternative.

````
We can use AWS Session Manager instead of copying the keys
 1. The private key in Bastion is risky as Bastion becomes the 
 key server so anyone get access to Bastion can access Private
 servers.
````
# When would you choose a Gateway Endpoint vs an Interface Endpoint vs just using the NAT Gateway?

````
Gateway Endpoint -> 1. Zero cost 2. Access rources within VPC
Interface Endpoint -> 1. Cost 2. When we want to connect to resources
on-prem via transit gateway or vpc peering.
NAT Gateway -> 1. Costly 2. when Private instances wants to connect to internet
````

# VPC Peering vs Transit Gateway — when does Transit Gateway start to make sense?

````
VPC Peering --> this is used when we have 2 - 5 VPCs max. Low Latency. No hop.
Transit Gateway --> when we have more VPCs then we can use transit gateway. All VPCs connects to
on transit gateway creating a mesh. Support transitive routing as well.
````