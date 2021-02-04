# Step 5 - Create a terraform.tfvars File

1. Back in the CloudShell, make sure you are in the local directory for your
terraform project. Type `cd ~/tech` and then press TAB.
2. Run the following command to open the Vim text editor:
`vim terraform.tfvars`
3. Press the "i" key to enter insert mode.
4. Paste in the following lines:

```
aws_pem = "~/cloud_workstation.pem"
aws_region = "us-east-1"
aws_creds_file = "~/.aws/credentials"
aws_profile = "myterraform"
```

5. Next hit ESC and then type `:wq` to save the file and then quit Vim
6. Type `ls` to see that the `terraform.tfvars` file has been added.

**[Watch the Video](https://youtu.be/EtXHvlSiQb0)**
