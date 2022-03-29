# PBS Tibit 4 of Y — Re-thinking a Web App – from Web Server to Cloud Stack

In the main PBS series we're hovering on the edge of moving from purely client-side JavaScript apps to full web apps with a client and *'server'* side, but as we approach that word *server* is becoming every more abstract. At this stage it really does just mean *"something on the other side of an HTTPS connection"* - the days of web servers just being remote computers are well and truly over.

I've been thinking about this a lot because I've just finished helping Allison migrate her website from the old-world single-server model, to a modern cloud architecture, and boosting her site's performance by a few hundred percent in the processes (no exaggeration ).

From the point of view of a visitor www.podfeet.com is a website, but from Allison's point of view it's a web app. To be more sepecific, it's an instance of the popular open source content management system (CMS) [Wordpress](https://wordpress.org/). Allison doesn't edit a folder full of HTML files, she uses a web interface and a third-party client to manage the site and its contents.

## Matching Podcast Episode

TO DO

## What is a Web Server?

A web server is a piece of **software** that listens for in-coming HTTP requests from the network, and replies with HTTP responses.

When a user visits `www.podfeet.com` their browser connects to a web server and asks it for Allison's web page. The server responds with some HTML (which embeds references to other content like images, style sheets, and JavaScripts that the browser has to fetch in turn). The same things happens when Alison visits her Wordpress admin area. When Allison uses MarsEdit something only slightly different happens — there is no browser in this case, but the app sends HTTPS requests to a special URL on Allison's site referred to as an API-end-point, and the server responds to the app appropriately. Another differene is that the app and the API exchange XML rather than HTML.

Regardless of whether its a browser or an app asking for information, the web server is receiving requests, and responding with some data, usually some HTML, CSS, JavaScript, XML, or an image. The big question for us today is how those responses get generated.

### Static -v- Dynamic Content

As we'll see later, the request sent by the web server may get passed around a few times before a web server process finally does the work to fulfil the request. But regardless of the path by which the request arrives, the web server process has two fundamental choices for how to answer:

1. Return the contents of a file
2. Execute some code to calculate the response

We refer to files that are returned as-is as *static content*, and code that gets executed as *dynamic content*.

### Web Server Actions

When a web server receives a request for a particular URL is uses its configuration to figure out what to do with it. Web servers have four basic choices for generating their response:

1. Map the URL to a local **cache** entry that's not expired yet and return it.
2. Map the URL to a **local file** and return its contents.
3. Map the URL to a **local executable file**, run it, and return the results.
4. Pass, or **reverse proxy**, the request to another web server, wait for a response, and parrot that back to the client. **Put a mental in in this, it will become very important later.**

There is lots of web server software out there. Some of it is extremely generic and does a decent job of just about everything, like say the [Apache web server](https://httpd.apache.org/), and some is extremely task-specific like the open source [Varnish web cache](https://varnish-cache.org/intro/).

## Where do Database Servers Come In?

If a web server has to execute code to fulfil a request, then that code can do anything code can do, including reaching out to other resources. Something many server-side web apps need to do is store data in a structured format that can be efficiently searched. That is literally what databases were invented to do!

A database server is a piece of **software** that listens for in-coming requests, usually in the Standard Query Language, or SQL, carries out the requested search, store, update, or delete request, and returns the result or outcome. Like web servers, most database servers can listen for incoming network connections, but many can also listen through file-system-based connections known as *sockets*.

For the purpose of this instalment I'm going to pretend there's only one kind of database, the so-called *Relational Database Management System*, or RDBMS. RDBMSes are the Rolls-Royce of data storage — they make robust data integrity promises, support atomicity across complex transactions, support roll-back when things go wrong, and provide all that with impressive querying speeds through advanced indexing techniques. RDBMSes are some of the most impressive software as yet developed by humanity IMO. But, and it's a big but, all that comes at a price — these things are resource hogs, and they don't distribute well. Because of this, a lot of social media-like sites are prepared to accept *eventual* data integrity in exchange for vastly reduced load, and ease of distribution, so there's a whole other class of database out there we're going to completely ignore!

In the corporate world the king of the databases is Oracle, and in the open source it's MariaDB, a community fork of MySQL that was branched from it when Oracle bought it. MySQL/MariaDB are lighter-weight databases than Oracle — they require fewer resources, but also offer less advanced functionality. For those in the open source world who want the power and complexity of Oracle there's PostgreSQL 🙂

## The Old Way — A Single-Server LAMP Stack

Allison's infrastructure leading into this big move was very much in keeping with the norms for a medium-sized website a decade or so ago. The entire site was delivered from one cloud-hosted virtual machine, it was a classic LAMP stack:

1. CentOS <strong>L</strong>inux powering the VM
2. The <strong>A</strong>pache web server serving all content
3. A local installation of <strong>M</strong>ariaDB storing the data
4. Apache executing the Wordpress <strong>P</strong>HP code using `mod_php`

Linux, Apache, MySQL (MariaDB), and PHP — LAMP.

With this setup there is a one-to-one mapping between the server powering the site, and the site itself.

For small sites this architecture works very well, and it can work fine for smaller medium-sized sites, but as a site grows, this approach begins to fall apart. You can defer a major rearchitecting for a while by first fine-tuning the configurations, and then throwing more resources at the single virtual machine, but you soon run out of runway, and your site will start hitting tipping points where its performance simply collapses.

### Problem 1 — You Can't Optimise One VM for Two Tasks!

LEFT OFF HERE!!!

### Problem 2 — Apache's Design has Bottlenecks

TO DO

### Problem, 3 — Efficiently Executing PHP Code is Hard

### Executing Code — LATER

You can probably imagine how a cache works, and how a web server could simply return the contents of a file, but how a web server executes code needs a closer look.

In the early days of web servers the approach was very simple — some file extensions got mapped to being executable files, and the web server would read the so-called *shebang line* at the top of the file and run it through the appropriate interpreter. If the shebang line said it was Perl, run it through Perl, if it was shell script, run it through Bash, etc..






### It's Web Servers all the Way Down!

Here's where things get interesting — **a single web request can be processed by many web servers, each adding some needed functionality**.

## Final Thoughts

TO DO