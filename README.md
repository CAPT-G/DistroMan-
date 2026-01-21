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
