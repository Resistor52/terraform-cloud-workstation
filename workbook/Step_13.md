# Step 13 - Customize the Ubuntu Instance

Now that we have an EC2 Instance that can be deployed via Terraform, lets customize
it. We will do so by making modifications to the `workstation1.sh` script that is
passed to the Ubuntu instance as it boots. But first, let explore the 'user data'
capability.

## Explore User Data (OPTIONAL)
According to the [cloud-init Documentation](https://cloudinit.readthedocs.io/en/latest/),
_Cloud-init is the industry standard multi-distribution method for cross-platform cloud
instance initialization. It is supported across all major public cloud providers,
provisioning systems for private cloud infrastructure, and bare-metal installations._
User data is passed to the instance at the time of launch via the AWS API. In our case
Terraform calls the AWS API. The
[Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)
details how this in implemented in AWS.

Now that we have launched an Ubuntu instance, let's poke around and see what we can learn.

1. Make sure you are in the project directory of CloudShell (Hint: `cd ~/tech` TAB)
2. Type `clear` to clear the terminal.
3. Type `terraform output` to see the output displayed the last time Terraform ran.

**INFO:** Learn more about the Terraform output command
[here](https://www.terraform.io/docs/cli/commands/output.html)

4. Open a new CloudShell tab and SSH into the instance as performed in the last lab.
Use the displayed ssh_connection_string.
5. At the SSH prompt, run `sudo cat /var/lib/cloud/instance/user-data.txt`. Look familiar?
This is where the user-data gets stored when it is passed to the system.
6. There is also a trick to fetch the user data script from the
[EC2 Metadataservice](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)
Run this rather long command from within the SSH session:

```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/user-data
```

7. Exit the SSH session and close the CloudShell tab.

## Enhance workstation1.sh

1. Let's improve the `workstation.sh` script. Open it in Vim, but do not enter insert
mode. Curser to the last line of the file and then press "dd" to delete the entire line.
2. Now enter insert mode (press "i") and cursor to the bottom of the file. Copy and
paste the following code and save it:

```
/bin/echo "Script Start Time: "$(/usr/bin/date) > /tmp/first-boot.log
/bin/echo "*****UPDATE*****" >> /tmp/first-boot.log
/usr/bin/apt update -y >> /tmp/first-boot.log 2>&1
/bin/echo "*****UPGRADE*****" >> /tmp/first-boot.log
DEBIAN_FRONTEND=noninteractive /usr/bin/apt upgrade -y >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL MATE 1 *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y --no-install-recommends ubuntu-mate-core >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL MATE 2 *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y --no-install-recommends ubuntu-mate-desktop >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL MATE 3 *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y mate-core >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL MATE 4 *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y mate-desktop-environment >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL MATE 5 *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y mate-notification-daemon >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL xrdp *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y xrdp >> /tmp/first-boot.log 2>&1
/usr/bin/systemctl enable xrdp >> /tmp/first-boot.log 2>&1
/bin/echo "***** Fix Broken Packages *****" >> /tmp/first-boot.log
/usr/bin/apt --fix-broken install -y >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL Firefox *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y firefox >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL chrome *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y gdebi-core >> /tmp/first-boot.log 2>&1
/usr/bin/wget -q https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb >> /tmp/first-boot.log 2>&1
/usr/bin/gdebi --n --q google-chrome-stable_current_amd64.deb >> /tmp/first-boot.log 2>&1
/bin/echo "***** INSTALL Python *****" >> /tmp/first-boot.log
/usr/bin/apt-get install -y python >> /tmp/first-boot.log 2>&1
/usr/bin/apt-get install -y python3-pip >> /tmp/first-boot.log 2>&1
/bin/echo "***** Create New User *****" >> /tmp/first-boot.log
/usr/sbin/useradd -m student >> /tmp/first-boot.log 2>&1
/usr/sbin/usermod -aG sudo student
/usr/bin/mkdir /home/student/Desktop >> /tmp/first-boot.log 2>&1
/usr/bin/chown student:student /home/student/Desktop >> /tmp/first-boot.log 2>&1
/usr/bin/ls -als /home/student/Desktop >> /tmp/first-boot.log 2>&1
/bin/echo 'student:ChangeMe' | /usr/sbin/chpasswd
/bin/echo "***** Create Firefox Launcher *****" >> /tmp/first-boot.log
/usr/bin/cat << EOF > /home/student/Desktop/Firefox.desktop
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon=firefox
Icon[C]=firefox
Name[C]=Firefox
Exec=firefox
Name=Firefox
EOF
/usr/bin/cat /home/student/Desktop/Firefox.desktop >> /tmp/first-boot.log
/usr/bin/chown student:student /home/student/Desktop/Firefox.desktop >> /tmp/first-boot.log 2>&1
/usr/bin/chmod 775 /home/student/Desktop/Firefox.desktop >> /tmp/first-boot.log 2>&1
/bin/echo "***** Create Firefox Launcher *****" >> /tmp/first-boot.log
/usr/bin/cat << EOF > /home/student/Desktop/Chrome.desktop
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon=google-chrome
Icon[C]=google-chrome
Name[C]=Chrome
Exec=google-chrome
Name=Chrome
EOF
/usr/bin/cat /home/student/Desktop/Chrome.desktop >> /tmp/first-boot.log
/usr/bin/chown student:student /home/student/Desktop/Chrome.desktop >> /tmp/first-boot.log 2>&1
/usr/bin/chmod 775 /home/student/Desktop/Chrome.desktop >> /tmp/first-boot.log 2>&1
/bin/echo "***** Create Terminal Launcher *****" >> /tmp/first-boot.log
/usr/bin/cat << EOF > /home/student/Desktop/Terminal.desktop
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=true
Icon=mate-panel-launcher
Icon[C]=mate-panel-launcher
Name[C]=Terminal
Exec=bash
Name=Terminal
EOF
/usr/bin/cat /home/student/Desktop/Terminal.desktop >> /tmp/first-boot.log
/usr/bin/chown student:student /home/student/Desktop/Terminal.desktop >> /tmp/first-boot.log 2>&1
/usr/bin/chmod 775 /home/student/Desktop/Terminal.desktop >> /tmp/first-boot.log 2>&1
/bin/echo "*****DONE*****" >> /tmp/first-boot.log
/bin/echo "Script Stop Time: "$(/usr/bin/date) >> /tmp/first-boot.log
/usr/bin/wall "NOTICE: First Boot Setup Has Completed"


```

**NOTE:** Each line redirects the output into a custom log file (`/tmp/first-boot.log`)
for troubleshooting purposes, but this is not strictly necessary, just a nice to have.
There are more elegant ways to achieve this, but this is quick and dirty.

**TROUBLESHOOTING TIP:** Make sure the first line of `workstation1.sh` still has
`#!/bin/bash`

3. Run `terraform validate` and then `terraform apply` to provision the change to AWS.
4. Once Terraform has finished executing, note the IP Address. Is it the same as the
one you just SSHed into? Nope, because Terraform had to destroy the old instance and
launch a new one with the new user data.
5. Open a new CloudShell tab and SSH into the instance as performed earlier. Use the
displayed ssh_connection_string.
6. In the SSH session, run `tail -f /tmp/first-boot.log` to monitor the progress of the
setup script. If the script has not finished before you SSH in, you should get a message that reads
"NOTICE: First Boot Setup Has Completed" when it does finish.
7. OPTIONAL: To review the log, use `less /tmp/first-boot.log` and press "q" to quit the
less pager.

## RDP Into Cloud Workstation
1. After confirming that the start-up sript has completed, we will RDP into our cloud
workstation. On a Windows system, click the Windows Key and then type "mstsc" to
launch the **M**icro**s**oft **t**erminal **s**ervices **c**lient
(RDP client). Macintosh users will need to have set up the MacOs client, per
[these instrictions](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac). Enter the RDP_address (without quotes) as displayed in the CloudShell.
Click "Connect," and try to connect.

We cannot. Why? Think about our security group rules. We are allowing port 3389 in from only the
IP address from where the Terraform script is run. Oops. Let's add an additional rule and variable
for the rule.

2. Open a new tab on your web browser and navigate to http://ipv4.icanhazip.com and copy
the IP address it displays. This is your public IP address.

3. Run `vim terraform.tfvars` enter insert mode, move to the bottom of the file and
add `alt_rdp_source_ip = "127.0.0.1/32"` But be sure to replace `127.0.0.1` with the address
obtained in the previous step.

4. Run `vim main.tf` and add the following line to the end of the variable blocks:
```
variable "alt_rdp_source_ip" {
  description = "An additional IP address from which to RDP"
}

```

5. While still in vim insert mode, add the following block just below the block for the
"sg-rdp" Security Group Rule:

```
resource "aws_security_group_rule" "sg-rdp2" {
  type              = "ingress"
  from_port         = 3389
  to_port           = 3389
  protocol          = "tcp"
  cidr_blocks       = [var.alt_rdp_source_ip]
  security_group_id = aws_security_group.work-sg.id
}

```
6. Save your changes and then run `terraform validate`
7. Deploy the changes using `terraform apply`
8. This time Terraform did not need to destroy and then relaunch another virtual
machine. Once the script has completed, you will now be able to RDP using the IP
address and credentials output by Terraform. Try it out.
10. Type `git add *` and then `git commit -m "Added content to workstation1.sh"`
11. Push it to Github: `git push origin main`
12. Check it out on Github by refreshing that browser tab.


**[Watch the Video](https://youtu.be/9JMb2ir5jC0)**
