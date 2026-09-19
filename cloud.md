# Cloud Architecture

This document defines the intended cloud and backend direction for Dance Social.

It distinguishes the **current prototype** from the **target private-beta architecture**.

## 1. Current State

The current repository is a frontend prototype.

Current persistence:

- browser `localStorage`.

Current authentication:

- demo-only username/password logic.

Current privacy:

- client-side prototype behavior only.

Current hosting requirement:

- any static host can serve the prototype.

This architecture is useful for product iteration but is not suitable for real users.

## 2. Target Private-Beta Architecture

Recommended baseline:

```text
Browser
  |
  v
React / Vite / Lovable frontend
  |
  +--------------------+
  |                    |
  v                    v
Supabase Auth      Supabase API
                       |
                       v
                  PostgreSQL
                       |
            +----------+----------+
            |                     |
            v                     v
        Storage                Realtime
            |
            v
      image / media files

Trusted jobs / privileged operations
  |
  v
Supabase Edge Functions or equivalent server functions
```

The objective is to keep the first production architecture small.

Do not introduce Kubernetes, microservices or a dedicated message broker for the private beta.

## 3. Frontend Hosting

The frontend can be deployed through one of:

- Lovable deployment,
- Vercel,
- Cloudflare Pages,
- Netlify,
- another static/edge-compatible host.

The deployment should support:

- HTTPS,
- environment variables,
- preview deployments,
- custom domain,
- SPA routing.

The frontend host should not contain privileged service-role credentials.

## 4. Supabase Responsibilities

Supabase is the recommended first backend because it covers the initial product requirements with minimal infrastructure.

Use it for:

- authentication,
- PostgreSQL database,
- storage,
- Row Level Security,
- optional Realtime,
- Edge Functions,
- scheduled functions where appropriate.

## 5. Authentication

Initial production authentication options:

- email + password,
- magic link.

Later:

- Google,
- Apple.

Invitation access should be enforced as part of signup/onboarding.

A user account and a valid invitation are separate concepts.

Suggested flow:

```text
receive invite
  -> open signup
  -> authenticate email
  -> consume invitation
  -> create profile
  -> onboarding
  -> enter app
```

Invitation consumption must be atomic to avoid duplicate use.

## 6. Recommended Core Schema

### profiles

```text
id uuid primary key references auth.users
username text unique
display_name text
city text
country text
role text
bio text
privacy text
avatar_url text
created_at timestamptz
updated_at timestamptz
```

### invitations

```text
id uuid
code text unique
inviter_id uuid
invitee_id uuid nullable
status text
expires_at timestamptz nullable
created_at timestamptz
consumed_at timestamptz nullable
```

### follows

```text
follower_id uuid
followed_id uuid
status text
created_at timestamptz
```

Possible statuses:

- pending,
- accepted.

Public profiles may auto-accept follows.

Private profiles should require approval.

### communities

```text
id uuid
slug text unique
name text
description text
city text
dance_style_id uuid nullable
privacy text
creator_id uuid
created_at timestamptz
```

Privacy values initially:

- public,
- private.

Later:

- hidden.

### community_members

```text
community_id uuid
profile_id uuid
role text
status text
joined_at timestamptz
```

Roles:

- member,
- moderator,
- admin,
- owner.

Statuses:

- pending,
- active,
- banned.

### posts

```text
id uuid
author_id uuid
destination_type text
destination_id uuid nullable
post_type text
body text
visibility text
created_at timestamptz
updated_at timestamptz
```

### comments

```text
id uuid
post_id uuid
author_id uuid
parent_comment_id uuid nullable
body text
created_at timestamptz
updated_at timestamptz
```

### reactions

```text
user_id uuid
target_type text
target_id uuid
reaction_type text
created_at timestamptz
```

### events

```text
id uuid
creator_id uuid
community_id uuid nullable
title text
description text
venue text
city text
latitude numeric nullable
longitude numeric nullable
start_at timestamptz
end_at timestamptz nullable
recurrence_rule text nullable
privacy text
capacity integer nullable
external_url text nullable
created_at timestamptz
updated_at timestamptz
```

### event_attendance

```text
event_id uuid
profile_id uuid
status text
visibility text
created_at timestamptz
```

Statuses:

- going,
- interested,
- maybe,
- waitlisted.

### reports

```text
id uuid
reporter_id uuid
target_type text
target_id uuid
reason text
details text nullable
status text
created_at timestamptz
resolved_at timestamptz nullable
resolved_by uuid nullable
```

## 7. Row Level Security

RLS is mandatory before real users.

### Profiles

Public profile:

- authenticated members may read public profile fields.

Private profile:

- owner always reads,
- approved followers may read protected fields,
- public preview fields may remain visible depending on product policy.

### Follows

A user may:

- create a follow request as themselves,
- read their outgoing relationships,
- read relationships where they are the target when required for approval,
- update only relationships they are authorized to approve/remove.

### Communities

Public:

- authenticated users may read.

Private:

- active members may read full community data,
- non-members may receive only safe preview metadata if product policy allows.

Only authorized members may post.

### Posts

Profile posts:

- authorization follows profile/post visibility.

Community posts:

- public community posts follow community policy,
- private community posts are returned only to active members.

This must be enforced in SQL policies, not filtered after retrieval in JavaScript.

### Events

Public events:

- readable by authenticated users, and possibly by signed-out users later.

Private events:

- creator,
- authorized community members,
- invited users,
- or explicitly permitted followers depending on the selected model.

### Reports

Only:

- reporter,
- authorized moderators/admins

should be able to access report data.

## 8. Storage

Recommended buckets:

### avatars

Stores profile images.

Rules:

- user can upload/update their own avatar,
- public read or signed-URL behavior depends on profile privacy strategy.

### community-media

Stores:

- community icons,
- banners.

Write access:

- owner/admin/moderator as appropriate.

### post-media

Stores:

- post images.

Do not begin with hosted video.

Use external video embeds/links first.

## 9. Realtime

Use Realtime selectively.

Good early candidates:

- new comments on an open post,
- live event attendance counts,
- notification updates.

Do not make the entire feed realtime merely because the feature exists.

Polling or refresh-on-focus is acceptable for low-frequency data during beta.

## 10. Edge / Server Functions

Use trusted server functions for operations that should not be performed with normal client permissions.

Examples:

- consume invite code,
- issue new invitation codes,
- moderate/bulk-remove content,
- send transactional email,
- create admin notifications,
- event import/extraction later,
- payment operations later,
- webhook handlers,
- scheduled event reminders.

Never expose the Supabase service-role key to the browser.

## 11. Notifications

Initial notification records should be stored in PostgreSQL.

Suggested types:

- new follower,
- follow request,
- follow approved,
- community join request,
- community invite,
- comment,
- reply,
- mention,
- event reminder,
- event update.

Delivery order:

1. in-app notifications,
2. email where useful,
3. push notifications later.

Do not begin with native push infrastructure before the web beta shows retention.

## 12. Event Recurrence

Recurring events are important enough to design correctly early.

Recommended approach:

- store recurrence rule on the parent event,
- generate/display occurrences as needed,
- allow instance exceptions,
- support cancellation of one occurrence without deleting the series.

Possible representation:

- RFC 5545 RRULE where practical.

Avoid duplicating months of weekly event rows with no recurrence model unless used as a deliberate temporary implementation.

## 13. Search

Private beta can begin with PostgreSQL search.

Search targets:

- profiles,
- communities,
- events,
- posts.

Later options:

- Postgres full-text search,
- pg_trgm for fuzzy matching,
- dedicated search service only if required by scale.

## 14. Event Import and AI

Future EventRadar architecture:

```text
source discovery
  -> fetch/reference
  -> structured extraction
  -> normalization
  -> duplicate detection
  -> confidence checks
  -> human confirmation where needed
  -> event record
```

Always preserve source attribution.

Do not allow AI to silently invent event information.

## 15. Scheduled Jobs

Potential scheduled tasks:

- upcoming-event reminders,
- recurring-event occurrence preparation,
- expired invite cleanup,
- stale pending-request cleanup,
- weekly community digest,
- event-source refresh later.

Use Supabase scheduled functions / cron-compatible infrastructure before introducing separate workers.

## 16. Observability

Private beta should have basic observability from day one.

Minimum:

- frontend error tracking,
- server-function logs,
- database query/error visibility,
- auth failure logging,
- moderation audit trail.

Possible tools later:

- Sentry,
- PostHog,
- Supabase logs.

Avoid collecting more personal behavioral data than needed.

## 17. Product Analytics

Track product events such as:

- onboarding_completed,
- profile_completed,
- follow_created,
- community_joined,
- community_created,
- post_created,
- event_viewed,
- event_created,
- event_going,
- invite_sent,
- invite_consumed.

Primary purpose:

- understand activation,
- density,
- retention,
- event utility.

Do not optimize solely for time spent.

## 18. Environments

Maintain at least:

### Local

Developer environment.

### Staging

Uses a separate Supabase project/database from production.

Use for:

- migrations,
- RLS testing,
- seed data,
- QA.

### Production

Real beta users.

Never point local experiments directly at production by default.

## 19. Environment Variables

Frontend-safe variables may include:

```text
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

Server-only secrets may include:

```text
SUPABASE_SERVICE_ROLE_KEY=
EMAIL_PROVIDER_API_KEY=
PAYMENT_PROVIDER_SECRET=
```

Server-only secrets must never be bundled into browser JavaScript or committed to Git.

Provide an `.env.example` when the cloud migration begins.

## 20. Migrations

Use versioned SQL migrations for schema and policy changes.

Do not make production schema changes manually without recording them in the repository.

Migration flow:

```text
write migration
  -> run locally/staging
  -> verify RLS
  -> test application
  -> deploy production
```

## 21. Seed Data

Staging may include seed users such as:

- Ayşe,
- Mert,
- Deniz,
- clearly labeled BOT users.

Production should not contain fake human activity.

Utility bots may exist in production only when clearly labeled.

## 22. Backup and Recovery

Before accepting meaningful user-generated content:

- enable automated database backups,
- understand Supabase point-in-time recovery availability for the selected plan,
- document restore procedure,
- keep migrations in Git.

Media storage strategy should also consider deletion and recovery expectations.

## 23. Security Before Private Beta

Required:

- real auth,
- RLS on sensitive tables,
- protected storage,
- invitation validation,
- rate limits for abuse-prone endpoints,
- report/block functionality,
- no browser service-role key,
- role escalation prevention,
- private community tests,
- private profile tests,
- private event tests.

## 24. Deployment Sequence

Recommended sequence:

### Step 1

Create separate Supabase staging project.

### Step 2

Add migrations for:

- profiles,
- invitations,
- follows,
- communities,
- community_members,
- posts,
- events.

### Step 3

Implement RLS before filling the database with real-user data.

### Step 4

Replace demo login with Supabase Auth.

### Step 5

Replace localStorage profile state.

### Step 6

Replace localStorage follows and communities.

### Step 7

Replace localStorage posts.

### Step 8

Replace localStorage events.

### Step 9

Add comments, reactions and notifications.

### Step 10

Create production project, apply migrations and perform a small invite-only launch.

## 25. Scale Philosophy

Do not design for millions of users before validating hundreds.

The architecture should comfortably support the early network while preserving clean migration paths.

Start with:

- one frontend,
- one Supabase project per environment,
- PostgreSQL,
- storage,
- a small number of edge functions.

Add infrastructure only when measured constraints require it.
