# DEPLOY BLOG WITH GITHUB PAGES AND HUGO
## Sources for ivanhan0511.github.io


[TOC]

## CONFIGURATIONS ON GITHUB

Create a normal repository, like ivan-blog as example

Create a respository, must with `<username>.github.io`




## DEPLOYMENT

```shell
git clone git@github.com:ivanhan0511/ivan-blog.git

# Create Hugo local site
cd ivan-blog
hugo new site . --force

# Visit https://themes.gohugo.io/ and choose a theme
# And add it as a submodule into the `theme` folder
git submodule add git@github.com:kakawait/hugo-tranquilpeak-theme.git themes/hugo-tranquilpeak-theme

# Copy and use `config.toml` from the theme
cp themes/tranquilpeak/exampleSite/config.toml .

# Create your new post
hugo new post/testpage.md

# Locoal testing
hugo server -D
```
```shell
git submodule add -b master git@github.com:ivanhan0511/ivanhan0511.github.io.git public
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




## USAGE OF THE THEME

### Configuration

```shell
vi config.toml
```


### Create content with Hugo
```shell
hugo new post/hello-world.md
```




## QUOTES

- `https://youngkin.github.io/post/createafreeblogsite/`
- `https://github.com/kakawait/hugo-tranquilpeak-theme/blob/master/docs/user.md`
