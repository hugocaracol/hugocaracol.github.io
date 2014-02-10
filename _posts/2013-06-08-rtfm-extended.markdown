---
layout: post
title: "RTFM extended"
date: 2013-06-08 12:24
comments: true
categories: [code]
---
As you know RTFM stands for Read the Fucking Manual. Today, I would like to introduce you to RTWFM which stands for **Read the Whole Fucking Manual**.

A lot of times when I thing something *should* work but doesn't, I use google to find the answer. But google as a *middle-man* tells me to talk to
stackoverflow folks. I go to them. They have answers, lots of them. Unfortunately (or not) sometimes none of the answers available help me solve the problem.
That is when I follow RTFM command.

<!-- more -->

I'm working on a Django project and got stuck when tried to get url reverse to work on templates. 
All searches on google pointed to [this](https://docs.djangoproject.com/en/dev/topics/http/urls/) page. I ***read*** it! At least I thought so.
After 2 evenings spent googling and changing code I decided to look into the source of django. I got illuminated. It looks like when I use url namespaces
I have to put a colon between the app and the view like **app:index**.

I got so frustrated and angry that I went to the page where I had spent so many time *reading* and searched for a colon (:). That was the time where I
as put on my place again, a very humble little place. I couldn't believed I had missed [this section](https://docs.djangoproject.com/en/dev/topics/http/urls/#url-namespaces).

This is a warning to **Read the Whole Fucking Manual** because sometimes you miss important things that can easily spare you time and frustration.

