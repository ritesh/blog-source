---
date: 2015-01-14T12:19:54Z
categories: [bsdiff, go, linux]
summary: If you're trying to deploy a large-ish binary file somewhere, you might find
  this useful
title: Chunky binaries and bsdiff
url: /2015/01/14/chunky-binaries-and-bsdiff/
---

I've lately taken an interest in golang as it seems to be the language _du jour_ for web-services. My goto (hah) framework for writing tiny web applications is usually Flask (Python), which is still great for many things. One of my biggest gripes with Flask, and Python web applications in general, is the approach towards deploying 3rd party dependencies along with your application. Go's approach to package management is a bit odd coming from Python-land, but it has the advantage of ensuring that your application doesn't require 3rd party packages installed at runtime. Oh and static typing and all that is cool, too. 

If you're not familiar with go, I highly recommend that you check out [A Tour of Go](https://tour.golang.org/welcome/1) and work your way through some of the examples to get a feel for it. One of the key features of go (and the one feature I care most about) is that your applications are native executable files that have dependencies baked-in and, in the case of web applications, come with an in-built HTTP server, instead of a myriad of scripts, configuration files etc. You can still chose to have configuration files for your application at runtime (e.g. a settings.json file), or set up environment variables as needed. This makes shipping your web application as easy as:

1. Compile your application (`go build`). You may need to cross-compile if you're deploying to a different architecture (e.g. x86-64). Cross-compiling is not as difficult as it sounds. 
2. Copy the generated binary over to the server that you want to run it. If you want, you could strip the binary to shave off a few MB.
3. Run it :)
4. Set up a reverse proxy to forward all traffic to your application. You only need to do this once.
5. There is no step 5. 

Contrast this with my current workflow for deploying Python web applications:

1. Ensure Python versions match on dev and prod. This can be a nightmare sometimes, but is avoidable by ensuring that dev and production match as closely as possible (use the same version of Ubuntu/Fedora/Debian). Not an issue if your dev box runs the same flavour of Linux as uat/prod.
2. Ensure that native libraries required by Python packages are available on both dev and production hosts (I'm looking at you `libpq`).
3. Have a well defined fabfile for deployments, or similar. This is not really a complaint, I think `fab deploy` sounds hilarious when said out loud and is also a nice way to deploy Python web applications.
4. Restart apache/nginx so that it reloads your WSGI application. I think you can `touch` a file and `mod_wsgi` reloads your application, but this has never worked reliably for me.
5. The way to set up debug logs for Python applications deployed with `mod_wsgi` is not straightforward. I gave up after spending a few hours trying, this shouldn't be that difficult! I'm stuck with trying to reproduce production issues on my dev box. 

Executable files are all well and good but copying them over to production hosts multiple times a day, usually over SSH, can be slow if you're on crappy DSL. If I need to do this for servers sitting inside my company's network, I have to ship files inside an SSH tunnel _wrapped_ inside a VPN connection. This makes it unbearably slow for files larger than 10MB. Why can't you just diff a binary file and upload _only_ the changed bits? Surely this is possible?

This is when I discovered `bsdiff` and its friend `bspatch`. I guess I'm fairly late to this party, since `bsdiff` has been around since the early 2000s (or earlier?). This pair of amazing utilities was written by the Colin Percival of [Tarsnap](http://tarsnap.com) fame. I understand that lots of binary applications use either `bsdiff` or an application that uses a similar algorithm to generate patches for binary applications.

The clue is in the name; `bsdiff` lets you `diff` an old binary with a new binary and generates a patch for you. You take this patch, give it to `bspatch` along with an old binary and it gets transformed to the new one. If the changes between the binaries are small, the generated patch file (as you'd think) is tiny. On average, the patch file is 2-3% of the full binary for the application I'm working on. Instead of copying over the binary to a production host once I've compiled my application, I copy the patch instead:

{{< highlight bash >}}
mv binary binary.old
go build
bsdiff binary.old binary binary_patch
scp binary_patch /path/to/production/binary

# On production host
mv binary binary.old
bspatch binary.old binary binary_patch
# Need to do this because the binary loses +x 
chmod +x binary
# All done
{{< / highlight >}}

Your production host now has the latest version of your binary. I have not tested it on anything else but binaries generated by `go build`, but there's no reason why it shouldn't work for binaries generated by other compilers.My next experiment will be to figure out how to reload the application after it receives a `SIGHUP`. Go gives you `"os/signal"` but I haven't had the chance to play around with it yet.
