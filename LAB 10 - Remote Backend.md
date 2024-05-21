## Remote State using Amazon Simple Storage Service 

### Task-1: Manually Create a S3 Bucket using AWS Console 

* Create a new S3 bucket in your Allocated Region by name: `youryame-terraform` (S3 allows lowercase only)
* While creating,
    - Select `"ACLs enabled"`
    - Uncheck `"block public access"` and select `"I acknowledge that the current settings might result in this bucket and the objects within becoming public"`
    - `"Enable versioning"`
    - Then, click on `Create bucket`
* Once done, To cross-check whether the Bucket is created or not, run the below command in CLI.
```
aws s3 ls 
```
### Task-2: Configure Remote State
```
cd ~
mkdir S3-Lab && cd S3-Lab
```
```
mkdir remote-state-lab && cd remote-state-lab
```
```
ls
```
Create a New Configuration File 
```
vi instance.tf
```
Add the below given lines, by pressing "INSERT"  
```
resource "aws_instance" "terraform-remoteState" {
  ami           = var.AMIS[var.AWS_REGION]
  instance_type = "t2.nano"
}

output "ip" {
  value = aws_instance.terraform-remoteState.public_ip
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
In `vars.tf` file ensure to replace your `region` and Include your `region's Ubuntu AMI ID` to the list.
```
variable "AWS_REGION" {
  default = "us-east-2"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-2 = "ami-0e83be366243f524a"
    us-west-2 = "ami-02e7fad8336aa2c57"
    eu-west-1 = "ami-029f9476"
  }
}
```
Once done, save the file and follow further steps.
```
vi provider.tf
```
```
provider "aws" {
  region = var.AWS_REGION
}
```
Now, Create a New Configuration File for storing "`terraform.tfstate`" file in the backend. (ie. `Amazon S3.`)

```
vi backend.tf
```
Add the given lines, by pressing "INSERT" and add your `bucket's region` and `Bucket Name`
```
terraform {
  backend "s3" {
    region = "<Replace your s3 bucket region>"
    bucket = "<Replace your s3 bucket name>"
    key    = "terraform/remotestate"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
cat backend.tf
```
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply
```
* Go to the S3 bucket and click on `terraform` > `remotestate` > In Properties Copy the `Object URL` and paste it in Browser.
  (By default it shows Access Denied)
* To view the content of the file, in S3 Bucket tab, Click on `permission` and click on `Edit` under `Access control list (ACL)` > `Everyone (public access)` > Check `"Read"` then check `I understand the effects of these changes on this object` and then Click on `Save changes`
* Refresh the Object URL Page in the browser (or again Copy-paste the `object URL` into the web browser).
* Now, You should be able to access the state file and View the resources.
  (It shows the attributes of a single resource in the Terraform state of `aws_instance.terraform-remoteState`.)

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


### Task-3: Enabling State Lock on Remote State
#### Create the DynamoDB Table
* Navigate to the DynamoDB service by searching for "DynamoDB" in the search bar.
* Click on the "Create table" button.
* Configure the Table :
* `Table name`: mehar-terraform-lock-table. `Make sure to change the Mehar to your name to avoid any conflicts`
* `Partition key`: Set the partition key to `LockID` and choose the data type as `String`.
* Leave the default settings for the other options (like secondary indexes, encryption, etc.).
* For `Capacity mode`, you can use `Provisioned` and set the Read capacity units and Write capacity units according to your needs. A common starting point is 5 for both, but you can adjust these based on your expected load.
* Click on the "Create" button to create the table.

#### Configure Terraform Backend

Update your Terraform configuration file ( backend.tf) with the backend configuration pointing to the new DynamoDB table.
Example Configuration for main.tf
```
terraform {
  backend "s3" {
    region = "<Replace your s3 bucket region>"
    bucket = "<Replace your s3 bucket name>"
    key    = "terraform/remotestate"
    dynamodb_table = "terraform-lock-table"
    encrypt        = true
  }
}
```
After configuring your backend, you need to initialize it again
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply
```
Open another console and also apply try to apply from there. You would get an error message, thus proving the State Locking has been enabled.

