# ipemailer
A tiny but handy script that notifies you via email (with the address of the new IP) if your dynamic IP changes.

<img src="https://i.imgur.com/jw7WLdd.png">

## Main purpose:
Created by me originally when I wanted to save money from
not registering a domain name.

I was with an ISP that gave out dynamic IPs for consumers,
probably to discourage people from running servers on a home
internet connection.
So many problems were had when my IP got changed either due
to the lease time being up or the modem restarted due to power outage(s).

## Requirements:
+ bash
+ mutt
+ gnutls-bin
+ openssl
+ libsasl2-modules-gssapi-mit
+ a Gmail account

### Installation:

Clone it:
```
git clone git@github.com:esn89/ipemailer.git
```

Get packages:
```
apt-get install libsasl2-modules-gssapi-mit gnutls-bin openssl mutt
```

Place the script in your $PATH for execution, I prefer
/usr/local/bin
```
cd ipemailer
cp ipscript /usr/local/bin/
```

Make a .muttrc file:
```
touch ~/.muttrc
```
and using your editor of choice, copy this into the .muttrc
```
set from = "youremail@gmail.com"
set realname = "whateveryouwish"
set imap_user = "youremail@gmail.com"
set imap_pass = "password"
set folder = "imaps://imap.gmail.com:993"
set spoolfile = "+INBOX"
set postponed ="+[Gmail]/Drafts"
set header_cache =~/.mutt/cache/headers
set message_cachedir =~/.mutt/cache/bodies
set certificate_file =~/.mutt/certificates
set smtp_url = "smtp://youremail@smtp.gmail.com:587/"
set smtp_pass = "password"
set move = no
set imap_keepalive = 900
set ssl_starttls=yes
set ssl_force_tls=yes
```
A word of caution is that, because this is stored in
plain-text in your home directory, I highly recommend making
a new Gmail account just for this purpose.  In the event
your server gets compromised, you don't lose your main email
account with your personal information too.

Or you could encrypt it on your machine like [so](https://pthree.org/2012/01/07/encrypted-mutt-imap-smtp-passwords/)

Set it as a cron job for automation:
```
crontab -e
```
add this line:
```
0 5 * * * /usr/local/bin/ipscript
```
This has the script run every day at 5am that way, the
longest I go without SSH-ing into my server, worst case is a day.

## Problems I have ran into:

If you get something along the lines of an SASL error, it
could be that Google is blocking 'less secure apps' from
sending email.  To disable it and allow mutt to send email
on your behalf, from your server go
[here](https://support.google.com/accounts/answer/6010255?hl=en)
to change the settings.

It could be that your server is not on the list of trusted
devices that you normally log in from, so you can change
that
[here](https://support.google.com/accounts/answer/2544838?hl=en)
