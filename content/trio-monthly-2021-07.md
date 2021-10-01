---
title: Trio Monthly - 2021/07
date: 2021-08-01T00:00:00Z
draft: false
tags:
 - trio-monthly
 - trio
---

Welcome to the July 2021 edition of Trio Monthly. This is a monthly blog post dedicated to 
highlighting what's going on in the Trio world, from developments in the core library to 
developments in new and existing libraries and application powered by Trio.

If you wish to get your library featured in a Trio Monthly post, please contact the author on Matrix
(``@lura:veriny.tf``).

## Trio Library

### Cyclic Garbage Fixes

Trio had another source of cyclic garbage eliminated this month, in 
[PR #2063](https://github.com/python-trio/trio/pull/2063).

> Trio now avoids creating cyclic garbage when a `MultiError` is generated and filtered,
> including invisibly within the cancellation system.  This means errors raised
> through nurseries and cancel scopes should result in less GC latency.

Trio has to do low-level things when it comes to frames, tracebacks, and exceptions, which sometimes
results in cyclic references being left around. These aren't picked up by the regular Python
reference count garbage collector, but by the cyclic detector instead, causing potential GC pressure
and application slowdowns. Eliminating these cycles allows the objects to be freed much sooner,
improving the speed and memory usage of all Trio applications.

## External Libraries

We have two libraries to feature this month.

### trio-parallel

The first of this is the ``trio-parallel`` library. As the author puts it:

> ``trio-parallel`` is a simple CPU-parallelism library for Trio. This month, an advanced subprocess 
> worker customization API, the ``cache_scope`` context manager, was merged, allowing choice of 
> initialization, timeout, and retirement policies, as well as defining a scoped lifetime for an 
> isolated, local worker cache. This feature has been pre-released in version 1.0.0a0 available on 
> PyPI for wider testing for a final 1.0 release later this year. Please see 
> https://trio-parallel.rtfd.io/en/1.0.0a0/ for details.

### Tractor

The second library featured is the Tractor library. As the author puts it:

> For those who haven't heard of us, ``tractor`` is a multi-processing-runtime library which applies
> structured concurrency from the ground up on process managment and IPC. We had our first alpha 
> release this past Feb. and now our second this Aug. 1st 🥳.
>
> The quick and dirty summary for this release is:
> - Updated our unidirectional streaming APIs to be more `async_generator.aclosing()`-like
> - Vastly improved our actor reaping during cancellation and error propagation for the 
>   `multiprocessing` spawner backend
> - Added a next-gen, fully bi-directional streaming API with complete, transitive SC semantics 
>   for IPC 🏄🏼‍♀️
> - Hardened our multi-process debugger support (ala `pdb++`) to prevent root process clobbers of 
>   children who have acquired the repl tty lock; this is thanks to our new ``tractor.Context`` 
>   cross-actor-task syncing protocol.
> - Started some tinkering with `msgspec` for an optional alternative to `msgpack-python`.
> - Improved our infected-`asyncio`-in-guest-mode cross-loop streaming semantics by simplifying to 
>   memory channel only style data passing between frameworks.
> - Added a ton of new examples to both the readme and repository  script set.
> - Removed both `tractor.run()` and support for invoking sync functions; instead we recommend users 
>   try `trio-parallel`!
>
> For more details see our [new release](https://github.com/goodboy/tractor/blob/master/NEWS.rst).

## Conclusion

That's all we have for this month. Onto August! 