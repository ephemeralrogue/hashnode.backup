---
title: "so many projects. so little time."
datePublished: Mon Jan 13 2025 02:15:05 GMT+0000 (Coordinated Universal Time)
cuid: cm5uevt5e000009mhgcru8glc
slug: so-many-projects-so-little-time
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736733369869/698b0a20-3e34-440f-acc9-980c8916d92a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1736733472855/d50b8b7e-b56b-4883-a12b-9dbd7d58c225.png
tags: design-repository, the-magi, hypersion-archive

---

i have my hands in innumerable projects, and i’ve taken to writing about them in long form in an effort to help keep everything in order in my brain. where do i begin?

## the design repository

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736733752237/c9955876-0621-4c9d-b0dc-119d7e015d17.png align="center")](https://github.com/LVNACY/design-repository)

the most recent thing i’ve been diving into is [the design repository](https://github.com/ephemeralrogue/design-repository), which i’ve written at length about [here](https://blog.ephemeralrogue.xyz/the-design-repository). as far as an open source project is concerned, i have decided to produce this as a bit of a white label/customizable piece of software. as it costs money to deploy a repo from a GitHub organization to Vercel, i intend to posit this as a simple framework by which others can fork and create their own resource database, using the frontend to easily explore all they add. in the future, if any funds roll in via sponsorship, i’ll roll it out and provide rails for collective contribution to the main database.

## the magi

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736734440147/fe7d9900-470c-49fe-9af1-4aa13e9a166d.png align="center")](https://github.com/ephemeralrogue/themagi)

the magi is the supercomputer underlying Nerv’s operations and providing the Evangelion with the data they need to overcome the angels in Neon Genesis Evangelion. i carry no grand ambitions with the project to make it something as integral to the survival of mankind with regard to human instrumentality, i simply like the name and find it applicable: the magi, the three wise men, for a three node deployment of elasticsearch. right now, the project exists as a simple docker compose file, alongside a number of configuration files. the end goal is to create a simple deployment for the consumption of logs of software in local development. more a pet project than anything, i hope to just learn learn learn about docker compose, container management, and eventually kubernetes via minikube. this week, i briefly reviewed some issues with the deployment, though i made little progress on correcting some of them.

## hyperion archive

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1736733687187/358d73b7-d812-4f04-884f-51152fa32ab7.png align="center")](https://github.com/ephemeralrogue/hyperion-archive)

the [hyperion archive](https://github.com/ephemeralrogue/hyperion-archive) is just a fun name i slapped on the docker compose configuration for running [AnythingLLM](https://anythingllm.com) with [Ollama](https://ollama.com) as its base llm. in an effort to deeply understand operating containers, i’m working to run whatever i can as containers. in this case, both AnythingLLM and Ollama provide dockerized versions of their software, so i worked on booting them up together via docker compose. in addition to this, i created a template for setting up a systemd service to spin up the containers when i want to use the service, and shutting them down when i finish. everything is, of course, easily customizable. if you wanted to run the service with persistence, you can simply enable the service, or edit the docker compose file and set the containers’ restart behavior to true.

## in closing

i think that’s it for this week. i have another project i’ve been taking a break from, which i’ll be returning to this coming week: integrating Stripe with a Discord bot. i want clients to easily manage their subscriptions from within the Discord server, along with providing role automation based on what service a client is subscribed to. so many things to work on. so little time.

i’m not all too prolific a poster, but you can find me on Bluesky [here](https://bsky.app/profile/ephemeralrogue.xyz).