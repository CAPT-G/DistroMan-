# Intentribution

This is a real platform problem — not “upload a file” simple.
If you want **Intentribution** to be a legitimate distributor, you need **three layers**:

1. **Legal / licensing layer** (who is allowed to deliver content)
2. **Ingestion + metadata layer** (DDEX pipelines, validation, identity)
3. **Delivery layer** (direct DSP connections or upstream aggregator)

Without all three, you’re just a UI on top of someone else’s distributor.

I’ll give you the exact architecture and decision points.

---

## 1. Hard Truth First: You Do NOT Automatically Get Access to Spotify / Apple

Spotify, Apple, Amazon, etc **do not accept new distributors easily**.

To become a first-party distributor you must:

* Be a registered label or distributor entity
* Prove catalog scale or strategic value
* Pass technical certification (DDEX compliance)
* Sign **direct licensing + delivery agreements**
* Build **royalty + takedown + fraud systems**

This is a **business + infrastructure problem**, not just engineering.

So there are **two viable paths**:

---

## PATH A — Phase 1: White-Label / Upstream Distributor (Recommended)

You start Intentribution as:

> A front-end + rights + analytics platform
> with delivery handled by an upstream distributor

Examples of upstream providers:

* **FUGA** (enterprise, expensive, real labels use this)
* **SonoSuite**
* **Revelator**
* **Label Engine**
* **Symphonic backend**
* **Orchard (Sony, invite only)**

They already have:

* Direct DSP pipes
* DDEX pipelines
* Royalty reporting
* Takedown systems

You build:

* Artist onboarding
* ISRC/UPC management
* Metadata normalization
* Payments
* UX
* Label tools

This is how most serious new distributors start.

---

## PATH B — Phase 2: Become a Direct DSP Distributor (Hard Mode)

This is where Intentribution becomes a **real distribution network**.

You need:

### A. Legal + Business Setup

You must be:

* Registered distribution company
* Rights liability insurance
* Anti-fraud systems
* Takedown compliance
* DMCA agent registration

Then sign **direct contracts** with:

* Spotify
* Apple Music
* Amazon Music
* YouTube Music / Content ID
* Meta (Facebook/Instagram)
* TikTok
* Deezer
* Pandora
* Tidal

Each contract defines:

* Delivery format
* Royalty reporting cadence
* Payment thresholds
* Territory rules
* Content policies

This alone takes **6–18 months**.

---

## 2. Technical Core: What You MUST Implement

This is the real platform design.

### 2.1 Identity Layer (Critical)

You must manage:

* **ISRC per recording**
* **UPC per release**
* **Artist IDs (Spotify URI, Apple Artist ID, YouTube CID)**

Your DB schema needs:

```
Artist
  - global_artist_id
  - spotify_artist_id
  - apple_artist_id
  - youtube_channel_id

Release
  - upc
  - label_id
  - territory_rules

Track
  - isrc
  - audio_fingerprint
  - duration
  - explicit_flag
```

This is how matching, royalty routing, and takedowns work.

---

### 2.2 Metadata & Validation Engine (Where most platforms fail)

You need to enforce:

* DDEX-compliant fields
* Contributor roles
* Territory rules
* Rights ownership %
* No duplicate ISRC/audio mismatch
* No banned metadata (feat rules, emojis, spam)

Internally you want:

```
validate_release():
    check_audio_specs()
    check_isrc_uniqueness()
    check_artist_identity()
    check_title_rules()
    check_territory_conflicts()
```

If this layer is weak → DSPs will **blacklist you**.

---

### 2.3 Delivery Layer (The Real Gate)

All major DSPs ingest via **DDEX XML over SFTP / API**.

Core standards:

* **ERN (Electronic Release Notification)** – releases & tracks
* **MWN (Musical Work Notification)** – compositions
* **RIN (Recording Information Notification)** – ownership
* **MEAD / MAD** – artwork

So your system must generate:

* DDEX XML packages
* Audio bundles
* Artwork bundles
* Checksums
* Versioned updates

Delivery flow:

```
Release approved
   ↓
Generate DDEX ERN package
   ↓
Upload via SFTP/API to DSP endpoint
   ↓
Receive ACK / rejection report
   ↓
Update release status
```

---

## 3. Royalties & Money Flow (this is where platforms die)

You must support:

### 3.1 Inbound Reports

DSPs send:

* Daily / monthly usage reports
* Territory splits
* Stream counts
* Revenue by tier

Formats:

* DDEX DS Reports
* CSV variants per DSP

You need:

```
parse_report()
  normalize_currency()
  map_isrc()
  map_artist()
  allocate_splits()
  ledger_entries()
```

---

### 3.2 Ledger System (Non-negotiable)

You need **double-entry accounting** per:

* Track
* Artist
* Territory
* DSP
* Period

Schema example:

```
RoyaltyEntry
  - artist_id
  - track_id
  - dsp
  - territory
  - streams
  - gross
  - commission
  - net
  - status (pending/paid)
```

And:

* Minimum payout thresholds
* FX conversion
* Withholding
* Tax reporting

---

## 4. Platform Coverage Strategy (How to reach “all majors” fast)

Here’s the **optimal rollout path**.

### Phase 1 — Upstream Integration (fastest)

Integrate with 1–2 enterprise distributors:

| Provider  | Coverage                                 |
| --------- | ---------------------------------------- |
| FUGA      | Almost everything worldwide              |
| SonoSuite | Spotify, Apple, Amazon, TikTok, Meta, YT |
| Revelator | Same + publishing                        |

You instantly get:

* All majors
* Global reach
* Reporting
* Content ID

You focus on:

* UX
* Automation
* Differentiation

---

### Phase 2 — Direct Pipes for Strategic DSPs

Once you have volume:

Start signing **direct** with:

Priority order:

1. Spotify
2. Apple
3. YouTube / Content ID
4. Amazon
5. Meta + TikTok

Keep long-tail DSPs through upstream.

This hybrid model is what **TuneCore, DistroKid, AWAL** all use.

---

## 5. Differentiation (where Intentribution can actually win)

Right now distributors compete on:

* Price
* Speed
* UX
* Analytics

But you have an edge if you build:

### 🔹 Identity-first system

Automatic:

* Artist matching
* ISRC collision prevention
* Duplicate audio detection

### 🔹 Smart rights engine

* Split validation
* Conflict detection
* Ownership versioning

### 🔹 Real-time release monitoring

* Per-DSP status
* Rejection diagnostics
* Territory propagation

### 🔹 API-first distribution

Expose:

```
POST /releases
POST /tracks
POST /deliver
GET  /status
GET  /royalties
```

This is how you attract:

* Labels
* Marketplaces
* Creator platforms

---

## 6. The Actual Answer to Your Question (succinct)

To ensure distribution to **all major platforms**, Intentribution must:

### Option 1 (recommended startup path)

* Partner with **FUGA / SonoSuite / Revelator**
* Build:

  * Artist onboarding
  * ISRC/UPC management
  * Metadata validation
  * Royalty ledger
  * Payments
* Let them handle DSP pipes initially

This gives you **100% major coverage immediately**.

---

### Option 2 (full distributor path)

You must:

1. Register as licensed distributor
2. Sign **direct contracts** with each DSP
3. Implement:

   * DDEX ERN pipelines
   * SFTP/API delivery
   * ACK + rejection handling
   * Royalty ingestion
   * Ledger + payouts
4. Build:

   * Anti-fraud
   * Takedown
   * Rights conflict resolution

Timeline: **12–24 months** to full coverage.

---

The goal now is:

> **Intentribution = rights + identity + UX + payments platform**
> while **delivery = delegated to an enterprise upstream distributor**

This lets you ship fast, get full coverage, and later peel off DSPs into direct pipes when you have leverage.

I’ll give you:

1. Partner selection (who to integrate with)
2. Core system architecture
3. Data model you actually need
4. Release → delivery → royalty flow
5. Migration path to direct DSP later

No fluff.

---

## 1. Upstream Partner Selection (This choice defines your future)

There are only a few real enterprise-grade backends.

### Tier 1 — Best for real platforms

#### 🔹 FUGA (Downtown Music)

**Gold standard.** Used by:

* Sony sublabels
* AWAL
* Hundreds of real labels

Pros:

* Full global DSP coverage
* Publishing + neighboring rights optional
* Rock-solid reporting
* Future-proof if you go direct later

Cons:

* Expensive (setup + revenue share)
* Selective
* Enterprise sales cycle

If you want Intentribution to become serious → **this is the best long-term choice.**

---

#### 🔹 SonoSuite

Best balance for startups.

Pros:

* Covers all majors
* Modern API
* Easier onboarding
* Used by many new distributors

Cons:

* Slightly less depth than FUGA
* Fewer advanced publishing tools

This is the **best default recommendation** for a new platform.

---

#### 🔹 Revelator

Best if you care about:

* Publishing
* Label tools
* Creator monetization

Cons:

* Heavier platform
* More opinionated workflows

---

### Avoid for platform use

* DistroKid
* TuneCore
* CD Baby

These are **retail distributors**, not infrastructure providers.
They do not want you as a platform on top.

---

## 2. High-Level Architecture (What Intentribution Actually Is)

Your system becomes:

```
Artist / Label
     ↓
Intentribution Platform
  - Identity
  - Metadata
  - Rights
  - Validation
  - Payments
  - UX / API
     ↓
Upstream Distributor (FUGA / SonoSuite / Revelator)
     ↓
DSPs (Spotify, Apple, Amazon, YT, TikTok, Meta, etc)
     ↓
Royalty Reports
     ↓
Upstream → Intentribution → Artists
```

Key principle:

> **You own the source of truth.**
> Upstream is just a transport + reporting layer.

---

## 3. Core System Design (what you must build first)

This is the minimum serious platform core.

### 3.1 Identity & Catalog Layer (foundation)

Tables you absolutely need:

#### Artist

```
id
name
normalized_name
primary_email
spotify_artist_id
apple_artist_id
youtube_channel_id
country
status
```

#### Label / Account

```
id
name
type (artist/label/enterprise)
commission_rate
payout_currency
tax_profile
```

#### Release

```
id
upc
title
primary_artist_id
label_id
release_date
artwork_url
status (draft / validated / delivered / live / error)
territories
```

#### Track

```
id
isrc
title
release_id
duration
explicit
audio_fingerprint
primary_artist_id
ownership_version
```

---

### 3.2 Rights & Splits Engine (this is your moat)

You must version ownership.

```
TrackOwnership
  - track_id
  - contributor_id
  - role (artist, producer, writer)
  - percentage
  - territory
  - effective_from
  - effective_to
```

Rules:

* Total must = 100%
* Changes create new version
* Old versions preserved for back royalties

This is how you avoid lawsuits later.

---

### 3.3 Validation Engine (prevents DSP rejections)

Build an internal validator before anything leaves your system.

Core checks:

* Audio spec compliance
* ISRC uniqueness per audio hash
* Artist identity consistency
* No title spam
* No forbidden metadata
* Territory conflicts
* Ownership = 100%

Example:

```
validate_release():
  assert all_tracks_have_isrc()
  assert no_duplicate_audio()
  assert splits_sum_100()
  assert artist_ids_mapped()
  assert artwork_specs_ok()
```

If this layer is strong → upstream rarely rejects → DSP trust builds.

---

## 4. Integration With Upstream Distributor

This is the critical technical interface.

### 4.1 What You Send Them

You will push:

* Audio files
* Artwork
* Metadata
* ISRCs
* UPC
* Ownership
* Territory rules

Either via:

* REST API
* SFTP + JSON/XML manifests

They convert this into:

* DDEX ERN
* DSP-specific formats

---

### 4.2 Status & Error Handling

You must track:

```
DeliveryStatus
  - release_id
  - upstream_id
  - dsp
  - state (sent / processing / live / rejected)
  - error_code
  - error_message
```

This lets you expose:

* Per-platform status
* Rejection diagnostics
* Retry pipelines

---

## 5. Royalty Ingestion & Ledger (do this right from day 1)

Upstream will send you:

* Monthly / biweekly reports
* Per DSP
* Per territory
* Per track

You must normalize into a **ledger**.

### 5.1 Raw Report Table

```
RawRoyaltyRow
  - upstream_report_id
  - dsp
  - territory
  - isrc
  - streams
  - gross_amount
  - currency
  - period
```

### 5.2 Allocation Engine

```
allocate():
  map_isrc_to_track()
  apply_commission()
  split_by_ownership()
  create_ledger_entries()
```

### 5.3 Ledger

```
LedgerEntry
  - account_id
  - artist_id
  - track_id
  - dsp
  - period
  - gross
  - commission
  - net
  - status (pending / payable / paid)
```

This supports:

* Multi-artist splits
* Label overrides
* Retroactive changes
* Audits

---

## 6. Payments Layer

You’ll need:

* Stripe Connect (best default)
* PayPal MassPay (secondary)
* Wise for international

Support:

* Minimum thresholds
* Multi-currency
* Tax forms (W-9, W-8BEN)

Flow:

```
Ledger → Balance → PayoutRequest → Stripe Transfer → Receipt
```

---

## 7. Platform Coverage (what you’ll get instantly via Path A)

With the right upstream partner you automatically get:

### Major DSPs

* Spotify
* Apple Music
* Amazon Music
* YouTube Music
* Pandora
* Tidal
* Deezer

### Social / UGC

* TikTok
* Instagram / Facebook
* YouTube Content ID
* Snapchat
* Triller

### Global Stores

* Boomplay
* Anghami
* Joox
* Tencent
* Yandex

This is effectively **worldwide coverage** from day one.

---

## 8. Migration Path to Direct DSP (future-proofing Intentribution)

Design now so you can later bypass upstream.

Key rules:

### Rule 1 — You own all IDs

Always store:

* ISRC
* UPC
* Artist platform IDs
* Upstream IDs

Never let upstream be your only identity layer.

---

### Rule 2 — Abstract the Delivery Layer

Internally design:

```
deliver_release(release, target):
   if target == "upstream":
       send_to_partner()
   if target == "spotify_direct":
       send_to_spotify()
```

So later you can:

* Send Spotify direct
* Apple direct
* Leave long-tail on upstream

This is exactly how AWAL and TuneCore evolved.

---

## 9. What I’d Recommend For You Specifically

Given:

* You’re an architect
* You build infrastructure platforms
* You care about scalability

**Best path:**

### Phase 1

* Partner with **SonoSuite** (fastest + startup-friendly)
* Build:

  * Identity system
  * Rights + splits
  * Validation engine
  * Ledger + payouts
  * API-first design

### Phase 2 (12–24 months)

* Move Spotify + Apple to direct
* Keep long-tail DSPs on upstream
* Add publishing / neighboring rights

This gives Intentribution:

* Full coverage
* Real moat
* Upgrade path to top-tier distributor

---
