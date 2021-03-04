# Step 6 - Create a terraform.tfvars File

A *.tfvars file holds parameters that the terraforms scripts will use. The purpose
of the file is to abstract the customization parameters from the code that will
typically not change.

1. Back in the CloudShell, make sure you are in the local directory for your
terraform project. Type `cd ~/tech` and then press TAB, then ENTER.
2. Run the following command to open the Vim text editor:
`vim terraform.tfvars`
3. Press the "i" key to enter insert mode.
4. Paste in the following lines:

```
aws_pem = "~/cloud_workstation.pem"
aws_region = "us-east-1"
availability_zone = "us-east-1a"
aws_creds_file = "~/.aws/credentials"
aws_profile = "myterraform"
```

5. Next hit ESC and then type `:wq` to save the file and then quit Vim

**NOTE:** Use ":q!" to quit without saving.

**REFERENCE:** [Vim Cheatsheet](https://vim.rtorr.com/)

6. Type `ls` to see that the `terraform.tfvars` file has been added.

**[Watch the Video](https://youtu.be/EtXHvlSiQb0)**
