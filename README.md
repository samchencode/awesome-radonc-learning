# ARRO Resources

ARRO Resources is a curated directory of radiation oncology learning and
reference material. It is designed primarily for residents, with additional
resources for medical students, attending physicians, and researchers.

The project is a single, offline-ready HTML application. It supports full-text
search, pagination, sorting, and include/exclude filters for media type,
audience, and tags. No server, build process, package installation, or hosting
is required.

Opening `index.html` locally is enough to use the directory. The interface works
offline; following links to external resources still requires network access.

## Project structure

| File | Purpose |
| --- | --- |
| `index.html` | The complete application, embedded YAML resource data, styles, interface code, and vendored dependencies |
| `README.md` | Project structure, schema, and active taxonomy definitions |
| `export.pdf` | Archival source document used to assemble and order the initial collection |
| `2026-07-28-ideas.md` | Metadata fields considered for a possible future schema expansion |
| `2026-07-28-user-facing-goal-categories.md` | Brainstorming for a possible goal-oriented navigation layer; these are not active tags |

The first element inside the document `<head>` is:

```html
<script id="embeddedSources" type="text/yaml">
```

The YAML array inside that element is the resource catalog and the primary
editable data. It appears before the application code so a nontechnical editor
can update the list without searching through the rest of the HTML.

The application vendors minified copies of js-yaml 4.1.1 and List.js 2.3.1
inside `index.html`. These allow the page to parse, search, filter, sort, and
paginate the YAML without downloading scripts.

The **Load a YAML file** control can preview another YAML array at runtime. It
does not modify `index.html` or save the uploaded data.

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
6. Add `literature-review` when discussing or synthesizing primary research is
   a significant purpose.
7. Add the remaining subject and learning-method tags conservatively.

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

Do not edit the vendored minified dependency scripts when changing resource
data.
