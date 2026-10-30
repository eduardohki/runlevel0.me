+++
title = "Home"
description = "Software Engineer & Technical Leader in Munich."
url = "/"
+++

<style>
  .profile-container {
    display: flex;
    align-items: center;
    gap: 2rem;
    margin-top: 2rem;
  }
  @media (max-width: 768px) {
    .profile-container {
      flex-direction: column;
      align-items: flex-start;
    }
  }

  /* Shared look with the CV page: soft cards with a neutral edge. */
  :root { --home-card: #ffffff; --home-card-edge: #c4c9d4; --home-muted: #6b6b6b; --home-line: #d9dbe0; }
  @media (prefers-color-scheme: dark) {
    :root { --home-card: #262934; --home-card-edge: #4a5060; --home-muted: #8f94a0; --home-line: #3a3e4b; }
  }
  main blockquote, .home-card {
    background: var(--home-card); border: 1px solid transparent; border-left: 4px solid var(--home-card-edge);
    border-radius: 10px; padding: 0.9rem 1.1rem; margin: 0.9rem 0; font-style: normal;
  }
  main blockquote p { margin: 0; }
  main blockquote em { font-style: normal; }
  main blockquote p + p { margin-top: 0.5rem; font-size: 0.9em; color: var(--home-muted); text-align: right; }
  .home-card p { margin: 0.2rem 0; }
  .home-card .home-meta { font-size: 0.9em; color: var(--home-muted); }
  .home-cv-link { font-size: 1.15em; }
  .home-links { display: flex; flex-wrap: wrap; gap: 0.3rem 1rem; }
  .home-links a { text-decoration: none !important; }
  @media print {
    main blockquote, .home-card { border: 1px solid var(--home-line); }
  }
</style>

<div class="profile-container">
  <div id="picture">
    <img src="/images/profile-picture.jpg" alt="Eduardo Hernacki, Software Engineer & Technical Leader" style="max-width: 250px; border-radius: 5px;">
  </div>
  <div id="title" style="align-self: flex-start;">
    <div style="text-align: left; margin-top: 0; line-height: 1.6;">
      <h1 style="font-size: 1.6rem; line-height: 1.6; margin: 0;">Eduardo Hernacki</h1>
      <div style="font-size: 1.4rem;">Software Engineer & Technical Leader</div>
      <div style="font-size: 1.2rem;"><em>Munich, Germany</em></div>
      <div style="margin-top: 0.5rem; font-size: 1.1rem; line-height: 1.5;">
        Hands-on practitioner who uplifts teams and practices, with expertise in the cloud and AI-augmented software engineering. Over a decade in regulated industries, and an organizer of Munich's BEAM and functional programming meetups.
      </div>
    </div>
  </div>
</div>

## How I Work

I lead by building: from architecture decisions to production code, setting engineering standards, pairing and running workshops. I enjoy tackling complex problems as part of a team, and I'm comfortable representing that team towards other teams, functions and stakeholders, technical or not.

Over more than a decade in regulated industries, I've learned that most software problems start with a lack of clarity, and that the way a team is structured shapes the software it builds.

## What People Say

> *"His communication skills are excellent, enabling him to effectively engage with both technical and non-technical stakeholders and drive alignment across different teams."*
>
> --- Delivery Lead at a financial services client

> *"Eduardo is an outstanding engineer --- a natural problem solver with deep cloud expertise. His impact reached far beyond the role he was hired for."*
>
> --- **Katherine Garrod**, former CISO & VP Digital Workplace, [PPRO](https://www.ppro.com)

> *"Eduardo quickly integrated into our team and contributed not only with his deep technical knowledge but also with an excellent collaborative spirit. He demonstrated impressive skills in planning and implementing solutions, was proactive, dedicated, and always open to feedback. Additionally, his positive attitude and strong interpersonal skills made working together a truly enjoyable experience."*
>
> --- **Alina Schuster**, Technical Product Owner, [SCHUFA Holding AG](https://www.schufa.de/)

> *"Working with Eduardo as a Principal Platform Engineering Consultant has been truly transformative for BetterDoc. He enabled us to establish and demonstrate compliant operations across our applications and infrastructure, ensuring we met the demanding regulatory and security standards."*
>
> --- **Steffen Glomb**, former Chief Product & Technology Officer, [BetterDoc](https://www.betterdoc.org/)

## What I'm Working On

These days I work on AI-augmented Engineering in the financial services industry, alongside the teams that use it, helping them get from intent to working software faster without compromising quality. AI amplifies whatever process it lands in, so most of the work happens around the model: clear requirements and the right context before any code, verification built into the flow, and shared standards many teams can rely on. It also means knowing where to stop: agents shouldn't guess what nobody has decided, replace an engineer's understanding, or touch data they shouldn't. The result is faster feedback, less toil and a shared language between product and engineering, while the team stays accountable for what it delivers.

## The BEAM & Functional Programming

I prefer functional programming, and I lean towards [Gleam](https://gleam.run) for modeling business logic and building web applications. I organize the [Elixir, Erlang & Gleam Munich Meetup](https://www.meetup.com/elixir-munich/) and the [FP Stammtisch Munich](https://luma.com/fp-munich), and I'm always keen to hear from teams running the BEAM in production.

## Talks

<div class="home-card">

**Functional Domain Modeling with Gleam**

How a strong type system and pure functions can make business logic readable to the people who own it, not only to engineers.

<span class="home-meta">[Code BEAM Europe 2025](https://codebeameurope.com/talks/functional-domain-modeling-with-gleam/) · Berlin · Nov 2025 · [Recording](https://www.youtube.com/watch?v=15SczP3dNG4)</span><br>
<span class="home-meta">[Elixir, Erlang & Gleam Munich Meetup](https://www.meetup.com/elixir-munich/events/304076641/) · Munich · Feb 2025</span>

</div>

<div class="home-card">

**Running Elixir on AWS without Kubernetes**

Why Kubernetes can be overkill on AWS, and how to run BEAM applications on ECS Fargate with AWS CDK, so teams can own their infrastructure in an integrated, cost-effective and secure-by-default way.

<span class="home-meta">[Elixir, Erlang & Gleam Munich Meetup](https://www.meetup.com/elixir-munich/events/310306789/) · Munich · Sep 2025 · [Recording](https://www.youtube.com/watch?v=SUexJAYotKM&t=479s)</span>

</div>

## Background

- Over a decade in software and cloud across financial services, payments, credit, healthcare and manufacturing
- Claude Certified Architect - Foundations (Anthropic, 2026)
- Based in Munich, fluent in **German, English and Portuguese**

<p class="home-cv-link"><a href="/cv/">Full resume/CV available here →</a></p>

## Get in Touch

Happy to connect, whether it's about software, technical leadership or the BEAM. I'm always on the lookout for speakers and hosts for our Munich meetups.

<p class="home-links"><a href="mailto:&#115;&#101;&#114;&#118;&#105;&#99;&#101;&#115;&#64;&#114;&#117;&#110;&#108;&#101;&#118;&#101;&#108;&#48;&#046;&#109;&#101;">&#115;&#101;&#114;&#118;&#105;&#99;&#101;&#115;&#64;&#114;&#117;&#110;&#108;&#101;&#118;&#101;&#108;&#48;&#046;&#109;&#101;</a> <a href="https://linkedin.com/in/eduardohki" target="_blank" rel="noopener noreferrer">LinkedIn<span class="sr-only"> (opens in a new tab)</span></a> <a href="https://github.com/eduardohki/" target="_blank" rel="noopener noreferrer">GitHub<span class="sr-only"> (opens in a new tab)</span></a> <a href="https://genserver.social/runlevel0" target="_blank" rel="noopener noreferrer">Mastodon<span class="sr-only"> (opens in a new tab)</span></a> <a href="https://bsky.app/profile/runlevel0.me" target="_blank" rel="noopener noreferrer">Bluesky<span class="sr-only"> (opens in a new tab)</span></a></p>
