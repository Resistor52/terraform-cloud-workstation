# Step 1 - Prepare the Environment

1. Use your web browser and log into https://aws.amazon.com
2. Navigate to the AWS CloudShell
3. Close any pop-up windows
4. Verify that `git` is already installed by typing:

```
which git
```

Note the path to the executable. This shows it is installed.

5. Check to see if terraform is installed by typing:

```
which terraform
```

Results show that it is not installed, so let's install it.

6. Open a new browser tab and navigate to
https://learn.hashicorp.com/tutorials/terraform/install-cli. Scroll down to
the second **Install terraform** heading. Select the **Linux** tab and then the
**Amazon Linux** Tab. This will show the following three commands:

```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install terraform

```

7. Paste these three commands into the CloudShell terminal to install Terraform.

8. Finally, verify that terraform is installed by typing:

```
which terraform
```

----------------------------------------------------------------------------------
**IMPORTANT:** Due to a quirk with AWS CloudShell (Unlike with the cloud shells
of Azure and GCP) you may need to reinstall Terraform if CloudShell times out.
----------------------------------------------------------------------------------

**[Watch the Video](https://youtu.be/INC9MbsmYAM)**
