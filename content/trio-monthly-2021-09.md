---
title: Trio Monthly - 2021/09
date: 2021-10-01T00:00:00+01:00
draft: false
tags:
 - trio-monthly
 - trio
---

*There is no August 2021 in Ba Sing Se.*

Welcome to the September 2021 edition of Trio Monthly. This is a monthly blog post dedicated to 
highlighting what's going on in the Trio world, from developments in the core library to 
developments in new and existing libraries and application powered by Trio.

If you wish to get your library featured in a Trio Monthly post, please contact the author on Matrix
(``@lura:veriny.tf``).

## Trio Library

### DTLS Support

Whilst it's only a provisional PR, Nathaniel J. Smith has been working on [adding DTLS support](https://github.com/python-trio/trio/pull/2047) to Trio.

For anyone unfamiliar, DTLS is TLS for UDP sockets. It's a much more obscure protocol than regular 
TLS, with not much widespread support. This PR makes Trio one step closer to being usable for 
everything network-related.

## External Libraries

### Tractor

The [Tractor](https://github.com/goodboy/tractor) project had a second alpha release this month.
Here's the juice, straight from the creator:

> `tractor` released a 3rd `.alpha` this month with the main attraction being the addition of task oriented _broadcast receiver_ API that is directly integrated with our IPC stream type - a la `tractor.MsgStream.subscribe()`. This lets us create an inter-actor (IPC) stream and then have many tasks pull from that stream using an interface mirroring that of [rust's `tokio::sync::broadcast` channels](https://docs.rs/tokio/1.11.0/tokio/sync/broadcast/index.html). This is a big boon for writing task-oriented code where you want to have many in-thread routines process the same stream in tandem where each task received a copy of each message.
> 
> Other improvements and updates include:
> 
>     * dropping Python 3.7 support
> 
>     * adopting `towncrier` for release automation
> 
>     * removing an internal _stream shielding_ API that was never documented and was a warty remnant from before we had `Portal.open_stream_from()`
> 
>     * fixed yet another tty hanging bug to do with the root actor shield acquiring the `pdb` lock
> 
> See the full deats in our [Release notes](https://github.com/goodboy/tractor/blob/master/NEWS.rst?rgh-link-date=2021-09-08T02%3A18%3A26Z).

## Python Itself

### PEP 654 Accepted!

PEP 654, the Exception Groups PEP, has been accepted for Python 3.11. This is a standard 
interpreter version of the ``MultiError`` concept that Trio uses.

This gives some certain advantages to Trio over the current ``MultiError`` implementation:

 - standard syntax means it's fully integrated into the ``try/except`` machinery
 - interpreter-level features are (usually) faster than Python-level features

 