#FOR EXAMPLE WE NEED TO MIGRATE OUR INFRASTUCTURE AWS TO TERRAFORM MANAGED SERVICE.
example:if you have infrastucture already in aws platform, lets assume you have created aws ec2 using awscli,cloud formation and now your organization moving their infrastucture to terraform managed service.

So, We need to migrate the infrastructure. 

#STEP1:

**create main.tf 
In the main.tf ,write below script (change instance-id and insatance-name if required.
```
provider "aws" {
    region = "us-east-1"
  
}
import {
    id = "i-08f1ec9539c68f236" # instance id  

    to = aws_instance.example1 #name of the instance
}
```

#STEP2:
We need to Initilize the main.tf fiel  using below command

```
terraform init
```
Generate the file using below command
```
terraform plan -generate-config-out=generate_resource.tf
```
The above command will generate the generate_resource.tf file(you can name it as what you like)
Then open generate_resource.tf file and copy resorces

#step3:
We need to add above copied resources in the main.tf file

```
provider "aws" {
    region = "us-east-1"
  
}
/*import {
    id = "i-08f1ec9539c68f236"  

    to = aws_instance.example1
}*/

resource "aws_instance" "example1" {
  ami                                  = "ami-0e86e20dae9224db8"
  associate_public_ip_address          = true
  availability_zone                    = "us-east-1d"
  disable_api_stop                     = false
  disable_api_termination              = false
  ebs_optimized                        = false
  get_password_data                    = false
  hibernation                          = false
  host_id                              = null
  host_resource_group_arn              = null
  iam_instance_profile                 = null
  instance_initiated_shutdown_behavior = "stop"
  instance_type                        = "t2.micro"
  key_name                             = "test"
  monitoring                           = false
  placement_group                      = null
  placement_partition_number           = 0
  private_ip                           = "172.31.95.143"
  secondary_private_ips                = []
  security_groups                      = ["launch-wizard-22"]
  source_dest_check                    = true
  subnet_id                            = "subnet-00d40438033250794"
  tags = {
    Name = "terraform-import-demo"
  }
  tags_all = {
    Name = "terraform-import-demo"
  }
  tenancy                     = "default"
  user_data                   = null
  user_data_base64            = null
  user_data_replace_on_change = null
  volume_tags                 = null
  vpc_security_group_ids      = ["sg-0ddefd4ee669563cb"]
  capacity_reservation_specification {
    capacity_reservation_preference = "open"
  }
  cpu_options {
    amd_sev_snp      = null
    core_count       = 1
    threads_per_core = 1
  }
  credit_specification {
    cpu_credits = "standard"
  }
  enclave_options {
    enabled = false
  }
  maintenance_options {
    auto_recovery = "default"
  }
  metadata_options {
    http_endpoint               = "enabled"
    http_protocol_ipv6          = "disabled"
    http_put_response_hop_limit = 2
    http_tokens                 = "required"
    instance_metadata_tags      = "disabled"
  }
  private_dns_name_options {
    enable_resource_name_dns_a_record    = true
    enable_resource_name_dns_aaaa_record = false
    hostname_type                        = "ip-name"
  }
  root_block_device {
    delete_on_termination = true
    encrypted             = false
    iops                  = 3000
    kms_key_id            = null
    tags                  = {}
    tags_all              = {}
    throughput            = 125
    volume_size           = 8
    volume_type           = "gp3"
  }
}
```
Above data in my case ,you may have another data for sure
** detele the generate_resoure.tf file**


**The final step is : Run below command **
```
terraform import aws_instance.example1 i-08f1ec9539c68f236  #example1(instance-name) and i-08f1ec9539c68f236(instance-id)
```
**Then we will see terraform.tfstate file,In the terraform.tfstate file we have all the data about that particular resource**




