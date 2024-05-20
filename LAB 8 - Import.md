## Terraform Import
Before performing the Terraform import lab, you'll need to create an EC2 instance manually in the AWS console to obtain the necessary information like the AMI ID, instance type, and any tags you want to associate with the instance.
Once the instance is running, you can run the following step to import the infrastructure.
```
mkdir import_lab && cd import_lab
```
```
vi import.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  region = "us-east-1"
}   

#Execute the below command
#terraform import aws_instance.test_instance <***Resource-ID***>

resource "aws_instance" "test_instance" {
  ami = "<***AMI_ID OF THE EC2 Instance to be IMPORTED***>"
  instance_type = "<***INSTANCE TYPE***>"
  tags = {
    name = "<TAGS if ANY / New Tags can be added as well>"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform init
```
```
terraform plan
```
```
terraform import aws_instance.test_instance <Resource ID>
```
Check the resource has been imported in the state file.
Now if you apply the configuration file no changes will be made to the infra
```
terraform apply
```
The pre-existing resource now can be destroyed from terraform.
```
terraform destroy
```
```
cd ..
```
```
rm -rf import_lab
```
