## what is CIDR?
Class inter domain routing.

<img width="870" height="601" alt="image" src="https://github.com/user-attachments/assets/f871cb26-8fd6-4988-9f2c-e223f8dc0314" />


First we'll create vpc and subnet pub as well private subnet each sub has atleast 256 ip ranges  its own ip ranges these ip ranges subset of vpc cidr range , we will create subnet in two diff data center one for pub & aother for private coz if something wrong it server the traffic from anoter one.
2: how to create route table and how to associate with subnet each subnet should  route table is essential when we route traffic suppose a user want to access machine 
route table play a crucial role how the request coming and go ut of vpc as well as subnet these route table is same for private & public subnet 
after that we will look NAT to provide inernet access to our private subnet   

implementation 1 vpc creation 2 sbnet creation public n private with diff datacenter 3 Route table pub & private & then create association with pub route table assocation with public subnet and same for private & then we have to associate public route table to IGQ same for private.
1 EC2 instance attached with public subnet 
2 EC2 instance we want to access some package but dont want to expose publically for this well attached public subnet to NAT gateway, it download package from internet but no one can access from internet to this instance , route tobe route to private RT & allocate elastic ip.  

## creating LB 
for creating loadbalancer we have we need to create taget gruop is logical group to setup LB it create a group of ec2 instances.  
In sort request is coming to LB and LB is redirecting to target group target group is group of resources here resources is EC2 instances, target group can be n number LB is now pointing to target grp & target grp is conatiain EC2 instances.

## Transit gateway is way is unique and enable comm b/w vpc

A transit gateway (TGW) is a network transit hub that interconnects attachments (VPCs and VPNs) within the same AWS account or across AWS accounts.

## what is vpc end point 

VPC Endpoints allow private, secure access to AWS services from within a VPC without using the internet, reducing risk and cost.  

## what vpc flowlog.

VPC Flow Logs record metadata about IP traffic going in and out of a VPC, subnet, or ENI, and are mainly used for security analysis and troubleshooting connectivity issues.  

To Create VPC flow first will create vpc and 2 create private n public subnet 3 Creation if IGW associate it 4 creat route tb. 

ENI id elastcic network interface is capturing the logs.  

## N/W LB vs APP LB 

network LB is works on layer4 it is work on tcp/udp protocol N/W is used when we have very high traffic coming to application want to server millions of request with low latency  pointting to http:..nlb-url  point to landing page of app

ALB is used path based routing  http: /foo and http:/bar

## AWS Global accelarator.

## if any subnet we will attach IGW it will be public subnet.private subnet NEVER attach IGW.  

Global accelarator access from  nearest location as per geograpical location.



