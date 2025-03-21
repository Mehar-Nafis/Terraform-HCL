## Commonly used State Commands

### terrafrom state List
This command lists all resources that are being tracked in the Terraform state. Print all the resource  addresses of all the resources and no other details
```
terraform state list
```

### terraform state show
Displays detailed information about a specific resource in the Terraform state, including its attributes and metadata.
```
terraform state show aws_instance.example
```

### terraform state mv
Moves an item in the Terraform state from one address to another. This is useful when you want to rename a resource or move items from one state file to another.<\br>
To rename a file
```
terraform state mv aws_instance.example aws_instance.new_example1
```
The above command will update the state file. Once the command is executed the name should be manually updated in the configuration file. Now if you execute a terraform apply, no changes should be applied for this resource

### terraform state pull
Manually pulls the state and outputs it as JSON. This is useful for debugging or inspecting the current state of your infrastructure.
```
terraform state pull > terraform_state.json
```

### terraform state rm
Removes one or more items from the Terraform state. This is typically used when you want to delete a resource from your infrastructure and stop managing it with Terraform.
```
terraform state rm aws_instance.example
```
The above command will update the state file. Once the command is executed the resource should be manually removed from the configuration file. Now if you execute a terraform apply, no changes should be applied for this resource


Use the `terraform destroy` command to clean the infrastructure used in this lab, 
```
terraform destroy
```
Once done, Remove the directory and Zip file using "`rm -rf`"
```
cd ~
rm -rf S3-Lab
```
**Note:** Also Ensure to delete the `S3 Bucket` (To delete, first empty the Bucket and then Delete it.)
