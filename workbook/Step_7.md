# Step 6 - Perform First git commit

1. Back in the CloudShell, run the following commands, replacing "you@example.com" with your email and
"Your Name" with your actual name (not your Github Username):

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

**REFERENCE:** https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup

2. Make sure you are in the local directory for your terraform project. Type
`cd ~/tech` and then press TAB.

3. In the terminal, type `ls` and you should see two files:

```
README.md  terraform.tfvars
```

4. Type `git status` to see if you have anything that needs to be committed. You
should see:

```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        terraform.tfvars

nothing added to commit but untracked files present (use "git add" to track)
```

NOTE: If you see the following, you are in the wrong directory:

```
fatal: not a git repository (or any parent up to mount point /)
Stopping at filesystem boundary (GIT_DISCOVERY_ACROSS_FILESYSTEM not set).
```

5. Type `git add terr` and then the TAB key and then ENTER.
6. Run `git status` again. You should see:

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   terraform.tfvars
```

7. Now, lets commit the change. Type `git commit -m "added terraform.tfvars"`.
Note that the portion of the command is your human-friendly commit message that
indicates what change is made by this commit. Your result should look something like:

```
[main 74ac2b7] added terraform.tfvars
 1 file changed, 5 insertions(+)
 create mode 100644 terraform.tfvars
```

8. Type `git status` again. You should see:

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

**NOTE:** Going forward, it is not necessary to type `git status` unless you need
to determine the status of your changes since the last commit.

9. Ok, time to push the change to Github. Type `git push origin main` and then provide
you your Github username and password. You should see something like:

```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 389 bytes | 389.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Resistor52/tech-tue-cloud-workstation-0015.git
   b2a5237..74ac2b7  main -> main
```

10. Lastly, switch to the browser tab that has your Github repository loaded and
hit refresh. You should see the terraform.tfvars file along with your commit message.

**[Watch the Video](https://youtu.be/wrTooPCBXsk)**
