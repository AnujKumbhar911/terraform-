# terraform-

Infrastructure as a code

A)terraform providers: aws, azure gcp, k8s, alibaba cloud, oracle cloud

common terraform commands:

Here are some of the most commonly used Terraform commands:

1. **terraform init**: Initializes a Terraform working directory by downloading provider plugins and setting up the backend. This command is typically run once in a new project or when the configuration dependencies change.

2. **terraform plan**: Creates an execution plan that describes the actions Terraform will take to reach the desired infrastructure state. It performs a refresh of the current state, compares it with the desired state, and generates a summary of the planned changes.

3. **terraform apply**: Applies the changes described in the execution plan created by `terraform plan`. It creates, modifies, or destroys resources to reach the desired infrastructure state. This command prompts for confirmation before making any changes.

4. **terraform destroy**: Destroys all resources created by the Terraform configuration, effectively tearing down the infrastructure. It prompts for confirmation before proceeding with the destruction.

5. **terraform validate**: Validates the syntax and configuration of the Terraform files without making any changes. It checks for errors or warnings in the configuration files and ensures that all required provider plugins are installed.

6. **terraform get**: Downloads and installs the provider plugins required by the Terraform configuration. It retrieves the latest versions of the providers specified in the configuration files.

7. **terraform refresh**: Refreshes the state of the deployed infrastructure, updating the Terraform state file with the latest resource information. It does not make any changes to the infrastructure but synchronizes the state with the actual resources.

8. **terraform output**: Displays the values of the outputs defined in the Terraform configuration. It shows the current values of any outputs that have been defined in the configuration.

9. **terraform state**: Manages the Terraform state, allowing you to view, modify, or remove state entries. This command is useful for advanced state management operations or troubleshooting.

10. **terraform workspace**: Manages Terraform workspaces, which are isolated environments for managing separate instances of infrastructure configurations. It allows you to create, select, and delete workspaces.

These are just a few of the most commonly used Terraform commands. Terraform provides many other commands and options for different use cases and advanced scenarios. You can explore more commands and their options by running `terraform help` or referring to the Terraform documentation.
==========================================================================================
important files in terraform:
1) main.tf or provider.tf
                        provider "aws" {
                          region = var.aws_region
                        }
                        
                        resource "aws_vpc" "my_vpc" {
                          cidr_block = var.vpc_cidr_block
                        
                          tags = {
                            Name = "MyVPC"
                          }
                        }
                        
                        resource "aws_instance" "my_instance" {
                          instance_type = var.instance_type
                          # ...
                        }






 
2) resource file: ec2.tf vpc.tf s3.tf: In Terraform, a resource file is a configuration file where you define the desired state of a specific infrastructure resource. It specifies the properties and settings that Terraform uses to create, modify, or delete the resource.

A resource file typically has a .tf extension and contains one or more resource blocks. Each resource block represents an instance of a particular resource type, such as an AWS EC2 instance, an Azure virtual machine, or a Google Cloud Storage bucket.

Here's an example of a resource file (example.tf) that defines an AWS EC2 instance:

                resource "aws_instance" "example" {
                  ami           = "ami-0c55b159cbfafe1f0"
                  instance_type = "t2.micro"
                  subnet_id     = "subnet-12345678"
                
                  tags = {
                    Name        = "ExampleInstance"
                    Environment = "Production"
                  }
                }

3) variable.tf --
            variable.tf file is used to define input variables that can be used throughout your Terraform configuration. Variables allow you to parameterize your configuration and provide flexibility and reusability. By separating variable definitions into a dedicated file, you can easily manage and update them independently of the rest of your configuration.
                       variable "aws_region" {
                      description = "The AWS region where resources will be created"
                      default     = "us-west-2"
                    }
                    
                    variable "vpc_cidr_block" {
                      description = "The CIDR block for the VPC"
                      default     = "10.0.0.0/16"
                    }
                    
                    variable "instance_type" {
                      description = "The EC2 instance type"
                      default     = "t2.micro"
                    }

4) output.tf: In Terraform, an output.tf file is used to define outputs that provide information about the infrastructure resources created or managed by your Terraform configuration. Outputs are a way to retrieve and display values from your infrastructure after it has been provisioned or updated.

The output.tf file typically contains one or more output blocks, each defining a specific output value. Here's an example of an output.tf file:

            output "vpc_id" {
              description = "The ID of the VPC"
              value       = aws_vpc.my_vpc.id
            }
            
            output "public_subnet_ids" {
              description = "The IDs of the public subnets"
              value       = aws_subnet.public.*.id
            }
    To retrieve and display the output values after running Terraform, you can use the terraform output command:
            terraform output vpc_id
            terraform output public_subnet_ids


5) backend.tf:
        backend.tf file is used to configure the backend, which is responsible for storing the Terraform state file and managing state operations. The backend determines where the state is stored and how Terraform interacts with it.

The backend.tf file typically contains a single backend block that configures the backend provider and its specific settings. Here's an example of a backend.tf file using the S3 backend for state storage on AWS:

                terraform {
                  backend "s3" {
                    bucket         = "your-s3-bucket-name"
                    key            = "path/to/terraform.tfstate"
                    region         = "your-aws-region"
                  }
                }
By configuring the backend, you define where Terraform stores and retrieves the state file. This allows for collaboration, state sharing, and consistent management of infrastructure over time.

It's worth noting that the backend configuration is typically specific to each Terraform workspace or project. You may have different backend.tf files for different environments or configurations, allowing you to store the state file in separate locations or use different backend providers.

6) terraform.tfvars:
In Terraform, a terraform.tfvars file is used to define input variable values for your Terraform configuration. This file allows you to set values for variables without modifying the actual Terraform code. It provides a convenient way to manage and override variable values separately from the configuration files.

The terraform.tfvars file is written in plain text and follows the syntax of variable assignments. Here's an example:
                    aws_region      = "us-west-2"
                    vpc_cidr_block  = "10.0.0.0/16"
                    instance_type   = "t2.micro"
                    
In this example, we define values for three variables: aws_region, vpc_cidr_block, and instance_type. The variable names in the file should match the variable names defined in the Terraform configuration files (e.g., variable "aws_region").

By using terraform.tfvars, you can easily provide input variable values when running Terraform commands, such as terraform apply or terraform plan. Terraform automatically loads variable values from the terraform.tfvars file if it exists in the current directory.

Alternatively, you can use the -var-file flag when running Terraform commands to specify a different .tfvars file:

        terraform apply -var-file="custom.tfvars"

        
This allows you to have multiple .tfvars files for different environments, configurations, or use cases.

It's important to note that the terraform.tfvars file is not committed to version control repositories, as it may contain sensitive information like access keys or credentials. It is typically added to the .gitignore file or excluded from version control in other ways.

By using a separate variable file like terraform.tfvars, you can keep your variable values separate from the configuration code, making it easier to manage and share values across teams or environments. It also allows for easy customization and reusability of your Terraform configurations.
==========================================================================================
never store credentials in main.tf 
use cmd >> aws configure list >> you get ~/.aws/config
cat ~/.aws/credentials you get all the credentials


==========================================================================================
create a s3 bucket and create a user with s3 permissions and give access keys in main.tf
*never store statefile in local machine, either store it on s3 bucket or azure storage
    snippet of backend.tf
                      terraform{
                      backend "s3" {
                        bucket = "terraform-bucket-demo" 
                        key = "path/to/terraform.tfstate"
                        region = "ap-south-1"
                          }
                        }
                      bucket is name of s3 bucket
                      key is path to store terraform.tfstate on the s3 bucket

    for first time configuring a backend.tf to migrate use following cmds:

        terraform init
        terraform validate
        terraform apply

    for reconfiguring backend.tf to migrate tht is if you have changed the backend.tf then use following cmds:
        terraform init -migrate-state
            promt is asked Enter a value: yes
            
                   
