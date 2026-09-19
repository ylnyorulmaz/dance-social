# Claude Guide

This file provides project-specific instructions for Claude or any Claude-based coding agent working in the Dance Social repository.

Read these files before making significant changes:

1. `PRODUCT.md`
2. `PRODUCT_ROADMAP.md`
3. `agent.md`
4. `README.md`

The current user request always takes priority over these documents.

## Product Mission

Dance Social is an invitation-led social network and community platform for dancers.

Its purpose is to help real dancers:

- discover people,
- join communities,
- find events,
- organize events,
- maintain a dance identity,
- continue persistent discussions,
- and turn online activity into real-world dance participation.

It should feel like a digital clubhouse for dance, not a generic engagement-maximizing social feed.

## Current Repository State

The current prototype is primarily implemented in:

- `index.html`

Current technology:

- plain HTML,
- CSS,
- JavaScript,
- browser `localStorage`.

Current data is local-only and demo-oriented.

Existing prototype capabilities include:

- demo login,
- invitation-code onboarding,
- dancer roles,
- dance-style onboarding,
- personal profiles,
- public/private profile setting,
- follow/unfollow,
- mutual connection state,
- other-user profiles,
- profile posts,
- communities,
- public/private communities,
- community feeds,
- members,
- community events,
- event creation,
- public/private events.

Do not describe these local-only implementations as production-ready.

## Product Direction

The intended next major transition is from a local prototype to a real multi-user private beta.

Preferred first production architecture:

- React/Vite or Lovable-compatible frontend,
- Supabase Auth,
- Supabase PostgreSQL,
- Supabase Storage,
- Supabase Row Level Security,
- Supabase Realtime only where useful,
- Supabase Edge Functions or equivalent trusted server functions.

Do not introduce unnecessary infrastructure before product validation.

Avoid Kubernetes, microservices and dedicated queues unless actual scale or operational constraints require them.

## Near-Term Priority

Build in roughly this order:

1. real authentication,
2. real invitations,
3. profiles,
4. follows / private follow requests,
5. communities and memberships,
6. posts and comments,
7. events,
8. recurring events,
9. Going / Interested attendance,
10. notifications,
11. moderation,
12. search and discovery.

Events are a core wedge, not a secondary feature.

Recurring events are especially important because dance scenes often operate around weekly socials, practicas, milongas and classes.

## Core Data Model

The backend should eventually contain at least these concepts:

- profiles,
- invitations,
- follows,
- dance_styles,
- profile_dance_styles,
- communities,
- community_members,
- posts,
- comments,
- reactions,
- polls,
- events,
- event_attendance,
- notifications,
- reports.

Use versioned migrations when the database layer is introduced.

## Privacy Rules

Privacy must be enforced server-side.

Frontend visibility checks are not security.

### Profiles

Target behavior:

- Public profile: visible to authenticated members.
- Private profile: follow request / approval controls access to protected content.

The current local prototype uses simplified visibility rules.

### Communities

Target behavior:

- Public.
- Private.
- Later: hidden / invite-only.

Private community posts must not be returned to unauthorized users.

### Events

Target behavior:

- Public.
- Private.
- Later: community-only or invite-only.

Attendance visibility may eventually have its own privacy setting.

## Follow / Connection Model

Preferred direction:

- following is asymmetric,
- private profiles can require approval,
- mutual following becomes `Connected`.

Do not force a Facebook-style friendship system unless explicitly requested.

## Invitation Model

The private beta is invitation-led.

A valid invitation should be a real backend record.

Invitation consumption should be atomic and should prevent accidental reuse when a code is single-use.

Do not remove the invitation concept simply to simplify signup.

Beginners are legitimate users and must not be excluded by the trust model.

## Community Rules

Any user may eventually be able to create a community unless product policy changes.

Community roles should support:

- owner,
- admin,
- moderator,
- member.

Private communities should support pending membership requests or explicit invitations.

Useful community features include:

- feed,
- discussions,
- questions,
- polls,
- events,
- members,
- rules,
- pinned posts,
- moderation.

## Events

Events should support:

- one-off events,
- recurring weekly events,
- title,
- description,
- dance style,
- date/time,
- venue,
- city,
- optional map coordinates,
- optional price,
- public/private visibility,
- optional community,
- Going / Interested / Maybe,
- later waitlists and ticket links.

Use a recurrence model rather than manually duplicating months of weekly events.

RFC 5545-style RRULE is a reasonable option.

## Feed Philosophy

The early feed should remain understandable.

Prefer:

- followed users,
- joined communities,
- nearby events,
- selected dance styles,
- chronological recency.

Avoid opaque ranking optimized solely for time spent or engagement.

## AI and Bots

Bots may be useful for:

- event discovery,
- practice suggestions,
- music discovery,
- community assistance,
- moderation.

Every bot must be clearly labeled `BOT`.

Never manufacture fake human activity.

AI should support the community, not impersonate it.

## Moderation

Before real-user launch, support at minimum:

- report user,
- report post,
- report event,
- block user,
- moderator content removal,
- account suspension.

Future moderation may include:

- spam classification,
- image safety classification,
- duplicate-event detection,
- harassment signals.

Because dance imagery can create false positives, avoid simplistic automatic image bans.

## Security

Before a real private beta:

- remove demo auth,
- keep secrets in environment variables,
- never expose a service-role key in the browser,
- enable Row Level Security,
- protect Storage buckets,
- validate invitation usage,
- prevent role escalation,
- protect private community data at query level,
- protect private profile data at query level,
- protect private event data at query level,
- add abuse rate limits where appropriate.

## Suggested Supabase Tables

A practical initial schema may include:

```text
profiles
invitations
follows
dance_styles
profile_dance_styles
communities
community_members
posts
comments
reactions
polls
poll_options
poll_votes
events
event_attendance
notifications
reports
```

Do not add tables without a clear product need, but do not overload unrelated concepts into one table simply to reduce table count.

## Storage

Suggested buckets:

- `avatars`
- `community-media`
- `post-media`

Do not build hosted video in the first beta.

Prefer external video links or embeds initially.

## Environments

When backend development begins, maintain at least:

- local/development,
- staging,
- production.

Use a separate staging database/project from production.

Never run experiments directly against production by default.

## Environment Variables

Typical frontend variables:

```text
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

Typical server-only variables:

```text
SUPABASE_SERVICE_ROLE_KEY=
EMAIL_PROVIDER_API_KEY=
PAYMENT_PROVIDER_SECRET=
```

Never commit real secrets.

Add an `.env.example` when environment variables are introduced.

## Coding Style

Prefer:

- focused commits,
- small reversible changes,
- readable code,
- explicit state,
- minimal dependencies,
- mobile-safe UI,
- straightforward data flow.

Avoid:

- unnecessary rewrites,
- speculative abstraction,
- premature infrastructure,
- giant unrelated commits,
- silently changing product semantics.

## Migration Rule

When migrating from `localStorage` to Supabase, avoid maintaining two permanent sources of truth.

Migrate feature by feature and make ownership clear.

Example sequence:

```text
auth
-> profile
-> invitations
-> follows
-> communities
-> posts
-> events
-> comments/reactions
-> notifications
```

Once a feature uses the backend reliably, remove obsolete local-only persistence for that feature.

## Testing Matrix

For any meaningful privacy or social feature, test:

- signed out,
- signed in,
- own profile,
- other public profile,
- other private profile,
- approved follower,
- non-follower,
- public community,
- private community member,
- private community non-member,
- public event,
- private event,
- page refresh,
- mobile viewport.

For RLS work, explicitly verify allowed and denied database operations.

## Definition of Done

A production feature is not done just because the UI exists.

It should have:

- working UI,
- backend persistence,
- correct authorization,
- loading state,
- error state,
- refresh-safe behavior,
- acceptable mobile behavior,
- relevant tests,
- updated documentation when behavior changes.

## Scope to Avoid for Now

Do not prioritize these before the core loop proves retention:

- TikTok-style feed,
- hosted video,
- livestreaming,
- advanced recommendation ML,
- cryptocurrency,
- large marketplace,
- complete school ERP,
- native mobile apps,
- elaborate gamification.

## Commit Messages

Use concise imperative messages such as:

- `Add Supabase authentication`
- `Persist community membership`
- `Enforce private profile access with RLS`
- `Add recurring event rules`
- `Fix profile navigation on mobile`

## Final Product Test

For every proposed feature, ask:

> Does this make it easier for real dancers to find people, communities or events and participate in real dance life?

If not, it is probably not an immediate priority.
