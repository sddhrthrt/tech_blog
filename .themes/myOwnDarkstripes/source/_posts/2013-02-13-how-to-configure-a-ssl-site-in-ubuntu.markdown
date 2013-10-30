---
layout: post
title: "How to configure an SSL site in Ubuntu?"
date: 2013-02-13 15:48
comments: true
published: true
categories: apache ubuntu howto site ssl
---

A part of my Software Engineering project is this (specially :P) assigned task
of reverse engineering a website for my department. This site for
[DASA](dasanit.org) applicants is a `https` site, obviously, and I had to
install this locally. Well, this just happens to be my first journey into the
world of php. I got the site from the maintainer, who is pretty new to the role
too. Now there were two folders, one which had all the files of the site,
another with the database dump.
<!-- more -->

Here is what to do when you are in a situation like this - create a new site
using HTTPS or something.

First, let's start with installing the L(inux)A(pache)M(ysql)P(hp/ython) stack
in your Ubuntu machine. There is an excellent method to do this:

``` bash Installing the lamp stack

sudo apt-get install tasksel 
sudo tasksel

```

Now select the LAMP stack option (usually the 4th). Press enter and you are
done with installing the LAMP stack. Visit `localhost` with your browser to
confirm - you should see something other than the usual boring error message
'page not found' 

Now you have to configure the site.

``` bash Configuring a new site in Apache2

#change to the /var/www directory 
cd /var/www

#change permissions - otherwise you will have to struggle later.  
#do this if this is your own computer. Otherwise change permissions of 
#individual directories as you go ahead.
sudo chown -R username:username /var/www

#This step is dangerous on someone else's computer too - but on your own
#personal machine, chill!  
sudo chmod +rwx -R /var/www

mkdir /var/www/dasa

#now it's time to create the ssl: 
#when you visit with your browser to https://localhost, we should get the same 
#page as you got previously, after following the steps below.

sudo a2enmod ssl 

#use the default ssl site configuration to create one of your own:
sudo cd /etc/apache2/sites-available/
sudo cp default-ssl ssl
sudo gedit ssl
```

Now edit the file and change the line in the top from `<VirtualHost _default_:443>`
to `<VirtualHost *:443>` and save the file.

``` bash Save and restart.
sudo a2ensite ssl 
sudo service apache2 restart

#That's pretty straightforward!  
#Now I can visit `https://localhost` 

```


Now all I had to do was copy the files from the folder given to me into the 
`/var/www/dasa` folder. I did and voila! It was working.

###Footnote:
Whenever you copy some files into the /var/www, you have to enable `+rwx` using
the command above. Instead, you could just add a folder mask to this folder so
whatever files you copy will get suitable permissions:

`umask 0000`

That will make sure all the files that come into the directory will not lose
any permissions.



