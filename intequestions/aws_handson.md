# 1> IAAS , here we manage 
Application , data , runtime , OS , middleware 

# 2> PAAS 
Application & data  AWS Elastic bean stack

# 3> paas 
its like plug and play best example Gmaol , microsfot 365  

# Instance types

General purpose T2 M5 M4 M3  
Memory optimized x1e X1 R4 R3  
Storage opt  h1 i3 D2  
Accelarated computing P3 P2 G3 F1  
Compute opt C5 C4 C3   

# IAM 
Represent an entity that is created in aws can be a person or a service No Persmission by default.  

## Access requirements

### Programtic access : User needs to make Api calls from program or use CLI to access AWS resources.  
### Management Console : User needs to access aws resources from management console.  

# IAM Group Groups or collection of IAM user. 

# IAM Policies: 
## Policies are json documents which mention what an user or group can do on AWS resources it defines authorization for aws resource.  

### conatins 3 componenets at least (EAR)
## Effect: weather action are allowed /denied on resources.  
## Action: what action are allowedd or denied eg create delete   
## resources: AWS resources like EC2 instance , ELB 

# Types of policies 
## AWS manged policy 
## cutomer managed policies  
## Inline policies (inheritance policies)

# IAM ROLES 

## Role is similar to an user/group which has permission/policies attached to it.  
## Roles are temporary access given to anyone who needs to perform the specific task mentioned in te role.  

# EC2 instance in private subnet need package update 
1 create natgateway attach to public subnet with your vpc & Allocate ealstic ip 

3 Edit route table of private subnet add route 0.0.0.0 target should be natgateway 

make sure public subent as route to IGW  whereas private subnet has route to natgateway 



