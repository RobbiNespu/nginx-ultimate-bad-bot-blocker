# Nginx Bad Bot Blocker, Referer Blocker, Anti DDOS, Bad IP Blocker and Wordpress Theme Detector Blocker
## The Ultimate Bad Bot and Referer Blocker for Nginx Web Servers including anti DDOS system and Wordpress Theme Detector Blocking

### Created by: https://github.com/mitchellkrogza

#### For Nginx Web Server - https://www.nginx.com/

### Recommend to saved as: /etc/nginx/conf.d/globalblacklist.conf

Why? .... because all files located in /conf.d/ are automatically loaded by Nginx in the main
nginx.conf file.

#### Includes the creation of a google-exclude.txt file for creating filters / segments in Google Analytics (see instructions lower down)
#### Includes the creation of a google-disavow.txt file for use in Google Webmaster Tools (see instructions lower down)

### WHY BLOCK BAD BOTS ?

#####Bad bots are:

-    Bad Referers
-    Spam Referers
-    Spam bots
-    Vulnerability scanners
-    E-mail harvesters
-    Content scrapers
-    Aggressive bots that scrape content
-    Bots or Servers linked to viruses or malware
-    Government surveillance bots
-    Botnet Attack Networks (Mirai)
-    Known Wordpress Theme Detectors (Updated Regularly)
-    SEO companies that your competitors use to try improve their SEO
-    Link Research and Backlink Testing Tools
-    Stopping Google Analytics Ghost Spam

Bots attempt to make themselves look like other software or web sites by disguising their user agent. 
Their user agent names may look harmless, perfectly legitimate even. 

For example, "^Java" but according to Project Honeypot, it's actually one of the most dangerous BUT a lot of
legitimate bots out there have "Java" in their user agent string so the approach taken by many to block "Java"
is not only ignorant but also blocking out very legitimate crawlers including some of Google's and Bing's and
makes it very clear to me that those people writing bot blocking scripts seldom ever test them. 

Unfortunately most bot blocker scripts out there are simply copy and pasted from other people's scripts and made 
to look like their own work. This one  was inspired by the one created by https://github.com/mariusv and I 
contributed to that project but went off into a totally new layout, cleaned it up big time and started from scratch.
It is now a completely independent project. It's clean, it works and has been thoroughly tested.

# Welcome to the Ultimate Nginx Bad Bot and Referer Blocker with and Anti DDOS system

### THE BASICS

This nginx bad bot bot blocker list is designed to be a global Nginx include file and uses the Nginx
map $http_user_agent, map $http_referer and geo $validate_client directives.

This way the .conf file is loaded once into memory by Nginx and is available to all web 
sites that you operate. You simply need to use an Include statement in an Nginx vhost conf file.

My methods uses no complex regex other than the Name of the Bot. Nginx case matching will do the rest. 
You can use Regex if you like but it's NOT needed and I proved it by testing with the Chrome extension
User-Agent Switcher for Chrome. (handy util and a must for everyone to test these kinds of blocking scripts)

- The user agent "Aboundex" is found without using "~*Aboundex" ... which means a case insensitive match and
is much simpler for anyone to maintain than other lists using complicated and messy Regex patterns.

- If we have a rule, like "~*Image\ Stripper" and a bot decides to change its User-Agent string to 
"NOT Image Stripper I Promise" he is picked up regardless and blocked immediately. 

I only capitalise bot names in my list for ease of reading and maintenance, remember its 
not case-sensitive so will catch any combination like "Bot" "bOt" and "bOT".

For those of you who SUCK with Regex my Nginx Bad Bot Blocker is your saviour !!!

### IT'S CENTRALISED:

The beauty of this is that it is one central file used by all your web sites.
This means there is only place to make amendments ie. adding new bots that you
discover in your log files. Any changes are applied immediately to all sites after
a simple "sudo service nginx reload". But of course always do a sudo nginx -t to test
any config changes before you reload.

### IT IS TINY AND LIGHTWEIGHT

The file is tiny in size. At the time of this writing and the first public commit of this
the file size including all the commenting "which nginx ignores" is a mere 58 kb in size.
It is so lightweight that Nginx does not even know it's there. It already contains hundreds 
of entries.

### IT IS ACCURATE AND IS FALSE POSITIVE PROOF

Unlike many other bad bot blockers out there for Nginx and Apache where people simply copy and
paste lists from others, this list has been built from the ground up and tested thoroughly and I
mean thoroughly. It comes from actual server logs that are monitored daily and there are at least 3-10
new additions to this file almost daily.

It has also been throughly tested for false positives using months of constant and regular testing
and monitoring of log files. 

All web sites listed in the bad referers are checked one by one before they are even added. Simply copying 
anything that look suspicious in your log file and adding it to a blocker like this without actually seeing 
what it is first .... well it's foolish to say the least.

### DROP THEM AND THAT'S IT

Nginx has a lovely error called 444 which just literally drops the connection. All these rules issue a
444 response so if a rule matches, the requesting IP simply get's no response and it would appear that your 
server does not exist to them or appears to be offline.

### RATE LIMITING FUNCTIONALITY BUILT IN

For bot's or spiders that you still want to allow but want to limit their visitation rate, you can use the 
built in rate limiting functions I have included. The file is extensively commented throughout so you should 
figure it out otherwise simply message me if you are having problems. 

## (Read configuration instructions below thoroughly)

## FEATURES OF THE NGINX BAD BOT BLOCKER:

- Extensive Lists of Bad and Known Bad Bots and Scrapers (updated almost daily)
- Alphabetically ordered for easier maintenance
- Commented sections of certain important bots to be sure of before blocking
- Includes the IP range of Cyveillance who are known to ignore robots.txt rules
  and snoop around all over the Internet.
- Your own IP Ranges that you want to avoid blocking can be easily added.
- Ability to add other IP ranges and IP blocks that you want to block out.

Usage: recommended to be saved as /etc/nginx/conf.d/globalblacklist.conf 

####PLEASE READ: 
The configuration instructions below !!!!

## WARNING:

 Please understand why you are using this before you even use this.
 Please do not simply copy and paste without understanding what this is doing.
 Do not become a copy and paste Linux "Guru", learn things properly before you use them
 and always test everything you do one step at a time.
 
## ANOTHER WARNING:
  Make sure to add all your own IP addresses the white list section near the bottom of the 
  globalblacklist.conf file !!!!

## MONITOR WHAT YOU ARE DOING:

 MAKE SURE to monitor your web site logs after implementing this. I suggest you first
 load this into one site and monitor it for any possible false positives before putting
 this into production on all your web sites.
 
 Also monitor your logs daily for new bad referers and user-agent strings that you
 want to block. Your best source of adding to this list is your own server logs, not mine.
 
 Feel free to contribute bad referers from your own logs to this project by sending a PR.
 You can however rely on this list to keep out 99% of the baddies out there.
 
## HOW TO MONITOR YOUR LOGS DAILY (The Easy Way):

With great thanks and appreciation to https://blog.nexcess.net/2011/01/21/one-liners-for-apache-log-files/

To monitor your top referer's for a web site's log file's on a daily basis use the following simple
cron jobs which will email you a list of top referer's / user agents every morning from a particular web site's log
files. This is an example for just one cron job for one site. Set up multiple one's for each one you
want to monitor. Here is a cron that runs at 8am every morning and emails me the stripped down log of
referers. When I say stripped down, the domain of the site and other referers like Google and Bing are
stripped from the results. Of course you must change the log file name, domain name and your email address in
the examples below. The second cron for collecting User agents does not do any stripping out of any referers but you
can add that functionality if you like copying the awk statement !~ from the first example.

##### Cron for Monitoring Daily Referers on Nginx

`00 08 * * * tail -10000 /var/log/nginx/mydomain-access.log | awk '$11 !~ /google|bing|yahoo|yandex|mywebsite.com/' | awk '{print $11}' | tr -d '"' | sort | uniq -c | sort -rn | head -1000 | mail - s "Top 1000 Referers for Mydomain.com" me@mydomain.com`

##### Cron for Monitoring Daily User Agents on Nginx

`00 08 * * * tail -50000 /var/log/nginx/mydomain-access.log | awk '{print $12}' | tr -d '"' | sort | uniq -c | sort -rn | head -1000 | mail -s "Top 1000 Agents for Mydomain.com" me@mydomain.com`

## CONFIGURATION OF THE NGINX BAD BOT BLOCKER:

####First Step: 

`sudo mkdir /etc/nginx/bots.d `
- copy the blockbots.conf file into that folder
- copy the ddos.conf file into the same folder

####Second Step:

- `sudo nano /etc/nginx/nginx.conf`

#####Add the following rate limiting zones

- `limit_req_zone $ratelimited zone=flood:50m rate=90r/s;`

- `limit_conn_zone $ratelimited zone=addr:50m;`

PLEASE NOTE: The above rate limiting rules are for the DDOS filter, it may seem like high values to you
but for wordpress sites with plugins and lots of images, it's not. This will not limit any real visitor to
your Wordpress sites but it will immediately rate limit any aggressive bot. Remember that other bots and user
agents are rate limited using a different rate limiting rule at the bottom of the globalblacklist.conf file.

####Third Step:

Open a site config file for Nginx (just one for now) and add the following lines.
##### IMPORTANT: includes MUST be added within a server {} block otherwise you will get EMERG errors from Nginx

- `include /etc/nginx/bots.d/blockbots.conf;`

- `include /etc/nginx/bots.d/ddos.conf;`

####Fourth Step:

 Make sure to edit the globalblacklist.conf file near the bottom there is a section to whitelist your own
 IP addresses. Please add all your own IP addresses there before putting this into operation.

####Fifth Step:

sudo nginx -t (make sure it returns no errors and if none then)
sudo service nginx reload

##Finally - Stopping Google Analytics 'ghost' spam
Simply using the Nginx blocker does not stop Google Analytics ghost referral spam 
because they are hitting Analytics directly and not always necessarily touching your website. 
You should use regex filters in Analytics to prevent ghost referral spam.
For this a simple google-exclude.txt file has been created for you and it is updated at the same time
when the Nginx Blocker is updated.

##To stop Ghost Spam on On Analytics
Navigate to your Google Analytics Admin panel and add a Segment. (New Segment > Advanced > Conditions)
This will need to be done on each and every site where you want this filter to be in effect. 
Google has a stupid limit on the length of the regex so you need to break it up into multiple exclude filters 


| Filter          | Session       | Include                                  |
| :-------------: |:-------------:|:----------------------------------------:|
| Hostname        | matches regex | ```yourwebsite\.com|www\.yourwebsite\.com``` |

| Filter          | Session       | Exclude                                                       |
| :-------------: |:-------------:|:-------------------------------------------------------------:|
| Hostname        | matches regex | Copy the entire contents from [google-exclude.txt](https://github.com/mitchellkrogza/nginx-ultimate-bad-bot-blocker/blob/master/google-exclude.txt) to this field |

#Or Even Better Check Out RefererSpamBlocker

Rather check out the awesome [Referer Spam Blocker](https://referrerspamblocker.com)
for Google Analytics which uses a collaborated source of spam domains and automatically adds all the filters to your
Analytics sites for you in 2 easy clicks and it is FREE.

##Blocking Spam Domains Using Google Webmaster Tools

I have added the creation of a Google Disavow text file called google-disavow.txt. This file can be used in Google's Webmaster
Tools to block all these domains out as spammy or bad links. Use with caution.


# IT FORKING WORKS !!!
## Just Enjoy now what the Nginx Bad Bot Blocker Can Do For You and Your Web Sites.

### If this helped you why not [buy me a beer](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=XP2AZ4S5HNAWQ):beer:

##WARRANTY OR LICENSE

- This is free to use and modify as you wish. 
- No warranties are express or implied.
- You use this entirely at your own Risk.
- Fork your own copy from this repo and feel free to change it to your needs or contribute to it.
- If you break it yourself, you fix it yourself.

##### Some other free projects

- https://github.com/mitchellkrogza/apache-ultimate-bad-bot-blocker
- https://github.com/mitchellkrogza/Fail2Ban-Blacklist-JAIL-for-Repeat-Offenders-with-Perma-Extended-Banning
- https://github.com/mitchellkrogza/fail2ban-useful-scripts
- https://github.com/mariusv/nginx-badbot-blocker
