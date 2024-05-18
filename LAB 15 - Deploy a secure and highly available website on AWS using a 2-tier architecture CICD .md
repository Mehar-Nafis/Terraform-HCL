## Deploy a secure and highly available website on AWS using a 2-tier architecture CICD

### Task 1: Create a new repo in Github

* Click on New
* In the repository name: Enter "Two-Tier-Architecture-with-Terraform"
* Click on Private
* Click on Create Repository
* Click on adding new files
* Upload the configuration files


### Task 2: Create a new workspace in Terraform Cloud

* Create a new organization
* Click on create a new workspace
* In tab 1, Select VCS (Version Control System) 
* In tab 2, select Github
* Once you click Github.com, a new window will popup. Sign into your GitHub account from there. Perform the verification and install Terraform on Git. 
* Now, we have the GitHub account connected with Terraform cloud. In tab 3 choose the repository where Terraform configuration is present, in this case, its `Two-Tier-Architecture-with-Terraform`.
* In tab 4, Provide the name for the workspace of  your own choice and click the Create Workspace button. Once the workspace is created, you will see a success message in a popup. 

### Task 3: Plan and Apply the changes 

* Now on the terraform cloud graphics, click Configure variables. In this demo, we will pass the credentials of AWS (Access key and secret key) to authenticate with users. 
* Click on add variable and provide the following details. Make sure you enable the sensitive check box. 

  `AWS_ACCESS_KEY = Your aws access key`
  
  `AWS_SECRET_ACCESS_KEY = Your aws secret key`

* Click on New Run and choose the option plan and apply
* Once Plan is successful, scroll down a bit, and it will wait for the confirmation/approval to apply the changes. Clickâ€¯Confirm & Apply 
* Provide a message in the textbox and click on Confirm Plan 
* You will see that the terraform apply is happening. 
* Verify that the resource has been created in your AWS Console 

### Task 5: Terminate the resources 

* On the settings tab, click on Destruction and Deletion 
* Now, click on `queue destroy plan`. 
* Provide the Workspace name and click on Queue Destroy plan 
* Now you will notice that queue is scheduled. The deletion queue has two stages. Plan and apply (to delete the infra) 
* Approve to start the delete operation by clicking Confirm & Apply
* Once completed, check the run tab to check the status. 
* Check the AWS console to verify that the resources are no longer active. 
