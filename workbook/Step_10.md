# Step 10 - main.tf - Add VPC resources

In this step, we will be adding resources to the `main.tf` to create a VPC. We will
test it by having Terraform deploy the resources and then we will verify the deployed
resources via the AWS Web Console.

1. In the CloudShell, make sure you are in the directory of the local git repo. Run
`cd ~/tech` and then press TAB, then ENTER.
2. Open the `main.tf` file by running `vim main.tf`
3. Enter insert mode by pressing the "i" key and cursor down to the bottom of the file.
4. Next copy and paste the following lines into the terminal:

```
resource "aws_vpc" "work-vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_support   = "true" #gives you an internal domain name
  enable_dns_hostnames = "true" #gives you an internal host name
  enable_classiclink   = "false"
  instance_tenancy     = "default"

  tags = {
    Name = "work-vpc"
  }
}

resource "aws_subnet" "work-subnet" {
  vpc_id                  = aws_vpc.work-vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = "true" #it makes this a public subnet
  availability_zone       = var.aws_availability_zone
  tags = {
    Name = "work-subnet"
  }
}

resource "aws_internet_gateway" "work-igw" {
  vpc_id = aws_vpc.work-vpc.id
  tags = {
    Name = "work-igw"
  }
}

resource "aws_route_table" "work-rtble" {
  vpc_id = aws_vpc.work-vpc.id

  route {
    //associated subnet can reach everywhere
    cidr_block = "0.0.0.0/0"
    //CRT uses this IGW to reach internet
    gateway_id = aws_internet_gateway.work-igw.id
  }

  tags = {
    Name = "work-rtble"
  }
}

resource "aws_route_table_association" "work-rta" {
  subnet_id      = aws_subnet.work-subnet.id
  route_table_id = aws_route_table.work-rtble.id
}

```

5. Close the Vim editing session by hitting ESC and typing `:wq`

What resources does this code snippet create? In order, it creates:
* A VPC named "work-vpc" with a network address of 10.0.0.0/16
* A Subnet named "work-subnet" that is associated with the new VPC
* An Internet Gateway that is associated with the new VPC named "work-igw"
* A Route Table named "work-rtble" with a route to the Internet Gateway
* A Route Table Association to link the new Route Table to the new Subnet

**INFO:** To locate the documentation on any of these resource, perform an Internet
search. For example, a search for "terraform resource aws_vpc" results in
https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/vpc.

6. Next, run `terraform validate` to make sure that our edits are good. The output
should read "Success! The configuration is valid."

**INFO:** Read more about the `terraform validate` command
[here](https://www.terraform.io/docs/cli/commands/validate.html)

**NOTE:** We don't need to initialize the directory again by running `terraform init`
again, but it won't harm anything because the command is
[idempotent](https://stackoverflow.com/questions/1077412/what-is-an-idempotent-operation).
When we ran init last time, it also validated the Terraform code whil it was at it.

7. Great, let's deploy these resources to AWS. Run `terraform apply` and then type
"yes" when prompted. If all goes well, you should see several messages with the last
line in green text that reads:
```
Apply complete! Resources: 5 added, 0 changed, 0 destroyed.
```

8. Now, navigate to the AWS VPC Service in the Web Console and look for the
following resources:
 > - [ ] A VPC named "work-vpc" with a network address of 10.0.0.0/16
 > - [ ] A Subnet named "work-subnet" that is associated with the new VPC
 > - [ ] An Internet Gateway that is associated with the new VPC named "work-igw"
 > - [ ] A Route Table named "work-rtble" with a route to the Internet Gateway
 > - [ ] A Route Table Association to link the new Route Table to the new Subnet

9. Now run `git status` and note that besides the `main.tf` file we expected to
show as modified, there are two new files:

* terraform.tfstate
* terraform.tfstate.backup

10. Add these two files to .gitignore HINT: vim .gitignore

**NOTE:** The tfstate files keep track of what Terraform has deployed to _your_
environment, and although they should be backed up in production, they do not
belong in a code repository, lest they confuse others who use your code.

11. Run `git status` again. Type `git add *` and then `git status`. Hmm, notice
that the "*" didn't pick up the `.gitignore` file because it is hidden. So add it
by typing `git add .gitignore`

12. Ok, time to commit it. Type `git commit -m "added VPC to main.tf"`

13. Push it to Github: `git push origin main`

14. Check it out on your browser tab that has your Github repo open. Notice the
change once you hit refresh.

15. Just for fun, lets deprovision these resources. Run `terraform destroy` and
then type "yes" when prompted. Look again in the Web Console. The resources are
no longer present.

**[Watch the Video](https://youtu.be/e_SpOR4az2M)**
