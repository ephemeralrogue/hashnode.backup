---
title: "Rootless Docker, Docker Contexts, and Docker Compose"
seoTitle: "Rootless Docker, Docker Contexts, and Docker Compose"
seoDescription: "Learn Docker Rootless Mode, context switching, and security best practices to improve container security while managing privileged contexts"
datePublished: Wed Jan 29 2025 22:47:48 GMT+0000 (Coordinated Universal Time)
cuid: cm6ihyq36000i09ld9usddvc1
slug: rootless-docker-compose-contexts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1738189557818/0d12cd29-d5ed-4f2a-bead-99e05cde9a3b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1738190836088/39b12ac3-f2ed-4c63-b94a-f919da471f76.jpeg
tags: compose, virtual-machine, linux, docker, containers, docker-compose, container-hardening, rootless-mode, rootless-docker, principle-of-least-privilege, mandatory-access-control, discretionary-access-control

---

Ever since i went down the cybersecurity rabbit hole, I’ve brought an awareness of security and systems hardening to all of my projects. Shortly after that little deep dive, I learned about container breakouts and best practices for securing containers, though it wasn’t until I began using virtual machines for practically everything that I learned about running Docker in [Rootless Mode](https://docs.docker.com/engine/security/rootless/).

note: this is written with Linux in mind. I run Debian virtual machines (VMs) for all of my development purposes, and so what I write applies predominantly to the Linux ecosystem. I have done only a cursory search with regard to container hardening and abstraction on Windows and macOS, and it appears to me options there are limited.

But I could be wrong. This is, however, a topic for another day. I’m really only keen on providing a solution to context switching in Docker Compose, as I ran into trouble with this and locating the solution was far more challenging than it should have been. If you’re interested in the overview, read on! If you want to skip to the solution, scroll down to “Switching Contexts.”

## What is Rootless Mode?

Docker containers, by default, run with escalated privileges, which, from a security standpoint, really is as bad as it sounds. When it comes to reducing your attack surface, you want to limit everyone and everything to having access strictly to what they need to operate and nothing more. This is the [Principle of Least Privilege](https://www.cloudflare.com/learning/access-management/principle-of-least-privilege/) and guides [Defense-in-Depth](https://www.cloudflare.com/learning/security/glossary/what-is-defense-in-depth/) systems like [Mandatory Access Control](https://en.wikipedia.org/wiki/Mandatory_access_control) and [Discretionary Access Control](https://apps.dtic.mil/sti/pdfs/ADA392813.pdf). A container running with escalated privileges thus violates a number of these directives.

With a deep knowledge of Linux capabilities and capability sets, you can use the `CAP_DROP` and `CAP ADD` directives in a Dockerfile, Compose file, and on the command line, to lock containers down. This is generally useful, but it assumes you’re running your containers in a local Linux environment, or deploying them to one, and as a number of assholes made it clear to me on Threads not too long ago, most software developers aren’t using Linux locally.

Beyond the operating system you’re using, and beyond having the knowledge of limiting system accessibility through capability choke points, there remains the fact that the main Docker process is running with privilege, which underlies the security of the whole system. This means anyone who has the capacity to run Docker commands has the capacity to gain escalated privileges over the host. This is just one concern [among many](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=docker).

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">Never compromise security for convenience.</div>
</div>

Rootless Mode for Docker abstracts the system process and sets it up to run under a non-root user. Instead of one Systemd service governing Docker, each user on a system has access to their own Docker service. This comes with additional trade-offs and takes a few extra steps to set up, but you should never compromise security for convenience. Once it’s set up, you’ve moved closer to operating under the Principle of Least Privilege.

Of course, there will be times when a containerized service you want to run requires privileges inaccessible in Rootless Mode. That’s where Docker contexts come in.

## Docker Contexts

Contexts in Docker aren’t new. If you’re familiar with Docker contexts, you’ve probably used them to run containers remotely. This is very cool, and very useful, but this feature can also be used in switching between privileged and unprivileged contexts.

```bash
docker context ls
```

When you set up Docker to run in Rootless Mode, it creates a `rootless` context and enables it automatically. Without any awareness of what’s happening under the hood after running the rootless script, all new Docker containers run in this rootless context. Furthermore, if you follow the script in setting up Rootless Mode, you eliminate “Rootful” Docker as part of the process.

Or so it seems.

In reality, there’s a sort of port that occurs, with Rootless Mode re-instantiating a system-wide systemd Docker service. For our purposes here, this means two things:

1. you have finer granular control over who has access to what. Remember that whole “Principle of Least Privilege” bit? It’s far easier to lock down who has access to the system-wide Docker socket; everyone else will have there own, and
    
2. a context exists in which you may run containers with escalated privileges when needed.
    

Being able to switch contexts means you’re not locked in to one or the other, though at first glance it may seem so. With `docker run`, it’s as simple as including the `--context` flag when running the command:

```bash
docker run --context default
```

Switching contexts when running the Docker Compose plugin is as straightforward as well, though the syntax isn’t very clear. I ran circles on the internet looking for how to do this appropriately. Here’s what I learned:

## Switching Contexts

I do not have standalone `docker-compose`, which may have simplified things a bit, as the syntax for switching contexts when spinning up an app is as straightforward as running plain ol’ `docker run`:

```bash
docker-compose --context default up
```

Using the plugin however, not so much:

```bash
docker compose --context default up
# this failed: "unknown flag: --context"
```

What I found infuriating was an internet search filled with results about `context` in the `build` directive in a Docker Compose file, and absolutely nothing about switching contexts on the command line when executing with the Docker Compose plugin. Even worse was the lack of explicit information regarding command line options in the Docker Compose documentation. Not even `--help` was helpful.

It wasn’t until I came across [this discussion](https://github.com/docker/compose/issues/9240) on the Docker Compose repository that I was given the syntax I needed to make this work:

```bash
docker --context default compose up
```

This shouldn’t have taken me a couple hours to locate. There are a lot of assumptions being made in the documentation, and it irks me that there isn’t a resource with every fucking possible permutation available.

```bash
docker [flag] [value] [plugin] [command] [flag] [value]
# docker flags first, then plugin, then plugin flags
# is this documented anywhere? because if it is, it's not easy to find
```

All the cheat sheets I looked up have the usual `docker compose up`, `docker compose down`, etc, and nothing about this shit.

Ah well, here we are. I’m done bitching about it.

Now, why would I want to context switch like this? This allows me to keep the `rootless` context in use, but use the `default` context for specific apps. In the case of the app that sent me on this quest, I’m still working on figuring out what capabilities one of the containers needs in order to operate properly without providing it escalated privileges in full. While that’s underway, I can at least run and use the app.

## Final Thoughts

Is it a good thing to be able to switch between privileged and unprivileged contexts? I think, with regard to the first point mentioned above, this provides greater flexibility when managing a system with multiple users. With regard to the second point, the jury (meaning me and my own considerations) is still out. it seems to me it would be better to figure out what privileges a container absolutely requires to run, and provide those capabilities on the command line. This, however, requires the user to have the ability to provide those capabilities. On a system running SELinux, this is, perhaps, viable. Otherwise, this could present additional challenges.

This is all a learning process. As I work to harden my system, my containers, my deployments, and my process, I’m working to implement better security practices where possible. And because this is a constant learning endeavor, I am often at odds with how I think I should be operating and where I actually am operating. There is still much to learn and much to do and building shit is how I’m going about doing it. One day, I’ll be running Docker entirely in Rootless Mode without having to touch the privileged context, with total granular control over what apps need what capabilities. Until then, context switching it is.