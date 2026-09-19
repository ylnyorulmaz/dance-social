# Product

> Working title: **Dance Social**  
> Brand candidates discussed: **Dancebee**, **DanceSync**, **Dancebit**, **Dance a Little**.  
> The final public brand is not yet locked.

## 1. Product Summary

Dance Social is an invitation-led social network and community infrastructure product built specifically for dancers.

It is designed for people who participate in social dance scenes such as Lindy Hop, Salsa, Bachata, Argentine Tango, Balboa, West Coast Swing, Solo Jazz, Kizomba, Hip-Hop, K-pop dance, folk dance, contemporary dance and other community-driven dance forms.

The product combines several things that are currently fragmented across generic platforms:

- personal dancer profiles,
- local and global dance communities,
- persistent discussions,
- event discovery,
- recurring social events,
- organizer announcements,
- following and social connections,
- dance-specific tools,
- and eventually AI-assisted discovery, moderation and community support.

The core idea is not to create another generic infinite-scroll social network. The goal is to create a **digital clubhouse for dancers**: a place where online participation strengthens real-world dance communities and where real-world dance activity brings people back online.

A useful shorthand for the product is:

**Orkut-style identity and communities + Reddit-style persistent discussion + Meetup-style events + a dance-specific social graph and utility layer.**

---

## 2. Vision

### Vision Statement

Create the default digital home for dancers: a trusted, useful and enjoyable network where people can discover communities, find events, meet other dancers, discuss dance, organize activity and maintain a persistent identity around their dance life.

### Long-Term Vision

Dance Social should become infrastructure for the global social dance ecosystem, not only a feed.

A mature version of the product could connect:

- dancers,
- beginners,
- instructors,
- DJs,
- organizers,
- dance schools,
- venues,
- festivals,
- shoe and clothing brands,
- musicians,
- photographers,
- videographers,
- and other dance-related businesses.

The long-term product should make it easier to answer questions such as:

- What can I dance tonight?
- Who is going?
- Which communities are active in this city?
- Where can I start learning Salsa?
- Who teaches Balboa here?
- What festivals are coming up?
- Which dancers do I already know?
- What did this community discuss last month?
- What music should I practice to?
- What events are my friends attending?
- Which community should I join after moving to a new city?

The network should become more useful as a person's real dance life becomes richer.

### Product Philosophy

Dance Social should optimize for:

1. **People over content volume.**
2. **Communities over creators.**
3. **Useful social activity over passive consumption.**
4. **Persistent knowledge over disappearing stories and chats.**
5. **Real-world participation over screen time.**
6. **Trust over maximum reach.**
7. **User choice over opaque algorithmic ranking.**
8. **Dance identity over generic social identity.**

The product should avoid becoming a TikTok or Instagram clone. Video, photos and creator content can exist, but they should support the network rather than define it.

---

## 3. Problem

Dance communities already use Instagram, WhatsApp, Telegram, Facebook, Discord and other tools. These services work, but they solve the dancer's problem only partially.

Common problems include:

- event announcements disappear quickly,
- recurring events are repeatedly reposted,
- useful questions and answers are buried,
- discussions are fragmented across chat groups,
- new dancers do not know which groups exist,
- a dancer may belong to many scenes and platforms,
- organizers repeatedly rebuild the same audience,
- social media feeds prioritize engagement rather than local usefulness,
- community history is difficult to search,
- moving to a new city requires rediscovering the local scene from zero,
- dance-specific identity is poorly represented,
- and useful dance tools live in separate apps or websites.

Dance Social attempts to combine identity, community, event discovery and utilities in one domain-specific network.

---

## 4. Target Audience

### Primary Users

#### Active Social Dancers

People who regularly attend classes, socials, practicas, milongas, workshops, festivals or dance meetups.

Their core needs are:

- finding activity,
- staying connected to their scene,
- following dancers they know,
- participating in community discussions,
- discovering new communities,
- and maintaining a dance identity.

#### Beginners and Dance-Curious Users

People who want to start dancing but may not yet know dancers personally.

They should be allowed into the network without pretending to be experienced dancers.

The system should distinguish between:

- active dancer,
- beginner / wants to start,
- instructor,
- organizer / DJ / venue / school.

Invitation-based access should create trust without excluding genuine beginners.

### Secondary Users

- instructors,
- DJs,
- dance schools,
- event organizers,
- venues,
- festival organizers,
- musicians,
- dance photographers and videographers,
- dance shoe and apparel brands,
- related lifestyle brands.

---

## 5. Launch Strategy

The product is intended for **all dancers**, not only one dance style.

The initial distribution wedge can be a dense existing network such as:

**Istanbul Lindy Hop -> invited dancers -> their other dance networks -> other communities and cities.**

This distinction is important:

- product scope = broad dance network,
- initial distribution = narrow, trusted community.

The goal is to create density before breadth.

A network with 300 active dancers in one connected scene is more useful than a network with 10,000 disconnected accounts.

---

## 6. Trust and Invitation Model

Dance Social begins as an invitation-led network.

Reasons:

- reduce spam,
- reduce fake accounts,
- reduce people joining purely to approach dancers romantically,
- create social accountability,
- create an explicit invitation graph,
- maintain community quality during the beta.

A member may eventually receive a limited number of invitations, for example 2-3.

The invitation model must not become hostile to beginners. A beginner invited by a dancer or school is a legitimate user.

Potential future trust signals:

- invited by,
- mutual dancers,
- joined communities,
- attended events,
- verified instructor,
- verified organizer,
- verified school,
- verified venue.

---

## 7. Current Prototype Capabilities

The current prototype is a client-side proof of concept stored in a single HTML application.

### Authentication Prototype

Current demo login:

- username: `test`
- password: `test`

This is not production authentication.

### Invitation Onboarding

Demo invitation codes are supported.

The onboarding flow currently asks for:

- user type / role,
- dance styles,
- name,
- city,
- username.

### Dancer Profiles

Profiles currently support:

- name,
- username,
- city,
- role,
- bio,
- dance styles,
- post count,
- community count,
- dance style count,
- follower count,
- following count,
- posts,
- communities,
- events,
- about information.

Users can edit their own profiles.

### Public / Private Profiles

Profiles may be:

- **Public** — visible to all members.
- **Private** — full content is visible only after a follow relationship under the current prototype rules.

The production privacy model will require more precise policy design.

### User Profiles and Social Graph

Users can open other users' profiles.

The prototype includes sample users for testing.

Users can:

- follow,
- unfollow,
- see whether another user follows them,
- reach a mutual **Connected** state.

The current connection model is intentionally lightweight and may evolve into one of the following:

- asymmetric following,
- mutual following,
- explicit friend/connection requests,
- or a hybrid.

### Home Feed

Users can publish posts to:

- their own profile,
- or a selected community.

Current post types represented in the prototype include:

- Post,
- Question,
- Poll,
- Photo,
- Event.

Not every post type has full specialized behavior yet.

### Communities

Any user may create a community.

Community fields currently include:

- name,
- dance / topic,
- city,
- description,
- privacy,
- creator.

Community pages include:

- Feed,
- Events,
- Members,
- About.

Users can:

- open communities,
- join communities,
- post inside communities,
- view members,
- view community events.

### Public / Private Communities

Communities may be:

- **Public** — visible and joinable by members.
- **Private** — content is restricted to members under the current prototype model.

A mature version should support:

- public,
- private,
- hidden / invite-only.

### Events

The prototype now supports creation of events.

Event fields include:

- title,
- date,
- time,
- location,
- optional community,
- description,
- creator,
- privacy.

Events may be:

- Public,
- Private.

Event visibility is currently simulated in the client prototype.

### Event Participation

Full event attendance states are planned but not yet complete.

The intended model is:

- Going,
- Interested,
- Maybe,
- waitlist where relevant.

### Navigation

The prototype currently supports navigation among:

- Home,
- Communities,
- Events,
- Profile,
- other user profiles.

### Persistence

The current application uses browser `localStorage`.

This means prototype data persists on the same browser, but:

- there is no shared database,
- there is no multi-device sync,
- there is no real multi-user state,
- security cannot be trusted,
- privacy cannot be enforced server-side.

---

## 8. Intended Core Product Features

### Social Layer

- dancer profiles,
- follow / connection system,
- profile posts,
- likes,
- comments,
- replies,
- notifications,
- mentions,
- optional direct messages later.

### Community Layer

- user-created communities,
- public/private/invite-only membership,
- moderators,
- community rules,
- pinned posts,
- discussions,
- questions,
- polls,
- media,
- event calendar,
- searchable archive.

### Event Layer

- one-off events,
- recurring weekly events,
- workshops,
- classes,
- socials,
- practicas,
- milongas,
- festivals,
- performances,
- outdoor meetups,
- attendance states,
- maps,
- ticket links,
- reminders,
- calendar export,
- event discussions.

Recurring events are particularly important because social dance scenes often repeat the same event weekly.

### Professional Layer

Verified profiles for:

- instructors,
- DJs,
- schools,
- organizers,
- venues,
- festivals.

Potential features:

- professional profile pages,
- classes,
- event management,
- analytics,
- attendee lists,
- ticketing,
- QR check-in,
- recurring scheduling,
- announcements.

### Discovery

Users should eventually be able to discover by:

- city,
- dance style,
- date,
- community,
- instructor,
- school,
- venue,
- festival,
- followed users,
- friends attending.

### Dance Tools

Possible utility layer:

- metronome,
- BPM counter,
- tempo trainer,
- practice timer,
- song-to-dance suggestion,
- playlist helper,
- practice tracking,
- saved songs,
- event calendar.

Utilities are useful because they create reasons to open the app even when the social feed is quiet.

---

## 9. AI and Bot Layer

AI should support the network, not impersonate people.

Bots must always be clearly marked as **BOT**.

Possible bots:

### EventRadar BOT

- discovers event announcements,
- normalizes them,
- deduplicates repeated announcements,
- creates structured event candidates,
- links to original sources.

### DanceTrainer BOT

- practice prompts,
- educational videos,
- drills,
- technique references.

### DanceDJ / Music BOT

- song discovery,
- BPM information,
- music of the day,
- playlists,
- historical notes.

### Welcome / Community BOT

- welcomes new users,
- surfaces unanswered questions,
- suggests communities.

### Moderation BOT

- spam classification,
- harassment signals,
- duplicate detection,
- image safety classification,
- suspicious account behavior.

AI-generated activity must never be disguised as human activity.

The purpose of bots is to reduce empty-room syndrome and improve utility, not manufacture fake community engagement.

---

## 10. Moderation and Safety

A dance social network has specific moderation risks.

Important risks include:

- harassment,
- unwanted romantic or sexual approaches,
- spam,
- fake dancer identities,
- event scams,
- impersonation,
- inappropriate images,
- stalking risks related to event attendance,
- public exposure of private event participation.

Required controls should eventually include:

- report user,
- report post,
- report event,
- block user,
- mute user,
- remove member,
- moderator tools,
- account suspension,
- organizer verification,
- privacy controls,
- attendance privacy,
- configurable profile discoverability.

Image classifiers should use confidence bands rather than simplistic automatic blocking because legitimate dance photography can trigger false positives.

---

## 11. Feed Philosophy

The initial feed should not depend on opaque engagement optimization.

A useful first feed may combine:

- followed users,
- joined communities,
- nearby events,
- selected dance styles,
- chronological activity.

Ranking should optimize for usefulness rather than outrage, virality or maximum time-on-app.

Users should be able to understand why something appears.

---

## 12. Business Model

The recommended model is:

**Keep normal dancer participation free. Monetize the economic activity around dance.**

### 12.1 Ticketing Commission

The platform may process or refer ticket sales for:

- socials,
- workshops,
- festivals,
- classes.

Possible take rate:

- approximately 3-8%, depending on payment architecture and service level.

### 12.2 Organizer Pro

Subscription features may include:

- recurring events,
- advanced event management,
- attendee export,
- QR check-in,
- waitlists,
- scheduled announcements,
- discount codes,
- analytics,
- conversion tracking,
- event notifications.

### 12.3 Dance School Pro

Potential SaaS features:

- course management,
- class schedules,
- registration,
- payments,
- capacity,
- attendance,
- student communication,
- instructor profiles.

### 12.4 Verified Professional Profiles

Potential paid or verified professional features for:

- teachers,
- DJs,
- schools,
- venues,
- organizers.

Verification itself should not become a misleading pay-to-trust badge. Payment and identity verification should remain distinct concepts.

### 12.5 Promoted Events and Posts

Organizers may pay to promote:

- events,
- workshops,
- festivals,
- classes,
- school announcements.

Promotions must be clearly labeled.

### 12.6 Marketplace

Possible marketplace categories:

- dance shoes,
- dance clothing,
- used items,
- festival ticket transfers,
- private lesson bookings,
- DJ bookings,
- choreography,
- photography.

The platform may take a transaction fee.

### 12.7 Qualified Leads

Schools and instructors may pay for qualified discovery or booking leads.

### 12.8 Sponsorship and Advertising

Relevant sponsors may include:

- dance brands,
- clothing,
- shoes,
- music services,
- travel,
- food and beverage,
- lifestyle brands.

Advertising should remain limited enough that it does not damage community trust.

### 12.9 Major Platform Event

The network could eventually organize its own annual or flagship gathering.

Possible structure:

- Swing area,
- Latin area,
- Tango area,
- Street / urban dance area,
- world / folk dance area,
- performances,
- workshops,
- DJs,
- live music,
- social dancing,
- community meetups.

The product could feed the event, and the event could feed the product.

Revenue could come from:

- tickets,
- sponsors,
- workshops,
- vendor booths,
- merchandise,
- partnerships.

---

## 13. Product Constraints and Limitations

### Current Technical Limitations

The current prototype:

- is a single HTML application,
- has no production backend,
- uses localStorage,
- has fake/demo authentication,
- has demo invitation codes,
- has simulated users,
- has no secure authorization,
- cannot enforce privacy,
- has no shared multi-user state,
- has no real image upload system,
- has no production notifications,
- has no email system,
- has no payments,
- has no real event attendance system,
- has no production moderation infrastructure.

The current version is a **product prototype**, not a secure beta.

### Product Risks

#### Cold Start

A social product can feel dead without active people.

Mitigations:

- launch into a dense real community,
- make events useful immediately,
- seed legitimate structured content,
- use transparent utility bots,
- focus on recurring activity.

#### Existing Platforms

Dancers already use:

- Instagram,
- WhatsApp,
- Telegram,
- Facebook.

Dance Social must provide value they do not provide well, especially:

- persistent community identity,
- searchable discussions,
- structured events,
- cross-community discovery,
- dance-specific social graph,
- utilities.

#### Fragmentation

Different dance cultures behave differently.

The platform must allow communities to develop their own norms without forcing every dance into one cultural template.

#### Scope Expansion

The concept naturally expands toward:

- social network,
- event platform,
- ticketing,
- marketplace,
- school SaaS,
- AI,
- messaging,
- media hosting.

The team must aggressively sequence these capabilities instead of building everything simultaneously.

---

## 14. What Success Looks Like

The first success condition is not millions of users.

A strong early signal would be a small but dense network where people repeatedly use the product to organize real dance life.

Example beta indicators among the first 100 users:

- 30+ users post or comment,
- 20+ users mark or attend events,
- meaningful community creation,
- week-two return behavior,
- recurring event usage,
- users inviting other real dancers,
- organizers voluntarily posting directly instead of being asked.

A few thousand daily active dancers across connected city scenes could already represent a meaningful product.

---

## 15. Core Product Principle

**Humans create the community. Software organizes it. AI supports it. Real-world dance gives it meaning.**
