---
layout: post
title: "Session \"hijacking\""
tags:
- session
categories: [PHP]
status: publish
published: true
---
PHP sessions are managed with a little (BIG) help from the cookies.

When a user requests a URL and the webpage makes use of php sessions, a cookie is set in the browser. Something like PHPSESSID=tfopsadrnngogere848k65get2. It is this cookie that will tell the server "who you are".

If you want to access the session of one of your users you have to know the session id of his current session. If you want to select a random user, look for the files inside the directory where PHP stores the session files (If you don't know where the session files are being stored, please search the  php.ini file for session.save_path. The file sess_tfopsadrnngogere848k65get2 refers to the session id tfopsadrnngogere848k65get2).

Now that you have the session id, open your website with the javascript console and run:

{% highlight javascript %}
//document.cookie = '<session_id>'
document.cookie = 'PHPSESSID=tfopsadrnngogere848k65get2'
{% endhighlight %}

Now on the address bar hit the enter key and *voilá*!
