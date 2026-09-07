---
layout: post
title: "Unix Signal Handlers in Qt"
tags: [qt, linux, unix-signals, systemd, embedded]
---

*Originally published on [Medium](https://medium.com/@shravansingh64/unix-signal-handlers-in-qt-b3ae003164d2).*

Recently I came across this issue.

I had a Qt application running on embedded Linux, and a separate
binary, communicating with each other over a virtual serial port.

![Two applications communicating over a virtual serial port](/assets/images/qt-signals-architecture.png)

The idea was that both applications should be independent of one
another: if the Qt application goes down, the other binary shouldn't be
affected, and vice versa.

This was all working fine — until I created a service file to start my
Qt application. Now, whenever I lost the virtual port, my Qt
application would crash, causing the service to restart it.

The question I had: why does this only happen when the application runs
from a service?

The answer: systemd supervises every process it starts. If a process
does not handle a signal, the default action kicks in and the process
is terminated — and systemd then reacts to that according to the
service configuration. Running from an interactive shell doesn't behave
the same way.

This is where I came across Unix signal handlers and how to use them in
Qt. Qt's own documentation covers the pattern:
[Calling Qt Functions From Unix Signal Handlers](https://doc.qt.io/qt-6/unix-signals.html).

But before using that recipe, we need to understand two things:

1. What is a socket pair?
2. What is sigaction?

## Socket pair

`socketpair()` creates two connected, unnamed sockets in one call. The
two ends are identical — anything written into one can be read from the
other — and their file descriptors are returned in a two-element array.
That's what makes it perfect for this pattern: the signal handler
writes one byte into one end, and a `QSocketNotifier` watching the
other end wakes up the Qt event loop safely.

It takes four arguments:

- **domain** — the communication domain. In our case `AF_UNIX`,
  meaning local to the host.
- **type** — the socket type. Here `SOCK_STREAM`, a stream socket.
- **protocol** — a specific protocol to use; `0` selects the default
  protocol for the socket type, which is what we want.
- **socket_vector** — the 2-integer array that receives the two file
  descriptors.

On success it returns 0; otherwise -1 with `errno` set.

## Sigaction

The `sigaction()` system call changes the action a process takes on
receipt of a specific signal.

For our case, the only question I had was about
`sigaction(SIGHUP, &hup, 0)` — why pass 0 as the third argument? The
third argument is an *output* pointer for the previous action; passing
0 simply means we don't need the old handler back.

`SA_RESTART` provides behavior compatible with BSD signal semantics by
making certain system calls restartable across signals — see
[signal(7)](https://man7.org/linux/man-pages/man7/signal.7.html) for
the details on system call restarting.

Once you know this, the code in the Qt documentation looks pretty
straightforward.

## References

1. [POSIX socketpair()](https://pubs.opengroup.org/onlinepubs/7908799/xns/socketpair.html)
2. [socketpair man page](https://manpage.me/?socketpair=)
3. [OpenBSD signal.h](https://github.com/openbsd/src/blob/master/sys/sys/signal.h)
4. [sigaction(2)](https://man7.org/linux/man-pages/man2/sigaction.2.html)
5. [POSIX sigaction](https://pubs.opengroup.org/onlinepubs/007904875/functions/sigaction.html)
6. [POSIX signal.h](https://pubs.opengroup.org/onlinepubs/007904875/basedefs/signal.h.html)
7. [OpenBSD socket.h](https://github.com/openbsd/src/blob/master/sys/sys/socket.h)
