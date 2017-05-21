+++
date = "2017-05-18T05:54:31+12:00"
title = "How to deploy your Hugo site to GitHub using Travis-CI"

+++

# Install Hugo
```
$ brew update && brew install hugo
```
# Basic Huo Usage
```
$ hugo new site haruair
$ hugo new post/hello-world.md
$ hugo
```

Once that's done, we want to generate out static content

```
$ hugo server --watch --buildDrafts
$ hugo server -w -D # same as the above
```
The strength of static blog is that it don’t need any program specific environment. There are several options, Amazon S3, Dropbox, etc. Even it’s possible to use some abandoned old geocity-ish service. Github provides Github Pages for this case and it’s free to use.

Create a Github account if you don’t have one. Then, Create a repository using <username>.github.io as a name.

Go to the your hugo directory, and make it as a git repository, then push it to Github.

```
$ git init
$ git checkout -b source
$ echo '/public/' >> .gitignore
$ git add .
$ git commit -m "Initial my blog"
$ git remote add origin haruair.github.io
```

I used source branch for the original source, the static file in public directory will goes into the master branch as an orphan branch.
```
$ hugo
$ export HUGO_TEMP_DIR=$(mktemp -d)
$ cp public/* $HUGO_TEMP_DIR
$ git checkout --orphan master
```

If you build the site manually, it’s just boring. Travis CI allows us to add a deploy script after build the static pages. I found the script from rcoedo.com, I just put my script below, just in case.

```
$ mkdir scripts && cd scripts
$ wget https://raw.githubusercontent.com/haruair/haruair.github.io/code/scripts/deploy.sh
$ git add deploy.sh && git commit -m "Add deploy script"
```

# Create a .travis.yml file

I do love pre-config file. The config is here. This config need to update as own details. The important part is before_install part. Delete this section from the yml file, then add it back with your ssh key. Yes, your ssh key!

Before add the key into the repository, it will encrypt using travis. Please be aware, do not add the original private key into the repository.

# Install travis

Travis CI provides a CIL tool for the work. Let’s install this tool, first. It’s using Ruby, therefore, you can install it via gem.

```
$ gem install travis --no-rdoc -no-ri
```
If you don’t have Ruby on your machine, please check the document from Travis CI.

## Create a new ssh key and encrypt it

It’s available to use an existing key, however, creating new one for Travis CI is good for the security. Make sure that ssh-keygen can be overriding a default key id_rsa, be careful before type the enter.

```
$ ssh-keygen
```
I created the ssh key named travis_key. The result of this commend is the pair of the key: travis_key and travis_key.pub. The first one is a private key, the other file is a public one.

Now, we can add travis_key into Travis using the commend below:

```
$ travis encrypt-file travis_key
```
If you are not logged in Travis before, you may need to run travis login first.

After then, travis add encrypt file and set the environment variable for the encrypt file. Make sure all files are included in git repository and push it to Github. Also, do not commit the “unencrypt key” into your repository.

# Register the new key in Github

In Github setting page, go to the ssh section and add travis_key.pub for the deploy. More informations here.

# Enable a repository on Travis CI

Go to the profile page of Travis CI, Find out the repository, then turn on the switch. It will do the build and will push back to Github master branch.

All done. Just add new post using Hugo, push to Github, Travis will handle all build problem.

The quickest and the easiest way is here. Clone my repository haruair.github.io, clean up all details and configurations, and use it. Please make sure that the credential is correct.

It’s a quite long description than I thought. Tools support the writing, however, the key of writing is based on all own ideas. Keep sharpening your idea using awesome writing tool Hugo.
