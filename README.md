# Module 12.16 of DevOps Bootcamp by [TechWorld with Nana](https://www.techworld-with-nana.com/)
Modularize Project

## Technologies used:
- Terraform
- aws
- Docker
- Linux
- Git

## Project description:
- Divide Terraform resources into reusable modules

## Implementation steps:
1. Add file 'outputs.tf' and move all output configurations from 'main.tf' to this new file
2. Add file 'variables.tf' and move all variable definitions from 'main.tf' there
3. Add folder 'modules' and two subfolders 'subnet' and 'webserver'
4. In each subfolder add filed 'main.tf', 'outputs.tf', 'providers.tf' and 'variables.tf'
5. Move resources for subnet, internet gateway and route table from 'main.tf' to 'subnet\main.tf'
6. Replace all references of resources which are not in subnet module with variables (e.g. `aws_vpc.my-app-vpc.id` -> `var.vpc-id`)
7. For each variable used in 'subnet\main.tf' add an entry in 'subnet\variables.tf'
8. Add `module`-Block for module 'subnet' to 'main.tf' and reference all needed variables there:
    ```
    module "myapp-subnet" {
        source = "./modules/subnet"
        vpc_id = aws_vpc.myapp-vpc.id
        subnet_cidr_block = var.subnet_cidr_block
        avail_zone = var.avail_zone
        env_prefix = var.avail_zone
        default_route_table_id = aws_vpc.myapp-vpc.default_route_table_id
    }
    ```
9. Add a output-block to 'subnet\outputs.tf' that outputs the whole subnet object and adjust reference to subnet in 'asw_instance' resource in 'main.tf'
10. Execute `terraform init` to initialize the new module
