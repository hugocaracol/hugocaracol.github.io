---
layout: post
title: "Migration to github pages"
date: 2013-01-04 23:07
comments: true
categories: github
---
{% highlight bash %}
$ ruby -rubygems -e 'require "jekyll/migrators/wordpressdotcom"; Jekyll::WordpressDotCom.process("wordpress.xml")'
{% endhighlight %}
