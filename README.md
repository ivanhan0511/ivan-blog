# DEPLOY BLOG WITH GITHUB PAGES AND HUGO
## Sources for ivanhan0511.github.io


[TOC]

## CONFIGURATIONS ON GITHUB

Create repository
Create a respository which is named like ivanhan0511.github.io




## DEPLOYMENT WITH HUGO

```shell
git clone git@github.com:ivanhan0511/ivan-blog.git
cd ivan-blog
hugo new site . --force
git submodule add git@github.com:kakawait/hugo-tranquilpeak-theme.git themes/hugo-tranquilpeak-theme
cp themes/tranquilpeak/exampleSite/config.toml .
hugo new post/testpage.md
git submodule add -b master git@ivanhan0511.github.io.git public
vi deploy.sh
```

```shell
#!/bin/sh

# If a command fails then the deploy stops
set -e

printf "\033[0;32mDeploying updates to GitHub...\033[0m\n"

# Create commit message
msg="rebuilding site $(date)"
if [ -n "$*" ]; then
	msg="$*"
fi

# Build the project.
echo ""
echo ""
echo "Committing changes to $(pwd)"
hugo -D

# Go To Public folder
cd public

# Add 'public' (Github Pages repo) changes to git and commit/push.
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin main

# Add this repos changes to git and commit/push. First 'cd' out of public
cd ..
echo ""
echo ""
echo "Committing changes to $(pwd)"
git add .
git commit -m "$msg"
git push origin master
```




## CONFIGURATIONS OF THEME




## QUOTES

- `https://youngkin.github.io/post/createafreeblogsite/`
- `https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/user.md`
