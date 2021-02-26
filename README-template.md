# ZZZZZZZZZZ
This terraform script deploys an Ubuntu Workstation with minimal additional software
installed. An example use case if a temporary sandbox system for surfing potentially
dangerous websites. _NOTE: Don't break the law, as AWS Terms of Service still apply
and this is not exactly covert._

## DEPLOYMENT
Git clone the repository and provision the cloud workstation by running the following
commands:

```
git clone XXXXXXXXXX
cd ZZZZZZZZZZ
```

Next modify the `terraform.tfvars` contents with the settings that are appropriate
for your AWS Account.

Now run these commands:

```
terraform init
terraform apply
```

Obviously, this assumes that you have both `git` and `terraform` installed.

## USAGE
The cloud workstation can be accessed via either SSH or RDP. The necessary information
will be an output when the script is complete.


## ADDITIONAL NOTES
The `workstation1.sh` is passed as user data to the instance and runs the first time
the system boots. The script generates a log entry for almost every command as it runs
and sends it to `/tmp/first-boot.log`

Since the script installs updates and some software, it may take a while. I like to watch
the first boot script by watching its log when connected via SSH. Just run:

```
tail -f /tmp/first-boot.log
```

Otherwise, if you are connected via SSH, you will get a system message when the boot
script has completed:

```
NOTICE: First Boot Setup Has Completed
```

After that point it is ready for an Remote Desktop (RDP) Connection. Use an RDP client,
such as Microsoft Terminal Services Client (MSTSC) to RDP to the IP address that is output
by the Terraform script.

**DON'T FORGET TO CHANGE THE RDP PASSWORD AFTER LOGON**
