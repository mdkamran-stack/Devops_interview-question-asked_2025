## what is CIDR?
Class inter domain routing.

<img width="870" height="601" alt="image" src="https://github.com/user-attachments/assets/f871cb26-8fd6-4988-9f2c-e223f8dc0314" />  


First we'll create vpc and subnet pub as well private subnet each sub has atleast 256 ip ranges  its own ip ranges these ip ranges subset of vpc cidr range , we will create subnet in two diff data center one for pub & aother for private coz if something wrong it server the traffic from anoter one.  
2: how to create route table and how to associate with subnet each subnet should  route table is essential when we route traffic suppose a user want to access machine   
route table play a crucial role how the request coming and go ut of vpc as well as subnet these route table is same for private & public subnet 
after that we will look NAT to provide inernet access to our private subnet       

implementation 1 vpc creation 2 sbnet creation public n private with diff datacenter 3 Route table pub & private & then create association with pub route table assocation with public subnet and same for private & then we have to associate public route table to IGQ same for private.
1 EC2 instance attached with public subnet     
**2 EC2 instance we want to access some package but dont want to expose publically for this well attached public subnet to NAT gateway, it download package from internet but no one can access from internet to this instance , route tobe route to private RT & allocate elastic ip.**  

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

## Route53

Route 53 is AWS’s DNS service that connects domain names to resources like EC2, S3, or Load Balancers. 

## to configure loadbalncer with route53.

<img width="849" height="377" alt="image" src="https://github.com/user-attachments/assets/494a4256-4b32-4ba5-ac0d-d8409c7b09a9" />


Once user is trying to access url: example.com it going to ggole domain finding NS for our route 53 request come to route 53 hosted zone once req enter hosted zone it goes to LB & LB forward req to EC2 instances.  

## Private subnet doesnt have internet access keep in mind.

## Target group is logical grouping of EC2 instances so that our LB can choose this particular group to forward the request.

Once we able to fetch our url then we have to setup from linking LB to EC2 instance .
but we need to create a record so our route 53 will redicrect to LB and LB redicrt to ec2 , after creating A record it will pointg to ec2 instance 

## Weighted routing we will be putting the percentage the share traffic load on multiple LB like suppose 2 LB then percentage we will define like 50 -50 or as per need.

for creating 2nd LB we have to create vpc route table IGW and target group and APP LB n security GB add sg to LB Finally we have to create weighted routing for percentage of load and putting A record point to LB  weight is 0-256 we will putting 127 so it will become 50 %

## Geolocation Routing .
We will create geolocation A record eg 1 from sweadan 2 nvergenia whatever location we select it will server for those location  

## Failover routing 
suppose if we have primary and secondary LB if anyone fails it will server from another one 

## Route53 is global resource it doesnt belong to any region or any vpc. 






