# Agent Guide

This file is the operating guide for coding agents working on the Dance Social repository.

Read this file together with:

- `README.md`
- `PRODUCT.md`
- `PRODUCT_ROADMAP.md`
- `cloud.md`

When these documents disagree, use the following priority for product intent:

1. explicit current user request,
2. `PRODUCT_ROADMAP.md` for sequencing,
3. `PRODUCT.md` for product behavior and vision,
4. this file for implementation conventions,
5. `README.md` for repository overview.

## 1. Product Context

Dance Social is an invitation-led social network and community platform for dancers.

The core objects are:

- users / dancer profiles,
- follows / connections,
- invitations,
- communities,
- community memberships,
- posts,
- comments and reactions,
- events,
- event attendance,
- reports and moderation state.

The product should feel like a digital clubhouse for dance, not a generic engagement feed.

## 2. Current Technical State

The current prototype lives primarily in `index.html`.

It currently uses:

- plain HTML,
- CSS,
- JavaScript,
- browser `localStorage`,
- demo users,
- demo authentication.

Do not mistake the current client-side privacy logic for secure authorization.

Until the backend migration is completed, clearly treat the application as a prototype.

## 3. Target Architecture

The intended production direction is:

- React/Vite or Lovable-compatible frontend,
- Supabase Auth,
- Supabase PostgreSQL,
- Supabase Storage,
- Row Level Security,
- Realtime only where it materially improves the UX,
- Edge/server functions for trusted operations.

See `cloud.md` for the detailed architecture.

## 4. Product Guardrails

When adding features, preserve these principles.

### Trust

The product is invitation-led during private beta.

Do not remove invitation concepts just to simplify signup unless explicitly requested.

### Beginners are legitimate users

Do not design the network as an elitist club for experienced dancers.

A user may explicitly be:

- dancer,
- beginner / wants to start,
- instructor,
- organizer,
- DJ,
- school / venue later.

### Privacy

Profiles, communities and events may be public or private.

Production access control must be implemented in database/backend policy.

Never rely on hiding HTML or frontend checks as a security mechanism.

### AI transparency

Any system-generated account or bot must be clearly labeled `BOT`.

Never simulate a human user without disclosure.

### Feed philosophy

Prefer understandable, useful ranking.

The early product should prioritize:

- followed users,
- joined communities,
- local events,
- selected dance styles,
- chronological recency.

Do not introduce opaque engagement-maximizing ranking without explicit product direction.

### Real-world utility

Prioritize features that help users:

- find something to dance,
- decide where to go,
- join communities,
- meet other dancers,
- organize events,
- continue persistent discussions.

## 5. Scope Discipline

Do not prematurely build:

- full video hosting,
- short-form video feed,
- livestreaming,
- complex recommendation ML,
- full marketplace,
- school ERP,
- native mobile apps,
- cryptocurrency,
- elaborate gamification.

The near-term priority is:

`auth -> invitations -> profiles -> follows -> communities -> posts/comments -> events -> attendance -> notifications -> moderation`

## 6. Coding Rules

### Keep changes small and testable

Prefer focused commits.

Each change should:

- solve one coherent problem,
- preserve existing flows,
- avoid unnecessary rewrites,
- keep the project runnable.

### Do not silently delete prototype behavior

When migrating a feature to a new implementation, preserve its intended behavior unless the product docs or user explicitly change it.

### Avoid fake completion

Do not describe placeholder UI as a completed production feature.

Use precise terms such as:

- prototype,
- demo,
- mocked,
- local-only,
- server-enforced,
- production-ready.

### Preserve data compatibility during transition

While localStorage remains in use, avoid changing keys casually.

Current keys include concepts such as:

- profile state,
- communities,
- posts,
- follows,
- events,
- joined communities.

During backend migration, provide a clean cutover strategy instead of accumulating two competing sources of truth.

## 7. Suggested Frontend Structure After Migration

A future frontend may use a structure similar to:

```text
src/
  app/
  components/
  features/
    auth/
    onboarding/
    profiles/
    follows/
    communities/
    posts/
    comments/
    events/
    notifications/
    moderation/
  lib/
    supabase/
    permissions/
  pages/
  styles/
```

Do not create this structure merely for appearance. Introduce it when the React migration actually begins.

## 8. Backend Rules

All sensitive mutations should be validated server-side.

Examples:

- consuming invitation codes,
- approving private-profile follow requests,
- approving private community membership,
- assigning moderator/admin roles,
- creating privileged notifications,
- banning users,
- payment/ticket operations later.

Row Level Security must be enabled on user-sensitive tables before real-user testing.

## 9. Privacy Semantics to Preserve

### Profile

Planned model:

- public: profile can be viewed by members,
- private: follow request / approval controls access to private content.

The current prototype uses simplified follow-based visibility.

### Community

Planned model:

- public,
- private,
- later hidden/invite-only.

Private community posts must not be returned by the backend to unauthorized users.

### Event

Planned model:

- public,
- private,
- later community-only / invite-only where needed.

Attendance visibility should eventually have its own setting.

## 10. Event Priority

Events are not a secondary feature.

Recurring dance events are a major product wedge.

When choosing between polishing generic social features and making event discovery / recurring events meaningfully useful, prefer the event utility unless the user says otherwise.

## 11. Testing Expectations

For every meaningful feature, test the relevant paths.

At minimum consider:

- signed out,
- signed in,
- own profile,
- other public profile,
- other private profile,
- follower,
- non-follower,
- public community,
- private community member,
- private community non-member,
- public event,
- private event,
- mobile viewport,
- page refresh / restored state.

For backend work, add tests or SQL verification for authorization rules where practical.

## 12. Security Checklist

Before real private beta:

- no demo credentials as real auth,
- secrets only in environment variables,
- RLS enabled,
- private rows protected at query level,
- storage buckets protected,
- invite codes cannot be reused incorrectly,
- privileged roles cannot be self-assigned,
- input sanitized,
- rate limiting for abuse-prone actions,
- reporting/blocking functional,
- audit-relevant admin operations logged.

## 13. Definition of Done

A feature is not complete merely because the UI exists.

A production feature is done when:

- UI works,
- data persists to the real backend,
- authorization is correct,
- error and loading states exist,
- mobile behavior is acceptable,
- the feature survives refresh,
- relevant tests pass,
- docs are updated when behavior changes.

## 14. Commit Guidance

Use concise imperative commit messages, for example:

- `Add Supabase profile persistence`
- `Enforce private community access with RLS`
- `Add recurring event creation`
- `Fix profile navigation on mobile`

Avoid commits containing unrelated large changes unless a migration requires them.

## 15. Product Question to Keep Asking

For each feature, ask:

> Does this make it easier for real dancers to find people, communities or events and participate in real dance life?

If not, it is probably not an immediate priority.
