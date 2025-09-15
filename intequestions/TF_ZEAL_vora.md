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
# Function in Terraform
Terraform has wide variety of functions available to achieve different set of use-cases.  
1: Numberic      functions abs, ceil, floor   
2: String        concat, replace,split,
3: collection Functions >> elements , jeys , lenght,merge , sort
4: Filesystem Functions: >> file, filebase64, dirname  

Terraform doesn't support use-defined-functions & so only the funtions built in to the language are availabe for use.  

https://developer.hashicorp.com/terraform/language/functions 

# Locals VS Variables.

Variable value can be defined in wide variety of places like terraform.tfvars, ENV Variables, CLI and so on.  

Locals are more of a priavte resources. you have to redirect modify the code..  

Locals are used when you want to avoid repeting the same expression multiple times.  

Important point to note.

Locals values are often refereed to as just "locals"  
locals values are created by a locals block (prulas), but you reference them as attributes on an object named local (singular)  

## Data Sources.

Data sources allow Terraform to use /fetch information defined outside of Terraform .

### data-source-03.tf IT will fetched the data from AWS account 
```sh
provider "aws" {
    region = "us-east-1"
}

data "aws_instances" "example" {}
```
# Data source by using this we can get the latest images and create on any region only we have to change region 

```sh
 provider "aws" {
  region     = "ap-south-1"
  
}

data "aws_ami" "myimage" {
  most_recent      = true
  owners           = ["amazon"]

  filter {
    name   = "name"
    values = ["Ubuntu_22.04-x86_64-SQL_2022_Express-*"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.myimage.image_id
   instance_type = "t2.micro"
  
  }
```

## TF has detailed LOG TF_LOG ENV variables.

LOG LEVEL     1: TRACE          2: DEBUG       3: INFO       4: WARN     5:  ERROR

WIndow to set   $env:TF_LOG="INFO"   

$env:TF_LOG_PATH="terraform.txt"

# Terraform Troubleshooting Model 
There are four types of issues that we coud experience with TF.  

1: Configuration Language   2: State file issue   3: Core Application  4: Provider level issue   

We can check terrfaorm ISSUe Github for bug and issue related stuff  

## Reporting Terraform Bugs  Before bug make sure Error is not related to Conf file not state it is related to code TF we are using.

https://github.com/hashicorp/terraform/issues  

# FMT Command

The TF fmt command is used to rewirte TF configuration files to take care of the overall formatting. 

## Dynamic Block allows us to dynamically construct repetable nested blocks 
Supported inside resource, data, provider, and provisioner blocks.  

## Now we are creating security group using Dynamic block we can add port in dynamic fashion as per req.

### dynamic-block.tf

```sh

variable "sg_ports" {
  type        = list(number)
  description = "list of ingress ports"
  default     = [8200, 8201,8300, 9200, 9500]
}

resource "aws_security_group" "dynamicsg" {
  name        = "dynamic-sg"
  description = "Ingress for Vault"

  dynamic "ingress" {
    for_each = var.sg_ports
    iterator = port
    content {
      from_port   = port.value
      to_port     = port.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }

  dynamic "egress" {
    for_each = var.sg_ports
    content {
      from_port   = egress.value
      to_port     = egress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }
}

```
## Terraform validate.

Terraform validat checks a configuration is syntactically valid Eg: arguments, undeclared variables.  

## Points to Notes

**similar kind of functionality was achived using terraform taint command in older version T.F** 
**for version0.15.2 and later, Hashicorp recommedn using -replace option with terraform apply.**


## This snippet is from the Splat Expression Video.

### splat.tf

```sh

provider "aws" {
  region     = "us-west-2"
  access_key = "YOUR-ACCESS-KEY"
  secret_key = "YOUR-SECRET-KEY"
}
resource "aws_iam_user" "lb" {
  name = "iamuser.${count.index}"
  count = 3
  path = "/system/"
}

output "arns" {
  value = aws_iam_user.lb[*].arn
}
```

## Terraform Graph is used for large enterprises.  

## Saving Terraform plan to file.

```sh
resource "local_file" "kamel_config" {
  content  = "Hello, World!"
  filename = "terraform.txt"
  
}
```
terraform plan -out infra.plan   

**Many organizations require documented proof of planned changes before implementations.**
**These changes further reviwed and approved, Running apply from plan ensures consistent desired outcome**

## Sometime we have version dependency , we want only specific version .

if our code is compatible with specific versions of terraform , you can use the **required_version** block to add your constraints.  
```sh
terraform {
  required_version = "1.9.1"
```

## we can use terraform -target option to target specific resources, modules part of opertion( apply/destroy)

### Base Code Used

```sh
resource "aws_iam_user" "this" {
  name = "test-aws-user"
}

resource "aws_security_group" "allow_tls" {
  name        = "terraform-firewall"
}

resource "local_file" "foo" {
  content  = "foo!"
  filename = "${path.module}/foo.txt"
}
```

### Commands used

```sh
terraform plan -target local_file.foo
terraform apply -target local_file.foo
terraform destroy -target local_file.foo
```

 ## Meta argument 
 it is used for if someone changes in aws console & i dont want to change this modification how to achive it see below  
 Meta-arguments in Terraform are special arguments (count, for_each, depends_on, provider, lifecycle, etc.) that control how resources are created, managed, and destroyed, rather than describing the resource itself.  

 Inside the lifecycle block we can use:

create_before_destroy → avoid downtime during replacement.

prevent_destroy → protect critical resources from deletion.

ignore_changes → tell Terraform to skip managing specific attributes.  

 ## below eg we dont want to change tags which is manually updated use ignore with lifecycle .

resource "aws_instance" "myec2" {
ami = amiid 
instance_type = "t2.micro"

lifecycle {
ignore_changes = [tags]
}
}


## Depends on meta argument 

Depends on first resource should create before creating any resource like **depends on**

```sh
resource "aws_instance" "myec2" {
   ami = "ami-0e670eb768a5fc3d4"
   instance_type = "t2.micro
   depends_on = [aws_s3_bucket]
}

resource "aws_s3_bucket" "example" {
bucket = "deo-s3-bucket"
  
}
```

## For each

for_each in Terraform is used to create multiple resources from a map or set. It gives us each.key and each.value to uniquely manage resources by name, unlike count which only uses numeric indexes.”

```sh
variable "user_name" {
  type = set(string)
default = ["alice", " kamran", "john"]
}
resource "aws_aim_user" this" {
  for_each = var.user_names
name = each.value
}
```
## Local EXEC for locally saved 
It runs commands on the machine where Terraform is executed (your local laptop/CI server), not on the remote resource.

Documentation Referenced:

https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec

### Base Code:
```sh
resource "aws_instance" "myec2" {
   ami = "ami-04e5276ebb8451442"
   instance_type = "t2.micro"
}
```

## Final Code:

```sh
resource "aws_instance" "myec2" {
   ami = "ami-04e5276ebb8451442"
   instance_type = "t2.micro"

   provisioner "local-exec" {
    command = "echo ${self.private_ip} >> server_ip.txt"
   }
}
```

## Remote exec
It lets you run commands or scripts on a remote resource (like an EC2 instance) after it is created.

It connects using SSH (Linux) or WinRM (Windows).


### Documentation Referenced:

https://www.terraform.io/language/resources/provisioners/remote-exec

https://www.terraform.io/language/resources/provisioners/connection

https://www.terraform.io/language/functions/file

### Base Code:
```sh
resource "aws_instance" "myec2" {
   ami = "ami-04e5276ebb8451442"
   instance_type = "t2.micro"
}
```

### Final Code:

```sh
resource "aws_instance" "myec2" {
   ami = "ami-04e5276ebb8451442"
   instance_type = "t2.micro"
   key_name = "terraform-key"
   vpc_security_group_ids = ["sg-0edf854d7112cfbf4"]

 connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key  = file("./terraform-key.pem")
    host     = self.public_ip
  }

 provisioner "remote-exec" {
    inline = [
      "sudo yum -y install nginx",
      "sudo systemctl start nginx",
    ]
  }
}
```

## Terraform module

TF module allows us to centralize the resource configuration and it makes it easier for multiple projects to re-use the terraform code for projects.  










