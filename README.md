# ARRO Resources

ARRO Resources is a curated directory of radiation oncology learning and
reference material. It is designed primarily for residents, with additional
resources for medical students, attending physicians, and researchers.

The project is a static HTML application designed for GitHub Pages. It supports
full-text search, pagination, sorting, and include/exclude filters for media
type, audience, and tags. It also provides goal-oriented resource lists with
configurable matching, exclusions, and grouping. It has no server, build
process, or package installation.

The interface loads two pinned JavaScript dependencies from jsDelivr, so using
the site or opening `index.html` locally requires a network connection.

## Project structure

| File | Purpose |
| --- | --- |
| `index.html` | The complete application, embedded YAML resource data, styles, interface code, and pinned CDN dependency references |
| `.github/workflows/pages.yml` | Publishes only `index.html` to GitHub Pages after a push to `main` |
| `README.md` | Project structure, schema, and active taxonomy definitions |
| `AGENTS.md` | Contributor guardrails, validation workflow, and mandatory pre-commit documentation audit |
| `ARCHITECTURE.md` | Runtime architecture, data flow, state, contracts, and known limitations |
| `export.pdf` | Archival source document used to assemble and order the initial collection |
| `2026-07-28-ideas.md` | Metadata fields considered for a possible future schema expansion |
| `2026-07-28-user-facing-goal-categories.md` | Active goal definitions, filter logic, counts, and rationale |
| `2026-07-28-user-facing-goal-category-revision-plan.md` | Decision and implementation record for the current goal model |

The application has two editable YAML blocks. The first element inside the
document `<head>` is:

```html
<script id="embeddedSources" type="text/yaml">
```

The YAML array inside that element is the resource catalog and the primary
editable data. It appears before the application code so a nontechnical editor
can update the list without searching through the rest of the HTML.

The second block defines goal-oriented navigation:

```html
<script id="embeddedGoalFilters" type="text/yaml">
```

Its YAML root is an object containing global goal options and a `goals` array.
Goal definitions select resources by tag or audience and may group matches for
display. Goal names are navigation shortcuts, not resource tags.

The application loads minified js-yaml 4.1.1 and List.js 2.3.1 from versioned
jsDelivr URLs. Subresource Integrity hashes ensure that the browser accepts only
the exact files reviewed for this project.

At runtime, the application always uses the embedded resource and goal YAML in
`index.html`. There is no separate catalog file or upload/preview flow.

## Resource schema

The YAML root must be an array of resource objects.

```yaml
- id: "example-resource"
  priority: 10
  title: "Example Resource"
  description: "A one-sentence explanation of the resource's scope and use."
  tags:
    - "core-reference"
    - "quick-reference"
    - "clinical-knowledge"
  audiences:
    - "residents"
    - "attendings"
  media_type: "books"
  url: "https://example.org/resource"
  last_verified_at: "2026-07-28"
```

### Fields

| Field | Required | Definition |
| --- | --- | --- |
| `id` | Yes | Stable, unique, lowercase kebab-case identifier. Do not change it merely because a title changes. |
| `priority` | No | Numeric editorial ranking. Higher values sort first by default. Resources without a priority sort below prioritized resources. |
| `title` | Yes | Human-readable resource name. Include an edition when it materially identifies a book or other publication. |
| `description` | Yes | Short, specific sentence describing scope and intended use rather than repeating the title or author list. |
| `tags` | Yes | Array describing subject matter, depth, evidence orientation, and use. Use the controlled vocabulary below. |
| `audiences` | Yes | Array identifying the groups for whom the resource is materially useful. |
| `media_type` | Yes | One normalized value describing how the resource is delivered. |
| `url` | No | Primary HTTP or HTTPS destination. Use `null` when no suitable URL is available. |
| `last_verified_at` | Yes | ISO `YYYY-MM-DD` date on which the URL and basic metadata were last checked. |

Taxonomy values are stored as lowercase kebab-case. The interface converts
dashes to spaces and title-cases values for display, so label overrides and
manually formatted variants are unnecessary.

## Goal lists

The **Goal lists** page applies the filters in `script#embeddedGoalFilters` to
the same 79-resource catalog used by the main library.

Current raw filter results are:

| Goal | Match logic | Raw matches | Grouping |
| --- | --- | ---: | --- |
| Build Clinical Framework | `clinical-knowledge` and any of `introductory-education`, `core-reference`, `comprehensive-reference`, `high-yield-overview`, `quick-reference`, or `guidelines` | 40 | `introductory-education`, `quick-reference`, `high-yield-overview`, `core-reference`, `comprehensive-reference`, and `guidelines` tags |
| Contouring & Treatment Planning | Any of `contouring`, `treatment-planning`, `constraints`, or `anatomy` | 30 | `contouring`, `anatomy`, `constraints`, and `proton-therapy` tags; unmatched resources appear under Other |
| Board Preparation | Any of `board-preparation` or `case-vignettes` | 23 | `case-vignettes`, `practice-based-learning`, `quick-reference`, `core-reference`, `physics`, and `radiation-biology` tags |
| Clinical Quick References | `clinical-knowledge` and any of `on-call`, `quick-reference`, `guidelines`, or `clinical-decision-support-tools` | 14 | `on-call`, `quick-reference`, `guidelines`, and `clinical-decision-support-tools` tags |
| Active Learning / Retrieval Practice | Any of `case-vignettes` or `practice-based-learning` | 12 | Media type |
| Staying Current | Any of `literature-review` or `newsletters`, excluding `core-reference` and `resource-list` | 8 | Media type |
| Professional Development | `professional-development` | 6 | Audience |

Counts overlap because one resource can match multiple goals. Collectively the
goals capture 78 of 79 resources; ROECSG Podcast Search Engine remains
available through normal library browsing.

Hovering a resource title in a goal list—or focusing it with the keyboard—shows
its description, tags, audiences, and media type. This tooltip is intentionally
limited to the Goal lists page; the Library table continues to display metadata
directly in its columns.

Books are currently hidden by default from all goal lists through
`options.hide_media_types`. Users can reveal them with the **Show Books**
control. Raw counts in the table above include books; the initial goal-page
counts do not. For example, Build Clinical Framework has 40 raw matches and 21
initially displayed non-book matches.

### Goal-filter schema

```yaml
options:
  hide_media_types:
    - "books"

goals:
  - id: "example-goal"
    title: "Example Goal"
    description: "A short explanation of the user's goal."
    filter:
      all_tags:
        - "clinical-knowledge"
      any_tags:
        - "core-reference"
        - "quick-reference"
      none_tags:
        - "resource-list"
      any_audiences:
        - "residents"
    group:
      by: "tags"
      tags:
        - "core-reference"
        - "quick-reference"
```

Every populated filter group must match:

- `all_tags`: the resource must carry every listed tag.
- `any_tags`: the resource must carry at least one listed tag.
- `none_tags`: the resource must carry none of the listed tags.
- `any_audiences`: the resource must include at least one listed audience.

Every goal requires a unique `id`, `title`, `description`, and nonempty
`filter`. Omitting a filter group leaves that condition unrestricted.

`group` is optional and changes presentation rather than matching:

- `by` accepts `media_type`, `audience`, or `tags`.
- Grouping by `tags` also requires a `tags` array.
- A resource carrying multiple configured group tags appears under every
  applicable tag heading.
- A matching resource without a configured group tag appears under Other.

The supported syntax is `group.by: "tags"` plus `group.tags`; `by_tags` is not
a recognized field.

## Taxonomy principles

`media_type`, `tags`, and `audiences` answer different questions:

- `media_type`: How is the resource delivered?
- `tags`: What does it cover, how deep is it, and how is it used?
- `audiences`: Who is it for?

Avoid encoding delivery format in a tag when `media_type` already expresses it.
For example, a website is not automatically an `online-reference`; the former
`online-reference`, `textbooks`, `study-guide-books`, and `study-guides` tags
were removed for this reason.

### Reference depth

These tags describe coverage depth. A resource should use no more than one of
the following:

| Tag | Definition |
| --- | --- |
| `comprehensive-reference` | Broad, detailed, authoritative tertiary material intended for sustained study or extensive lookup, including uncommon details and edge cases. |
| `core-reference` | Systematic coverage of the standard curriculum or commonly needed knowledge without attempting to be exhaustive. |
| `high-yield-overview` | Selective educational review emphasizing the most important concepts, decisions, evidence, or exam material. |

### Orthogonal reference characteristics

These describe other dimensions and may coexist with a reference-depth tag:

| Tag | Definition |
| --- | --- |
| `quick-reference` | Material organized for rapid lookup, such as a field guide, pocket handbook, table, calculator, or concise indexed reference. This describes retrieval style, not depth. |
| `literature-review` | Material whose purpose includes discussing, summarizing, or updating readers on primary research. This describes proximity to the evidence rather than breadth. |
| `clinical-knowledge` | Material used for clinical reasoning and decision-making: evaluation, diagnosis, staging, treatment selection or sequencing, management, prognosis, and the supporting clinical literature. It excludes resources limited to the technical radiation treatment-planning process. |

A resource can therefore be both `core-reference` and `quick-reference`, or both
`high-yield-overview` and `literature-review`. A comprehensive textbook and a
literature-update podcast occupy different evidence roles even when both are
clinically useful.

### Clinical knowledge versus treatment planning

Use `clinical-knowledge` for deciding **what should be done for a patient**:

- diagnostic evaluation and workup;
- staging and prognosis;
- observation, surgery, systemic therapy, or radiation selection;
- treatment intent, indications, sequencing, and management;
- toxicity management and follow-up;
- clinical evidence supporting those decisions.

Use `treatment-planning` for deciding **how radiation will technically be
planned and delivered**:

- simulation and immobilization;
- image registration;
- target and organ-at-risk definition;
- beam, field, or modality design;
- plan optimization and evaluation;
- technical delivery workflow.

A broad clinical resource may have both tags. A contouring atlas, dose-constraint
table, or technical planning demonstration should not receive
`clinical-knowledge` solely because it is used in patient care.

## Tag glossary

### Content and use

| Tag | Definition |
| --- | --- |
| `ai-tools` | Artificial intelligence is a central function or subject of the resource. |
| `anatomy` | Teaches or provides reference material for anatomy relevant to oncology or treatment. |
| `board-preparation` | Designed to support written boards, oral boards, initial certification, or maintenance of certification. |
| `case-vignettes` | Uses patient cases or simulated clinical scenarios as a primary teaching structure. |
| `clinical-decision-support-tools` | Interactive calculator, application, or question-answering system that helps apply information to a clinical case. Static clinical references use `clinical-knowledge` without this tag. |
| `constraints` | Provides radiation dose or dose-volume limits used during plan design or evaluation. |
| `contouring` | Teaches or supports target-volume or organ-at-risk delineation. |
| `guidelines` | Formal clinical recommendations, consensus guidance, or appropriateness criteria. Do not use merely because a resource cites guidelines. |
| `introductory-education` | Orientation or foundational material for learners new to radiation oncology or a defined part of the field. |
| `on-call` | Practical material focused on urgent clinical questions, emergencies, handoffs, or call preparation. |
| `physics` | Covers the physical principles, dosimetry, equipment, or technical foundations of radiation therapy. |
| `practice-based-learning` | Uses questions, exercises, quizzes, mock cases, or other active practice rather than presentation alone. |
| `professional-development` | Covers careers, mentorship, leadership, academic medicine, research careers, communication, finance, or professional skills. |
| `proton-therapy` | Substantially teaches or supports proton therapy rather than mentioning it incidentally. |
| `radiation-biology` | Covers biological responses to radiation, fractionation, normal-tissue effects, tumor response, or related mechanisms. |
| `resource-list` | Primarily points to or organizes other resources rather than serving as the underlying educational content itself. |
| `treatment-planning` | Covers the technical process of simulation, target definition, plan design, evaluation, or radiation delivery. |

### Format-like tags retained in the current data

| Tag | Definition |
| --- | --- |
| `lectures` | Identifies lecture-style material, including collections whose individual items are lectures. Because this can overlap `media_type`, avoid adding it when delivery format alone is the only reason. |
| `newsletters` | Identifies newsletter series. This currently overlaps the `newsletters` media type and is retained for existing filtering behavior. |

## Audience values

| Value | Definition |
| --- | --- |
| `medical-students` | Useful to medical students exploring radiation oncology or completing an oncology-related rotation. |
| `residents` | Useful during radiation oncology residency; this is the collection's primary audience. |
| `attendings` | Useful to practicing radiation oncologists or other attending physicians beyond trainee-level review. |
| `researchers` | Materially useful for clinical investigators, physician-scientists, or other oncology researchers. |

Audience values are not exclusive. Add every group for which the resource has
substantial value, but do not add audiences based on incidental usefulness.

## Media types

Use one normalized media type per resource:

| Value | Definition |
| --- | --- |
| `applications` | Interactive software, calculator, search tool, or web application |
| `books` | Textbook, handbook, pocket book, or other monograph |
| `courses` | Structured multi-part course or bootcamp |
| `lectures` | Recorded or hosted lecture series |
| `newsletters` | Recurring newsletter publication |
| `other` | Resource that does not fit another established type |
| `podcasts` | Audio-first episodic program |
| `slide-decks` | Presentation slides or slide-based handbook |
| `spreadsheets` | Spreadsheet, table workbook, or spreadsheet-style dataset |
| `webinars` | Recorded or live webinar series |
| `webpages` | Static website, page, wiki, or web-based written reference |

Keep this list low-cardinality. Add a new media type only when an existing value
would materially misrepresent delivery.

## Classifying a resource

Use this sequence when adding or revising an entry:

1. Select one `media_type` based on delivery.
2. Add all materially relevant `audiences`.
3. Determine whether it supports `clinical-knowledge`, technical
   `treatment-planning`, or both.
4. If applicable, choose at most one coverage-depth tag:
   `comprehensive-reference`, `core-reference`, or `high-yield-overview`.
5. Add `quick-reference` when the resource is deliberately optimized for lookup.
6. Add `on-call` when urgent clinical questions, emergencies, handoffs, or call
   preparation are a substantial focus.
7. Add `literature-review` when discussing or synthesizing primary research is
   a significant purpose.
8. Add the remaining subject and learning-method tags conservatively.

Tags should describe a substantial use of the resource, not every topic it
mentions.

## Editing the catalog

1. Open `index.html` in a text editor.
2. Edit only the YAML inside `script#embeddedSources` near the top unless an
   interface change is intended.
3. Keep the YAML root as an array and preserve two-space indentation.
4. Use a unique stable `id`, controlled taxonomy values, and a one-sentence
   description.
5. Use a valid URL or `null`.
6. Update `last_verified_at` when the destination and metadata are checked.
7. Open `index.html` in a browser and confirm that the library loads, filters
   appear, search works, and no YAML error is shown.

Do not edit the CDN dependency tags when changing resource data.

## Editing goal lists

1. Open `index.html` in a text editor.
2. Edit the YAML inside `script#embeddedGoalFilters`.
3. Preserve the top-level `options` object and `goals` array.
4. Use only supported filter rules: `all_tags`, `any_tags`, `none_tags`, and
   `any_audiences`.
5. When grouping, use `group.by` and add `group.tags` when `by` is `tags`.
6. Keep goal IDs unique and use only taxonomy values present in the resource
   catalog.
7. Open `index.html#goals` and confirm that every goal renders, counts are
   sensible, grouping headings appear, and the hidden-media controls work.

When resource tags change, recalculate goal counts and update
`2026-07-28-user-facing-goal-categories.md`. A resource should enter a goal
because the source is materially focused on that use, not because the topic
appears incidentally somewhere in a broad collection.

## GitHub Pages deployment

The Pages workflow deploys a minimal artifact containing only `index.html` and a
generated `.nojekyll` marker. The archival PDF and project notes are therefore
not served as website files.

To publish the site:

1. Push this repository to GitHub with `main` as its default branch.
2. In the repository's **Settings → Pages**, set **Source** to **GitHub
   Actions**.
3. Push to `main` or manually run **Deploy to GitHub Pages** from the Actions
   tab.

Every subsequent push to `main` republishes the page.
