---
title: 'OS X to Fedora 20: The User Experience, Part 2'
author: Harrison Ripps
type: posts
date: 2014-08-26T04:00:00+00:00
url: /2014/08/26/os-x-to-fedora-20-the-user-experience-part-2/
tumblr_asciiteam_permalink:
  - https://asciiteam.tumblr.com/post/173163658157/os-x-to-fedora-20-the-user-experience-part-2
tumblr_asciiteam_id:
  - 173163658157
categories:
  - Uncategorized
tags:
  - cloud
  - dropbox
  - fedora
  - linux
  - osx

---
Just tuning in? Have a look at [Part 1][1] to find out how I installed Fedora 20 on a MacBook Air and why I am so interested in trying to duplicate or even surpass the user experience that I previously enjoyed with OS X.

### The Cloud

Cloud-based mail, calendaring and file synching have really levelled the playing field between the desktop OSes. It doesn’t matter if you are using OS X or Linux if you read your e-mail in a web browser. Cloud-based services are also the foundation of another important aspect of my day-to-day life: my “gadgets ecosystem”. Even though I’m spending most of my time on Fedora 20 now, my phone and tablet still run iOS. As you will see, this strongly influenced the way that I’ve leveraged the cloud in my system setup.

Here are the services that I needed to put in the cloud:

  * Mail
  * Address Book
  * Calendar
  * To-Do List(s)
  * Notes
  * Files (read: a small number of files that would be handy to all of my devices)
  * Password Management

Some of these were no-brainers, others drove me to try and end up loving some interesting new services…

### Mail

People don’t normally think of mail as a cloud service, but for most folks, it is. I could run my own mail server but I like the fact that my e-mail stuff is being handled by someone who’s full time job is e-mail, and that the service is configured for high availability. That said, part of the reason that I use a native mail client rather than just using my mail service’s web client is so that I can also have a local copy of all of the e-mail. This makes for faster searches and I always have those e-mails on hand if the mail service goes offline.

That all said, mail is pretty much the original cloud service and [IMAP][2] keeps everything in sync for me, so problem solved.

### Address Book

It may seem odd to think of an address book as being separate from a mail service, but in the cloud, they are. If you use Gmail, Hotmail, Yahoo mail, or any of the other popular free e-mail services, you never have to worry about this distinction. This is because Fedora and OS X both understand the concept of bundled internet accounts. You tell your computer “I have a Google account” and then your computer asks you which services to use from the bundled list.

But I don’t use Gmail for mail, and furthermore I actually dislike Google’s approch to putting every last e-mail address and Google Plus profile in my address book without my consent. For a long time I used iCloud’s address book, but that’s a no-go from a Linux system. So, enter [ownCloud][3].

ownCloud could actually be the answer to a few of the questions in this post, but in my setup I use it expressly for my address book. This lives on an [ownCloud server hosted on OpenShift][4]. Advantages:

  * Use of the industry-standard CardDAV protocol makes this solution work for my Linux laptop and my mobile iOS devices
  * Two-factor authentication for heightened security
  * Hosting on OpenShift means that: 
      * My app is monitored and restarted if necessary
      * I can automate backups with the rhc utility

### Calendar

I have to grudgingly give a nod to Google on this one. By solving calendaring and then building a social network with built-in event scheduling, they’ve created a siituation where it is hard to use anything else. ownCloud supports calendaring, but scheduling events across different calendaring systems seems very spotty.

I threw up my hands on this one; too many folks that I need to share my non-work plans with are all on Google, so I have to live with Google for this as well.

### To-Do Lists

Apple’s “Reminders” app adds a lot of value beyond making lists. Probably the strongest feature there is the ability to geotag a task (e.g. Remind about picking up milk when I get to the grocery store). What “Reminders” doesn’t do is enable me to quickly create tasks from the Terminal, which is where I am working most of the time.

I’ve ended up splitting the difference on this one. If I need to geotag a task, I’ll create it in Reminders app on my iPhone. Otherwise, I have found an awesome task list solution in [Todo.txt][5].

Todo.txt is great for a few reasons:

  * Available from the command line
  * Can be cloud-enabled via [Dropbox][6] integration<sup id="fnref:1"><a href="1" class="footnote-ref" role="doc-noteref">1</a></sup>
  * Data is stored in a plain text file
  * Native apps exist for getting at your lists from iOS / Android devices

There’s a blog post idea in my future where I talk more about why plain text file formats are so great, but for now, the takeaway is that todo.txt uses a winning combination of simplicity and optional cloud availability.

### Notes

Finding an alternative to Notes.app can be dirt simple depending on what you want out of the app. At a minimum, you could simply designate a folder in your DropBox for notes and voila, problem solved. This is particularly true in Fedora 20 and OS X, both of which come with automatic file indexing.

However, I wanted faster navigation between notes and more targeted searching, and that led me to try out [nvpy][7]. This oddly-named [Python][8] of [Notational Velocity][9] makes use of global hotkeys and a very complete set of app-wise key combinations to provide a note-keeping and note-searching powerhouse. It can sync with [SimpleNote][10]’s API but it also does perfectly well writing Notes to text files, which brings us back into DropBox territory.

`nvpy` also runs on OS X, but [nvALT][11] looks a little slicker on OS X and can handle the same DropBox-synced note files as `nvpy`. There is one difference: `nvALT` will allow you to create notes with whitespace in the titles while `nvpy` will not, so get into the habit of putting underscores in the first line of the note.

The main benefit to this approach is that I can still get at those same notes from iOS. [Elements for DropBox][12] is a Markdown-friendly notes app that has no trouble displaying the notes that I create in nvpy.

As an aside, ownCloud has a plugin for Notes, but the only way to see / use the notes is through the web UI or the native ownCloud apps. If I _really_ wanted to solve this I’d go with the “just use text files” solution, but we get access from Fedora, OS X, and iOS with DropBox.

### Files

By now I think you can guess which direction I took. With DropBox and ownCloud to choose from, I’ve gone with DropBox simply because so many more apps can get at those files. If I had very sensitive data to store, I could use ownCloud at the cost of less ease of access.

### Password Management

On OS X I am huge fan of [1Password][13]. However, that’s not going to help me on Fedora. The best solution that I could find was [LastPass][14], which runs as a browser plugin under OS X, Linux, and anything else that can run modern builds of FireFox, Chrome or Safari.

LastPass is free for basic services, but if you want a native mobile app, you need to subscribe. My cheapskate workaround is simply to log into LastPass with my account and manually copy passwords when I need to use them on iOS.

### The TL;DR on Cloud Services

Cloud services offer a great way to level the playing field between Fedora 20 and OS X. By shifting my cloud usage to apps that could leverage DropBox or presented well-know web services APIs, I was able to create a setup that makes Fedora 20 work as well as OS X and still be able to get at the same data from my iOS-based mobile devices.

<div class="footnotes" role="doc-endnotes">
  <hr />
  
  <ol>
    <li id="fn:1" role="doc-endnote">
      <p>
        DropBox shows up a lot in this article. The main reason is that DropBox offers ubiquitous file storage with widely adopted support. Many iOS apps continue to offer DropBox-based syncing as an alternative to iCloud, and DropBox runs on Linux systems as well. This has inadvertantly placed DropBox as the best low-level cloud storage option for a wide variety of uses. <a href="1" class="footnote-backref" role="doc-backlink">&#x21a9;︎</a>
      </p>
    </li>
  </ol>
</div>

 [1]: https://asciiteam.io/post/173163611822/os-x-to-fedora-20-the-user-experience-part-1
 [2]: https://en.wikipedia.org/wiki/Internet_Message_Access_Protocol
 [3]: https://owncloud.org/
 [4]: https://hub.openshift.com/quickstarts/4-owncloud
 [5]: http://todotxt.com/
 [6]: https://www.dropbox.com/
 [7]: https://github.com/cpbotha/nvpy
 [8]: https://www.python.org/-port
 [9]: http://notational.net/
 [10]: http://simplenote.com/
 [11]: http://brettterpstra.com/projects/nvalt/
 [12]: http://t.umblr.com/redirect?z=https%3A%2F%2Fitunes.apple.com%2FUS%2Fapp%2Fid382752422%3Fmt%3D8&t=ZjI5ZDJhOTkzOGUyYjE4MGUzZWM1YWVmYjJlYzEzOTk2NTgzMzZjOSxaV3FDd05GWA%3D%3D
 [13]: https://agilebits.com/onepassword
 [14]: https://lastpass.com/