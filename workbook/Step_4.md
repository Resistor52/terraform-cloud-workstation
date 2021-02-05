# Step 4 - Create an AWS CLI profile

## Important Notes
* Technically, the AWS CloudShell can run AWS CLI commands without running the
`aws configure` command, but we want this script to be able to run anywhere, not
just the CloudShell.
* Normally, the CloudShell uses the permissions of the user who has logged into
the Web Console.
* DO NOT run aws configure on an EC2 instance, lest your `~/.aws/credentials` file
get compromised!
* Since the CloudShell can only be accessed via the Web Console, running
`aws configure` is an acceptable risk for this workshop.

## Create a new IAM User
1. Navigate to the IAM Service in the AWS Web Console
2. Click on **Users** and then **Add user**
3. Create a new user named "tech-tuesday" and click only the "Programmatic acess"
checkbox.
4. Click the blue **Next: Permissions** button.
5. Click the **Attach existing policies directly** button and click the checkbox
to the right of the "AdministratorAccess policy." Click the blue **Next: Tags**
button.
6. Click the blue **Create user** button.
7. Copy the Access key ID to a temporary text file (Don't even save this file.)
8. Click the **Show** hyperlink to expose the Secret Access key. Copy this to the
temporary text file as well. Click the **Close** button.

## Configure the User Profile in the CloudShell
1. Navigate back to the AWS CloudShell. Click to plant your curser in the Cloudshell
terminal.
2. Enter the command `aws configure --profile myterraform` into the CloudShell and
press enter.
3. Next paste in the Access key ID from the temporary text file and then the Secret
Access Key.
4. Enter `us-east-1` for the Default region name and `json` for the Default output
format.
5. Test it by running the following command in the terminal: `aws sts get-caller-identity`

Did you get the results you expected? This command shows the user that you logged
into the web console with and not the user that we just created.

6. Let's run it again, with the profile specified:
`aws sts get-caller-identity --profile myterraform`

**[Watch the Video](https://youtu.be/UlP6B4B0bUY)**
