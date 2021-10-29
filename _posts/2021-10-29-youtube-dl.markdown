---
layout: post
title:  "youtube-dl on Ubuntu 20.04"
date:   2021-10-29 16:40:00 -0700
categories: media ubuntu linux
---
[Youtube-dl](https://ytdl-org.github.io/youtube-dl/index.html) is a useful
command-line based program for downloading videos off YouTube. For instance,
you can use it to download music and podcasts to be listened to offline. This
is a great alternative to using subscription services for people that don't
want to get locked into using proprietary software or paying monthly fees.

Rather than installing `youtube-dl` through through Ubuntu's repositories, we
will go directly to [the source
code](https://ytdl-org.github.io/youtube-dl/download.html). As shifting
policies at YouTube mean that quick updates might be needed to make some
download succeed, having the latest version of `youtube-dl` available is a
must!

After following the official install instructions for UNIX, try running:

{% highlight bash %}
youtube-dl --update # or use the -U flag
{% endhighlight %}


On Ubuntu 20.04 from a fresh install, the command `python` will fail&mdash;this was done so
that there would be no ambiguities whether some program uses Python 2 or Python 3. 

Adding a symbolic link (following [this thread on askubuntu](https://askubuntu.com/questions/1037666/youtube-dl-python-not-found-18-04) solved the problem:

{% highlight bash %}
sudo ln -s /usr/bin/python3 /usr/local/bin/python
{% endhighlight %}

Now, just run the update command every once in a while (or do something fancy
like setting a `cron` job) to keep the software up to date.

