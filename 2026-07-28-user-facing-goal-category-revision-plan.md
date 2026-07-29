# User-Facing Goal Category Revision Plan

Status: all three items are implemented. Items 1 and 2 were implemented on
2026-07-28. Item 1's grouping and item 2 were refined, and item 3 was
implemented, on 2026-07-29.

The revision should accomplish three things:

1. Make Board Preparation more focused, especially by reducing the number of
   general-purpose books.
2. Make Staying Current include ARROgram while excluding static core
   references.
3. Add a goal for learning clinical radiation oncology that is broader than
   board preparation but more focused than every resource tagged
   `clinical-knowledge`.

The recommendations below use the 79 resources currently embedded in
`index.html`.

## Working Principle

A resource should enter a goal because the resource is focused on that goal,
not merely because one part of a broad collection could help with it.

Goal categories may overlap, but each goal should have a clear user intent.
Broad source-level tags on collections such as webinar archives and resource
lists should be checked against this focus rule.

## Current Problems

### Board Preparation Before Item 1

Before the accepted change, the filter was:

- `board-preparation` OR `physics` OR `radiation-biology`

It returned 26 resources, including 11 books. The automatic inclusion of every
physics or radiation-biology resource pulls in broad textbooks that contain
those subjects but are not primarily board-review resources.

### Staying Current Before Item 2

Before the accepted change, the filter was:

- `literature-review`

It returned 10 resources. It excluded ARROgram because ARROgram had only the
`newsletters` tag. It also included two static resources tagged
`core-reference`:

- Rad Onc Talks
- Handbook of Evidence-Based Radiation Oncology

### General Clinical Learning

There is no goal for users who want to build clinical knowledge without
specifically preparing for boards or following current literature.

Using `clinical-knowledge` AND NOT `literature-review` by itself would return 44
resources. That is too broad because it would mix textbooks and teaching
resources with quick-reference tools, guidelines, question banks, and planning
resources.

## Recommended Changes

### 1. Narrow Board Preparation Through a Curated Tag — Implemented

Change the goal filter and grouping to:

```yaml
filter:
  any_tags:
    - "board-preparation"
group:
  by: "tags"
  tags:
    - "practice-based-learning"
    - "quick-reference"
    - "core-reference"
    - "physics"
    - "radiation-biology"
```

Treat `board-preparation` as an intentional inclusion tag instead of
automatically including every resource tagged `physics` or
`radiation-biology`.

Group the displayed resources by the five board-study modes above. Resources
with more than one grouping tag appear under each applicable heading, while
resources with none of the five tags remain available in the automatic
`Other` group.

Add `board-preparation` to these dedicated physics and radiation-biology
resources:

- ARRO Radiobiology Resource List
- RadBioForRadOnc
- Radiobiology for the Radiologist
- ARRO Physics Resource List
- Primer on Radiation Oncology Physics
- The Physics of Radiation Therapy

Remove `board-preparation` from these books because their primary use is
clinical lookup or general learning:

- Pocket Radiation Oncology (2nd Ed.)
- Pocket Guide to Radiation Oncology

The following broad books would stop matching automatically. They do not
currently need a tag removed because they lack `board-preparation`:

- Perez and Brady's Principles and Practice of Radiation Oncology (8th Ed.)
- Walter and Miller's Textbook of Radiotherapy, Radiation Physics, Therapy and
  Oncology
- Fundamentals of Radiation Oncology (4th Ed.)

Implemented result: **21 of 79 resources**, with all 15 former non-book results
retained and the number of books reduced from 11 to 6.

Because the global goal-page option hides books, Board Preparation displays
**15 unique resources** grouped as follows:

- `practice-based-learning`: 2
- `quick-reference`: 1
- `core-reference`: 1
- `physics`: 4
- `radiation-biology`: 4
- `Other`: 6

These group counts overlap because multi-tagged resources appear under each
applicable heading.

The six remaining books would be:

- Radiation Oncology: A Question-Based Review
- Radiobiology for the Radiologist
- The Physics of Radiation Therapy
- Absolute Clinical Radiation Oncology Review
- Radiation Oncology Study Guide
- Radiation Oncology Review for Boards and MOC

ARRO Webinars and Zaorsky Educational Threads remain candidates for a future
focus review, but that review is not part of the accepted first change.

### 2. Refocus Staying Current — Implemented

Implemented filter:

```yaml
filter:
  any_tags:
    - "literature-review"
    - "newsletters"
  none_tags:
    - "core-reference"
    - "resource-list"
```

Removed `literature-review` from these static learning references:

- Rad Onc Talks
- Handbook of Evidence-Based Radiation Oncology

Removed `literature-review` from this broad resource list:

- Zaorsky Educational Threads

Keep their `core-reference` tags so they can appear under the proposed general
clinical-learning goal.

ARROgram enters Staying Current through its existing `newsletters` tag; it
does not need the less precise `literature-review` tag.

Refined result: **8 of 79 resources**, including ARROgram and no books,
`core-reference` resources, or `resource-list` resources.

The implemented list is:

- ASTRO Refresher
- QuadShot News Podcast
- Red Journal Podcast
- Practical Radiation Oncology (PRO) Podcast
- ARRO Webinars
- ACRO Resident Webinars
- ARROgram
- QuadShot News Newsletter

ARRO Webinars and ACRO Resident Webinars remain in the accepted result for now.
Zaorsky Educational Threads is excluded as a broad resource list and no longer
carries `literature-review`.

### 3. Add Build Clinical Framework — Implemented

Title: **Build Clinical Framework**

Purpose: for users building a broad clinical radiation oncology framework from
introductory teaching, core and comprehensive references, high-yield reviews,
and quick-reference resources.

Implemented filter and grouping:

```yaml
filter:
  all_tags:
    - "clinical-knowledge"
  any_tags:
    - "introductory-education"
    - "core-reference"
    - "comprehensive-reference"
    - "high-yield-overview"
    - "quick-reference"
group:
  by: "tags"
  tags:
    - "introductory-education"
    - "core-reference"
    - "comprehensive-reference"
    - "high-yield-overview"
    - "quick-reference"
```

The learning-resource tags keep this more focused than using
`clinical-knowledge` alone. The goal may intentionally overlap with Board
Preparation, Staying Current, and other goals.

The filter returns **37 of 79 resources**. It captures eight of the nine
resources that did not previously match a goal:

- Rad Onc Talks
- ACRO Deck
- Rad Onc Wiki
- HemOnc.org
- RadOnc Smart Review
- The Fellow on Call
- AJCC Cancer Staging Manual
- Pediatric Radiation Oncology

It also provides an appropriate home for general resources moved out of
Board Preparation or Staying Current, including the pocket references, broad
textbooks, Rad Onc Talks, and Handbook of Evidence-Based Radiation Oncology.

The current global `hide_media_types` option hides books from every goal page.
Build Clinical Framework therefore displays **18 unique resources** grouped as
follows:

- `introductory-education`: 5
- `core-reference`: 4
- `comprehensive-reference`: 3
- `high-yield-overview`: 7
- `quick-reference`: 4

These group counts overlap because resources carrying multiple group tags
appear under each applicable heading.

## Filter Capability

The goal-filter implementation now supports `all_tags`, `any_tags`,
`none_tags`, and `any_audiences`. Exclusions use:

```yaml
none_tags:
  - "tag-to-exclude"
```

Implemented approach:

- Add `none_tags` to goal-filter normalization, matching, and the displayed
  filter summary.
- Keep the goal logic in the embedded goal-filter YAML.
- Do not add goal names themselves as resource tags.

An alternative YAML-only fallback would be:

- Add a curated resource tag such as `general-clinical-learning` and use it as
  the positive filter for Build Clinical Framework.

The fallback is not currently needed and remains less desirable because it
makes a navigation goal part of the resource taxonomy.

## Implemented Goal Coverage

Current raw filter results:

- Board Preparation: 21
- Contouring & Treatment Planning: 30
- Quick Clinical Lookup: 14
- Active Learning / Retrieval Practice: 13
- Staying Current: 8
- Incoming Learners: 5
- Build Clinical Framework: 37
- Professional Development: 6

Because goals overlap, these figures should not be added together. Collectively
they capture **78 of 79 resources**.

The only uncaptured resource would be ROECSG Podcast Search Engine, which is a
discovery utility rather than a learning goal. It can remain available through
normal browsing without creating a goal solely to force complete coverage.

## Implementation Record

1. Updated resource tags in the embedded source YAML for accepted changes.
2. Updated the embedded goal-filter and grouping YAML for all three goals.
3. Updated the main user-facing goal-category document with the accepted
   filters, explanations, counts, and represented-tag snapshots.
4. Validated every goal result and identified the sole resource that matches no
   goal.
