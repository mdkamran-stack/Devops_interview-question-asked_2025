## Provider
A provider is a plugin that lets TF manage an external API.

## Terraform INIT will dowload provider plugins

## TF destroy  cmd terraform destroy -target aws_instance.myec2  (target play a crucial role to target resource whcih we have to delete.

The -target option can be used to focus TF attention on only a subset of resources.while we need to destroy resource we have to comment it out from TF file
or remove the resources, coz next time we tf apply it will again create the resources.  
we can achive detroy by commenting out as well commnet out and tf apply it will destroy resources.  

## TF state file : 
TF stores the state of the infra that is being created from the TF files, This state allows TF to map real world resource to your existing configuration.

## Desired state current state. 
DESIRED state: TF primary func is to create modify & destroy infra resources to match the desired state describe in TF conf.
Current state: Current state is the acutal state of resource that is currently deployed.
TF tries to ensure that the deployed infra is based on the desired state.

## dependency lock file : TF dependency lock file allows us to lock spaecific version of the provider.

## How recommended folder Structure Looks like.

1: Main terrafomr configuration file.  
2: variable.tf file that defines all the variables.  
3: terraform.tfvarsd file that defines values to all the variables.  

**Point**
if file name is terraform.tfvars > Terraform will automaticaly load values from it.  
if file name is different like prod.tfvars > you have to explicitly define the file durint plan / apply operation.  

## VAriable Defination Preedence ( It will take the values with higher precedence )

Terraform loads varibale in the following orders with later sources taking precedence over earlier ones.  

1. Environment variable
2. The terraform.tfvars file, if present
3. The terraform.tfvars.json file if present
4. Any *.auto.tfvars or *.auto.tfvars.json files, processed in lexical order of their filename.
5. Any -var and -var-file options on the command line.
   **exmaple**

   ENV varibale of TF_VAR_instance_type = "t2.micro"
   value in terraform.tfvars = "t2.large"

**Final result = "t2.large"** Always it will take higher value.  
Explicitly we can define terrform plan -var="instance_type=m5.large"  >> It will take m5.large we can define any value as it is.  

# Terraform Data Types

## Primitive Types
- **string** → Sequence of characters  "hello"
- **number** → Integer or floating-point number  
- **bool** → Boolean value (`true` or `false`)  

## Collection Types
- **list** → Ordered sequence of values of the same type  ["us-west-1a","us-wast-1c"]  
- **map** → Key-value pairs  like{name ="mabel",age =52}  
- **set** → Unordered collection of unique values  

## Structural Types
- **object** → Group of named attributes with specific types  
- **tuple** → Ordered sequence of values with potentially different types
# Introducing Count Arguments  

The Count argument accepts a whole number and create that many instances of the resource.  
We can create 10 instance at once.  

EG: resource "aws_instacen" "second_ec2"{
ami = "ami-number"
instance_type = "t2.miro"
count =10

```
resource "aws_instance" "example" {
  ami           = "ami-0360c520857e3138f" # Amazon Linux 2 AMI
  instance_type = "t2.micro"
  subnet_id = "subnet-059d18259e1dfef68"
  count = 3

  tags = {
    Name = "payment-system-${count.index + 1}"
  }
}
```

NOw we have to create 3 users.  

resource "aws_iam_user" "this" {  
name = "payments-user-${count.index}"  
count = 3  
}  

variable "users" {  
type =list 
default = ["alice", "kamran", "zunair"]  
}  

resource "aws_iam_user" "that" {
name = var.users[count.index]  
count = 3  
}  

# Conditional Expression.

### Base Code of conditional-expression.tf

```sh
variable "environment" {
  default = "development"
}

resource "aws_instance" "myec2" {
    ami = "ami-00c39f71452c08778"
    instance_type = "t2.micro
}
```

### Final Code Used In Examples:

```sh
variable "environment" {
  default = "production"
}

resource "aws_instance" "myec2" {
    ami = "ami-00c39f71452c08778"
    instance_type = var.environment == "development" ? "t2.micro" :"m5.large" 
}
```
#### Using the NOT EQUALS to Operator !=
```sh
variable "environment" {
  default = "production"
}

resource "aws_instance" "myec2" {
    ami = "ami-00c39f71452c08778"
    instance_type = var.environment != "development" ? "t2.micro" :"m5.large" 
}
```

#### Empty Value Based Example

```sh
variable "environment" {
  default = "production"
}

resource "aws_instance" "myec2" {
    ami = "ami-00c39f71452c08778"
    instance_type = var.environment != "development" ? "t2.micro" :"m5.large" 
}
```


### Example with Multipl Variables and Conditional Expressions

```sh
variable "environment" {
  default = "production"
}

variable "region" {
  default = "ap-south-1"
}

resource "aws_instance" "myec2" {
    ami = "ami-00c39f71452c08778"
    instance_type = var.environment == "production" && var.region == "us-east-1" ? "m5.large" : "t2.micro"
}
```


