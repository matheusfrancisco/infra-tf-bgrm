# Terraform to create AWS Infra


This is a [Terraform](https://www.terraform.io/) project for managing AWS resources.

It can build the next infrastructure:

* A [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
* A public [Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the `VPC`
* An [IGW](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) to enable access to or from the Internet for `VPC`
* A [Route Table](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Route_Tables.html) to associate `IGW`, `VPC` and `Subnet`
* An [EC2 Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) in the public `Subnet` with the HTTP(s) & SSH access

1. [Install Terraform](https://learn.hashicorp.com/terraform/getting-started/install.html)
2. Create AWS account
3. If the file `~/.aws/credentials` does't exist, create it and add you Terraform profile to the file. For example:
   ```text
   [terraform]
   aws_access_key_id = Your access key
   aws_secret_access_key = Your secret access key 
   ```
4. Create S3 bucket to store Terraform state
5. Create config file `./src/main/backend/config.tf` that will contain information how to store state in a given bucket. See [example](./src/main/backend/example.config.tf).
6. Create SSH key pair to connect to EC2 instance: if is the first time  you've being build this repo
   ```bash
   cd ./src/main/provision/access

   # it creates "ec2-key" private key and "ec2-key.pub" public key
   ssh-keygen -f api-ec2-key
   ``` 

1. `cd ./src/main`
2. `terraform init -backend-config="./backend/config.tf"`
3. `terraform plan`
4. `terraform apply`

After building the infrastructure you can try to connect to you `EC2 instance` via SSH:

1. `cd ./src/main`
2. `ssh -i ./provision/access/api-ec2-key ubuntu@ec2-54-85-50-240.compute-1.amazonaws.com `

To check HTTP access you can install `apache2` on your EC2 instance:

1. `sudo apt install apache2` (on EC2 machine)
2. `sudo service apache2 start` (on EC2 machine) 
3. Check in browser: `http://[EC2 public IP]/`. You can see `Apache2 Default Page` (Something like [this](https://annex.exploratorium.edu/))

To destroy infrastructure:

1. `cd ./src/main`
2. `terraform destroy`


# TODO

Add new inbound route to tcp api
