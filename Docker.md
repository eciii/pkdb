dotCloud was founded on 2008 by Solomon Hykes and two other founders. Originally dotCloud was a PaaS provider that, like any other PaaS provider at the time, was in charge of deploying, maintaining and scaling its customers' web applications.

But to do that they also created a whole new paradigm for the packaging, distribution and execution (collectively known as shipping) of software. This new paradigm was based on a set of fairly new Linux kernel technologies at the time, _namespaces_ and _cgroups_, that where collectively known as _Linux containers_ (LXC).

To implement this new paradigm they developed a tool that they called _Docker_. The software shipping paradigm that Docker enabled came to be known later as _Docker containers_.

---

Hykes explained the reasons behind Docker at a [talk in 2013](https://www.youtube.com/watch?v=3N3n9FzebAA). According to him Docker was created to solve the very specific problem of _software shipping_. During the whole life-cycle of a software release, the software needs to move around a lot, from the developers' machines to the testing and integration machines, to the final production servers. All these machines have different hardware and software specifications and the developer needs to be sure that the software will run reliably on all of them. This is the software shipping problem that Docker intended to solve.

At the same talk Hykes also gave examples of how some technologies at the time provided unsuitable solutions to the problem. On the one hand there are the programming language environments, like Python virtualenvs, that provide useful but incomplete software encapsulation because all the system dependencies are left out. On the other hand there are the virtual machines that encapsulate the whole software system but also the hardware, making it a very inflexible and bulky shipping solution.

But Docker, being based on the Linux containers technologies, found the perfect balance and was able to encapsulate the whole software system while at the same time leaving the hardware details out. In the words of Hykes:

> \[Docker is] the result of the last five years working at dotCloud, trying to figure this problem out, and finally reaching a point where we feel like we found a good solution that is simple and elegant enough to share with everyone.

---

Docker was developed in Go, which was at the time a fairly new programming language, and used LXC under the hood.

---

On March 2013, in a [talk](https://www.youtube.com/watch?v=wW9CAH9nSLs) at  PyCon US 2013, Solomon Hykes presented Docker for the first time in public. Shortly after, also in March 2013, Docker was released as open-source.

One year later, with the release of version 0.9, Docker replaced LXC with its own component, _libcontainer_, which was also written in the Go programming language.

On October 29, 2013 dotCloud was renamed Docker.
