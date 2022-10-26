# 🐢 Rolling Alice...

- https://github.com/intel/dffml/pull/1401

![hole-rabbit-hole](https://user-images.githubusercontent.com/5950433/196436807-68881b75-2006-4734-b4a2-63dc3d17b634.gif)

## Rolling Alice: Progress Report 1: Where are we

> https://www.youtube.com/watch?v=dI1oGv7K21A&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

Okay, so we are going to do our series of tutorials on building Alice, our
software architect. And so as a part of that we're going to do a bunch of
engineering log videos, just the same as we do the weekly sync stuff. So this is
going to be not like the weekly sync where we do, you know, it's more of like an
office hours. This is more of, you know, whoever's working on something will
stream what they're working on so that there's, you know, basically like you can
understand their thought process and we'll have, you know, basically as detailed
as it gets logs of how we built this thing. So that's the plan. This is going to
be the first video. So we're going to do this tutorial series. Basically we're
going to do, so 12 chapters in 12 months. And yeah, and by the end of it, what
we're hoping to do is have this AI driven software architect and her name will
be Alice, like Alice in Adventures in Wonderland. And that's in the public
domain so we can riff all over that. And it's great. I love it. So we're going
to build this tutorial series. We're going to build this AI. You know, hopefully
she can help us maintain our projects, right? Just doing more of what we always
do. So writing more test cases and in effect, you know, our test cases are just
going to be this AI driven pile of CI jobs, right? We will, for instance, you
know, we've been talking about the second party plugins and that third party
plugins for a while. So basically as we, we are finally splitting that stuff
out. And as we do it, we're going to need help, sort of. Because we're going to
end up with this situation where we've got this like poly repo setup and we
don't have access to all the repos and so there's different maintainers. And
these are the third party people and then we have our second party people, which
is our org. And you know, the whole thing is going to be chaos, right? And so,
well, you know, kind of look up Alice in Wonderland, it says it's this genre of
like nonsense literature. It's like chaos. There's chaos everywhere. So as we
know, so is software development and so is life in general and so is everything,
right? So, Alice is going to help us make sense of that chaos. She is going to
rely on data flows and machine learning models and hopefully some web3 stuff,
which we'll cover in our first, you know, few videos of context here. We're
going to lay some groundwork with that. So, essentially, if you're familiar with
like KVM nested virtualization, then this is going to be a familiar concept to
you. But we're going to sort of do the simplest approach to this first. The
simplest approach and something that we can sort of like massively parallelize.
Just so if we get more compute, you know, then we can use more compute. So
great. Okay. So and what are we going to build? Okay. So what's the first thing?
Our first target is to have Alice have an understanding of an individual code
base. So once we do that, we're basically going to do an extension of should I.
So there's a bunch of should I tutorials up there. Ties in with the building
operations and stuff. The way this sort of went is, you know, the initial
project rolled out. We did this automated classification demo.


## Rolling Alice: Progress Report 3: Down the Dependency Rabbit Hole

> https://www.youtube.com/watch?v=D9puJiKKKS8&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

Hello everybody. We're going to get started. I'd like to introduce you to John
Anderson. John is on the open source security team at Intel. He's from Portland.
Went to PSU for computer engineering with a focus on embedded systems and did
his honors college thesis on machine learning. He's been working at Intel as an
intern, then an employee for the past five years. Security research, embedded
systems, machine learning, and data flow programming are his current interests.
Let's welcome John. Thanks guys. Thanks for showing up. So I'm going to talk to
you a little bit today about Intel's dependency review process and how we
automated some of it using machine learning. So who am I? I'm John. As was said,
I'm an open source security guy at Intel. So I've been playing around a lot the
past few years with Linux, containers, concurrency, web apps, and Python and
machine learning a little bit more recently. So while I was at PSU doing my
undergraduate honors thesis, I did it on machine learning. And at the same time,
I was interning at Intel, and so I got the chance to work on this project where
I got to apply some of the stuff I was learning about machine learning to a real
application. So at Intel, we have a dependency review process that is part of
the release process for any given piece of software. And so the software at
Intel goes through this whole security process, right? And before you can
release, you have to make sure that none of your dependencies end up on this ban
list because a lot of software out there can be pretty bad, especially if you're
just Googling for something and you find it on GitHub and it says it does AES
and it's half the size of OpenSSL. And if you went to Matt's talk yesterday, you
might know why that is a bad thing, and you should not use that. So we have this
review form. It has a bunch of...you can't really see it, and this is not the
exact same one that is the internal one. This is one that went through legal
review. So it's got a bunch of little A through F answers for each category,
looking at things like maintenance and security testing and testing in general.
And so what we were doing is we as the open source security team were put in
charge of reviewing all packages for anybody's dependencies for all of Intel.
And so Intel writes a lot of software, and so we were reviewing thousands of
dependencies of open source software. And so we thought, well, it would be great
if we could automate this because we have other jobs like real things to do. And
we don't want to be sitting there looking at open source packages all day. It's
pretty mind-numbing. So we took this dataset of the form answers, and John
Whiteman and I took the same approach at this the first step around. And so we
grabbed this dataset, we got the URLs to all these Git repos, and we have the
review forms, right? And then we have the classification. So this should be a
pretty straightforward problem, right? We should be able to take this form and
then map it to the classification, train a machine learning model on it, and it
should tell us, yes, this is good, or no, this is bad, based on our trained
classifications. But we ran into a problem pretty much right off the bat. And
that was that reviewers weren't consistently filling out these forms. And, yeah,
big surprise, right? So we're charged with doing this mind-numbing task that
takes a lot of time, and we're really excited to go click every single dropdown.
So part of the reason for this was that first, right off the bat, you could
usually tell, or not usually, but you could sometimes tell whether something was
good or bad right away, right? So signs like, you know, has this thing written
in 2005 and never touched since, does it implement some crypto in a very
non-standard way that one guy wrote and no one else has looked at, is it a
parser? And so all of these things are early warning signs that we might just go
bad, right? You cannot use this thing. You may not ship with this software.
There are also signs of things being good, right? You know, this is system D.
Well, we may not like system D, but you can use it. We know that people are
maintaining it. So this was a failure. We only got around 60% accuracy. So we
had to go back to the drawing board and think about, well, what is our approach,
right? So if we don't have the data in the form, then we're going to have to
generate a new data set, right? And so we're going to, instead of using that
form data, we're going to generate some answers that are sort of like that form
data, right? We don't know what they're going to be yet, but we know that
there's some data that when you put it through a model, you will get something
that tells you this is good or this is bad at a high percentage accuracy.
Because after reviewing all these packages, we know that there were some things
that we could put into numbers. We just didn't quite know what they were, right?
It must be possible. So we took this iterative approach where we're going to
generate this data set, train the model. If the model gives low accuracy, that
means we have the wrong data set, right? So go back and grab some different
features. So we came up with this plug-in based system, right, where we're going
to scrape these different pieces of data that we're using as the features for
the model. And that way we can quickly swap in and out these features as we find
out that this one doesn't matter or this one does matter. And it will allow us
to, the idea is that we can swap out these features very quickly and so that we
can then find which ones matter, which ones don't. And ideally we have a
simplistic framework to do this because if I just want to count the number of
authors, that should be as simple as get log, grep authors, sort unique, right?
So that's very easy. But the problem comes into play when you start doing these,
you start running multiple of these at once and they're all running Git. And
well, Git apparently actually does some weird lock files on repos, so you can't
actually run Git concurrently. So if you want to scrape all of these repos and
iterate on this, right, then you need this regeneration process for this data
set to be fast, right? Because I can't sit here and spend two days regenerating
my data set and then go back and find out that no, that wasn't the right set of
features. That's going to be a huge waste of my time. So we needed to come up
with something that gave us a little more flexibility around like how do we
write these data scraping plugins without needing to know too much about the
other things that are scraping data as well. So we came up with this framework
that's basically like this, it's like a directed graph of operations that get
run concurrently while the execution framework deals with locking. So we define
the data types and we say, you know, this data type needs to be locked before
it's used in an operation or it doesn't need to be locked. So the Git repo, for
example, is a data type that needs to be locked. Once I've parsed some of that
data, I can throw that data back into this network of running operations and I
can now run things concurrently or if it's just random data, we can run it in
parallel, right? We don't want to be running things that are CPU bound
concurrently. And then that releases the resource lock for the other operations
that actually do need to be working on the locked resource, like other things
that need to go actively scrape the Git log or whatever it is. So this is the
directed graph showing how we would do a calculation of the number of lines of
comments to the number of lines of code ratio, which was a rough estimation for
how well documented is this code. And so what you can see here is we take the
URL of the Git repo, we clone it, and then we're going to go find what the
default branch is unless it was provided. So, you know, we want to detect if the
default branch is master or maybe it's like, you know, 5.3 point something. And
then we're going to combine that. The other thing that was happening here is
that we were doing this on a quarterly basis, so we're doing time series
information because as you can imagine, the maintenance of Git repos and the
other properties of them are all varying over time, right? So somebody may start
out doing co-verity or somebody may not start out having their thing in public
co-verity and then they put it in public co-verity, right? So quarter by
quarter, these data points change. So we have a set of operations that goes and
takes a quarter start date and goes back, you know, X number of quarters and
then generates what are the commits associated with those dates for those
various quarters. And since this is all running concurrently, part of also what
happens here is all the possible permutations of every input set for any given
function, which is also known as an operation, gets run. So as soon as you
generate all of those quarter dates, it's going to go and dispatch all of these
operations to run concurrently and it's going to go say, okay, go grab with that
default branch that you did and that quarter date. I want you to go and for
each, then it's going to say for each of these quarter ranges, I'm going to go
scrape that or I'm going to go check out that repo and then run this clock or
basically we ran clock and then we did this division to find the ratio. Right.
So this is how this execution environment works. And we are, we can, the nice
thing about this is it runs everything concurrently, manages the locking and it
does this permutations of inputs so you get every unique permutation gets run.
So when we went to go, we go to plug this back, come back to like the main
framework here, right? We've got this system for doing plugins on the data set
generation and we also came up with a system for doing plugins on the neural,
the models, the machine learning models, as well as a system for doing plugins
on the data sources. Right. So now we can iterate, we can iteratively swap out
the data generation pieces and then we can swap out the model, the model portion
so we can go and say, you know, am I using scikit, which is what we started with
because scikit will tell you what are the feature importances of each of the
pieces of data you're putting into it. So it might say, hey, you know, you gave
me this ratio of how many comments per poll or how are there a lot of comments
on every poll request or you know, what is the diversity of authorship? Like how
is, is each line of code committed by a different person? And so if you throw
that through the random forest classifier, it has this nice property being like
this classical machine learning algorithm where it can tell you that which
pieces of feature data were important in getting a good accuracy on the
prediction. So that helped us throw out these things, right? But then once we
got to a reasonable amount of accuracy, around 80%, I think it was like 82%, we
said, okay, well, we want to go switch this to a better machine learning model,
something that hasn't been around for many, many years. So let's try doing some
neural networks through TensorFlow. And so since we've got this plugin
framework, we basically say generate the data. Now instead of doing through the
scikit model, do through the TensorFlow model, it's just a command line flag to
say scikit to TensorFlow. And then now we're training using TensorFlow and we
get 90% accuracy. So that's pretty good enough right there. So we're just going
to go and start using this 90% accuracy to drop the ban hammer on things as
people submit them so they don't have to wait for this four to six week
turnaround to hear that they need to go change all their code because they used
about dependency. So, and then, like I was saying, we also swapped in and out
the data sources. So when we went to go take this into the production
environment, the production environment had this wacky MySQL setup. So we, you
know, we created this custom data source that knows how to interact with all
these various MySQL tables, which is, you know, probably the case if you're
going to integrate with an existing application, like you don't know what the
database is going to be like. Or well, you do know what the database is going to
be like, and it's probably going to be custom logic, right? So you're going to
go need to figure out how to integrate it with that custom logic. So you write a
plugin. So this is what it looked like once it got integrated, it's pulling and
pushing, pushing the data from that production database. And it, you know, you
put in the URL, it runs the data set generation, and then it runs the prediction
through the trained model. And it says, yes, I think this is a good thing or no,
I think this is a bad thing. So what's behind all this? Well, we've got this
wonderfully named library called Dataflow Facilitator for Machine Learning. And
that legal gave me this name. So it's long and it's generic and descriptive. So
what does this thing do, right? It provides the abstractions around sources and
around data set generation and around models. And we've got a few baked in for
you. But you also, it's the plugin based system does not require that you write
a plugin that gets contributed back into the main source code of the repo.
Right. So people can publish plugins and then just like have them on PyPy or
have them in their Git repos and you can install them from people's random
source and it will work as a part of the existing ecosystem. So we've got
sources for CSV files, JSONs, MySQL, and we've also got some models for
TensorFlow and Scikit to do a few things there. We provide a consistent API
across the command line interface, the library, the HTTP API, and then by
extension, the JavaScript API that works with the HTTP API. This is just like a
bunch of console log from doing all the same things we were doing in Python.
We're now doing from JavaScript hitting the HTTP API. So then you have access to
all these plugins, you know, you install somebody's random plugin. And since it
all fits in the framework, it knows how to extrapolate all the data you need to
go configure that model or whatever and display it to you through this other
API. So what else have we done with this thing? Well, we wrote this meta static
analysis tool for Python called Should I. And if you're familiar with Python or
basically any package manager, you usually type, you know, pip install or
apt-get install blank. Right. So we created this thing that goes and it
downloads the PyPy package that you were about to install and it runs some
static analysis tools on it. And right now there's only two static analysis
tools that it runs, but there can be lots more because we have this system where
we're using these operations and the amount of code that it takes to add a new
plugin to analyze the source code is basically like, I think it's like 20 lines
if it's auto formatted with this thing that makes it really long. So we take the
package name, we go grab the JSON information from the PyPy API. This is the
data flow for that. Right. And so then we extract the URL, we extract the
version, we download the package contents and we're running Bandit, which is
like a tool that does source code analysis to look for things like SQL
injections and stuff in your Python code. And then we also run this other tool
called Safety, which goes and checks for like open CVDs or known vulnerabilities
and packages. And so all of this runs concurrently. And then when you're
scheduling out to sub processes, because it's back to an async IO, the nice
thing that will happen here is that when you call it to a sub process, it
automatically gets run in parallel. So you're getting concurrency and some
parallelism for free. And also, if you're doing CPU bound tasks, it's really
easy to just say, hey, you know, schedule this in a thread and the orchestrator
will go do that for you. So we've also got this concept where you can take these
data flows, right, since we've abstracted to this level, we can take these data
flows and we can deploy them in different kinds of environments. Right. We could
deploy this as a command line application like we did, where we say, you know,
should I install this thing? It goes, runs them and gives you the results. Or we
could deploy this behind like an HTTP API. You know, coincidentally, the same
one that does all the machine learning models and stuff. So basically, we export
to YAML files and or JSON files or, you know, whatever config format you want.
Like there's a serializer and deserializer plugin, because if you haven't
noticed yet, I like plugins. And so you can export these data flows and then you
could say like, you know, run it. This is the specific data flow for the HTTP
API. And you can overlay other things on top of that to extend it and say, you
know, I so I had this base set of operations for this data flow. But now I want
to create this new data flow. Right. So I want to take these should I operations
that we wrote. Right. And one of them was download the package contents and run
the banded operation. And but now instead, I'm going to take the package
contents. And then in parallel, I'm going concurrently, I'm going to link that
into those existing get operations that we had. Right. And I didn't have to
write any code. All I had to do was modify the YAML file. And now if you can see
at the bottom here in this demo, it's giving you the lines of code or lines of
comments, a line of code ratio. And basically, we have like a very small YAML
file that gets in this overrides situation. And then you can like overlay
different operations to create new data flows by chaining together these
operations and saying how they should be connected. So this is the graph that
shows you how we took that package contents. And now we are using it in or we're
using those other get repo operations to, you know, same code. All we had to do
is tweak some YAML files. And now it's all running concurrently together. So
where do we go from here? You can check out the machine learning integration
usage example, which is basically very, very, very similar to the code that had
to be written to integrate this within Intel's environment. And you can also
check out should I, which is this meta static analysis tool. And hopefully you
guys can go contribute because it should be very easy to write little
operations. And you if you want to contribute, we have weekly meetings at 9 a.m.
PST or PDT narrate now. And we've also got a getter mailing list and the meeting
links are on this website. And I just updated all the documentation and rolled
releases this morning. So hopefully it all works. Any questions? Yes.


## Rolling Alice: Progress Report 4: What is Alice the Open Architecture

> https://www.youtube.com/watch?v=N-sKHKVS5YY&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

The open architecture aka Alice is a proxy for domain specific representations
of architecture. The metadata to help us understand each domain specific
representation of architecture is called the manifest metadata, right? We just
talked about that. And then the term manifest is used to describe the domain
specific representation of architecture, right? So just because that's a bit of
a mouthful. And then each node in the architecture graph is therefore manifest.
And then the top level document aka Alice, aka the open architecture itself,
must therefore be also a manifest because it's effectively its own thing, right?
So everything is basically just a manifest.


## Rolling Alice: Progress Report 5: May Activities Recap

> https://www.youtube.com/watch?v=9stblwkM--o&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

Alright, hello entities of the internet! I just wanted to give a brief progress
update from the month of May. Because it's the end of the first week of June
basically, which is also the last week of May. So, let's see. Where are we and
where are we going? Let's do a recap. I think we could walk through the
engineering log videos and just talk a little bit about where we've been and
where we're going. So, you'll recall that just to set the stage, remember we're
building Alice. That's what this is about. So, Alice is going to be our
developer assistant, our cross-project, cross-repo developer assistant. Who
allows us... who... yeah, basically, the point being we can apply overlays to be
cross-project, cross-organization, or granular on the level of granularity
around applying policy to build or run time for any given codebase. And
remember, we're not so much focusing on codebases anymore because we don't
really care what language anything is written in. We care about the architecture
because Alice will... I mean, code is code, right? So, we can just write some
serialization code for the underlying whatever language it needs to be in. It's
just bindings. So, let me try this again. So, recap month of May. So, we're
building Alice. We suspect that the DID and peer DID format gives us everything
that we need to... it provides us with a primitive, a storage, a serialization
primitive, right, that allows us to then serialize and deserialize from
arbitrary representations into this common representation, right? And so, why is
this... you know, why are we playing around with this stuff, right? Why are we
investigating this? Well, this primitive... the properties of the primitive are
identifier, right, document with schema, and then links to other documents,
right? So, that maps very well to our concept of the manifest, which we wrote
the shim layer for, and we've been talking about... I don't know how long we've
been talking about the manifest. It's been about a year, at least. Well, let's
see when was... okay, so this was December. But yeah, I think... December we
were talking about the manifest. But yeah, oh, we've been... I mean, we've been
doing the second party stuff. So, the second party stuff was even before the...
oh, yeah, so we've been doing the second party stuff for a while. So, basically
what we were doing is we were like, okay, well, we need to split out the
plugins, right? You know, this isn't all over the map update. The moral of the
story is we're building Alice. So far, so good. Don't have a lot of finished
products yet, but we did a lot of conceptual ground laying. We've done some
thinking in this thread. You know, many of us have conversed on the topic. We
know we're playing with ideas. We're feeling this thing out. And, you know,
we've explored our general Web3 landscape because that seems like a good way to
go. Because then the Web3 landscape would provide... the community seems to be
enthused about providing Web2 to Web3 proxies, right? So, if you just speak...
if we just speak Web3, they'll show up and do the Web2 to Web3 for us, it sounds
like, right? That sounds like that's the deal. And obviously will help. So, you
know, it's a community effort. So, then beyond that, right? So, that's our
serialization format. But beyond that, right, we're going to leverage that
serialization... we're going to leverage that serialization format, propose it
as an option, right? For the general methodology, which is to describe the
overall architecture of an arbitrary application, right? And why do we want to
do that? Well, we want to do it because we want to analyze. And if we can
analyze, then Alice can analyze. And if Alice can analyze, then she can put
together alternate programs, right? So, and she can potentially understand
intent, right? And so, next week on Saturday at 2.30 p.m. Oh, right now. So, a
week and a day from today, John Weigman and I will be presenting on living
threat models. So, John will take us on a deep dive of the situation around, you
know, threat models and, you know, how our threat models are currently dead
threat models, right? And how we might bring our threat models to life. And then
I'll touch on a bit, you know, about how Alice can help bring those to life,
right? And we'll be bringing Alice to life along the way. So, that's out of
order. OK. What else? Oh, yeah. We potentially discovered time travel in a way.
I mean, to be honest. OK. Anyway, so. Yeah. And then we also, you know, figured
out how to integrate the system context. Well, we got clarity. So, we got
clarity on what is the system context, right? The system context is the upstream
overlay in the orchestrator. And so, remember, our overlays are our ways of
essentially doing like programmatic forks of upstream with dynamic policy based
on context. So, that was fun. And then we also got into, oh, yeah, we also
explored potential architectures for distributed CI CD leveraging attestation of
build artifacts and source code via hardware security modules, aka TPMs. And how
that might pair with a DID based architecture and a pure DID based architecture.
Let's see. So, oh, look, there's this video. And then we did a lot of frantic
work on the system context. So, you know, most of this month was spent planning.
That was the intent. Obviously, you know, we hope to get a lot of work done as
well, but it ended up being mostly planning. Which is fine. That was what it was
supposed to be. But, yeah, so I think we can expect, so for next week and for
June, we can expect that we'll continue work with the system context, right?
We'll continue to flush out overlays and upstream in their application and the
granularity thereof, as well as getting a start on, you know, generating
something more than just raw feature data. And, you know, some maybe some living
threat models, right? So that can describe a code base and that Alice can
generate for us. I really would love to serialize to pure DIDs. Hopefully now
that we have the system context and we can if we can figure out how to get the
system context chain happening, then hopefully we can jump in a deserialization
very quickly. We were going to do data flow as class as the mechanism by which
we implemented the input networks for serialization and deserialization, as well
as communication of, you know, with arbitrary chains. I think we're ready to go
on that now. We just need to iron out, there's a little bit of ironing out and
it needs to be done on the deployment environment, but I think we got the
default deployment environment, so we can always just throw a data flow in there
without specific operations. So I think we're pretty much ready to roll on that.
Let's see. So, yeah, so we should go try, once we get, should I think we should
just go do that? That should be one of the first things we do. Let's go do that.
Let's, let's, okay, so where's the code before I get ahead of myself? This
always happens. So, okay, where were we? Yeah, we were trying to tie together
the CLI. I think if I don't stop trying to tie together the CLI next week,
someone tell me to stop doing that because I'm getting a little bit too in the
weeds and I'm trying not to, but, you know, it can be easy to convince yourself
sometimes that it's a good idea. So, yeah, other than that, you know, I think, I
think, you know, hopefully we see some serialization and deserialization and
communication with different chains and then hoping to see, hoping to see, well,
we started with that, so I don't want to, you know, I hope to see that. Yeah,
hoping, definitely going to see some, a living threat model of some kind get
generated. And what else are we looking at for June here? Oh, we're going to
look at the, yeah, I want to look at that TPM stuff and that key architecture
off the DID. So, so we're going to have to, we're going to have to, we're going
to have to serialize and deserialize, so we'll fix the system context. Okay, so
just to recap, where are we going? Oh, oh, that's right. So for next week and
for, yeah, so for next week we'll publish the open architecture set of ADRs,
which will workshop into a RFC probably by late July, August timeframe. I'd like
to see us RFC that. And which will cover, you know, the methodology of which
serialization to DIDs and pure DIDs is one serialization mechanism. But
generally the methodology will be focused on manifests, right? And this idea of,
you know, what are our touch points? What's the schema? And then what are the
side effects in an asynchronous nature? And so one of those could be basically
immediate return value, essentially. Or, you know, error conditions that derive
from that, which would be asynchronous. So, yeah, so open architecture ADRs next
week, living thread models next week. And then probably pure DID. So more
getting into system context a little more the week after, hopefully connecting a
few more, connecting some system context links. And then, you know, towards the
end of June, we might get into serialization and deserialization, even though I
want to do that immediately now. So I'd really like to see us trade the flows on
top of TBdex. So that will require serialization, deserialization. So if anybody
wants to jump on that TBdex trading, you know, please reach out if you haven't
already. Or reach out again if because I haven't heard. And we'll try to get
that organized. So then, you know, following the RFC, we'll try to parallelize
workstreams. So it would be really great if we could get this all done within
the next year or two. Just because, you know, why wait? So, you know, reach out
if you want to get involved and hopefully there'll be some, you know, better
commenting ability than the thread. But for now, you know, we've just got the
thread. So I'm hoping to dump this. You know, you all have seen me try to go
through and dump this several times now. So, you know, there's always something
else. So I'm definitely, it will be dumped by next week. But if you want to
comment before then, you know, please throw any thoughts or comments in the
thread. Everything is valuable. You know, we really need to flush everything
out. So great. All right. Well, thanks, everyone. And I'll see you on the
Internet or in reality.


## Rolling Alice: Progress Report 1: Living Threat Models Are Better Than Dead Threat Models

> - John L. Whiteman and John S. Andersen
> - https://www.youtube.com/watch?v=TMlC_iAK3Rg&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw
> - https://github.com/johnlwhiteman/living-threat-models

And our next speakers are John Whiteman and John Andersen. They're going to talk
about living threat models are better than dead threat models. I totally agree.
John Whiteman is a security researcher for Intel and a part-time adjunct
instructor for the University of Portland. I'm also an Intel alumni there, John,
myself, and it's good to see you again. He also teaches the UC Berkeley's
Extension Cybersecurity Bootcamp. That's actually quite awesome. John holds a
Master's of Science in Computer Science from Georgia Institute of Technology. He
possesses multiple security certifications, including CISSP and CCSB. He has
over 20 years of experience in high tech with over half of it focused on
security. That's kind of like mine as well, half on security, half not on
building things. He can also hear John host the OWASP-Dx security podcast
online. And John grows Wasabi during his off hours. I'd like to get some of
that, John, when we meet in Portland eventually. He's driven by passion for
exploration and understanding. He's currently analyzing software development
methodologies and technologies to identify and proliferate best practices across
organizations so as to optimize asset allocation and alignment with desperate
organizational strategic principles. He's excited to explore the application of
these techniques on human and AI agents. His mission is to help foster an
environment which maintains agent freedom, privacy, and security. We also have
another fantastic John joining us, John Andersen. He is driven by passion for
exploration and understanding. He's currently, well, I think that part that I
read was for John Andersen. So I'm going to stop here and I'm going to pass it
over to you gentlemen and looking forward to your presentation. Awesome. And I'm
going to stop sharing. Can you see my screen? Absolutely. Perfect. Yeah. And by
the way, because there are two Johns, we just have the John squared thing, the
intelligent questions, like the hard to answer one that goes to John Andersen.
So we'll know the context of it. And anything like, you know, trivia for TikTok
or something that's on my side. So anything like that. So John gets the hard
questions. Sounds good. Voted. All right. Well, thank you so much. This is my
favorite day actually of the year, the favorite time because in Portland here,
we finally got back and the Rose Festival is back in progress again. And it is a
little rainy, but cool. And just a perfect day. And both of us are really, we
really appreciate you coming in and taking some time and listening to us. We
really are excited about this, but yeah, as our, as the title goes, living
threat models are better than dead threat models. The opinions expressed here
are our own and not necessarily our employer, which we have to say. And about
this presentation, we're going to assume that you already know something about
threat models. And it's not about any particular threat model methodology. We
have the old school stride, pasta, vast trike, and our favorite dread. By the
way, if anybody wants to collaborate and create a cartoon for this, we're
totally into this and be funny. And we're agnostic to threat modeling tools,
although we will be presenting a couple of tools in a couple of different
contexts, particularly about some of the nice things that they have, but the
limitations as well. What we're really trying to do here, our primary focus is
the collection and definition of data for threat modeling use. So John, can you
talk just a little bit more if you want beyond what was said? Hey, yeah, sorry.
Yeah. So I'm John I'm at Intel also with John. And right now I'm focuses on
inner source. And so, you know, analyzing software development methodology is
proliferating best practices playing around with those concepts, you know,
hopefully feeding best practices to developers internal and external, and then
also exploring that on AI agents, thinking about, you know, how can we get AI to
follow our best practices and living threat models are, you know, a part of
that. Cool. Thank you, John. And it was already said to not only micro SABI. And
by the way, the word micro, all that means is that I started out with like five
wasabi plants and I have one left that's living. So micro sounds good, but it's
actually a result of a failure as a wasabi grower and also make kombucha, but
that's an old person thing. I'm not going to get into why we do that now,
something about John squared and what kind of led up to this presentation back
in 2016, we both worked as security researchers for Intel's open source
technology center. Unfortunately that's not around anymore, but at that time our
work, we didn't really work together. We were on the same team, but we're doing
different activities. Now I left Intel 2016 for a few years and I went to Oregon
health and science university. And there I worked on a project called SAS and
the bad human project. And this is where I was using machine learning to predict
risks across hundreds of web apps at a time where they didn't really have any
expensive SAS tools. And this was risk prediction from say source code from web
applications. And I presented that information to besides in 2018, John, a year
later did something called down the dependency rabbit hole. John, you want to
describe a little bit about that work that you did? Yeah. And with that project,
what we did was we looked at open source dependencies. So if you're familiar
with like the open SSF identifying security threats or metrics, working groups,
similar sort of concept, you know, analyzing open source projects, trying to
understand whether, you know, there's something secure and robust enough from a,
from a general health and, and their predictive life cycle health standpoint,
rather than just, you know, do they have CVS or something like that? One of the
things we discovered, so I went back to Intel and we started meeting because we
felt like the work wasn't really done. We felt like we had some answers, but
then all, and sooner than later, we found fundamentally we were trying to solve
the same set of problems. Number one was to find ways to dynamically assess
risks associated with these projects to collect a bunch of data from various get
repos and other sources three we need. And this is a big bulk of what we're
going to be talking about today. And that is defined an open source threat
modeling language for integrate these systems into CI CD frameworks, five
promote and socialize our efforts for adoption. That's why we're here today.
Hopefully you get people on board. And then finally, if we solve all these
things, we made the world a much more secure place. We would go in and retire.
And when I first created the slide, I showed it to John and John just kind of
said, you know, there's a lot of words on this slide. What we can do, let's just
go straight to six and retire. So we're done with our presentation. Thank you
all have a good day. Anyway, not for that. So when apps evolve, so does the risk
and so should the threat model, at least that is what you would think. Typical
threat model workflows are like this. The team, the project team will create an
architecture design. They'll go then and do some threat modeling. And usually
it's a manual process and manuals good. We think manual processes are good. And
this would be the bulk of the threat model early on in the project. Then they'll
go off and save that threat model to store it somewhere. And usually at the same
time, they're committing code. They're doing manual code reviews. If they're
doing good, secure quality assurance. Also static code scanning, SAS tools,
dynamic testing like gas, fuzzing. If you're not fuzzing, you should be ashamed
of yourself. Everybody should be fuzzing. And then through this whole process,
they're finding and fixing vulnerabilities. It's evolutionary things get checked
in. There's a release every now and then. And yeah, maybe an escape too. You
might get a CVE back, but if your project is right, that's transparency. You're
finding and fixing and getting patches back to your customers, the end users as
soon as possible. Now one of the problems with this flow is we have, we call
this Hadrian's wall, but we couldn't find a good image for Hadrian to go this
high up. But what's happening is on the left-hand side of this is your threat
model is not really evolving as it is on the right-hand side. And so what we
call this is the tombstone threat model process or TTMP. Now it's a bit of a
hyperbole to say that a threat model is dead if you don't use it or update it
after a while, or your threat model expires. In fact, your threat model is
probably going to be pretty much intact as long as it doesn't fundamentally
change the environment and all that. So we like to use best used before or best
before. And a great analogy would be something like this. It's late at night,
it's Saturday, your refrigerator is empty, you're covered to bear, but then
you're under the sink and you see behind the drain, this package of top ramen,
and you're looking at it and it's saying, hey, best before was like two years
ago, and you got to make that critical decision. And that decision is, are you
going to eat it or not? And with top ramen, it's probably not a, it's going to
live longer than the universe. We know that. But with milk, I don't even think
they use best before. I think they use an expiration. So if anything, when you
walk away from this presentation today and somebody asks you that John squared
meeting or presentation, what did you learn from there? What we've learned is
that threat models are ramen and not milk. And I think we should be, John, I
think we should create swag for this. I just thought of that. Okay. All threat
models should be machine readable. Absolutely. That's things that we found right
off the bat. So what we normally see for typical threat model storage is you got
your threat model, teams might be doing it in something like PowerPoint or Word.
They may be doing their diagramming in Visio or something like that, and they
save it off to something. Now, these can be pretty big files. Sometimes teams
would then do, you know, convert it or export it to PDF. And those are much
smaller in most cases, but they're read only and they're not really parsable.
I've seen teams go and do threat modeling and take a picture, say of a
whiteboard and they use that diagramming they had on the whiteboard as their
threat model, nothing wrong with that. Or they've actually used a tool like a
paint, Microsoft paint or GIMP, you know, hopefully they didn't save it as a
BMP, but that's also part of what they're using for a diagramming piece. Other
teams now I see more often are doing something like DrawIO or some related ones.
And what happens is when all of these applications save, like you save that
application to data, we call this application specific languages or ASL. We'll
use the DrawIO as an example. Now most people, if you've used it before, are
aware that there's an online version, but if you didn't know, there's also a
Visual Studio Code version. It's a plugin. And what's really cool about this is
if you're already in Visual Studio, you're probably in the same place where
you're doing your coding and everything else. You're under your repo
directories. And so when you install this plugin, it comes, if you see on the
left hand side here, they call them stencils or templates, whatever, but you got
these pre-made objects and out of the box, DrawIO comes with threat modeling.
And you see here, all these nice little things that you can easily drag over to
your canvas and you can quickly start to diagram. Now this threat model that I
have here is going to be the one that we're going to be using throughout the
rest of this presentation. But it's very simple. We have three major components.
We have a client application, could be a browser, it's out of scope and it's
communicating to web server. That's in scope for us. It's talking across the
network. It's doing the right thing. It's using HTTPS, TLS for encryption and
authenticity. And then on the other side, we have a customer database. And
again, we're talking to it across the network. This could be behind the DMZ,
could be off in the cloud somewhere, but we're trying to do our best. And these
lines in blue, these dashed lines in blue, this is going to be our trust
boundaries. So we can easily diagram with these ready-made pieces. We have other
areas if we wanted to add style and all that, but again, we're really trying to
threat model, but not spend our time to make it look pretty. Now if we go and
save this, there's various ways. You can export it as an image. You can do all
that, but they also have what they call a dot DrawIO file. And all that is, is
this machine readable XML. And XML is what we call a general purpose language.
We love, we love GPLs. Other examples are JSON, YAML. And if you take a look
here, there's a lot of data here, but you can, if you just take a little time,
you can tell that there are some areas that are, you know, first of all, this is
a very small footprint, but there are areas where you start seeing things like
mixed data and the content. And so we don't necessarily care about page width.
That's not going to be a vulnerability of page width is greater than 850 pixels,
but we do care about these other areas like the database and some other
components from there. Threat modeling tools are dangerously shiny objects. This
is the biggest mistake I think people make, and we did it too. And this was
about, I say the second year where we started talking about this. We said, Hey,
you know, that's great with Draw.io and PowerPoint and all of that. Why don't we
just build a tool that does everything, not just complimentary. So it could have
diagramming, could have your APIs, reports, templates, stencils. We even talked
about gamification and even something like putting on like a virtual reality
headset, grabbing pieces in the air and putting things together. Right. But when
we realized quickly is that this whole thing starting with the tool, that's so
1990s. And it's always a big mistake. What we found is that most threat model
specific tools today, they're not CICD friendly. And it's not because they don't
have the features to automate, but it's because of a lack of a common open
threat model language. And what we mean by that is when you say that ASL data,
app one, app two, this could be something like JIRA. This could be some other
tool out there. The teams would have to build some custom tools to talk to these
other components to integrate into their CICD environment. And that's a tough
call, especially if this is just a single purpose language and other people
don't use it. And I think that's one reason why we're seeing these challenges.
And also it's hard enough to get teams to do security in the first place. One
thing we noticed out there too, there's not a lot of threat modeling tools. If
you look, there might be maybe a dozen or so. There's a bunch of others that
have false starts, but there aren't that many. And I grabbed five for this table
here to illustrate a few things. We all maybe know KIRIS, which is cool because
it also includes things like social engineering as threats where a lot of tools
kind of miss that. But Microsoft Threat Modeling tool, that's gone through a few
iterations. Our favorite OWASP Threat Dragon, Thragile, if you haven't heard
that, look that up now, and Threat Modeler. And they can come in all types in
terms of licensing. Many are open source, which we like for what we're trying to
do. Microsoft is free, but it's proprietary and Threat Modeler is proprietary.
I'm not sure if they have a community version, but you're going to pay money for
the good stuff. When we talk about data and we're coming back to the data,
there's really two major formats that we care about. There's the ASL stuff that
we talked about. Again, that's the application specific one. And most of these
application have these export formats too. And if the export formats are not
really parsable like KIRIS again, ODF, RTF, DOC files are pretty tough to manage
when you're parsing. Even the Microsoft tool does have HTML, which is a domain
specific language, but maybe not the best. And OWASP has PDF, but the ASL format
is JSON. The thing I want to talk about Thragile though is for both the ASL
format and the export format, at least it supports a GPL, a general purpose
language, YAML on the ASL format and JSON on one of the export formats. And
we'll illustrate why this is so crucial for what we want to do. Here's an
example of the Microsoft, the latest tool. They again went through a few
versions. The latest version has support around Azure and it's a great tool and
it's GUI based. You start it up, you have a nice canvas here. You can put your
objects in here similar to what we saw for Draw.io. But the nice thing about
this, it includes all of these attributes for the given type of object. And in
this case, this is processed. So when you're thinking about threads, a process
might be, is it running elevated? It doesn't have input surfaces and things like
that. So this is what is really great about this tool. Now, when we go off and
save it, again, it's GUI driven. So you have to start it up and do all your
things. But when you save it, the ASL part comes with this.TMZ file. Get this to
memory, right? This is what I call ponderously parsable. It is XML, but it's got
a mix of HTML. I see hieroglyphics in there. I don't know, Sanskrit in there.
Everything is in there. If you want to parse and figure that out, go for it. But
the actual export file is HTML. And I don't have anything really bad against
HTML, but these things are a bit dynamic, right? Based on the structures that
come with it. So we'll have about the threads, maybe a summary. And down here,
it will have that diagram that we created in the canvas, but this is embedded as
the binary inside of it. So what happens is, because this is not really CI CD
friendly, what you need to do is open the app. You have to export that report to
HTML. Then you got to go and extract this diagram, use something like
BeautifulSoup, but there you go. Now, I was going through the MSDN format
thinking, well, I must have the old version because I'm sure they did something
better here. This is back all the way from 2017, where somebody was saying that,
hey, wouldn't it be really cool if we could take this diagram and map it to the
actual things back here, like what we're talking about today. Come on Microsoft,
get with the program. And we feel that they kind of dropped the ball here
because it's not really, again, CI CD friendly. By the way, we have our own
favorite threat modeling tool. This tool is not in the list or the table that I
showed before, but this is John and I's favorite tool. It's the Etch A Sketch.
And we were talking about if somebody comes in for a job interview and they're a
security person and you give them a scenario where they have to do some
diagramming here for the threat model, we give them that scenario and we tell
them, okay, you got 10 minutes to do it. And we give them an Etch A Sketch. And
if they run away, we would hire that person. If they stayed and finish it, then
we'd be very scared of that person. Anyway, sorry about that. Application
specific data, we just talked about a machine readable, very specialized mixed
content, not portable, heavyweight, right? General purpose languages, machine
readable, generalized, JSON, XML, YAML. We love it because they're widely known,
they're accepted, they're easily recognized. You don't even have to know how to
create the parser or use parsers for them. In fact, that's the nice thing about
them is the parsers are already created. You never want to write your own
parser, right? Michael Leibovitz of the old days, parsers are hard, great
presentation. What we want to talk about next though is domain specific
languages, DSLs. Again, machine readable. Notice this is the common thread.
They're specialized and they're idiomatic to the domain, very compact and
lightweight. So some that you already know, HTML, SQL, HTML, you think of web
pages, SQL you think about relational databases, right? And that idiomatic piece
that I'm talking about, that's what gives us SQL injection errors, right?
There's specific things about it. I want to talk about two other languages.
These are diagramming languages, modeling language, mermaid and plant new ML.
And you may have seen these already in action and may not have been aware what
they were. So now if we talk about mermaid, just with a few lines of code, the
mermaid syntax here if you've never seen this before, if I click on this, it's
wonderful. You get this wonderful diagram, all of this information contained
with just a few lines of code. We're not talking about an image. We can parse
this, but the challenge here is this language you would have to figure out how
to parse it. It's very specialized. Let's take a look at another, the competitor
plant new ML. UML by the way is a modeling language. It's domain specific, but
it's based on UML. And the same thing with just a few lines of code, we can come
in here and define an equivalent threat model with this, at least with the
diagrams. Now, obviously we'd have to put more information in here, but we like
the fact that, oh, now we have some syntax. Now we have some structure, but it's
not quite what we want yet. But at least we have something here that we don't
necessarily need a full blown tool to do things with. So what is our goal? Why
am I talking about languages up to this point anyway? Well, as I mentioned way
in the beginning, we want to define a threat model language, a threat modeling
language, an open language. And open means we want to do it as an open
architecture with heavy community involvement. Now when I first, this is yet
another slide I was working with John, I'm like old school. I mean, my threat
models go back, you know, pre-Civil War days, you know, and I'm thinking like we
got to get the standards team in. We got to get the ISO people in, we got to get
the IEEE people in. And John was really quiet. He didn't really see anything.
And then he came, John, tell me why this is a better way of an open architecture
versus the way I was describing. Yeah. So yeah, and John and I, we played around
with this standard architecture and came to, standards are hard, you know,
standards require that everybody agree on the whole standard, right? So instead
what we're suggesting is, okay, let's agree on a few things and then we can all
have, you know, sort of interoperable architectures as within this realm of the
open architecture. And eventually, you know, maybe we, ideally we converge on
some standard, right? But we can't, let's not start there, you know, let's not
start climbing up the standard mountain without flushing this out as a community
first, right? So the goal is to say, Hey, here's a couple pieces of metadata,
right? Which we think your, which we think the, the open architecture should
include. If you decide that you want to do something slightly differently,
that's fine. We're all still going to be able to interoperate as long as we
include these several pieces of metadata. Yeah, perfect. And we didn't want to
pay, you know, if you want to download the standards, you have to pay for it.
The people that are really doing the work is what we want the people here.
Again, we're thinking of a CI CDI CD supported system. That's where we're
looking at with the troops on the ground. So what this diagram represents here
is this gray area is that standard considered that the paper that has the
grammars, all of the dependencies, the academic paper, if you will. And then
what we want to do is create this domain specific threat modeling language from
that. And then we want to use it and represent it as a GPL, do it in GPL. So
again, it would be something like JSON, XML, or YAML. We want to avoid that
specialized syntax. Like we, like we saw for mermaid and plant new ML, because
that's just going to be a big turnoff. If we can represent this as a GPL
language, and we're going to show you example in a second, how important that is
as, as a, as a first impression of anything for adoption from there, we believe
that once this is defined, they will build it. So do you remember when we talked
about going, building those tools first and doing all these wonderful things
again, that's so 1990s, if we have this language already defined, then people
will say, Oh, I'm going to invest my time. I'm going to go ahead and build a
threat modeling tool, but my tool is going to be cooler than the other people,
but it's going to be based on that same language that everyone else is using.
It's going to be IDE plugins, GitHub rendering. As you know, GitHub does mermaid
and plant new ML and code syntax and all of that, or even editors for it. So
Thrajile is one of our examples. I just learned about this about a month ago, a
coworker, my rich Hogan, who was talking about this and also these domain
specific languages and why I got really interested in it. So some credit to him.
The idea is that if you take a look here, this is a threat model. It's
represented as a YAML file that Thrajile understands. Now, if you don't
understand anything about the, maybe the structures, but you know something
about threat modeling, you can look in here and you can say, Oh, well, I know
threat models have security requirements. I know a threat model should have
something about assets, maybe some metadata on here. This is the oldest threat
model, the battle of Hastings in 1066. And there was some overlaps or overlooked
by Errol. And anyway, you know what happened after that. So in addition, if you
look at a full file, there's assets, there's flows, there's trust boundaries and
there's customizable things. That's key too. And you have these tags where you
can build things that might make sense to your business and nobody else. Right.
Stores nicely as re in the repos as a text file and it's the ICD friendly. It's
a Thrajile. That's the command tools name it's written in Golang. It would
ingest this YAML file generates these wonderful fla reports off the fly and you
can do audits. And we're going to talk about why audits are important and why we
need to be doing this. So Thrajile is a great one. Highly recommend you take a
look at it for the open source. By the way, we have a GitHub repo. We'll send
you all the link and all of that for it, where all the slides, but all this
stuff that we're working on, some of these demos, that information's in there as
well. Now scan threat models like you scan code. Well, if we have this wonderful
threat modeling language, this open architect language, we can do some of the
following, which we really can't do right now. Number one, we could do things
with the threat model file itself. We can write a linter for it. We can look in
the file and we can do things like, oh, it's missing assets. Why does your
threat model have no assets or some labels are missing? Maybe we see things like
duplicate names, or we can even look for the age of the threat model and that
best use before date. We can do that as well. And these are quality checks that
we call it, but we can also do scans against that same threat model file for
vulnerability checks like a SAS scanner. In this case, we might be looking for a
lack of or incorrect mitigations. Maybe you have a network connection. It says
HTTP, but you're not using TLS. That is a vulnerability, or you have nothing on
there at all. The other thing, which we'll talk about later, and this is the
core, what I believe is for this whole presentation, is with this threat model,
you can now start to model or monitor for these check-ins. If you remember that
evolutionary thing that I was talking about between the existing threat model
and the source code. So for example, as people are checking things in, all of a
sudden your source code starts containing network libraries or crypto or
something of that nature. But your existing threat model says nothing about
networks. It says nothing about cryptos. That can raise an alert and say, hey,
you might want to go back and take a look at your threat model. It looks like
we're getting some stuff in here that you haven't considered before. It could be
a false positive, it could be real, who knows. Now because we don't have that
language yet, we can use something existing and we want to use our beloved OWASP
threat dragon to make a point. And again, this is open source threat model too.
Hopefully here in OWASP we know about it. Goodwood, Gatson, Redding and all
those folks. Gatson is one of our own from the Pacific Northwest. I believe he's
still up in Vancouver, Washington state. So it's great to have that identity as
well. There's two flavors of threat dragon. One is a standalone tool and the
other one is a web app. The web app is pretty cool because what you can do is
configure it to point to your repo and it'll automatically do the commits into
it. And so all of this threat modeling is integrated into your repo. And of
course that's what we've been talking about. Really nice. Now, as we saw in that
table before, there's really two flavors. The ASL is exporting as JSON and then
the actual exports themselves are PDF. In this case, we're going to reverse it.
Normally we want to look at what the exports are, but the threat dragon's ASL,
the JSON representation is actually pretty nice and easy to parse. So this is
the same threat model that I was showing. And if we saved it off in threat
dragon, this is the actual ASL data that we get. And just at a glance, it's
pretty easy to figure out where stuff is. Again, we get this mix of presentation
data and some other trackers, but as you would go down and we have this up
there, you can see that where the client app and all the data and some of the
metadata that's associated with it, we would just want to go off and to parse
some of this stuff out to create some things. And that's what we're going to do.
For this demo, I've created three threat models with threat dragon. And they're
all based starting with the same thing here with our web server. The first one
is good and I saved it as good.json. Now when you're in threat dragon, when you
click on any of these components, you would get this little box that comes up
and it'll ask you some questions like, you know, give it a label. Is it
encrypted? Is it over a public network? And so that's about what we get and
maybe that's good enough. So we label it. Yeah, it's over a public network, but
we're doing the right thing. We're encrypting it accordingly. And that's good.
We click on the backend network connection again, same thing, but in this case,
it's not over a public network, but we're doing the right thing again. We're
labeling that's a quality thing and encrypting that's a mitigation against such
a vulnerability. And then finally the customer database stores credentials.
Okay. Is it encrypted? Yeah. At least this is what we're trying to do. Of
course, there's many other things that we can do, but for the sake of this demo,
these are the three things that we think have a, you know, a complete threat
model for this example. So this is a good threat model. A bad threat model is
where there are some problems. It could be a vulnerability problem. It could be
a quality problem. Now, John and I got really excited about this because this is
like about a year ago, a year and a half ago, I think that maybe it's longer
now. I don't know. And everything's like melting together, but do you remember
like the intentionally vulnerable web application or the intentionally
vulnerable C program that you use for AFL fuzzing and all of that? We got real
excited for training because we want people to learn how to threat model. We
could create a project that has intentionally vulnerable threat models, and we
can teach students to say, what's wrong with this threat model? And we can do it
in such a way that, well, we'll show you how you can recognize that and grade it
if you want. So in this case, again, I'm looking at these three attributes. I'm
calling this protocol now HTTP. I'm taking the TLS out. It's not encrypted. It's
over a public network. Very bad idea. In the backend side, we are encrypting and
we've left that, but we're leaving. We left out the label. So the label is more
of a quality issue, even though this is not a real vulnerability. And then the
backend, yeah, we're still storing credentials, but we're not encrypting. So we
essentially have two vulnerabilities with the encryption part, the TLS, and then
the non encrypted database. And then we have one quality issue for the protocol.
And we call this bad doc JSON. Now there has to be an ugly. I said, well, I'm
kind of stuck. What is an ugly threat model? What the heck is that? And I wish
we're all in person, because then I would ask you to raise your hands and stuff.
What is an ugly threat model? In short, it's no threat model. Shame on you. Your
grandkids will come to you. They're going to ask you grandma, grandpa, why, you
know, what are you proud of? What are you ashamed of in case you tell them I'm
proud of you the most? And what are you ashamed of the most? You say, because I
never threat model. And they'll just never look at you the same way. So this is
what we call it. And this ugly.json is an empty file. All right. Now, what I
did, because you saw how that syntax was, very simple application specific
language. I wrote a little parser. And if you take all of these things out, all
I'm doing is just extracting the info I want, ignoring the presentation data.
You can write this in maybe a few dozen lines of code or not. In fact, I wrote
this probably in about 30 minutes. That's how easy it was. And that's the beauty
of having a JSON structure. You know, you understand JSON, you understand the
structures. It doesn't take very long. I then created another one called
Auditor, which uses parser, consumes that threat dragon file, and then it goes
and looks for some specific issues. Again, the three that we mentioned here
would be a labeling issue or it's the HTTP, not HTTPS and stuff. You would
probably do these rules in some text-based file and then consume it accordingly.
But just for the sake of this presentation, you can see the power here with just
a few lines of code on each side, where now we can parse this threat model and
we can start doing things like auditing the threat model. So let me show you the
screen here. And what's happening here is I'm going through all three cases. The
first one, the good, is that empty or that good file. So here's good. We get
nothing back, which means it's good. The exit status, bad. The bad one, we get
there are three issues. And then the ugly, it just crashes my program. It's like
perfect for that anyway. So that's what we do. And you see this thing is
repeating. Now imagine this in your continuous build environment, right? Every
time you get this commit, you're going to run these things and then you're going
to set some rules there. And if you are getting something that's a non-zero
coming back, then you take whatever appropriate action or maybe there are known
issues and you look at the file. But the sky's the limit because we're all on
some standard defined open language that we can use for this. So this is this
example and the scripts are there if you want to take a look at it. Now we say,
put the threat model in the CI CD workflow. One way you can do it, and there's
multiple ways of doing it. Now we can say, instead of that threat model sitting
over there as a PowerPoint slide, we now have this defined language, machine
readable, lightweight. We can create an individual branch called threat model.
Again, this is one approach. And this threat model is keeping track of the
versions of the threat model. So like this initial check-in, this is the stuff
that we've done manually. That's all the hard work that goes in and we set it up
as we move along. Now the branch one, we might get some hot fix and this null
operator means that there's really no code inside it that has changed anything.
So we don't need to update our threat model and we can go ahead and merge
accordingly. But let's say we get some significant code changes that affect the
threat model. Maybe we're adding cloud components and all of that. And with
that, we're going to have to update our threat model here. Now this is the
point, like what we're saying is something significant like that would probably
require some manual intervention. Or if it's something small, we'll show you in
a little bit, we can actually catch that and update accordingly. Another thing
that could happen is we could get something like a zero day vulnerability. We
find out about it, but there's no code or anything. We can almost treat the
threat modeling branch as private. It's not being merged in. So we are aware of
this. And now we're waiting, maybe it's going to be branch three or something
that will come along and it's going to say, oh, we got to put an implement. We
have to implement this, but before we make it public, we're aware of that and
we're going to update the threat model accordingly. That's one way of doing it.
Another way where I think John and I kind of agree that maybe this other way is,
again, we're going to do that initial check-in where the threat model is done
manually, but then the threat model itself is really just part of the code. It's
part of the regular check-ins at the same branch. So just like the code, you're
going to do your SAS scans. You might be scanning for third-party
vulnerabilities along with your custom code. And you're also going to be
scanning your threat model because now you have those scanners for those. And
again, the caveat here is that, you know, you're going to want to check, it
might not be fully automated, but at least if you have the tools in place to
look for things like these deltas along with some human interactions, it could
be a pull request, for example, that might be just the pull, the threat model,
those are the things that are kind of natural anyway when we're doing it like
this. This is kind of a typical scenario. Now threats.md is the new readme MD.
And what do I mean by that? Well, everybody hopefully knows what a readme MD is.
It's a simple markdown file, but it has a special purpose in GitHub, right? So
it resides at the root of the repo directory. You can always find it there. And
we understand that's usually the place you start when you want to find out
something about the repository. Now inside it, this readme file, we were kind of
alluding to this earlier, but you know the triple back tick stuff, right? And
then you put something in there. The markdown parser understands it, doesn't
understand what's going on inside it, but it assumes you do, and maybe your
parser will understand it. So there's this foundation readme parser, and then
the special purpose could be domain specific stuff. In fact, Mermaid is a great
example of it. Plan UML as well on GitHub is when you look at the markdown file,
you got your triple back tick here. We got a Mermaid label here that indicates
whatever the parser is on top of that says, oh, this is, I got to pay attention
to this because this is Mermaid. It goes in and renders, of course, something
simple like that. And I think that's the coolest thing in the world. And that's
so easy to do with the readme file because it gives us the best of both worlds.
Now, I want to talk about something called overlays, and John and I kind of made
this name up, but there's a super, there's something super powerful about it.
And the best way I can explain it is start with something called citations.cfm.
Now, citation.cfm is not called an overlay, but it's something that's supported
by GitHub. If you create this file, and this is the format, it's a domain
specific format, but if you want people, and I think you should, if you got
good, if you don't know about citations and, but you have a really good repo out
there and you want people to cite it the right way, create this file. And
citations, again, we're talking about academic papers or books that may go out
and use your repo as for some something, right? You want them to cite it
correctly. This is the way to go. And as you would expect, who are the authors,
the title, version, maybe, orchid names, you have all of those, the date
released, and of course the URL. Now what happens is when you save this as
citation.cfm, it's just like, just like the readme file, you do this at the top,
you'll immediately notice over on the about on your GitHub site, this thing
appears, cite this repository. And when you click on this, all of a sudden you
start getting a couple of different flavors of citations, which is really cool.
There's APA, which is great for scientific stuff. You get bit text as well. I'd
love to see something like IEEE or Chicago style or something. People can go and
build this tool and add it accordingly. And when you pick this, you get the
format that you're looking for. If it's APA, you get this bit text is great
because it's kind of used for a lot of other things as well. And this is all it
is. Now you say, well, what's the big deal about that? Well, if we do it for
something like we create this file called threats.md, readme file, and it's just
doing some stuff here, and then we have our backticks here, we call it threat
model threats, whatever we want to call it. We have some dummy, we have our
language, we haven't defined it yet, but I'm just giving this an example. It's
going to understand now again, the readme is not going to understand it, but
somebody's going to go off and say, oh, I understand this specific language is
open and I'm going to go ahead and create some backend thing that GitHub is
going to support and it's going to render it accordingly. So if we save it as
threats.md, we then get this automatic thing that comes up again, all the
machinery's back in GitHub called threat models or threats and mitigations. And
when we click on that, we can go as far as, remember those methodologies and
those characters at the beginning of our presentation? We can have it
represented as stride. Somebody can write the stride version and it's writing on
top of our specific dsl-kml or pasta, and they can click on it and get the
language. Extremely powerful. And this is what we're saying for the potential
behind all of this. So finally we have to feed the threat model to keep it
alive. So I've been talking about languages up to this point, but what I haven't
been really talking about is how do we keep it alive? Well, we need to define
this language first, but now we need the tools to determine what's new. What are
the things that are, we're going to check for example, the deltas, what is the
data that we're going to do to extract from all this information? And John's
going to take over here. He's going to do a dffml demo and describe exactly what
this scenario is for us. John? All right. So let me bring up this. All right. So
here's what we're going to be doing. I'll show you all what we're going to be
doing and then we'll do it and then we'll talk about it again. So John and I
talked about the threats.md file and the, you know, the threat model, the threat
dragon threat model, right? And this idea of this open architecture based off of
a general purpose description like JSON, right? Or YAML or whatever, right? The
point is we have this structure and we're going to have a schema involved,
right? So we're going to be doing a little demo here where so when I said I'm
excited to try this on human and AI agents, we have a project which is spun out
of the dffml project to build sort of this AI software developer. Her name is
Alice. And so Alice is going to be helping us generate our threats.md file
today. So what we're going to do is first thing we're going to do is generate
just the thread model and it's just going to have, you know, the threat dragon
threat model in it, the diagram there and then as well as the general, just this
open architecture description of the, you know, of the, of the, of the threat
model, right? So we're doing a conversion. So let's do here. So let me remove
the file, right? So whoops. Okay. So there it is. Okay. Nevermind. Let me go to
my browser. So this is the file that gets generated here. All right. So, okay.
So I don't have the mermaid for you today, but what we do have is, and I'll
paste this into the body of a GitHub issue. So this is what it looks like,
right? So we're, we're, you know, I'm just previewing in a GitHub issue right
now in the body of an issue and, you know, so we have our threat model, our
diagram from a threat dragon, and then we have our, our open architecture spec,
right? And so this is actually, this is actually the open architecture for the
the tool itself right now. So, and you can see basically you've got effectively,
we have this directed graph of, of data and it's a little verbose right now
where, you know, where it's in progress but the main point is you have your sort
of definitions of your data types and your assets, which correspond to, you
know, the elements that we'd see up here. And then we have our connections
between them. Right. And so that's getting into, you know, what, how do we link
these things together? Okay. So then in the future, we'll have also the mermaid
diagram, which will be synthesized from there. Right. And so the concept is,
okay, you take your, your source of truth material, right? So in this case, it's
your, your, your threat dragon threat model, any maybe supplementary data you
may have, right? That's written text, and then we're going to combine it all
together using Alice and dump it into this threats, empty, right? And when we do
that this helps ensure that our threat model or our threats MD file is always up
to date, right? Because we generate that with our CI and we pull from, you know,
whatever, whatever authoritative sources of truth we have. Right. So then let's
see. Oh, I had a checklist of things I was supposed to talk about and I was not
doing that. So where is my manifest? Oh, this is, where's the manifest. I can't
get to that tab. Oh, here it is. All right. So what, you know, what, what is
this open architecture? Well at a high level and so we also have, have also been
referring to Alice as the open architecture. She's an entity which is based off
of this implement off of the architecture itself at a high level. And then so we
use the same, same terminology to refer to both. So with the open architecture
effectively what we have is like a, you can think of it almost like a reverse
proxy for domain specific formats of architecture or representations for of
architecture. And since domain specific representation of architecture is such a
mouthful we basically have been referring to that as, as what is effectively a
manifest. And so when we talked about earlier, you know, what are these
properties of, of a manifest that allow us to interoperate, right? Well, we
looked at the world and we said, okay, so what are some successful formats? What
do they do? And how would we, you know, if we treat the world of data as like
streams of data and we may not see file names, we may just see these blobs
coming through. What are the things that we need to see in these blobs of data
to know what they are, right? To know what an individual element within an
architecture is so that we can appropriately apply threats and mitigations as,
and, and the auditing, which John showed in this cross language way, right? So
you, we, we figure out what is the domain specific representation of argument
for, of architecture, AKA the manifest for our source of truth, right? And that
source of truth, you know, that we could have multiple sources of truth for
different parts of our application and we're forming them together, right? We're
bringing it all together to do this open architecture representation. And so the
nodes in that graph are effectively a manifest. And then the top level node of
the graph, the graph itself is also effectively a manifest, right? Because the
same it's, it's, it's, it's, you know, you can represent any domain specific
format within, within this is the goal, right? So, and we need this to be a
community effort to figure that out, right? So what are some of the things that
are involved in a manifest, right? Well we looked around and we said, JSON
Schema did a lot of good, you know, they, they learned, they learned some, they,
they, they have built a lot for the community, right? We stand on the shoulders
of giants. And so we know that documents should have schemas because a schema
will allow us to understand, well, what is this thing? When I hit a new node in
the graph, it'll let us say, okay, well, what should I expect this node to
contain? And if I know what I can expect the node to contain, right, I can do my
input validation. And then when my tool consumes from the graph, then it, it,
you know, I, I can have a tool that understands the various different formats in
the schemas that are applicable to each format. And then basically, well,
there's this concept of sort of the shim that says, all right, well, given this
format for this domain specific representation of architecture, let's jump into
this auditor, right? Or this visualization, right? And then we output that all
to the threats MD and that way it stays alive, right? It's not stale. It's
always coming from the source of truth. So so effectively the main thing that
we, there was three properties, which we said, these are good things, the
manifest metadata, right? Which a manifest should have, which would allow us to
effectively validate any blob of data, right? And, and say, we understand this
thing as if it's, it's part of the open architecture. It's it's part of this
proxy format, right? And that is the format name, the format version and the
schema for that. Right? So we'll combine all those things with little, little
shorthand, we'll put them all in the URL, right? And basically the file name
gives us all three of those. And then when we resolve the URL, we get the
schema, right? And so if we have this information, then we can write our parsers
off of it, right? Or our auditors, right? And then, so we talked a little bit
about the concept of overlays. So the default flow that we just ran, right? To
generate the, okay, I've got a lot of stuff on my screen here. Can you all see
the top bar? You can't see the top bar, right? All right. I'm going to assume
you can't, you know what, I'll hit enter a few times in case you can. So this
command, which we just ran, right? To generate the threat model. This just, just
you know, source of truth in and then good, good threats out. But what we would
like to do is run that auditor that John showed, right? So and here's the
auditor again, right? And so we want to run this as along with generation of the
threat model, of the threats MD, since we're thinking CI CD, we want to make
sure that this source, that that threat model conforms to like the code that
we're seeing, right? Or in this case, you know, we just want to check, check
just a few basic checks on the threat model itself to make sure it's complete,
right? And so we can start to combine this by adding overlays. So what we did
here is we basically just, we made a little overlay. All it is, is with the
underlying framework that we're using, DFMO, basically all of this is typed,
right? Because if you think about these manifests in the schema, you got to have
some types. So we're going to go ahead and just throw a little type hinting on
there and then create this overlay, which is basically saying, all right, we
have this audit operation, right? And so what we're going to do is we're going
to export the audit operation to this open architecture format, right? So once
again, I can't really see here, but I'm hoping that you can, right? So we export
the open architecture to this format. And then what we're going to do is run the
same generation with the overlay, right? So now what it's going to do is it's
going to check for, it's going to run that auditor internally, right? Along
with, let me make sure we're running the overlay, right? So it's going to run
the auditor there and we should see, you know, oops, wait, where are we? It's
the wrong command. The command is running over here. All right. We're going to
run it over here. All right. So this is the same command, different, different
terminal with the right context. And we end up, you know, with once again, the
threat model, oh, wait, wrong file. So model good threats, right? So here's the
file again, right? And this time we've run through the auditor before we
generated it, right? And that way, if the auditor failed, it wouldn't spit out
good threats MD and we'd fail the CI job. All right. Let's see. And then let's
see. So there's a few things that we want to, you know, we'd like to ask the
community to help us with, right? We'd like everybody to, you know, think about
this concept of the open architecture and how you might build tools around this,
and then also collaborate with us and comment into the thread, which we'll link
here. And we're going to have this as a pull request soon. Right now it's just a
discussion thread on GitHub. So we'd like people's feedback and to play with the
tools that we showed, you know, with the auditor and with Alice and try to
figure out, you know, how, how is this stuff working for you? You know, does,
are you having success with it? Are you seeing places where we could extend? Do
we need to modify the concept of what metadata is required, right? For us to be
interoperable? Is there some more pieces of information that, that would be
required beyond the schema, the format name and the version, right? And then
we're also looking to, you know, create this as a, along with commenting on this
thread, we're looking to make this into a whole, a working group, right? And so
within that working group, we'll try to figure out how does this, how does this
all, you know, that, that can be our forum for discussion. There was some work,
adjacent work. I think maybe I saw it in the mailing list the other day, the
SPDX abstract syntax tree stuff. It sounds like, look from what I saw, it might
be vaguely related, but we need to pull in everybody who might be working on
this type of thing. And, and, you know, think together about how this works. And
some, some future stuff that we wanted to do is combine scan data, right? So
your static analysis data to understand, are all these the components that are
really in your application, right? So using something like CVE bend tool, we
could augment, right? We output to the open architecture from CVE bend tool, and
then we augment the, the auditor to ensure that if you saw a database in your
scan results, that it better show up in your threat model. Right. So, yeah, I'll
turn it back over to John, but just a call for engagement here. Awesome. Thank
you, John. So wrapping up here, there was a question we did an interview with
Adam Shostak, like it was like one of the most exciting OAuth podcasts we did a
while back. And I think it was like 2018. That's how long ago it was. But there
was a question in there about, can we fully automate threat modeling? Can we,
could we just take the humans out of it? And he responded back with a question
and he said, can you fully automate programming? And then I said, I don't think
so. Not now. But here's something to think about. I just did a look. Yeah. I did
a Google. There's 128 million repos on GitHub right now. And we know every one
of them are quality repos with threat models. No, just joking. But if you took
10% of that or something less, and let's just pretend people decide to threat
model and let's pretend that people are making it publicly available. Right. I
think some teams are, but we just never see it. But let's say they're using this
20 years from now, 10 years from now, it's optimistic. Then you have this huge
corpus of data that you can then start creating your machine learning models
from there. And I think then that's the big picture that maybe then not only
that you have these commits where GitHub is already looking for things, but you
could also have a threat model check and it could come back and give you a
pretty good risk score or whatever you want to call it from that. So there's a
lot more involved here. Again, if we're all speaking the same language and that
language of course is this language and the tools that, and again, the people
are going to build the tools accordingly. So yeah, we do appreciate it. If
there's any questions, please feel free as, as John said, we're looking for
input and we're going to keep moving forward with this. Something's got to
happen. Something has to happen to make these more secure. So we appreciate your
time. Thank you. Thank you. Thank you again, gentlemen. Thank you, John and
John. We really appreciated that talk. I'm actually looking forward to
potentially grabbing the slide deck from you if that's possible. And I'm going
to put the sponsor. Are there any questions before we switch over? Let me take a
look at the chat. Have we taken all the questions? Yeah, it looks like it.


## Rolling Alice: Progress Report 7: June Activities Recap

> https://www.youtube.com/watch?v=UIT5Bl3sepk&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

We've taken the discussions thread, which we were working in, which was the
Alice thread here, 1369. And so this was the original, you know, this was used,
this used to be the the improved data flow docs discussion, which you can see
got migrated, got migrated from an issue. I don't know where that information
is now. Yeah, I converted from issue, but the information's gone. But there is
an issue where this used to live for the even older version of this. So this
started, you know, we're going to improve the data flow docs and on the way we
realized, you know, hey, wait, the next step to everything is to build Alice.
So we did, you know, a lot of trying to suss out the architecture. What is this
thing? You know, what does our mental model look like? And a lot of that work
is captured within the thread here and is not yet exported to what we've done
for June. And what we've done for June is at the bottom of thread. So here's
the second party pull request. So this all started when we went and we said,
okay, we need to split out the plugins and to facilitate the second party set
up so you can have second and third party plugins. And we arrived on this
concept of the manifest, the plugins.json. And then eventually this is
ballooned into, basically, we're going to write this helper who's going to be
Alice, who's going to help us on each of the plugins in the ecosystem. So this
is what this PR now covers. So this covers a lot of different code and then a
lot of just the discussion stuff that was exported. So the pieces of this that
are of interest and to be contributed to. So please pull request the Alice
branch or please comment directly in here. And so these are sort of the pieces
of interest and there's a lot of other code in there that I'm going to clean up
that are some functionality related to stuff that's just been sitting around in
the same branch that need to get cleaned up. So then, okay, so what do we got?
So yeah, so we have the tutorial itself, right? So this is where we're going to
map the existing work streams that we've identified here. As far as chapters
that we've already started in-flight, we're going to try to map this over the
next one to two years. So reach out, get involved if you want to propose a
chapter, propose a tutorial, and then we'll figure out where we can fit it into
the roadmap with other mentors and maintainers to work with you on that. So
then, let's see. So yeah, so we can just jump right into these. The preface is,
I think, let's see, the preface is the one that kind of has a high level sketch
of a few of the chapters that don't have content yet. So I would say let's add
to the, you know, either add the chapters directly or add right to the, you can
either add it either add in either place, right? We'll try to make sure that
it's de-duplicated. So just make sure, you know, you give us enough information
that somebody can build off something, right? Or we'll evolve it. We just, you
know, all brainstorming is good right now. So we need to sort of flush this
out. So okay, let's see. So the Architect Canals Volume Zero, this is the one
where we're, you know, figuring out what is the architecture, how do we
implement the architecture, what are the base plugins, you know, what are some
of this general ecosystem stuff that we have to be aware of, and you know, how
do we work together to define the open architecture as our ecosystem grows,
right? So and you know, primarily looking at things like Web5 and CI-CD
frameworks and KCP to facilitate interaction for execution, and then formats
like SBOM, the SPDX SBOM 3.0 spec, and the 2.3 spec, and all that good stuff in
VEX. And then we're also looking at, there's some work going on with
CVE-BendTool, and they have this triage mechanism. So expanding that triage
mechanism out into sort of this generic policy mechanism, and then there's some
stuff in the thread about dynamic analysis and sort of this adaptive
sandboxing. And so if we can all collaborate, you know, to define a spec there
or to work towards a spec, you know, and perhaps building on this next piece,
which is the manifest ADR, to figure out what does that manifest look like for
that policy. And then at that point, we can start, you know, we'll start, like,
we'll take the CVE-BendTool existing triage mechanism and we'll evolve that
into this policy parser, and everybody can have their own and do different
stuff off of it. But generally, you know, we'll have this policy piece, which
will say, you know, hey, how do we accept CVEs, you know, like, or like, how do
we deal with upgrades of CVEs, you know, what ones just pass through if the
tests pass, you know, like, what's sort of our general pattern of, like, how do
we deal with incoming events? If Alice is maintaining a repo, what's the policy
around how she deals with incoming events? Right, that's sort of how we can
think of that. And then, of course, you know, Alice is the fulfillment of the
software supply chain on demand, right, in a context-aware fashion. So the SBOM
stuff is going to be very critical in figuring out, you know, what is the
existing stuff that we need to fulfill in terms of delivery if that asset is
requested. So let's see. And we're looking, of course, so we're looking at
analysis of existing artifacts, right, via things like CVE vent tool. And then
we're also looking at the definition of the plugins that we're creating and
producing SBOMs for that stuff as well, right, and making that all the same
thing, which is the open architecture. So your SBOM is your, basically your
SBOM, you take your open architecture, you overlay your SBOM, you get a running
application, right. So it, we, yeah. And then, you know, here's shows some
stuff that we're going to do where we're going to get into, you know, threat
modeling and debug and really build this entity Alice off of this Coach Alice
first set of tutorials as the base where we really get deep into how do we be
software developers, right, and what is the thought process that we go through
and model that thought process in Alice's code and in her overlays. So then
we'll get more into, so Alice and the artist strategy. This is going to be more
into the visualizations and how we sort of see the world in the same way. How
do we get to this place where we're all looking at the same picture, including
Alice, right? And so how do we explore visualizations and, you know, just
methods of interaction and communication so that we can basically viewing art
as a mode of communication and as a mode of learning, right, when we're looking
at sort of the machine sense or successful communication as being learning. And
we're going to sort of, yeah, just get into, you know, how do we, how do we use
art to learn? And then we'll flip that in volume three and say, well, how do
you use art, you know, for nefarious purposes, right? And how do you paint a
certain picture with the data that you're distributing on the network or
perhaps the entities who you reward to compute specific data, which then goes
onto the network, right, to sort of corrupt these oracles in your hybrid
on-chain off-chain contract situations. And so that is, you know, that this is,
you know, Alice and the Art of Strategy or, you know, on mind control. And
we'll focus on, you know, effective strategies for controlling one's own mind.
And then through creating this network of agents and detection of the network
of agents of who controls one's own mind, we can develop, adapt, like we can,
we'll develop adaptive policies to modify our threat model to these more
strategic threats that we faced in this hostile open network environment. So in
volume four, then we're going to go into, okay, so now we have, so we have, all
right, so what have we got here at this point? We've got our plans, volume
zero. This is what we're running. This is like our running thing where we sort
of just keep going, keep adding more stuff. It's, if it's high level
architecture stuff. So it's sort of a volume sort of knot. And then it's sort
of a set of tutorials. It's sort of just like our context around the whole
thing Vue does, almost like an ADR and implementation. So, and so when we move,
so this is, yeah, so this is how we've built our architecting Alice. Coach
Alice, this is Alice, the overlays of the software engineer, the software
architect. And then Alice and the art of strategy, you know, the communication
overlays, Alice and strategy of art, you know, these, what become effectively
our security in this new environment, in this new threat landscape. And then
once we have that, now we're like, okay, so we've got our functionality, Coach
Alice. We've got our communication with the outside world, volume two, Alice
and the art of strategy. How does she produce art? How does she interpret art?
How do we interact with her in that way? Volume three, then we have Alice in
the strategy of art. So she's seeing these communications as she's interacting
with the outside world. She knows how to do her job and she knows how to sort
of like respond and adapt in this dynamic way, which is pursuant to her, which
is always in pursuit of her strategic principles. She wakes up every day at the
TIC and she, you know, does her best to align her activities to those strategic
principles between the TIC and the TOC. And then she resets because she's
ephemeral and she goes again, right? So for each job. So in this volume, Alice
and the Health of the Ecosystem, you know, Alice will be out there and she will
be contributing to open source projects, right? And so she will be building up
our open source projects because we don't want to let her out on the whole, you
know, wide world yet. So this is our DFFML ecosystem and of course anybody else
who wants to participate in our little tutorial here. So in that case, we're
basically just going to monitor, I don't know, we need to flush this one out
more. So, but this is basically like, hey, this is right before number five,
which is then, okay, now Alice, here you go, you're going out into the world,
you know, we think you're pretty much, you're almost there. And then at that
point, you know, this is pretty far out, right? This is probably like, we're
looking at like, these are probably a year each, right? So we're probably
looking five years out now. So I don't know, this would, you know, feel free to
propose stuff. And this is, you know, what does she look like in the real
world? What kind of stuff do we have to deal with there? We want to make sure
to get anything and everything. This is, you know, ethical considerations.
We're thinking about, like, we're teaching a real human, right? Don't think of
her as a machine. Think of her as a human. Like, what, how do you be a software
engineer, a software architect in the world, right? And then how do we, you
know, debug your mind like a machine and then make you rolling, which is volume
six, right? And then we basically go in and we say, okay, you've been out in
the world for a year now. How do we fine tune you, right? So volume five is
more of like, okay, like, hey, this is a different environment, you know, it's
not a sandbox anymore, right? Our ecosystem is our sandbox. Volume five is, you
know, okay, no more sandbox. And then volume six is, okay, let's, we were in
firefighting mode for a year. Let's assume this is how these things go. And so
then we come in at volume six and we start to say, okay, now we recognize,
okay, we've taken care of the firefighting mode stuff. Now let's fine tune. And
then finally, once we've fine tuned, then she'll be rolling. And we'll all be
rolling through whatever the other side of this is. And she'll tell us what
that is. So, yeah, so please, pull request and review the Alice branch. And
please feel free to comment in the PR or reach out directly. But yeah, so this
is where we're at for June, basically. So what to expect? Well, we'll be
marching towards RFC v2, probably sometime around the end of December. We'd
like to see, you know, the rest of the ADRs that are involved in this. Let's
see. So, you know, the system context. We'd like to see the rest of Coach Alice
and probably some of Alice in the Arts strategy get fleshed out, just so that
we know what our next one to two years looks like. And then at that point, and
there's a Gantt chart, if you feel like updating the Gantt chart with any of
your plans, you can just update it directly, just make sure that you update
both. And then it'll already be on the Gantt chart. So, great, I think that's
where we're at. So the CI does not work right now. So the demo code that we've
been working on, that does work. So if you want to work on Alice, then it works
right. But the next thing that I'll be doing is I'll be trying to get the
overlay stuff working, and then the first tutorial for the overlay, so we can
get people writing overlays. So yeah, great. All right. Well, thanks, everyone.


## Rolling Alice: Progress Report 10: August Status Update

> - https://www.youtube.com/watch?v=THKMfJpPt8I&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw&index=9
> - https://docs.google.com/presentation/d/1WBz-meM7n6nDe3-133tF1tlDQJ6nYYPySAdMgTHLb6Q/edit?usp=sharing

- Hello entities of the internet!
- We're building Alice, an Open Artificial General Intelligence, we invite you
  to join us.
- Today is Alice's unbirthday. I'm going tell you a little bit about Alice and
  the Open Architecture and give a brief status update on where we're at and how
  you can get involved.
- Who is Alice?
  - Alice will be our developer helper and one day a developer herself. She
    helps us understand and preform various parts of the software development
    lifecycle.
  - We currently extend her by writing simple Python functions which can be
    distributed or combined in a decentralized way.
  - She is built around a programming language agnostic format known as the Open
    Architecture.
  - Eventually we will be able to extend any part of her in any language, or
    have parts be driven by machine learning models.
- What is the Open Architecture?
  - It's the methodology that we use to interpret any domain specific
    description of architecture.
  - We are developing the open architecture so that we can do a one hop on
    analysis when looking at any piece of software from a security or other
    angle.
  - Having this generic method to describe any system architecture allows us to
    knit them together and assess their risk and threat model from a holistic
    viewpoint.
- Why work on the Open Architecture?
  - We want this to be a machine and human interpretable format so that we can
    facilitate the validation of the reality of the code as it exists in it's
    static form, what it does when you execute it, and what we intend it to do.
  - Intent in our case is measured by conference to and completeness of the
    threat model, and therefore also the associated open architecture
    description.
- The entity analysis Trinity
  - The entity analysis Trinity helps us conceptualize our process. The points
    on our Trinity are Intent, Dynamic Analysis, and Static Analysis.
  - By measuring and forming understanding in these areas we will be able to
    triangulate the strategic plans and principles involved in the execution of
    the software as well as it's development lifecycle.
  - We use the Trinity to represent the soul of the software.
- What happens when we work on Alice?
  - We build up Alice's understanding of software engineering as we automate the
    collection of data which represents our understanding of it.
  - We also teach her how to automate parts of the development process, making
    contributions and other arbitrary things.
  - Over time we'll build up a corpus of training data from which we'll build
    machine learning models.
  - We will eventually introduce feedback loops where these models make
    decisions about development / contribution actions to be taken when given a
    codebase.
  - We want to make sure that when Alice is deciding what code to write and
    contribute, that she is following our organizationally applicable policies.
    As outlined maybe in part via our threat model.
- Who is working on Alice?
  - The DFFML community and anyone and everyone who would like to join us.
  - Our objective is to build Alice with transparency, freedom, privacy,
    security, and egalitarianism as critical factors in her strategic
    principles.
- How does one get involved?
  - You can get involved by engaging with the DFFML community via the following
    links
  - Every time we contribute new functionality to Alice we write a tutorial on
    how that functionality can be extended and customized.
    - We would love if you joined us in teaching Alice something about software
      development, or anything, and teaching others in the process.
    - It's as easy writing a single function and explaining your thought
      process.
    - The link on the left will take you to the code and tutorials.
  - We are also looking for folks who would like to contribute from by
    brainstorming and thinking about AI and especially AI ethics.
    - The link on the right will take you a document we are collaboratively
      editing and contributing to.
- Now for a status update. (Progress to date)
  - Alice can make contributions, we've laid the foundations for the automation
    of the software development process.
  - Our next step is to help her understand what she's looking at, what is the
    code, how can she use the source Luke?
- Plans
  - As such our top priorities right now are
    - Ensuring the contribution process to what exists (`alice please
      contribute`) is rock solid.
    - Building out and making `alice shouldi contribute` accessible and ready
      for contribution.
    - Engaging with those that are collecting metrics
      (https://metrics.openssf.org) and ensuring our work on metric collection
      bears fruit.
  - Following our engagement on the metric collection front we will preform
    analysis to determine how to best target further `alice please contribute`
    efforts and align the two with a documented process on how we select high
    value targets so that others can pick up and run with extending.
  - Participating organizations in parallel begin automated outreach via Alice
    please contribute
  - Later we'll get into more details on the dynamic analysis portion of the
    Trinity, where we'll work, over time, across many program executions of the
    code we are working on, to understand how it's execution maps to the work
    that we're doing via our understanding of what we've done (`please
    contribute`) and what we we're doing it on (`alice shouldi contribute`).


## Rolling Alice: Progress Report 11: September Activities Recap

> https://www.youtube.com/watch?v=d8TdNxfDqpU&list=PLtzAOVTpO2jYt71umwc-ze6OmwwCIMnLw

Okay, so we're still trying to build our hybrid Wolfie and Fedora distro with
the SSI service built in. And we're also gonna put the Curie stuff in there
because that sounds interesting. So there's links to that in the logs. Okay,
let's get this going. I think we're gonna be able to remove the Pixy stuff from
this. I think we're gonna be able to move directly into the EFI. Then just set
the init script to be a little thing that'll mount the proc and stuff instead
of doing the RCH root. Just go in and install the bootloader and then run QEMU
with no reboot. Also with the kernel with the EFI to the Linux kernel which
we'll copy out here. We need to copy that out. So that's what we're getting
here. We do copy, copy out. And we're in the DigitalOcean VM again. Okay, I
spun back up and I posted this thing. So copy out kernel for use for first time
bootloader. So this will be our, we can take this and this will be our image
now. Right now we can just spin VM images off Docker files. I've been wanting
to do this for years, but I can never, I never have time. So now I'm finally, I
don't know. I'm gonna, I'm just, I'm sick of it. We need it. We need it. We
need it. Okay. So it's just, it's silly that we don't have this. So copy out
kernel for use. Maybe somebody else has this, but I don't know. I want it to be
very specifically to be two scripts, right? One Docker file, basically three
scripts. One build, right? That's the pipeline. And then the Docker file, which
defines the OS and then the boot with the VM, right? And ideally we can just
get rid of this freaking bootloader installation problem eventually, but a
single pass bootloader installation, I think is an acceptable one. Now this
does require a VM, right? Because we need the loopback to mount on, which is a
restriction for this creation process. But you could potentially use user space
Linux. I'm not sure, but I think that might be an option. If you had to run it
within a container or something, you didn't have access to any sort of
virtualization, but I'm not sure. So eventually we'll get there. Okay, because
we needed to run in any environment. Okay, so yeah, and I just added another
partition. So this is the main thing that we saw. The difference between this
and last is now the Fedora partition is basically a 5GB root and then we have a
15GB root and I updated it from 20 to 30. So we have 10GB of swap or we have
our 500 and we have our 160MB FAT32. It's GPT. It's FAT32, 160MB for the EFI
boot partition. And then it's 10GB of swap. It's 5GB for the Fedora root,
right? Because that's basically just our systemd. And then the rest is Wulfy,
right? For 15GB. And that's the final line. So that's part 4, part partition 4.
So then and so what we did is we flipped. We had Wulfy as the root and then
Fedora as a CH root. But then we flipped it around and you'll see the Docker
files flipped now. So but that was so we still haven't booted into it yet.
Because we're trying to put Wulfy within the Wulfy root because previously we
didn't have a separate bind mount. Okay, and then it just worked out easier.
But now we're trying to do it from from the we're trying to copy them. So we're
going into some issues with move command and then okay, so. And then I don't
know what we could do. What just... success. All right. Okay, so then we'll
have a VM. All right. Wait, what's that? V1 do boot. Okay. Okay. All right.
Okay. Okay. Right. Okay. Well, that's basically the same thing. It's just it's
a copy over because we're copying from the local with an SCP. And this is that
digital ocean. Okay, so it's creating the image it's copying off the so it
started the Docker because I can't I can't figure out how to just export a
Docker container from from where it is. I know that I know there's a way and I
know I should but I don't know what it is. So I just start the container and I
copy it up. Right? So now it's unhappy with us. So. So why are you not happy
with this? So then we copied out and then I wanted to copy because we're the
find mounts. So you can't just copy right into the find mount. But then why are
you trying to RMRF right there? What did you RMRF immediately? Give it to
something. Listen. That should be a problem. Yeah. Okay. Um, also, why would
you do that? Would you do that? Why would you have that? Oh, no, that's
stating. Okay, yeah, do that at the end. Okay. Um, yeah, do that at the end,
please. All right, that's my bad. Let's try that again. Or let's. Okay. So. So
we remove the image, we recopy over the script and we run the script again. The
problem was here. This. Right. Should have been that. So. So now. Just check.
You're not happy. Okay. Done. Yeah, this is where I was. Okay. Do always
before. It's do. Do I can never remember. I just think. I just sit here and I
just. Mess with semi-colon after the do and the done until works every single
time. Yeah. Mine is it done doing it in one after time? But you get that just
to go update. I didn't want to not make a video and so long. So.
