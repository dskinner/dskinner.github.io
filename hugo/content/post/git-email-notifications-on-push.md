+++
date = "2012-11-19T12:23:00-04:00"
title = "Git Email Notifications on Push"
+++

So I'm doing a private collab and hosting a git repo off my server. Got annoying pretty quick with these occasional emails "hey pushed xyz change that could affect abc for you, make sure to pull the latest"

Enter this script, <a href="http://git.kernel.org/?p=git/git.git;a=blob;f=contrib/hooks/post-receive-email;h=60cbab65d3f8230be3041a13fac2fd9f9b3018d5;hb=HEAD">post-receive-email</a>

To get going with it, I did the following, (line breaks for readability here):

```
wget -O '/user/local/bin/post-receive-email' 'http://git.kernel.org/?p=git/git.git;a=blob_plain;f=contrib/hooks/' 'post-receive-email;h=60cbab65d3f8230be3041a13fac2fd9f9b3018d5;hb=HEAD'

chmod a+x /usr/local/bin/post-receive-email
```

Next, link to this in your repo with `ln -s /usr/local/bin/post-receive-email hooks/post-receive` and add something like this to config

```
[hooks]
    mailinglist = "jane@email.com, john@email.com"
    envelopesender = no-reply@email.com
    emailprefix = "[GIT] "
```

And that will notify the mailing list of any push that occurs. There's a number of other options worth exploring by reading the post-receive-email bash script.
