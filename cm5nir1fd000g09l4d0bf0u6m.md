---
title: "the design compendium"
seoTitle: "Design Compendium Overview"
seoDescription: "Explore building a design repository to enhance frontend skills and organize resources, using Next.js and GraphQL for dynamic presentations"
datePublished: Wed Jan 08 2025 06:28:58 GMT+0000 (Coordinated Universal Time)
cuid: cm5nir1fd000g09l4d0bf0u6m
slug: the-design-compendium
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1737427412847/9f4ec1b2-cc1e-4315-ab72-0f16fd7d2b87.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1736316658811/84512be8-c5b7-4756-a9fa-0853a92e5019.png
tags: frontend-development, design-repository

---

i’ve been sitting on an idea for a design repository. i do graphic design, along with wearing a host of other hats for an online fitness company i run. being sole proprietor and sole employee means i do everything, including development. but that’s neither here nor there. as part of the work i do designing fliers and posters and social media graphics, i’m constantly on the lookout for resources and assets i can add to my repertoire.

because [i know nothing about frontend development](https://blog.ephemeralrogue.xyz/i-know-nothing-about-frontend-development), i want to build something that will not only help me develop frontend design skills, but also be useful. there are a ton of repositories on GitHub, and many of them are phenomenal, like Brad Traversy’s [design-resources-for-developers](https://github.com/bradtraversy/design-resources-for-developers). however, i haven’t found a useful and well-designed website that provides listings of resources they way they’re organized in READMEs across all of these different repos. so, i’m gonna build one. and it’s likely gonna suck. and that’s okay.

you gotta be okay with being terrible when you’re first learning. you get good when you’ve been terrible enough but have kept at it.

## the concept

here’s a rough concept of the web stack for the site:

![an illustration of the technologies to be used for the webstack along with notations. these are described later in the article.](https://cdn.hashnode.com/res/hashnode/image/upload/v1736301933889/65e4ecf8-8ec9-4a45-aff4-68d77bbb9641.jpeg align="center")

obviously, i’m running node. the frontend will be built using the Next.js framework. i’ll build a GraphQL API to interact with the MongoDB backend, though this will be for retrieval of data only. no additions to the database will be made through the frontend.

each resource will be stored as an object with the following schema:

```javascript
{
    id: String,
    title: String,
    description: String,
    URL: String,
    tags: []
}
```

data will be pulled by tag and organized on the frontend accordingly, so resources with multiple use-cases may appear in any number of categories.

## considerations for contributions

i have no way of anticipating if this project will gain any public attention and interest whatsoever. but if it does, this is how i would like to manage contributions to the resource database:

![illustration of planned contribution workflow, described later in the article.](https://cdn.hashnode.com/res/hashnode/image/upload/v1736310983357/706c7103-ad66-45ac-a3d9-b190ba47d28b.jpeg align="center")

basically, proposed resources are vetted via general discussion and review on GitHub or Discord. i plan to draft guidelines for what constitutes quality resources to be added. once a suggested resource is accepted, a moderator, maintainer, or admin will run a command to push the update to the database. because pages on the site will be dynamically rendered, the resource should appear on the site as soon as the entry is made in the database.

theoretically, of course. i could have this all bass-ackwards, because, again, [i know nothing about frontend development](https://blog.ephemeralrogue.xyz/i-know-nothing-about-frontend-development), but it would be pretty awesome if it works!

i’ve created a public repository on GitHub for this project, and it is currently quite empty, but if you have any desire to keep up with or potentially contribute to project development, feel free to give it a star and drop me a line on [Bluesky](https://bsky.app/profile/ephemeralrogue.xyz) or [Discord](https://discord.gg/nh7mqGEfbw). the repo can be found here:

[![Two pencils on a bright yellow background with the text "the DESIGN COMPENDIUM" in bold white letters.](https://cdn.hashnode.com/res/hashnode/image/upload/v1737427450852/5d7ff7d7-d25b-44c8-9ff0-f7abde29ce59.jpeg align="center")](https://github.com/LVNACY/design-compendium)