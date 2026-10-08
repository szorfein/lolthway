---
title: "How to use the Vercel CLI"
description: "Link and deploy your project faster without having to enter into the site web."
date: 2026-10-08
lang: en
category: notes
tags: ["Vercel", "Tools"]
cover: notes
---

First, you have to create a free account on [Vercel](https://vercel.com/signup)

## Install the tool

You probably need to configure your shell for `npm` with

```zsh
PATH="$PATH:$HOME/.npm/bin"
export npm_config_prefix="$HOME/.npm"
```

After that, just install vercel globally

```sh
npm i -g vercel@latest
```

## Login to your account

```sh
vercel login
```

## Deploy a new project from a directory

For a single repo with one project.

```sh
cd lolthway
vercel
```

You will have some question/answer:

```sh
> Create a new project
> Name? lolthway
> Connect this Git repository to automatically deploy changes on every push? y
> Detected Astro (dist)
> Customize settings? N
```

After all that, your project should be deployed at the address `https://lolthway.vercel.app`

## Add environment variable

```sh
vercel env ls
vercel env add SITE_URL
```

And write the value you want e.g: `https://lolthway.vercel.app/`

## Link your project to a Git(hub) repository

```sh
vercel link
```

Like this, every commit will trigger a new deployment to Vercel.

## Deploy

See if all is ok with:

```sh
vercel deploy
````

And if no error:

```sh
vercel deploy --prod
````

## Manage other projects

```sh
vercel project ls
vercel project add
vercel project rm
vercel project inspect [project-name]
```

## Logout

```sh
vercel logout
```
