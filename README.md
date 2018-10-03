# Terraform

Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions.
Configuration files describe to Terraform the components needed to run a single application or your entire datacenter. Terraform generates an execution plan describing what it will do to reach the desired state, and then executes it to build the described infrastructure. As the configuration changes, Terraform is able to determine what changed and create incremental execution plans which can be applied.

### Installation
1. Install unzip
```sh
sudo apt-get install unzip
```
2. Download latest version of the terraform
```sh 
wget https://releases.hashicorp.com/terraform/0.11.1/terraform_0.11.1_linux_amd64.zip
```
3. Extract the downloaded file archive
```sh
unzip terraform_0.11.1_linux_amd64.zip
```
4. Move the executable into a directory searched for executables
```sh
sudo mv terraform /usr/local/bin/
```
5. Run it
```sh
terraform --version
```
# Modules
  - Target Group
  - Elastigroup
  - RDS
  - Route53
  - EC2
  - Elastic IP


### Target Group 
#### [aws_lb_target_group]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| name |The name of the target group. If omitted, Terraform will assign a random, unique name. | - | no |
| port |The port on which targets receive traffic, unless overridden when registering a specific target. | - | yes |
| vpc_id | The identifier of the VPC in which to create the target group. | - | yes |
| target_type | The type of target that you must specify when registering targets with this target group. | - | no |

#### Outputs
| NAME | DESCRIPTION |
| ------ | ------ | 
| arn  |The ARN of the Target Group (matches id) |
#### [aws_lb_listener]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| load_balancer_arn |The ARN of the load balancer. | - | yes |
| port |The port on which the load balancer is listening.| - | yes |
| default_action | An Action block. | - | yes |
| target_group_arn | The ARN of the Target Group to which to route traffic. | - | yes |
| type |  The type of routing action. The only valid value is forward. | forward | yes |

#### [aws_lb_listener_rule]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| listener_arn |The ARN of the listener to which to attach the rule. | - | yes |
| priority |The priority for the rule between 1 and 50000.| - | no |
| action | An Action block. | - | yes |
| target_group_arn | The ARN of the Target Group to which to route traffic. | - | yes |
| type |  The type of routing action. The only valid value is forward. | forward | yes |
| condition |A Condition block. | - | yes |
| field |The name of the field. Must be one of path-pattern for path based routing or host-header for host based routing. | - | yes |
| values | The path patterns to match. A maximum of 1 can be defined. | forward | yes |


### ElastiGroup[spotinst_elastigroup_aws]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| name |The group name.| - | yes |
| description |The group description.| - | yes |
| product | Operation system type. Valid values: "Linux/UNIX", "SUSE Linux", "Windows". | - | yes |
| max_size | The maximum number of instances the group should have at any time. | - | yes |
| min_size | The minimum number of instances the group should have at any time. | forward | yes |
| desired_capacity |The desired number of instances the group should have at any time. | - | no |
| instance_types_ondemand  |The type of instance determines your instance's CPU capacity, memory and storage. | - | yes |
| instance_types_spot | One or more instance types. | - | yes |
| ondemand_count | Number of on demand instances to launch in the group. | - |no |
| region | The AWS region your group will be created in. | - | no |
| target_group_arns | List of Target Group ARNs to register the instances to. | list | no |
| subnet_ids | List of Strings of subnet identifiers. | list | no |
| health_check_type | The service that will perform health checks for the instance. | - | no |
| health_check_grace_period | The amount of time, in seconds, after the instance has launched to starts and check its health. | - | no |
| security_groups | A list of associated security group IDS. | list | yes |
| image_id| The ID of the AMI used to launch the instance. |map | no |
| iam_instance_profile| The ARN of an IAM instance profile to associate with launched instances. |-| no |
| key_name| The key name that should be used for the instance. |- | no |
| user_data | The user data to provide when launching the instance. |- | no |
| fallback_to_ondemand| In a case of no Spot instances available, Elastigroup will launch on-demand instances instead.|- | yes |
| orientation| Select a prediction strategy. |balanced| yes |
| tags| A mapping of tags to assign to the resource. |- | no |


### RDS[aws_db_instance]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
|  identifier |The name of the RDS instance| - | no |
| instance_class |The instance type of the RDS instance.| - | yes |
|  multi_az |Specifies if the RDS instance is multi-AZ| - | no |
|snapshot_identifier | Specifies whether or not to create this database from a snapshot. This correlates to the snapshot ID you'd find in the RDS console.| - | no |
|  vpc_security_group_ids |List of VPC security groups to associate.| - | no |
| tags |A mapping of tags to assign to the resource.| - | no |
| final_snapshot_identifier |The name of your final DB snapshot when this DB instance is deleted. If omitted, no final snapshot will be made.| - | no |
| skip_final_snapshot |Determines whether a final DB snapshot is created before the DB instance is deleted| true | no |
### RDS[aws_security_group]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
|  name |The name of the security group.| - | no |
| ingress |Can be specified multiple times for each ingress rule.| - |no|
|  from_port |The start port| - | yes |
|  to_port |The end range port| - | yes |
|  protocol |The protocol. | - | yes |
|  cidr_blocks |List of CIDR blocks.| - | no |


### Route53RecordSet[aws_route53_record]
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| zone_id |The ID of the hosted zone to contain this record.| - | no |
| name |The name of the record.| - |yes|
| type |The record type. Valid values are A, AAAA, CAA, CNAME, MX, NAPTR, NS, PTR, SOA, SPF, SRV and TXT.| - | yes |
| ttl |The TTL of the record.| - | yes |
| records |A string list of records. | list | yes |


### EC2[aws_instance]
          
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| ami  | The ID of the AMI used to launch the instance.| - | yes |
| ebs_block_device |The EBS block device mappings of the Instance.| - | yes |
| instance_type | The type of the Instance. | - | yes |
| subnet_ids |The VPC subnet ID. | list | no |
| key_name| The key name of the Instance. |- | yes |
|  vpc_security_group_ids |List of VPC security groups to associate.| - | yes |
| iam_instance_profile| The name of the instance profile associated with the Instance. |-| no |
| user_data | The User Data supplied to the Instance. |- | no |
| tags| A mapping of tags to assign to the Instance. |- | no |




### EC2[aws_eip]
          
#### Inputs
| NAME | DESCRIPTION | DEFAULT | REQUIRED |
| ------ | ------ | - | - |
| instance  | EC2 instance ID.| - | no |
| vpc |Boolean if the EIP is in a VPC or not.| true | no |







### Backend with S3
```sh
terraform {
  backend "s3" {}
}

data "terraform_remote_state" "state" {
  backend = "s3"
  config {
    bucket     = "{bucket-name}"
    region     = "{region}"
    key        = "terraform.tfstate"
  }
}
```


### Building Infrastructure with Terraform
  ```sh
  terraform init -reconfigure -force-copy -input=false -backend-config "bucket=mybucket" -backend-config "region=ap-southeast-1" -backend-config "key=terraform.tfstate"
  ```
  ```sh
  terraform plan -out tfplan
  ```
  ```sh
  terraform apply "tfplan"
```
### Destroying Infrastructure with terraform
```sh
terraform destroy 
```


### References

* [Terraform](https://www.terraform.io/intro/index.html)
* [Terraform-aws-modules](https://www.terraform.io/docs/providers/aws/) 





