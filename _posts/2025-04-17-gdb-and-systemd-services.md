---
layout: post
title: "GDB and systemd services"
tags: [linux, gdb, systemd, debugging]
---

*Originally published on [Medium](https://medium.com/@shravansingh64/gdb-and-service-file-807c9ae64955).*

Recently I had a bug in a program that made me realize the importance
of gdb. Usually you can attach gdb directly to the program you want to
debug. In this case it was a binary that kept crashing:

    gdb ./<application_name>

Then on the gdb console, run the application:

    (gdb) run

If we hit an issue, we can backtrace:

    (gdb) bt

The problem I was facing, however, only occurred while running the
application from a systemd service. So I needed to attach gdb to the
process spawned by the service instead.

To do that, I created a gdb configuration file named
`gdb-configuration.conf` with the following parameters:

    set target-async on
    set pagination off
    set non-stop on
    set logging on

I like to enable logging so I can share the gdb dump with colleagues —
it often helps solve the problem a little quicker. By default the log
file is named `gdb.txt` and is stored in `/home/root`.

With the configuration file in place, I start gdb with it:

    gdb -x gdb-configuration.conf

And attach to the running process:

    (gdb) !pgrep <application_name>
    <pid>
    (gdb) attach <pid>
    (gdb) continue -a &

I used `continue` to get past some non-threatening warnings that were
acting like breakpoints.

That's it — this should help you attach gdb to a process spawned by a
systemd service.
