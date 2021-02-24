# Step 12 - main.tf - Add Ubuntu Instance

1. As before, edit `main.tf` and add in the following lines to the bottom of the
file:

```
resource "aws_instance" "workstation1" {
  ami                    = data.aws_ami.ubuntu.id
  instance_type          = "t3a.xlarge"
  key_name               = var.ssh_key
  vpc_security_group_ids = [aws_security_group.work-sg.id]
  subnet_id              = aws_subnet.work-subnet.id
  user_data              = file("workstation1.sh")
  tags = {
    Name = "workstation1"
  }
}

output "ssh_connection_string" {
  value = "ssh -i ${var.aws_pem} ubuntu@${aws_instance.workstation1.public_ip}"
}

output "RDP_address" {
  value = aws_instance.workstation1.public_ip
}

output "RDP_UserName" {
  value = "student"
}

output "RDP_Password" {
  value = "ChangeMe"
}

```

**IMPORTANT:** Did you notice that this script will launch a t3.xlarge? This will cost
$0.1664 per Hour according to the
[EC2 On Demand Pricing Page](https://aws.amazon.com/ec2/pricing/on-demand/). Feel
Free to change it to another valid instance type.

**NOTE:** We added a new block type, an output block. This defines information to be
displayed to the terminal when the script runs. It can display:
* the value of strings, such as the RDP_UserName;
* object properties, such as RDP_address; and even
* strings concatenated with data, as in the ssh_connection_string

**INFO:** Read more about the terraform output block
[here](https://www.terraform.io/docs/language/values/outputs.html)  

2. Save the file and exit vim. Run `terraform validate` to ensure that the code
looks good. Does it?

No, we get the following results:

```
Error: Reference to undeclared input variable

  on main.tf line 130, in resource "aws_instance" "workstation1":
 130:   key_name               = var.ssh_key

An input variable with the name "ssh_key" has not been declared. This variable
can be declared with a variable "ssh_key" {} block.
```

It looks like we just added a variable `var.ssh_key` that was not defined when we
declared the others. Let's add it to the code.

3. Open `main.tf` with Vim and add the following line after the other variables:

```
variable "ssh_key" {
  description = "The AWS Key Pair to use for SSH"
}
```

4. Let's add it to the bottom of the `terraform.tfvars` file while we are at it.

```
ssh_key = "cloud_workstation"
```

5. Run `terraform validate` again. You should see another error:

```
Error: Invalid function argument

  on main.tf line 137, in resource "aws_instance" "workstation1":
 137:   user_data              = file("workstation1.sh")

Invalid value for "path" parameter: no file exists at workstation1.sh; this
function works only with files that are distributed as part of the
configuration source code, so if this file will be created by a resource in
this configuration you must instead obtain this result from an attribute of
that resource.
```

The terraform script is also expecting user data to pass to the EC2 instance in
the form of a script named `workstation1.sh` but this script does not exist yet.
Let's create it.

6. Run the following commands to make a placeholder script that we can
improve in subsequent steps:

```
echo '#!/bin/bash' > workstation1.sh
echo 'date' >> workstation1.sh

```

7. OPTIONAL: Examine the file you just created. Type `cat workstation1.sh`
8. Run `terraform validate` one more. Now you should see the success message.
9. Run `terraform apply` to provision the resources to AWS.

**NOTE:** Did you notice that terraform only needed to deploy one additional resource
because the other ones were already deployed in the previous step? Vey cool!

10. Visually verify the EC2 Instance via your AWS Web Console.
11. Let's log into it. Copy the contents of the ssh_connection_string value (excluding
the quotation marks). Then click on the "Actions" drop down field, in the upper right
corner of the CloudShell. Select "New tab." Paste the clipboard contents into the
terminal and hit ENTER. Select "yes" when prompted if you want to connect.
12. Type "exit" in the SSH session terminal and close that CloudShell tab.
**Imortant:** Leave the other CloudShell tab open!

**NOTE:** You will not be able to RDP yet. We add that capability in the next step.

13. Type `git add *` and then `git commit -m "Added EC2 Instance to main.tf"`
14. Push it to Github: `git push origin main`
15. Check it out on Github by refreshing that browser tab.


**[Watch the Video](https://youtu.be/vxrtcXOcPBE)**
