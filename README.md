# Dance Social

**Dance Social** is a working prototype for an invitation-led social network and community platform built specifically for dancers.

The product is intended to become a digital clubhouse for dance: profiles, communities, persistent conversations, events, social connections and dance-specific utilities in one place.

The working brand is **Dance Social**. Other names under consideration include **Dancebee**, **DanceSync**, **Dancebit** and **Dance a Little**.

## Product Idea

Dance communities are currently spread across Instagram, WhatsApp, Telegram, Facebook, event pages and school websites. These tools are useful, but the dancer's identity, community discussions and event graph are fragmented.

Dance Social brings those pieces together around a dance-specific social graph.

A useful shorthand is:

> Orkut-style identity and communities + Reddit-style persistent discussions + Meetup-style events + a dance-specific social graph.

The product is designed for many dance scenes, including Lindy Hop, Salsa, Bachata, Argentine Tango, Balboa, West Coast Swing, Solo Jazz, Kizomba, Hip-Hop, K-pop, folk, contemporary and other community-driven dance forms.

The initial launch wedge can be a dense real-world scene such as Istanbul Lindy Hop, while the product itself remains cross-dance and cross-city.

## Vision

Create the default digital home for dancers: a trusted network where people can discover communities, find events, meet other dancers, organize activity and maintain a persistent identity around their dance life.

The long-term ambition is larger than a feed. The network can eventually connect dancers, instructors, DJs, schools, organizers, venues, festivals and dance-related businesses.

See [PRODUCT.md](./PRODUCT.md) for the full product vision and business model.

## Current Prototype

The repository currently contains a client-side prototype in `index.html`.

Implemented prototype flows include:

- demo login,
- invitation-code onboarding,
- dancer / beginner / teacher / organizer roles,
- dance-style selection,
- personal profiles,
- profile editing,
- public/private profile setting,
- following and mutual connection state,
- opening other users' profiles,
- profile posts,
- community creation,
- public/private communities,
- community feed, members, events and about tabs,
- posting into a personal profile or selected community,
- event creation,
- public/private events,
- local persistence in the browser.

### Demo Login

- Username: `test`
- Password: `test`

Demo invitation codes currently include:

- `DANCE2026`
- `LINDY2026`
- `SALSA2026`
- `TEST`

These credentials exist only for prototype testing.

## Important Prototype Limitation

The current application uses browser `localStorage`.

It is **not** a production backend and does not provide real security, shared multi-user state or server-side privacy enforcement.

Public/private controls in the current prototype demonstrate product behavior only. Production privacy must be enforced by the backend and database authorization layer.

## Intended Production Stack

The current direction is:

- modern React/Vite or Lovable-generated frontend,
- Supabase Auth,
- PostgreSQL through Supabase,
- Supabase Storage,
- Supabase Row Level Security,
- Supabase Realtime where useful,
- server/edge functions for trusted backend operations.

See [cloud.md](./cloud.md) for architecture and deployment guidance.

## Core Product Loop

```text
invite
  -> onboarding
  -> profile
  -> follow dancers / join communities
  -> discover or create events
  -> post / comment / interact
  -> attend real-world dance activity
  -> return
```

The product should optimize for real-world participation and useful community activity rather than maximizing passive screen time.

## Product Principles

- People over content volume.
- Communities over creators.
- Real-world participation over screen time.
- Persistent knowledge over disappearing chat.
- Trust over maximum reach.
- User choice over opaque engagement ranking.
- Bots and AI must always be clearly labeled.
- Privacy rules must be enforced server-side in production.

## Repository Documents

- [PRODUCT.md](./PRODUCT.md) — product definition, vision, target audience, features, business model, risks and limitations.
- [PRODUCT_ROADMAP.md](./PRODUCT_ROADMAP.md) — immediate, short-, medium- and long-term roadmap.
- [agent.md](./agent.md) — instructions and constraints for coding agents working in this repository.
- [cloud.md](./cloud.md) — target cloud architecture, Supabase model, deployment and security guidance.

## Next Build Priority

The next major milestone is converting this prototype into a real multi-user private beta.

Priority sequence:

1. Create the Supabase project and schema.
2. Replace demo login with real authentication.
3. Implement real invitation records.
4. Persist profiles, follows, communities and posts in PostgreSQL.
5. Add server-enforced privacy with Row Level Security.
6. Persist events and attendance.
7. Add comments, likes and notifications.
8. Add recurring events.
9. Seed the first real dance communities and invite the first beta users.

The smallest meaningful private-beta test is:

> Do real dancers use the product to decide where to dance, interact with their community and return without being manually prompted?

## Status

**Stage:** interactive product prototype  
**Backend:** not yet implemented  
**Data:** local browser storage  
**Launch mode:** invitation-led private beta planned
