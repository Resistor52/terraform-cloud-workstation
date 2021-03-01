# Step 14 - Finish the Documentation for the Project

No project is complete without the documentation. In this step, we will create a README
to inform others how to use our Terraform script.

1. confirm you are in the "tech-tuesday-cloud-workstation" directory and then copy
a template for the README.md from the Internet:

```
wget https://raw.githubusercontent.com/Resistor52/terraform-cloud-workstation/main/README-template.md -O README.md
```

2. Take a look at the top of the new README.md file using `head -n20 README.md` and you
will see lines that contain the following:

```
git clone XXXXXXXXXX
```
and then

```
cd ZZZZZZZZZZ
```

3. Next, we will use some command line kung-fu to modify these lines to your git repo name:

```
REPO=$(grep url .git/config | cut -d'=' -f2)
sed -i "s|XXXXXXXXXX|$REPO|" README.md
DIR=$(pwd | cut -d'/' -f4)
sed -i "s|ZZZZZZZZZZ|$DIR|" README.md
```

4. Run the head command to see the resultant changes:

```
`head -n20 README.md`
```

5. Type `git add README*` and then `git commit -m "Updated README"`
6. Push it to Github: `git push origin main`
7. Check it out on Github by refreshing that browser tab.
