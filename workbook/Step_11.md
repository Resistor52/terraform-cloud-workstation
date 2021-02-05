# Step 11 - main.tf - Add Security Group

1. As before, edit `main.tf` and add in the following lines to the bottom of the
file:

```
resource "aws_security_group" "work-sg" {
  name   = "work-sg"
  vpc_id = aws_vpc.work-vpc.id
}

resource "aws_security_group_rule" "sg-ssh" {
  type              = "ingress"
  from_port         = 22
  to_port           = 22
  protocol          = "tcp"
  cidr_blocks       = ["${chomp(data.http.myip.body)}/32"]
  security_group_id = aws_security_group.work-sg.id
}

resource "aws_security_group_rule" "sg-rdp" {
  type              = "ingress"
  from_port         = 3389
  to_port           = 3389
  protocol          = "tcp"
  cidr_blocks       = ["${chomp(data.http.myip.body)}/32"]
  security_group_id = aws_security_group.work-sg.id
}

resource "aws_security_group_rule" "allow_all" {
  type              = "egress"
  from_port         = 0
  to_port           = 0
  protocol          = "-1"
  cidr_blocks       = ["0.0.0.0/0"]
  security_group_id = aws_security_group.work-sg.id
}


```

Note how the Security Group is associated with the VPC and the Security Group Rules
are associated with the Security Group. Notice anything interesting about the cidr_block
attribute on the first two Security Group Rules?
[Chomp](https://www.terraform.io/docs/language/functions/chomp.html) is used to remove
the newline characters of the string returned by the body property of the "myip" object.
Recall defining that block at the beggining of our script?

2. Run `terraform validate` to ensure that the code looks good.
3. Run `terraform apply` to provision the resources to AWS. This time we will not
tear down the resources, so DO NOT run `terraform destroy`
4. Visually verify the resources via your AWS Web Console.
5. Type `git add *` and then `git commit -m "Added Security Group to main.tf"`
6. Push it to Github: `git push origin main`

**INFO:** Learn about other Terraform functions, like chomp,
[here](https://www.terraform.io/docs/language/functions/index.html)

**[Watch the Video](https://youtu.be/Kr_YAMbrnWU)**
