optionsbleed ++
============

This is a proof of concept code to test for the
[Optionsbleed](https://blog.fuzzing-project.org/60-Optionsbleed-HTTP-OPTIONS-method-can-leak-Apaches-server-memory.html)
bug in Apache httpd (CVE-2017-9798).

Modified by MickCue: Uses requests and supports cookies to test authenticated URLs. 

usage
=====

<pre>
optionsbleed [-h] [-n N] [-a] [-u] hosttocheck

Check for the Optionsbleed vulnerability (CVE-2017-9798).

positional arguments:
  hosttocheck  The hostname you want to test against

optional arguments:
  -h, --help    show this help message and exit
  -n N          number of tests (default 10)
  -a, --all     show headers from hosts without problems
  -u, --url     pass URL instead of hostname
  -c, --cookie  pass cookie value e.g. -c Session=53454bfgb
  -m, --methods test for malicious HTTP methods

Tests server for Optionsbleed bug and other bugs in the allow header.
Tests for HTTP methods
Tests for clickjacking

</pre>

output explanation
==================

The script outputs the returned Allow header and prefixes it with
an explanation what is wrong with it.

[bleed]
-------

A corrupted header was sent, very likely the Optionsbleed bug.

[empty]
-------

An empty header was sent, which is bogus.

[spaces]
--------

The list of methods was space-separated instead of comma-separated.

This is a bug, but harmless. It may be
[Launchpad bug #1717682](https://bugs.launchpad.net/launchpad/+bug/1717682).

[duplicates]
------------

Some methods were sent multiple times in the list.

This is a bug, but harmless. It may be
[Apache bug #61207](https://bz.apache.org/bugzilla/show_bug.cgi?id=61207).

[ok]
----

Nothing wrong with this header (only sent with -a/--all).
