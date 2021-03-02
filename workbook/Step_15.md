# Step 15 - Teardown and Test

The last step in the process is to tear down our Cloud Workstation, destroy our CloudShell
environment and test everything.

1. Tear down the Cloud Workstation:

```
terraform destroy
```

2. Click on the **Actions** dropdown field in the upper right corner of the CloudShell
and select "Delete AWS CloudShell home directory" and then confirm the deletion.

3. When a new CloudShell environment is ready, install Terraform as per Step 1:

```
wget https://releases.hashicorp.com/terraform/0.14.7/terraform_0.14.7_linux_amd64.zip
unzip terraform*.zip
mkdir ~/bin
mv terraform ~/bin/
rm terraform*.zip
```

4. Navigate to the Github repository that you created during this workshop and follow
the instructions in the DEPLOYMENT section.

5. Make a copy of the PEM file on CloudShell from the backup you made in Step 5.

6. Open a new CloudShell tab and SSH into the cloud workstation using this new PEM file.

7. When the boot up script has completed, RDP into the system and verify the presence of
the expected functionality.

8. Destroy the cloud workstation when you are finished with it, using `terraform destroy`


**[Watch the Video](https://youtu.be/CFBc_rCU2z0)**
