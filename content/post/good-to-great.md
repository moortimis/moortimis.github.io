+++
date = "2017-05-18T05:54:31+12:00"
title = "How to deploy your Hugo site to GitHub using Travis-CI""
draft = true
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
$ git checkout -b code
$ echo '/public/' >> .gitignore
$ git add .
$ git commit -m "Initial my blog"
$ git remote add origin haruair.github.io
```
