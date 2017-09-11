# [moortimis.github.io](https://moortimis.github.io)

A static blog repository based [Hugo](https://gohugo.io). 

[![Build Status](https://travis-ci.org/moortimis/moortimis.github.io.svg?branch=master)](https://travis-ci.org/moortimis/moortimis.github.io)

## A Few Tips

Up and running in a few types.

```
$ brew update && brew install hugo
$ git clone https://github.com/moortimis/moortimis.github.io.git --recursive
$ cd moortimis.github.io
$ git checkout -b source
$ hugo server -w
```

Writing a new post is simple. It depends on the theme, fyi.

```
$ hugo new post/2017/hello-world.md
$ hugo new fixed/my-life.md
```

The theme is [cocoa-hugo](https://github.com/nishanths/cocoa-hugo-theme)
with my custom css.

