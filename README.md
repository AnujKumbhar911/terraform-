# terraform-

Infrastructure as a code

A)terraform providers: aws, azure gcp, k8s, alibaba cloud, oracle cloud


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
            
                   
