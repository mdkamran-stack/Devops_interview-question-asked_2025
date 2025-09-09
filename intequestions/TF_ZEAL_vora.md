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














## 

