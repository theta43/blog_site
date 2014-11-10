---
layout: post
title:	"Email and the Internet - Exposition"
date:	2014-11-05 19:31:20
categories: email tutorial guides
---

## Abstract
This is the beginning of a series of blog posts intended, not to
teach anyone about the structure of modern internet mail, but as a place for me
to practice technical writing. Any ancillary knowledge acquired from the
reading of this material should be regarded as a mere coincidence.

The "Blog"-style are a laughably inadequate format for discussing technical
documentation. At best, I can provide an overview of the terms and, perhaps, a
brief introduction to the topics presented. Anyone intending to use this as a
source of learning material should seriously reconsider other sources. There
are numerous locations on the internet that are better suited for teaching; and
vastly superior formats for presenting.

It should be made clear that -- while I do care about the integrity of the
information presented -- I consider this project of minimal importance and
least deserving of my time.

Let's get started.

## Overview
The Simple Mail Transfer Protocol, or SMTP, is an internet standard specified
by the Internet Enginering Task Force. Formally specified in RFC 821, and
supplanted by extensions defined in [RFC 5321][]: SMTP is concerned
with efficiently and reliably transfering mail over the internet.

Historically, SMTP was concerned with submission and relaying mail between
servers. This proved to have practical limitations, and as such the standard
split into what is now the "Message Submission for Mail" protocol defined in
[RFC 6409][].

By modern standards the SMTP defines the Server-to-Server interaction of the
mail transfer. Each server may act: as a client, when receiving a message from
another server; as a server, when delivering mail to the next relay or
at the end of line where the message is handed to the "Mail Delivery Agent". 

Typically, SMTP servers will support both Submission and Relaying, so we don't
have to concern ourselves with setting them up independently. We do, however,
have to determine what mail needs to be relayed(and who can do it), and what
mail needs to be delivered. It is considered bad practice to be an "Open
Relay", worthy of blacklisting.

## DNS
The MX-record(Mail eXchanger) in the DNS record is used to identify the MX
server of the given domain.

For example:

{% highlight bash %}

$ host theta.pw
> [removed]
> theta.pw mail is handled by 10 mx.theta.pw.

$ host mx.theta.pw
> mx.theta.pw has address 81.4.111.220
> [removed]

{% endhighlight %}

When attempting to send mail to <james@theta.pw>, your "Mail User Agent" will
perform a DNS-MX lookup of "theta.pw" to decide which server to connect to.

## Ports
RFC 5321 reserves port **25** for SMTP. Residential ISPs will typically redirect
connections on port 25 to their own servers instead of forwarding to your home
router.

Unfortunately, this makes hosting your own residential email server
impossible. Fortunately, this has reduced the amount of mail spam. Large
bot-nets used to be responsible for the large volume of spam; now these
spammers are using a relatively small number of servers.

RFC 6409 reserves port **587** for mail Submission. ISPs typically do not block
or redirect this port, so most people can *send* mail from their home computers
but can't ever be a relay, or the final destination for mail delivery.

Some admins still use port 465, which has now officially been reserved by some
other irrelevent server. The standard, and best-practice, dictate port 25 for
SMTP and port 587 for Submission. [RFC 3207][] defines the
SMTP-extension detailing encrypted connections with TLS: STARTTLS.

## POP and IMAP
SMTP and Mail Submission are used for delivering mail from the user, through
the relays, to the server that decides the mail belongs with them. When a user
wants to retrieve their mail -- while it's technically possible to use SMTP --
a different protocol is conventionally used, namely, IMAP or POP.

These two protocols will be discussed in more detail later.

## Software Implementation
In the free-software world, the [leading][Mail Server Survey] SMTP/Submission
implementation(based on number of installations) is [Exim][]. We will discuss
the numerous configuration options for this program later.

For IMAP and POP, the software I prefer is [Dovecot][]. I prefer this
implementation over the numerous others mostly because of its support for a
large collection of authentication mechanism, integration with Exim, and its
clean code style.

Though any standards-compliant implementation will work just as well. Use
what's easiest/safest to implement in your environment. For the purposes of
this series I will assume you're using Exim/Dovecot.

[RFC 5321]: https://tools.ietf.org/html/rfc5321
[RFC 3207]: https://tools.ietf.org/html/rfc3207
[RFC 6409]: https://tools.ietf.org/html/rfc6409
[Mail Server Survey]: http://www.securityspace.com/s_survey/data/man.201308/mxsurvey.html
[Exim]: http://exim.org
[Dovecot]: http://dovecot.org
