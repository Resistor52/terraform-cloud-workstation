# Step 9 - Terraform Init and .gitignore

1. In the CloudShell, confirm you are in the "tech-tuesday-cloud-workstation"
directory. Then type `ls -als` to see all files, including the hidden ones. You
should see something like:

```
4 drwxrwxr-x 4 cloudshell-user cloudshell-user 4096 Mar  2 21:09 .
4 drwxr-xr-x 7 cloudshell-user cloudshell-user 4096 Mar  2 21:09 ..
4 drwxrwxr-x 8 cloudshell-user cloudshell-user 4096 Mar  2 21:10 .git
4 -rw-rw-r-- 1 cloudshell-user cloudshell-user   12 Mar  2 21:09 .gitignore
4 -rw-rw-r-- 1 cloudshell-user cloudshell-user  901 Mar  2 21:05 main.tf
4 -rw-rw-r-- 1 cloudshell-user cloudshell-user   86 Mar  2 20:59 README.md
4 -rw-rw-r-- 1 cloudshell-user cloudshell-user  128 Mar  2 21:00 terraform.tfvars
```

2. Now run `terraform init` and observer the output. It should look like this:

```

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/http...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/http v2.0.0...
- Installed hashicorp/http v2.0.0 (signed by HashiCorp)
- Installing hashicorp/aws v3.26.0...
- Installed hashicorp/aws v3.26.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

```

**NOTE:** If the output does not look like this, check both `terraform.tfvars` and
`main.tf` for typos.

3. Next, type `ls -als` to see if any new files were added. You should notice two
new additions:

```
4 drwxr-xr-x 3 cloudshell-user cloudshell-user 4096 Mar  2 21:07 .terraform
4 -rw-r--r-- 1 cloudshell-user cloudshell-user 1897 Mar  2 21:07 .terraform.lock.hcl

```

**NOTE:** Recall that the period in front of a file name or directory makes it hidden

**INFO:** Read more about the `terraform init` command
[here](https://www.terraform.io/docs/cli/commands/init.html)

4. Now run `terraform plan` as suggested in the output of the previous terraform
command. It reads:

```
No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.

```

**INFO:** Read more about the `terraform plan` command
[here](https://www.terraform.io/docs/cli/commands/plan.html)

That really should not be a surprise, because we have not added any resources to
deploy to the script. We will do that the next step.

5. First, let's do a `git status`. This command reports three untracked items:
  * terraform.lock.hcl
  * .terraform/
  * main.tf

6. Add the two files:

```
git add main.tf
git add .terraform.lock.hcl

```

7. Rather than adding the `.terraform/` directory to our git repo, we will exclude
it using a `.gitignore` file. Type `vim .gitignore` to create the file and start to
edit it. Press "i" to enter insert mode, type `.terraform/` and then hit ESC to exit
insert mode. Then type ":wq" to exit vim.

8. Now, if you run `git status` you will see our two tracked files, but not the
`.terraform/` folder. However, now there is the `.gitignore` showing as an untracked
file. Add it using `git add .gitignore`.

**INFO:** Read more about `.gitignore` [here](https://git-scm.com/docs/gitignore).

9. Do one last `git status` to see that all looks well. Now we can commit the changes.
Run `git commit -m "partial of main.tf"`

10. Push the code to Github, as before using `git push origin main` providing your
credentials as prompted.

11. Lastly, check out the updates on Github by selecting the appropriate browser tab
and refreshing the page.


**[Watch the Video](https://youtu.be/CW9YNyfPJx8)**
