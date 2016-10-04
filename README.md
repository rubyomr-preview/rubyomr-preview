# Introducing the "Ruby + OMR Technology Preview"

This technology preview showcases how the [Eclipse OMR project](https://github.com/eclipse/omr) can be integrated
into the Ruby VM. To do this work, we have now created [Ruby + OMR](https://github.com/rubyomr-preview/ruby) as a
[Ruby](https://github.com/ruby/ruby) fork. Our intention is to use this fork to contribute Ruby + OMR code changes
back into Ruby.

**Our current Ruby + OMR changes are based on Ruby 2.2 and located in the [ruby_2_2_omr](https://github.com/rubyomr-preview/ruby/tree/ruby_2_2_omr)
branch.** You can build everything from source, or, use the docker images below. 

Going forward, we will be working in this fork to update to the master branch with the goal of submitting a pull request
against Ruby. We are open to feedback at any point on how best to structure the changes we've made or on how to
best propose these changes into Ruby.

Because we have used some components in the Ruby + OMR Technology Preview that have not yet been open sourced via
the Eclipse OMR project (for example, the Ruby JIT), we cannot yet include souce code changes for these components.
But you can continue to try them out via our docker images in this project, and we welcome your feedback!

Our sincerest hope is that this preview helps us to work with the existing Ruby community to integrate
those parts of Eclipse OMR that the Ruby community finds beneficial.

Our lawyers are keen to tell you that any reference to the Invoice in the License means that you are authorised to
use one copy of the downloaded image.

If you can't wait to get started, you can go directly to the <a href="#quickstartguide">Quick Start Guide</a>

Otherwise, you may be wondering…

# What is “OMR” ?

The [Eclipse OMR project](https://github.com/eclipse/omr) is an open source community growing around
reusable and easily consumable core components (like thread library, platform abstraction library, garbage
collector, etc.) for building all kinds of language runtimes, from Java to Ruby to Smalltalk and beyond.
Many of the components were contributed by IBM from the IBM J9 Java Virtual Machine (JVM), an enterprise
class JVM implementation representing hundreds of person years of development creating scalable, high
performance runtime technology. But going forward, this open project is accepting contributions
from anyone.

The OMR components have 3 features that distinguish the Eclipse OMR project from other projects that aim to
reuse technology for building language runtimes: 1) OMR has no language semantics, 2) OMR is not a
language runtime, and 3) OMR components can be consumed by any language runtime. The first two features
distinguish OMR from efforts to implement languages on top of a mature language runtime like the Java Virtual
Machine (by implementing in Java) or the CoreCLR project (by implementing via CLR bytecodes). Both
these other efforts require mapping a language's semantics into the semantics of another language (either
Java or CLR). In contrast, OMR components can be brought into an existing language runtime with its
own bytecode set and bytecode semantics (or abstract syntax tree semantics). OMR can therefore be
used to build any kind of language runtime, free from the influence of Java or another language's semantics.

One of the most important motivators for the Eclipse OMR project is the increasing importance of cloud computing
platforms, where the polyglot of programming languages must all cooperate seamlessly in what can really
be a diverse environment. Language runtimes must be monitored and managed adaptively to adjust to the
current operating conditions in the cloud platform. But not all runtimes have the same kinds of
responsiveness built in because most language runtimes today actually share very little technology.
can hurt or even sabotage longer term success. But every language runtime community needs to go through
this process, and as existing runtimes become more and more sophisticated, the path to success for new
languages becomes even more difficult.

By introducing shareable core technology components for building all kinds of language runtimes,
the Eclipse OMR project hopes to both make the process easier for new languages to bootstrap
themselves as well as to provide mechanisms for existing runtimes to fill gaps or to reduce
maintenance costs so that communities can focus more of their efforts on the opportunities and
problems that are specific to their language.

To learn more about the Eclipse OMR project, please visit us at [the Eclipse OMR Github page](https://github.com/eclipse/omr).

# So what’s in this Ruby + OMR preview?

To make sure the Eclipse OMR technology really could work in runtimes other than Java, the OMR team has
been working to create several proof points to use the technology with different language
runtimes (like Ruby!). This technology preview release represents the current state of our Ruby proof
point, with new GC, JIT compiler, and method profiling capabilities. It's not a toy: we've got it
running Rails applications. We cannot claim it is ready for use in production, but we think it's good
enough to be able to meet the goals outlined at the beginning of this document.

We presented talks about our Ruby + OMR proof point at Ruby Kaigi 2015:

* Matthew Gaudet at Ruby Kaigi:
  [Experiments in sharing Java VM technology with CRuby](http://www.slideshare.net/MatthewGaudet/experiments-in-sharing-java-vm-technology-with-cruby)

* Robert Young and Craig Lehmann at Ruby Kaigi:
  [It's dangerous to GC alone. Take this!](http://www.slideshare.net/craiglehmann/the-omr-gc-talk-ruby-kaigi-2015)

# Building Ruby + OMR

The Eclipse OMR project strives to make it very easy to integrate components into a language runtime.

The Ruby VM needed the following high level sets of changes to incorporate the OMR technology:
1. configure.in changed to add OMRDIR and OMRGLUE options which default to assume omr in ruby's top directory
2. makefile.in changed to configure, build, and clean up OMR and OMR Ruby Glue under appropriate targets
3. makefile.in changed to build and link against OMR and Ruby glue objects and static libraries
4. various changes to files to invoke the methods in the Ruby glue and OMR project as appropriate

Once these changes were made, the Ruby + OMR Technology Preview release can be built via:
```
$ git clone https://github.com/rubyomr-preview/ruby.git --branch ruby_2_2_omr --recursive 
$ cd ruby
$ autoconf
$ ./configure SPEC=<specname> --with-omr-jit
$ make
$ make install
```
Since the Ruby + OMR code has only been tested on Linux x86-64, Linux PPC-LE-64, Linux PPC-BE-64
and Linux 390-64 the acceptable values for `<specname>` are:
```
1. linux_x86-64
2. linux_ppc-64_le_gcc
3. linux_ppc-64
4. linux_390-64
```

Building with this simple sequence of commands will download the latest version of the Ruby + OMR Technology
Preview and Eclipse OMR, build them together and install them onto your system. That's it!

There are more configuration options than the simple set listed above, which is just a high level
description. Search for "OMR" and hopefully you'll find the tags we used self explanatory. If not,
feel free to ask questions!

To make it even easier for people to try out the Ruby + OMR Technology Preview without having to clone
repositories or build, we also have three docker images available that come with the Ruby + OMR Technology
Preview already built and pre-installed. Docker images make it easy to avoid fussing with platform
specifics.  The images have a preinstalled Ruby 2.2.x with built-in OMR technology. Also included is
a monitoring agent which can be used with IBM Health Centre to visualize Ruby method profiles and garbage
collection performance while your Ruby application is running.

The OMR technology itself is included only as part of the prebuilt binaries which you can access by
simply running ruby (/usr/local/bin/ruby). Unfortunately, not all of the OMR project is available in
the open (we're working on it *really* hard). The real place to look for the most up to date source
code is at [The Ruby + Eclipse OMR Ruby repo](https://github.com/rubyomr-preview/ruby).

Not all of the OMR technologies are active by default in this version of Ruby.  Environment variables
activate those technologies that are not on by default, like the method profiling support when you
connect to ruby with IBM Health Center (also included in the docker image: see the User's Guide!) or to
turn the JIT compiler on. For example, to activate the JIT compiler technology, you'll need to set
`OMR_JIT_OPTIONS="-Xjit"` which turns on the JIT where it will compile methods that are invoked more than
1000 times. `OMR_JIT_OPTIONS="-Xjit:count=N"` adjusts the invocation count before JIT compilation so that
you can play around a bit.

For full details on how to activate each OMR component and the various configuration options available
in this technology preview, please see the
[User's Guide in the Wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki) !

<a name="quickstartguide"></a>
# Quick Start Guide

The following files are in this project:

* This README.md file
* A LICENSE directory describing in excruciating detail just how little you can
  rely on in this technology preview

You can also find the [User's Guide in our wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki).

To start using the Ruby + OMR Technology Preview, please follow the directions for the platform you're using: Linux on X86,
Linux on Z, or Linux on OpenPOWER. The Linux X86 Docker image has been updated to the latest version and we will be updating the Linux on Z and Linux on OpenPOWER soon.

## Linux on X86, 64-bit

1. If you do not already have docker installed, follow these directions to get started:

   [Installing Docker For Linux on x86](http://docs.docker.com/engine/installation/)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/sy7pgu7pqbyht9j3g3tvko77ska1k8x1.tgz -O rubyomrpreview-x86_64.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-x86_64.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby + OMR Technology Preview:

        root@d2ae8cf89313:/# ruby --version
        ruby 2.2.5p285 (Eclipse OMR Preview r1) (2016-03-29) [x86_64-linux]

6. Play to your heart's content!

## Linux on Z, 64-bit

1. If you do not already have docker installed, follow these directions to get started:

   [Installing Docker For Linux on Z](http://www.ibm.com/developerworks/linux/linux390/docker.html)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/1bzikt7fmfdejpvp4zqglnss64tcwls2.tgz -O rubyomrpreview-s390x.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-s390x.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby + OMR Technology Preview:

        bash-4.2# ruby --version
        ruby 2.2.3p97 (OMR Preview r1)(2015-04-14) [s390x-linux]

6. Play to your heart's content!

## Linux on OpenPOWER, 64-bit

   [Installing Docker for Linux on OpenPOWER](https://www.ibm.com/developerworks/library/l-docker/)

2. Download the rubyomrpreview docker image from Box.com with the command:

        $ wget https://ibm.box.com/shared/static/hdqgfvvnnq1idf6qvlsv9r0rw5dzwbhc.tgz -O rubyomrpreview-powerpc64le.tgz

3. Load the docker image locally:

        $ docker load -i rubyomrpreview-powerpc64le.tgz

4. Run the docker image (you can omit the -p 1883:1883 if you won't be using Health Centre or if you won't
   be running the message broker--see the [User's Guide](https://github.com/rubyomr-preview/rubyomr-preview/wiki)-- inside the container):

        $ docker run -p 1883:1883 -it rubyomrpreview/rubyomrpreview /bin/bash

5. Verify you can successfully run Ruby + OMR Technology Preview:

        root@7305793b79e9:/# ruby --version
        ruby 2.2.3p97 (OMR Preview r1) (2015-04-14) [powerpc64le-linux]

6. Play to your heart's content!

To see how to use the various OMR technologies, please look for Tracing,
Garbage Collector, and Just In Time (JIT) compiler sections in the
[User’s Guide in the Wiki](https://github.com/rubyomr-preview/rubyomr-preview/wiki) !

# We would love to hear your feedback!

As we said above, we’re making this technology preview openly available so that everyone can
try it out and let us know what's good and what's not so good.  We welcome all feedback!

We’re excited to finally get into the open with this project, and look forward to hearing what you
think!

We’re going to use the issue tracking associated with the rubyomr-preview github project to track the
feedback, so if you’d like to tell or ask us anything,
[please open an issue](https://github.com/rubyomr-preview/rubyomr-preview/issues)
