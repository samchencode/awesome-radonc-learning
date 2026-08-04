# Architecture

## System Summary

ARRO Resources is a client-only radiation oncology learning directory packaged
as one `index.html`. It is designed for GitHub Pages and has no application
server, database, package manager, build step, or persistence layer.

Two YAML documents embedded in the HTML drive the application:

```text
script#embeddedSources ──> js-yaml ──> normalizeResource ──> resources
                                                           ├─> Library view
script#embeddedGoalFilters ─> js-yaml ─> normalizeGoalFilter └─> Goal lists
```

The Library view provides search, sorting, pagination, and facets. The Goal
lists view applies curated filter rules to the same in-memory resources and
groups the results for browsing.

## Repository and Deployment

| Path | Runtime role |
| --- | --- |
| `index.html` | Entire production application: embedded data, CSS, markup, and JavaScript |
| `.github/workflows/pages.yml` | Builds the minimal Pages artifact and deploys it |
| `README.md` | Schemas, taxonomy, active goal definitions, and operating documentation |
| `AGENTS.md` | Contributor and agent guardrails |
| `ARCHITECTURE.md` | Technical handoff |
| `2026-07-28-user-facing-goal-categories.md` | Goal rationale and derived count snapshots |
| `2026-07-28-user-facing-goal-category-revision-plan.md` | Historical goal-design record |
| `export.pdf` | Archival input; not used at runtime |

The Pages workflow runs on pushes to `main` and manual dispatch. It copies only
`index.html` into `_site`, adds `.nojekyll`, uploads that directory, and deploys
it. Documentation and archival files are not published with the site.

## Document Layout

`index.html` is organized in this order:

1. `script#embeddedSources`, a YAML array containing the resource catalog.
2. `script#embeddedGoalFilters`, a YAML object containing global goal options
   and the goal list.
3. Metadata and the complete responsive stylesheet.
4. Shared header and hash navigation.
5. `main#resource-browser`, the Library view.
6. `main#goal-browser`, the default Goal lists view.
7. Shared footer.
8. Pinned js-yaml and List.js CDN scripts with Subresource Integrity.
9. One application IIFE containing all runtime JavaScript.

The single-file structure makes deployment and nontechnical YAML editing
simple, but increases merge-conflict and accidental-formatting risk. Prefer
small, localized edits.

## External Dependencies

The browser loads:

- js-yaml 4.1.1 from jsDelivr to parse both embedded YAML documents.
- List.js 2.3.1 from jsDelivr for Library search, sorting, filtering state, and
  pagination.

Both script tags use pinned versions, `crossorigin="anonymous"`, and SHA-256
SRI hashes. There is no bundled fallback, so the application requires network
access even when `index.html` is opened locally. If either dependency is
unavailable, the shared error UI explains that the CDN library could not be
loaded.

## Boot Sequence

The application IIFE executes after markup and both dependency script tags:

1. Mobile facets are collapsed when the initial viewport is 720px or narrower.
2. Frequently used elements are cached in the `elements` object.
3. `showPageFromHash()` selects `#library` only for that exact hash; every
   other hash or no hash shows Goal lists.
4. `showInitialLibraryNudge()` briefly highlights the Library navigation link
   when Goal lists are the initial view.
5. `loadDefaultSource()` parses goal YAML first, then resource YAML.
6. `renderResources()` validates and normalizes resources, renders the Library,
   initializes or reindexes List.js, renders Goal lists, and attaches handlers.

Parsing, schema, or dependency failures flow through `showError()`, which
updates both views and the hero status.

## Embedded Data Contracts

### Resource catalog

`script#embeddedSources` must parse to an array. Each record requires:

- `id`
- `title`
- `description`
- `tags`
- `audiences`
- `media_type`
- `last_verified_at`

`priority` and `url` are optional. `tags` and `audiences` must be arrays.
`priority`, when supplied, must convert to a finite number. After
normalization, `renderResources()` rejects duplicate IDs.

The runtime converts values to strings but does not enforce kebab-case,
taxonomy membership, date shape, or URL reachability. Those are editorial
contracts documented in `README.md` and enforced through review.

The current catalog has 79 resources. Most are prioritized; unprioritized
resources sort below prioritized resources. A `null` URL is valid and renders
an unlinked title.

### Goal configuration

`script#embeddedGoalFilters` must parse to an object whose only root fields are
`options` and `goals`.

The only supported global option is:

```yaml
options:
  hide_media_types:
    - "books"
```

Each goal requires `id`, `title`, `description`, and `filter`. Goal IDs must be
unique. The supported filter fields are:

- `all_tags`: every value is required.
- `any_tags`: at least one value is required.
- `none_tags`: every listed value is disqualifying.
- `any_audiences`: at least one audience is required.

At least one filter field must be nonempty. Unknown filter fields are rejected.
`resourceMatchesGoal()` combines all populated fields with logical AND.

Grouping is optional:

```yaml
group:
  by: "tags"
  groups:
    - label: "Clinical Lookup"
      any_tags:
        - "quick-reference"
        - "guidelines"
```

`group.by` accepts `tags`, `media_type`, or `audience`. Unknown group fields are
rejected. Tag grouping normally uses `group.groups`; every entry requires only
the supported fields `label` and `any_tags`, and `any_tags` must contain at
least one value. The normalizer assigns an internal ID based on group order.
The older `group.tags` array remains supported as a compatibility shorthand,
but `tags` and `groups` cannot be populated together.

`groupGoalResources()` treats each labeled group's `any_tags` as logical OR.
Tag grouping is intentionally nonexclusive: a resource appears in each group
it matches. A matching resource that matches no configured group appears in
Other. Media-type groups are exclusive. Audience groups may repeat a resource
because audiences are arrays.

## Runtime State

The application keeps all state in memory:

| State | Purpose |
| --- | --- |
| `resources` | Normalized resource records currently loaded |
| `goalFilters` | Normalized active goal definitions |
| `goalOptions` | Global goal display options |
| `revealedGoalMediaTypes` | Default-hidden media types the user chose to reveal |
| `resourceList` | List.js instance for the Library |
| `handlersAttached` | Prevents duplicate event-handler registration after YAML reload |
| `goalTooltipSequence` | Generates unique tooltip IDs on each goal render |

There is no local storage, server state, analytics state, or URL-encoded search
state. Reloading the page restores embedded data and default UI state.

## Library View

`renderResources()` creates one resource row per record and builds facet counts
from the current dataset. It also creates a combined hidden search index from:

- title
- description
- media type
- audiences
- tags

List.js provides a 100 ms debounced search, priority/title/media sorting, and
12-item pagination.

### Facet semantics

Library filters have include and exclude controls for media type, audience, and
tag. Activating one action clears the opposite action for the same value.

- Filter families combine with AND.
- Multiple included tags require all selected tags.
- Multiple included audiences match any selected audience.
- Exclusions reject a resource carrying any selected excluded value.
- Clicking a tag pill on a resource activates that tag's include filter.

Active filters render as removable chips. The runtime catalog always comes from
`script#embeddedSources`; there is no file picker or replacement YAML preview
flow.

## Goal Lists View

`renderGoalResults()` performs the following pipeline for each goal:

1. Apply `resourceMatchesGoal()`.
2. Remove media types hidden by default unless the user revealed them.
3. Sort by descending priority, then title.
4. Group matches when the goal defines `group`.
5. Render linked titles, media labels where useful, and metadata tooltips.

Goal counts shown in the interface are post-hide display counts. Documentation
may also report raw filter counts, so those two numbers must be labeled
clearly. Books currently match filters normally but are hidden by default and
can be restored with Show Books.

Goal resource tooltips expose description, tags, audiences, and media type on
hover or keyboard focus. Tooltip IDs are regenerated for every render to keep
`aria-describedby` relationships unique.

The Goal lists page also renders a fixed table of contents from the active goal
definitions. It links to each goal section with `#goal-{id}` anchors, stays
collapsed to numbered markers until hover or keyboard focus, and is hidden on
narrow viewports and in print.

Goal lists are ordered by their order in the YAML. A resource may match several
goals. Complete coverage is not required; the current intentional non-match is
ROECSG Podcast Search Engine, which remains available in the Library.

## Navigation and Presentation

Navigation is hash-based and does not use a router:

- `#library` shows the Library.
- `#goals`, no hash, or any other hash shows Goal lists.

The application updates `aria-current`, view visibility, and the document title
when the hash changes. When Goal lists are the initial view, the Library
navigation link briefly receives a visual nudge so users can still discover the
full searchable catalog.

CSS is embedded and includes desktop, tablet, mobile, print, and
`prefers-reduced-motion` behavior. Goal groups use a masonry-style multi-column
layout on wider screens and one column on narrow screens. Library metadata
changes from a table-like grid to cards on mobile.

## Security and Output Safety

- `escapeHtml()` protects all dynamic text inserted through HTML templates.
- `safeUrl()` resolves URLs and permits only HTTP and HTTPS.
- External links open in a new tab with `noopener noreferrer`.
- CDN scripts are pinned and protected by SRI.
This is a curated-content application, not a hardened arbitrary-YAML sandbox.
Continue treating embedded data as untrusted at render time.

## Change Impact

| Change | Expected follow-up |
| --- | --- |
| Add, remove, or retag a resource | Recalculate facets, every goal count, goal group membership, and uncaptured resources; review README and goal-category snapshots |
| Change a goal filter or order | Recalculate raw and displayed counts; update README and goal-category documentation |
| Change goal grouping | Verify duplicate membership and Other behavior; update grouping documentation |
| Add a schema field | Update normalizers, render/search consumers, README, this document, and malformed-input checks |
| Change default-hidden media | Verify toggle state and raw-versus-displayed documentation |
| Change navigation or rendering | Test both routes, keyboard behavior, responsive layout, and error states |
| Change a CDN dependency | Update URL, SRI hash, README, this document, and browser tests |
| Change deployment | Update workflow, README, this document, and `AGENTS.md` if contributor procedure changes |

Before committing any change, follow the mandatory documentation audit in
`AGENTS.md`.

## Known Limitations

- No automated test suite, linter, formatter, or build validation exists.
- Runtime startup depends on two external CDN requests.
- External link health and `last_verified_at` are maintained manually.
- Derived counts in documentation can drift when tags or filters change.
- The single large HTML file increases merge-conflict risk.
- Tag and audience grouping may intentionally duplicate resources.
- Only `index.html` is deployed, so runtime cannot fetch repository notes.

## Validation Strategy

Use the checks in `AGENTS.md` as the canonical contributor workflow. At a
minimum, parse both YAML blocks, confirm unique IDs, recalculate affected goal
counts, run `git diff --check`, and test impacted browser behavior through a
static HTTP server.
