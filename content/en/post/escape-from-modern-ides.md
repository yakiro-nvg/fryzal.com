+++
title = "Escape from modern IDEs"
date = 2019-05-16
draft = false
authors = ["Nguyen, Giang (G. Yakiro)"]

tags = ["tool", "architecture"]
summary = "How to build a rich IDE without Microsoft-class engineers?"
+++

Modern development toolset tends to be a “mega editor”. Which offers so
many features that probably makes developer shocked at the first time they
touched it. What is this opera of buttons?

{{< figure src="/img/escape_from_modern_ides/godot.png" title="Godot Game Engine" >}}

I'm a big fan of the basic UNIX tool. Back to early days, there was no IDE
but only a lot of small, independent tool which focused on well-defined problems.
This design obviously is easier to master, loosely coupled and reusable.

# What wrong with mega editor?

Well, "integrated" is a very good factor so I will not blame it anymore, despite
the fact that it does contribute to the problem, indirectly. What actually wrong
is the mega editor. There are examples everywhere, let's think about what tool that
you usually use 10% of feature, but takes more than 5 seconds to open?

To be precise, software architecture isn't that kind of bloat. We know coupling is
bad, and we encourage reusability. Nothing prevents mega editor's software components
from being well-designed. Actually, most of backend components does. We don't need
to bring up the whole IDE to compile an application.

However, let's say what we want is just to change the position in a GUI layout. Why
did we need to touch an IDE, then wasted a couple of seconds to wait for loading of
bloated component, that we don't use?

# Micro service & Micro frontend

The concept is simple, micro-frontend = independent frontend components. Then
combined with micro-backend, we have a micro-tool. Which could be developed, tested,
deployed and used independently from the whole. There is no need to open unnecessary
tool if there is no integration demands.

{{< figure src="/img/escape_from_modern_ides/micro-tools.png" title="Vertical micro-tool stacks" >}}

# Framework is dangerous

Novice system architect tends to build "framework" for everything. This is a big leap
forward from junior engineering roles, who never thinking too much on architectural
viewpoint. I also encourage my colleague to do that, just for education purpose. Frankly
speaking, most of frameworks which I saw is crap.

The biggest problem is, framework requires all of your component to conform to a
specific libraries and technologies. That introduces system-wide coupling which may
becomes serious problem in large scale projects. Framework tends to improves productivity
and reusability etc. However, it cursed your codebase which makes any significant change
to it is virtually impossible. There are just so many things that just break and need
to be fixed at the same time.

---

Most of IDE forces us to stuck with a framework. Which sounds not bad until we got
some counter example. What happen if we need to use a specific GUI / rendering library,
proprietary or legacy tool integration?

We can't eliminate framework at all, as it's good for "integration". We want seamless
experience so there is a good point for it. However, be careful about what we added,
keep it simple and small. In micro-tool architecture, we split core business and
integration to two separated layers. There is no framework restriction at the core
business layers, feel free to use whatever you want. The integration layer is a good
candidate for framework. However, we don't force tool writers to stick with an
implementation.

# Microtools integration

Usually, a working session starts with just a few tools, may be only one. After-while,
more tools will be spawned, but we still expect the newly one be able to talk with the
currents. Let's say we are using UI viewer, then we noticed that a sidebar animation
feels not quite right, so it would be tuned a little bit.

Animation editing is pretty complex so it requires another editor. Fortunately, this
was "deeply" integrated, so what we need to do is just hitting a button, it will
automatically bring up their buddy so we could change the animation and review its
impact in the UI viewer at the same time.

But, what happen if there is an animation editor which was opened already. Maybe we
should reuse this instance. Because, changes was autosaved so it's still be ok to just
switch the object that is being edited. This UX is standard for most of IDE nowadays.
There are components that people don't want to see more than one instance per context.

{{< figure src="/img/escape_from_modern_ides/sa-luggage.jpeg" title="\"Swiss army luggage\". Photo by <a href=\"https://unsplash.com/@randomlies?utm_source=medium&utm_medium=referral/\">Ashim D'Silva</a> on Unsplash" >}}

What will decided whether a tool instance should be reused or not?

At the highest level of the toolset, we need a component called "Workspace". Which is
required to ensures integrated user experience. It composites micro-tool GUI, runs
policies, manage modes and so on.

---

Now, as the number of tools grow up, they suffers from fragmentation. There are common
GUI components which could be shared among these tools. How to make them reusable, even
if they are using different technology?

{{< figure src="/img/escape_from_modern_ides/sketch-inspector.png" title="Sketch Inspector" >}}

By more decomposition, we could treat these components as micro-tools too. Inspector looks
quite complex and vary a lot. In fact, it is abstracted & reusable. We can make a
micro- inspector, which works on generic schema likes JSON. We are free to use whatever
technology stack since it exposes API over IPCs.

# Fail-safe Architecture

In practice, fail tolerance is very importance for tool development. Nobody want to
waste 2 hours of work just by an unexpected crash. Modern IDE pays lot of attention to
ensure that no stupid fail could become a disaster.

Depends on what could be considered as "stupid", I doubt my toolset could be just full
of "stupid crashes". I will try my best to not ship it to you. However, as long as it
still be created by a human, bugs are simply unavoidable.

---

To be confident about the possibility of zero unexpected failure. We need to pay an
unreasonable cost, both money and time to do endless of testing e.g. unit tests, integration
tests, formal proofs etc. However, what is the point of million dollars investment to try to
hunting down all of that stupid bugs, while just only one of "not so stupid bug" could
destroy everything?

> Program testing can be used to show the presence of bugs, but never to show their absence.
>
> -- Edsger W. Dijkstra.

{{< figure src="/img/escape_from_modern_ides/fail-safe-bike-locking.jpeg" title="\"Fail-safe bike locking\". Photo by <a href=\"https://www.flickr.com/photos/dustinq/\">Dustin Quasar</a> on Flickr" >}}

The best approach to achieve fail-safe, considering return on investment and complexity
of an IDE is just "let it crash". Of course, I don't encourage you to not testing at all
and just ship bugs to the user. Aiming for fail-safe, we should prioritize "recover from
an expected crash" prior testings.

---

So, how "let it crash" achieves architectural fail-safe guarantee?

First, we need to reduce the scope that an arbitrary crash could impact. In this case,
it is an OS process. Depends on the technology stack of each tool, further reduction of
impact scope could be available. However, an OS process is the smallest unit that we care
about from architectural viewpoint.

Then, a good practice is to decompose each tool even smaller. At least, the layer that
handles business logic should be separated from presentation. Not only for separated of
concerns, but also for recoverability from any front-end crashes, as long as the state
was not harmed.

# Conclusion

Micro-tool is a better approach for IDE development. However, coordination between these
tool is key for integrated user experience. In next posts, we will dig deep into these
problem and try to make an PoC, which may be valuable than just theory.