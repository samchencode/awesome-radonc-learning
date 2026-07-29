# User-Facing Goal Categories

These are ideas for a goal-oriented navigation layer. Goals act as shortcuts
into the existing filters rather than becoming tags themselves. Accepted
changes are identified in their respective sections.

Coverage figures below are a snapshot of the 79 resources embedded in
`index.html` on 2026-07-29. The number beside each represented tag is the number
of matching resources carrying that tag. Tag counts overlap because each
resource can carry multiple tags, and a resource can appear under multiple
goals.

## Proposed Goals

### Board Preparation

For users preparing for written boards, oral boards, initial certification, or
maintenance of certification.

Suggested filter:

- `board-preparation`

Group matching resources by:

- `practice-based-learning`
- `quick-reference`
- `core-reference`
- `physics`
- `radiation-biology`

Physics and radiation-biology resources intended for board study carry the
`board-preparation` tag explicitly. Broad textbooks no longer enter this goal
merely because they cover physics or radiation biology.

Current result: **21 of 79 resources**, including 6 books.

The goal page hides books and displays **15 unique resources**:
`practice-based-learning` (2), `quick-reference` (1), `core-reference` (1),
`physics` (4), `radiation-biology` (4), and `Other` (6). Group counts overlap
because a resource appears in every group whose tag it carries.

Tags represented: `board-preparation` (21), `clinical-knowledge` (13),
`high-yield-overview` (8), `physics` (5), `practice-based-learning` (5),
`radiation-biology` (5), `resource-list` (5), `treatment-planning` (4),
`case-vignettes` (3), `ai-tools` (2), `anatomy` (2),
`comprehensive-reference` (2), `contouring` (2),
`professional-development` (2), `core-reference` (1), `guidelines` (1),
`introductory-education` (1), `literature-review` (1), and
`quick-reference` (1).

This accepted change narrows the former 26-resource result by reducing the
number of books from 11 to 6 while retaining all 15 non-book resources.

Statistics does not currently represent enough of the collection to warrant
inclusion as a separate goal.

### Contouring & Treatment Planning

For users looking for contouring practice, atlases, dose constraints, planning
guidance, or technical delivery resources.

Suggested filter:

- `contouring` OR `treatment-planning` OR `constraints`
- `anatomy` (tx planning is the main reason we care about anatomy)

Current result: **30 of 79 resources**.

Tags represented: `treatment-planning` (26), `clinical-knowledge` (16),
`contouring` (13), `comprehensive-reference` (7), `quick-reference` (6),
`anatomy` (5), `core-reference` (5), `high-yield-overview` (5),
`board-preparation` (4), `constraints` (4), `physics` (4),
`radiation-biology` (4),
`practice-based-learning` (3), `professional-development` (3),
`proton-therapy` (3), `case-vignettes` (2), `introductory-education` (2),
`ai-tools` (1), `literature-review` (1), and `resource-list` (1).

### Quick Clinical Lookup

For users preparing for clinic, reviewing a new patient, checking a treatment
recommendation, or looking something up during a tumor board.

Suggested filter:

- `clinical-knowledge`
- AND at least one of `quick-reference`, `guidelines`, or `clinical-decision-support-tools`

Current result: **14 of 79 resources**.

Tags represented: `clinical-knowledge` (14), `quick-reference` (9),
`core-reference` (6), `guidelines` (4), `treatment-planning` (3),
`board-preparation` (2), `clinical-decision-support-tools` (2),
`constraints` (2), `ai-tools` (1), `case-vignettes` (1),
`comprehensive-reference` (1), `contouring` (1), and `resource-list` (1).

`clinical-knowledge` should not be used alone for this goal because it applies
to a large portion of the collection.

### Active Learning / Retrieval Practice

For users who want to learn actively through questions, mock cases, exercises,
or clinical scenarios.

Suggested filter:

- `case-vignettes` OR `practice-based-learning`

Current result: **13 of 79 resources**.

Tags represented: `clinical-knowledge` (13), `board-preparation` (8),
`practice-based-learning` (8), `case-vignettes` (6),
`high-yield-overview` (5), `treatment-planning` (4), `ai-tools` (2),
`contouring` (2), `anatomy` (1), `comprehensive-reference` (1),
`core-reference` (1), `introductory-education` (1),
`professional-development` (1), `quick-reference` (1), and `resource-list`
(1).

This and Quick Clinical Lookup replace the less intuitive Case Preparation
concept.



### Staying Current

For users looking for journal discussions, evidence summaries, journal clubs,
podcasts, newsletters, and other literature updates.

Suggested filter:

- `literature-review` OR `newsletters`
- AND NOT `core-reference` or `resource-list`

Current result: **8 of 79 resources**.

Tags represented: `clinical-knowledge` (7), `literature-review` (7),
`newsletters` (2), `professional-development` (2), `anatomy` (1),
`board-preparation` (1), `contouring` (1), `high-yield-overview` (1),
`introductory-education` (1), `physics` (1), `radiation-biology` (1), and
`treatment-planning` (1).

ARROgram enters through `newsletters`. Static resources used for learning from
scratch are excluded through `core-reference`; Rad Onc Talks and Handbook of
Evidence-Based Radiation Oncology no longer carry `literature-review`.
Resource lists are also excluded, and Zaorsky Educational Threads no longer
carries `literature-review`. ARRO Webinars and ACRO Resident Webinars remain in
the goal for now.

Media type can be used to refine this goal to podcasts, newsletters, webinars,
or other preferred formats.

### Incoming Learners

For incoming residents and other learners who are getting started in radiation
oncology.

Suggested filter:

- `introductory-education`
- AND the `residents` OR `medical-students` audience

Current result: **5 of 79 resources**.

Tags represented: `clinical-knowledge` (5), `introductory-education` (5),
`professional-development` (3), `high-yield-overview` (2),
`treatment-planning` (2), `ai-tools` (1), `anatomy` (1),
`board-preparation` (1), `contouring` (1), `core-reference` (1),
`literature-review` (1), `practice-based-learning` (1), and `resource-list`
(1).

The audience requirement helps distinguish resident orientation from resources
designed only for medical students.

### Build Clinical Framework

For users building a broad clinical radiation oncology framework from
introductory teaching, core and comprehensive references, high-yield reviews,
and quick-reference resources.

Suggested filter:

- `clinical-knowledge`
- AND at least one of `introductory-education`, `core-reference`,
  `comprehensive-reference`, `high-yield-overview`, or `quick-reference`

Group matching resources by:

- `introductory-education`
- `core-reference`
- `comprehensive-reference`
- `high-yield-overview`
- `quick-reference`

Current filter result: **37 of 79 resources**.

Tags represented: `clinical-knowledge` (37), `treatment-planning` (14),
`high-yield-overview` (12), `core-reference` (11),
`comprehensive-reference` (10), `board-preparation` (9),
`quick-reference` (9), `practice-based-learning` (6),
`introductory-education` (5), `contouring` (4), `physics` (4),
`professional-development` (4), `radiation-biology` (4),
`case-vignettes` (3), `anatomy` (2), `constraints` (2),
`literature-review` (2), `ai-tools` (1), `guidelines` (1), and
`resource-list` (1).

The current global goal-page option hides books. The page therefore displays
**18 unique resources** for this goal: `introductory-education` (5),
`core-reference` (4), `comprehensive-reference` (3),
`high-yield-overview` (7), and `quick-reference` (4). Group counts overlap
because a resource appears in every group whose tag it carries.

### Professional Development

For users looking for career development, mentorship, leadership, academic
medicine, research careers, communication, finance, or other professional
skills.

Suggested filter:

- `professional-development`

Current result: **6 of 79 resources**.

Tags represented: `professional-development` (6), `clinical-knowledge` (4),
`high-yield-overview` (3), `introductory-education` (3),
`treatment-planning` (3), `anatomy` (2), `board-preparation` (2),
`contouring` (2), `literature-review` (2), `resource-list` (2), `ai-tools`
(1), `physics` (1), `practice-based-learning` (1), and
`radiation-biology` (1).

`research-methods` should not be introduced because research methodology is not
a significant portion of the current collection.

## Suggested Interaction Model

- Present goals as prominent shortcuts above or alongside the detailed filters.
- Selecting a goal applies its mapped filters without changing resource
  metadata.
- Treat alternatives joined by OR as one goal-level match.
- Treat conditions joined by AND as required parts of the goal.
- Allow users to refine goal results with audience, media type, or individual
  tags.
- Clearly show which filters a selected goal activated.
- Allow more than one goal to apply to the same resource.
- When grouping by tags, allow a multi-tagged resource to appear in each
  applicable group.

## Ideas Not Yet Ready

### On-Call

On-Call remains a promising goal for emergencies, urgencies, and practical call
preparation. The current metadata does not identify these resources reliably
enough to build the shortcut yet. Do not add an `on-call` tag solely to support
this idea.

## Current Decisions

- Use goal categories as filter shortcuts rather than tags.
- Replace Case Preparation with Quick Clinical Lookup and Practice With Cases.
- Do not add `case-preparation`.
- Do not add `on-call` yet.
- Do not add `research-methods`.
- Do not add or emphasize `statistics`.
- Keep `literature-review` as the umbrella for evidence updates.
- Include `newsletters` in Staying Current and exclude `core-reference` and
  `resource-list`.
- Keep ARRO Webinars and ACRO Resident Webinars in Staying Current for now.
- Add Build Clinical Framework using `clinical-knowledge` plus at least one
  introductory, core, comprehensive, high-yield, or quick-reference tag.
- Group Build Clinical Framework by those five learning-resource tags.
