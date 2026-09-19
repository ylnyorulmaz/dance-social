# Product Roadmap

> This roadmap describes the intended sequence after the current client-side prototype.  
> It is a working product roadmap, not a fixed commitment.

## 1. Current State — Prototype Foundation

The current prototype already demonstrates the basic product model:

- demo login,
- invitation-code onboarding,
- dancer role selection,
- dance-style selection,
- profiles,
- profile editing,
- public/private profile setting,
- posts,
- posts to personal profiles,
- posts to communities,
- community creation,
- public/private communities,
- community pages,
- community feed,
- members,
- about,
- community events area,
- follow/unfollow,
- mutual Connected state,
- other user profiles,
- event creation,
- public/private events,
- basic local persistence.

Current architecture:

- static HTML/CSS/JavaScript,
- browser localStorage,
- sample/demo users,
- no real backend.

The next stage is to turn this product prototype into a real multi-user private beta.

---

# Phase 0 — Immediate: Stabilize the Prototype

## Goal

Make the current prototype internally consistent before replacing localStorage with a backend.

## Tasks

### 0.1 Fix Navigation and State Edge Cases

- verify every profile navigation path,
- verify back navigation,
- verify community -> profile -> community flows,
- verify mobile navigation,
- handle URL/hash refresh behavior,
- remove state inconsistencies.

### 0.2 Privacy Model Cleanup

Define exact semantics for:

- public profile,
- private profile,
- public community,
- private community,
- invite-only community,
- public event,
- private event.

Decide whether private profiles require:

- follow,
- mutual connection,
- or explicit approval.

Recommended production behavior:

- following remains asymmetric,
- private profile follow requests require approval,
- Connected means mutual following,
- private community membership requires approval unless invited.

### 0.3 Post Types

Turn the prototype labels into real behaviors:

- Post,
- Question,
- Poll,
- Photo,
- Link,
- Event.

For Poll:

- question,
- options,
- vote,
- results.

For Question:

- question badge,
- accepted/best answer later if useful.

### 0.4 Event UX

Complete:

- Going,
- Interested,
- Maybe,
- attendance count,
- event page,
- event creator,
- event discussion,
- recurring-event UI.

### 0.5 Responsive Pass

Test:

- iPhone,
- Android viewport,
- tablet,
- desktop.

The current desktop-three-column model needs a deliberate mobile interface.

---

# Phase 1 — Immediate Engineering: Real Backend

## Goal

Replace the local prototype with a real private beta.

Recommended initial stack:

- **Frontend:** Lovable-generated React application or equivalent modern React/Vite stack.
- **Backend:** Supabase.
- **Database:** PostgreSQL via Supabase.
- **Authentication:** Supabase Auth.
- **Storage:** Supabase Storage.
- **Realtime:** Supabase Realtime where useful.

## Core Database Model

### users / profiles

Fields may include:

- id,
- username,
- display_name,
- city,
- country,
- role,
- bio,
- privacy,
- avatar_url,
- created_at.

### dance_styles

- id,
- name,
- category.

### profile_dance_styles

- profile_id,
- dance_style_id,
- level,
- role / preference if relevant.

### invitations

- id,
- code,
- inviter_id,
- invitee_id,
- status,
- expires_at,
- created_at.

### follows

- follower_id,
- followed_id,
- status,
- created_at.

Private profiles may require a pending state.

### communities

- id,
- name,
- slug,
- description,
- city,
- dance_style_id,
- privacy,
- creator_id,
- created_at.

### community_members

- community_id,
- profile_id,
- role,
- status,
- joined_at.

Roles:

- member,
- moderator,
- admin,
- owner.

### posts

- id,
- author_id,
- destination_type,
- destination_id,
- post_type,
- body,
- visibility,
- created_at,
- edited_at.

### comments

- id,
- post_id,
- author_id,
- parent_comment_id,
- body,
- created_at.

### reactions

- user_id,
- target_type,
- target_id,
- reaction_type.

### polls

- post_id,
- settings.

### poll_options

- id,
- poll_id,
- text.

### poll_votes

- poll_id,
- option_id,
- user_id.

### events

- id,
- creator_id,
- community_id,
- title,
- description,
- dance_style_id,
- venue,
- city,
- latitude,
- longitude,
- start_at,
- end_at,
- recurrence_rule,
- privacy,
- capacity,
- external_url.

### event_attendance

- event_id,
- profile_id,
- status,
- visibility.

Statuses:

- going,
- interested,
- maybe,
- waitlisted.

### reports

- reporter_id,
- target_type,
- target_id,
- reason,
- status,
- created_at.

## Security

Before public beta:

- Row Level Security,
- server-enforced privacy,
- rate limiting,
- invite validation,
- moderation controls,
- storage policies.

Privacy must never depend only on frontend code.

---

# Phase 2 — Private Beta MVP

## Goal

Ship to real dancers as quickly as possible.

Target beta:

- 20-30 trusted initial dancers,
- approximately 2-3 invites each,
- potential first network of roughly 100-300 users.

## MVP Features

### Authentication

- email login,
- magic link or password,
- invitation-code signup.

Optional later:

- Google,
- Apple.

### Onboarding

Collect:

- name,
- username,
- city,
- role,
- dance styles,
- optional level,
- optional lead/follow/both/none field depending on dance.

### Profile

- avatar,
- name,
- username,
- city,
- dance styles,
- role,
- bio,
- privacy,
- followers,
- following,
- posts,
- communities,
- events.

### Community

- create,
- join,
- leave,
- public/private,
- moderators,
- feed,
- members,
- about,
- events.

### Posts

Ship:

- normal post,
- question,
- poll,
- photo,
- link.

Avoid hosted video initially.

Allow YouTube/Vimeo links instead.

### Events

This is a priority feature.

Ship:

- create event,
- recurring weekly event,
- public/private,
- community event,
- city,
- venue,
- price/free text,
- Going,
- Interested,
- attendee visibility,
- event reminders.

### Feed

Initial feed should be simple:

- followed users,
- joined communities,
- relevant local events,
- chronological ordering.

No complicated ML ranking.

### Notifications

Start with:

- new follower,
- follow request,
- community invite,
- post comment,
- event reminder,
- event update.

### Moderation

Ship minimum:

- block,
- report,
- community moderator delete,
- admin suspend,
- basic spam protection.

---

# Phase 3 — Soft Launch Validation

## Goal

Determine whether dancers voluntarily return.

Do not judge success by registrations alone.

## Metrics

### Activation

Measure whether users:

- complete onboarding,
- join a community,
- follow another dancer,
- open an event,
- make a post,
- mark Going.

### Density

Measure:

- connections per user,
- community membership overlap,
- posts per active community,
- event attendance interactions.

### Retention

Watch:

- Day 1,
- Day 7,
- Week 2,
- Week 4.

### Event Utility

Important metrics:

- event views,
- Going rate,
- recurring-event return behavior,
- event-to-profile discovery,
- event-to-community joining.

## Initial Success Heuristics

Among approximately 100 early users:

- 30+ post or comment,
- 20+ interact with event attendance,
- 20+ return in week two,
- several communities become self-sustaining,
- at least some organizers post without manual prompting.

---

# Phase 4 — Short Term: Event Graph

Approximate horizon: after private beta demonstrates basic retention.

## 4.1 Recurring Events

Support:

- every Tuesday,
- every first Friday,
- weekly class series,
- exceptions and cancellations.

Recurring events are a major competitive advantage because dance scenes are highly repetitive.

## 4.2 Rich Event Pages

Add:

- attendance,
- comments,
- organizers,
- venue,
- map,
- ticket link,
- playlist,
- photos later,
- event updates.

## 4.3 Calendar

Views:

- Tonight,
- Tomorrow,
- This Week,
- Weekend,
- calendar month,
- city,
- dance style.

## 4.4 Event Import

Allow organizers to:

- paste event URL,
- paste Instagram announcement text,
- import structured event details.

AI can suggest extracted fields, but the organizer confirms before publishing.

## 4.5 Calendar Export

- Apple Calendar,
- Google Calendar,
- ICS.

---

# Phase 5 — Short Term: Community Depth

## 5.1 Discussions

Persistent threaded discussions.

Useful categories:

- beginner questions,
- technique,
- music,
- shoes,
- festivals,
- travel,
- practice,
- history,
- local scene.

## 5.2 Search

Search across:

- people,
- communities,
- posts,
- questions,
- events,
- venues.

## 5.3 Community Moderation

- moderators,
- rules,
- pinned posts,
- approval settings,
- member removal,
- private join requests.

## 5.4 Community Identity

Potential:

- banner,
- icon,
- color,
- description,
- links,
- recurring event calendar.

---

# Phase 6 — Short / Medium Term: Telegram Companion

## Goal

Use an existing behavior instead of forcing every interaction into the app.

A Telegram bot could answer:

- Anything new?
- What is happening tonight?
- What events are this weekend?
- Any new Lindy posts?
- Who is going to X?

Organizer actions could include:

- create event,
- update event,
- cancel recurring instance,
- send announcement.

The bot should use the same backend data rather than becoming a separate product.

WhatsApp integration can be evaluated later depending on API constraints.

---

# Phase 7 — Medium Term: AI Utility Layer

AI should become useful only after real data exists.

## EventRadar

Pipeline:

1. discover,
2. extract,
3. normalize,
4. deduplicate,
5. link source,
6. human confirmation where needed,
7. publish.

Never silently scrape and republish copyrighted content as if original.

## DanceTrainer BOT

- drills,
- practice suggestions,
- selected instructional videos,
- technique Q&A.

## Music BOT

- songs,
- BPM,
- genre,
- dance suitability,
- playlist discovery.

## Feed Assistance

Possible:

- summaries of busy communities,
- unanswered questions,
- event recommendations,
- "since you were away."

Avoid engagement-maximizing black-box ranking.

---

# Phase 8 — Medium Term: Professional Accounts

## Organizer Pro

Potential subscription product:

- recurring-event management,
- audience analytics,
- scheduled posts,
- reminder campaigns,
- QR attendance,
- attendee export,
- promo codes,
- conversion analytics.

## Instructor Profiles

- verified instructor status,
- class listings,
- private lessons,
- booking inquiry.

## School Pages

- class schedules,
- instructors,
- registrations,
- events,
- location,
- announcements.

## Venue Pages

- recurring dance nights,
- calendar,
- maps,
- organizer links.

---

# Phase 9 — Medium Term: Payments and Ticketing

Only build once events show real traction.

## Ticketing

Support:

- free registration,
- paid ticket,
- capacity,
- waitlist,
- cancellation,
- check-in.

Potential business model:

- transaction fee,
- Organizer Pro subscription.

## Workshops / Courses

- bundles,
- passes,
- workshop registration,
- class series.

Payment architecture must account for:

- Turkey,
- European markets,
- taxes,
- organizer payout requirements.

---

# Phase 10 — Medium / Long Term: Marketplace

Possible categories:

- dance shoes,
- clothing,
- used equipment,
- festival passes,
- ticket transfers,
- private lessons,
- DJs,
- photographers,
- choreography.

Marketplace should be added only if organic activity indicates demand.

---

# Phase 11 — Long Term: Global Dance Passport

A dancer's profile can gradually become a portable dance identity.

Potential profile history:

- dances,
- communities,
- cities,
- festivals,
- events,
- schools,
- teachers,
- participation milestones.

This must remain opt-in.

The product should avoid turning social dance into a competitive score system.

---

# Phase 12 — Long Term: City Graph

The product could model dance scenes city by city.

Example:

**Istanbul**

- Lindy Hop communities,
- Salsa communities,
- Tango communities,
- schools,
- venues,
- events,
- organizers,
- teachers,
- people.

A dancer visiting another city could immediately understand the local ecosystem.

This is one of the strongest possible long-term network effects.

---

# Phase 13 — Long Term: Dance School SaaS

If organizer/school demand is validated:

- course creation,
- student registration,
- payments,
- attendance,
- instructor scheduling,
- capacity management,
- messaging,
- CRM,
- recurring classes,
- analytics.

This can become a separate B2B revenue engine built on top of the network.

---

# Phase 14 — Long Term: Flagship Event

Once the network has meaningful density, organize a real-world platform event.

Possible working concept:

**Dance Social / Dancebee Festival — Istanbul**

Areas may include:

- Swing,
- Salsa/Bachata,
- Tango,
- Street,
- World/Folk,
- open workshops,
- performances,
- community sessions,
- live music,
- DJ sets,
- social dancing.

The app becomes the event infrastructure:

- tickets,
- schedule,
- people attending,
- community meetups,
- notifications,
- maps,
- discussion.

The event then creates new user growth for the network.

---

# Things Explicitly Not to Build Yet

Until the core loop works, avoid spending major time on:

- TikTok-style short-video feed,
- full video hosting,
- livestreaming,
- complex recommendation ML,
- cryptocurrency,
- elaborate gamification,
- huge marketplace,
- complete school ERP,
- native iOS/Android apps before web retention,
- expensive AI infrastructure,
- dozens of post types,
- follower vanity mechanics.

---

# Priority Order

If forced to reduce the roadmap to the most important sequence:

1. Real backend and auth.
2. Real invitations and profiles.
3. Communities.
4. Posts/comments.
5. Events.
6. Recurring events.
7. Going / Interested.
8. Notifications.
9. Moderation.
10. Search/discovery.
11. Telegram companion.
12. Event aggregation AI.
13. Organizer Pro.
14. Ticketing.
15. School tools.
16. Marketplace.
17. Flagship real-world event.

---

# Next Concrete Build Sprint

The next implementation sprint should be:

## Sprint A — Backend Conversion

- initialize Supabase,
- create schema,
- Supabase Auth,
- profiles,
- invitation table,
- communities,
- community memberships,
- posts,
- follows,
- events,
- RLS policies,
- replace localStorage data layer.

## Sprint B — Real Social Loop

- comments,
- likes,
- follow requests,
- notifications,
- Going / Interested,
- recurring events,
- real user directory.

## Sprint C — First Private Beta

- seed Istanbul dance communities,
- invite initial dancers,
- onboard organizers,
- import recurring events,
- collect behavior,
- fix retention blockers.

The key question after these sprints is simple:

**Do real dancers use Dance Social to decide where to dance, talk to their community, and return without being asked?**
