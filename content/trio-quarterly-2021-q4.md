---
title: Trio Quarterly - 2021/10 -> 2021/12
date: 2021-12-31T00:00:00Z
draft: false
tags:
 - trio-monthly
 - trio-quarterly
 - trio
---

This is the Q4 2021 edition of the Trio Quarterly newsletter, replacing the old Monthly one as a
quarterly schedule is a better fit for the project.

If you wish to get your library featured in a Trio Monthly post, please contact me on Matrix
(``@lura:veriny.tf``), or ping me in the Gitter (same username). Any Trio-related project is welcome!

## Trio Library

### OpenSSL 3.0 support

The unforgiving passage of linear time comes for us all, including OpenSSL. OpenSSL version 3.0
was released recently, alongside support for it in Python 3.10. Unfortunately, Trio did not support
it properly due to internal error handling issues. This has been fixed with 
[PR #2203](https://github.com/python-trio/trio/pull/2203), so now you can enjoy Trio applications
on newer OpenSSL versions.

## External Libraries

### trio-parallel

The [trio-parallel](https://github.com/richardsheridan/trio-parallel) project had a new release this
quarter, exiting alpha.

> trio-parallel, the simple library for CPU-parallelism in Trio, released a 1.0.0 version to PyPI, including bugfixes, revamped documentation, and a finalized WorkerContext API. Check it out at trio-parallel.readthedocs.io ! 

### Tractor

The [Tractor](https://github.com/goodboy/tractor) project had two new releases this quarter:

Alpha3:

> `tractor` saw its 4th alpha release with some interesting improvements in the core cross-actor cancellation mechanics as well as the start of a public expose of what were our internal `trio` helper API goodies and we officially now have optional `msgspec` support!
> 
> The quick feature summary:
> 
>    * We switched to making windows use the `trio`/`subprocess` spawner by default which means windows users now get multi-core debugger support out of the box.
> 
>      Support for optional [`msgspec`](https://github.com/jcrist/msgspec) lib usage for those who want a faster and typed codec alternative to [`msgpack-python`](https://github.com/msgpack/msgpack-python).
> 
>    * We exposed a new sub-package `tractor.trionics` which is where we'll be stashing all our "higher level" `trio` related helper APIs going forward. For example checkout our new [`gather_contexts`](https://github.com/goodboy/tractor/blob/master/tractor/trionics/_mngrs.py?rgh-link-date=2021-11-01T18%3A13%3A56Z#L39) concurrent context manager enter-er (kinda like a concurrent version of `asyncio.gather()` but for context manager instances).
> 
>    * Dropped Py 3.8 support as we decided to roll with a new model of supporting only the latest 2 major releases which means we also now support 3.10.
> 
>    * Change the core message loop to handle task and actor-runtime cancel
>       requests **immediately** instead of scheduling them as is done for rpc-task
>       requests. Previously cancel requests were actually _scheduled_ as rpc requests but this has changed to instead invoke a synchronous style cancellation which resulted in a massive reduction in teardown race conditions particularly in our anti-zombie-process machinery and it's interaction with the debugger system.

Alpha4:

> This _quarter_ in `tractor` we had our 4th alpha release with some super hip features landing as well as a ton of internal runtime improvements with the end result of a much more deterministic inter-actor cancellation system as well as better defined cross-actor task _context_ semantics.
> 
> The slew of details is in [our changelog](https://github.com/goodboy/tractor/blob/master/NEWS.rst?rgh-link-date=2021-12-17T17%3A46%3A31Z) but just in case you wanted the TL;DR:
> 
>    * We re-licensed the code base under AGPLv3 going foward (solely to close the SaaS loophole in the old license).
> 
>    * Added a new "infetced `asyncio` mode" which let's you spawn actors which run the `asyncio` event loop, start `trio` in [guest-mode](https://trio.readthedocs.io/en/stable/reference-lowlevel.html?highlight=guest%20mode#what-is-guest-mode) on the loop, then accept RPC requests from peer actors to drive `asyncio` tasks in a SC-per-task oriented way.
> 
>    * Support for _error relay_ across `tractor.Context` inter-actor linked tasks meaning if a task errors in one actor the error is raised (inside an appropriate boxed exception) to the other side's cancel scope. This was part of ensuring if a stream is opened only on one side of a bi-directional stream then a new `StreamOverrun` error will be raised on the sending side to avoid unexpected backpressure hangs.
> 
>    * A new  `trionics.maybe_open_context()` an actor-scoped async multi-task
>       context manager resource caching API.
> 
>    * Deterministic handling of _actor cancel requests_ which are implemented currently as special RPC calls (instead of built into our messaging protocol) such that `Portal.cancel_actor()` won't not just be abandoned without waiting to see if it was successful joy
> 
>    * Support `debug_mode: bool` on a per-actor basis.
> 
>    * A bunch of bug fixes that fell out as part of landing all these features including better KBI handling in contexts, better graceful daemon-actor cancellation, fixes to the broadcast channel `trionics` implementation to correctly terminate on underlying stream closure.