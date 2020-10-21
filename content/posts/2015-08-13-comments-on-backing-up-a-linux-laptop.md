---
title: Comments on Backing Up a Linux Laptop
author: Harrison Ripps
type: posts
date: 2015-08-13T04:00:00+00:00
url: /2015/08/13/comments-on-backing-up-a-linux-laptop/
tumblr_asciiteam_permalink:
  - https://asciiteam.tumblr.com/post/173163388212/comments-on-backing-up-a-linux-laptop
tumblr_asciiteam_id:
  - 173163388212
categories:
  - Uncategorized
tags:
  - linux
  - open source

---
My primary workhorse right now is an [HP Omen][1] running Fedora 22. Thinking and talking about how to restore my work environment from a total failure, I came to the conclusion that there are three primary things that need to be preserved:

**Item 1**: My entire user directory under `/home`, since that’s where all my stuff is.

**Solution**: [Crashplan][2] is impressing me &#8211; slick commercial-quality UI, completely free for mounted “local” disk backups and peer-to-peer backups through their central broker. Naturally they also offer managed off-site backups for a cost, but they don’t ram it down your throat. I think they’ll be getting some of my business.

**Item 2**: A list (preferably in [Kickstart][3] format) of user-installed software packages, so that I can restore all of the apps I use.

**Solution**: this is very distro-specific, but `dnf` (`yum`’s heir apparent) has a kickstart-friendly report that I will integrate into a once-a-day kickstart generator script: `dnf history userinstalled`

**Item 3**: A snapshot of everything under `/etc`, which tells the computer how my apps and service should be run.

**Solution**: I found an awesome utility called [etckeeper][4] that turns `/etc` into a git (or mercurial, or svn) repo and can automatically push changes out to multiple targets. On its own it is nice, but the dnf-etckeeper packages hooks this behavior directly into the package manager &#8211; so, every time I run dnf for an update or a new package, my `/etc` snapshot is updated as well.

* * *

Thinking about this now, I realize that for completeness I need to copy `/var/spool/cron/*` somewhere as well. The only cron job I’m running is the tail-end of an IFTTT hack to gets photos off of my iPhone and into shotwell, but that’s fodder for another post.

 [1]: https://en.wikipedia.org/wiki/VoodooPC
 [2]: http://www.code42.com/crashplan/
 [3]: https://github.com/rhinstaller/pykickstart/blob/master/docs/kickstart-docs.rst
 [4]: http://etckeeper.branchable.com/