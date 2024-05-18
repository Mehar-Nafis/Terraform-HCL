## Workspace

### Create the Configuration File
```
cd ~
```
```
mkdir workspace-lab && cd workspace-lab
```
Now Create File configuration file instance.tf.
```
vi instance.tf
```
```
provider "aws" {
  region = "us-west-2"  # Change to your preferred region
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"  # Replace with a valid AMI ID for your region
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```
Initialize Terraform in your working directory.
```
terraform init
```
Create a new workspace (e.g., dev) and switch to it.
```
terraform workspace new dev
```
Apply the Terraform configuration to create resources in the dev workspace.
```
terraform apply
```
Create another workspace (e.g., prod) and switch to it.
```
terraform workspace new prod
```
Apply the Terraform configuration to create resources in the prod workspace.
```
terraform apply
```
To Switch back to the dev workspace to manage resources in that environment.
```
terraform workspace select dev
```
To display the name of the currently selected workspace.
```
terraform workspace show
```
To list all available workspaces.
```
terraform workspace list
```
To Delete the dev workspace (make sure you are not currently in the dev workspace and you have execute the `terraform destroy` command to delete the resources created in that workspace).
```
terraform workspace select default  # Switch to another workspace first
```
```
terraform workspace delete dev
```
