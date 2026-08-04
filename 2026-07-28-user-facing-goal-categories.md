# User-Facing Goal Categories

These are ideas for a goal-oriented navigation layer. Goals act as shortcuts
into the existing filters rather than becoming tags themselves. Accepted
changes are identified in their respective sections.

Coverage figures below are a snapshot of the 79 resources embedded in
`index.html` on 2026-07-29. The number beside each represented tag is the number
of matching resources carrying that tag. Tag counts overlap because each
resource can carry multiple tags, and a resource can appear under multiple
goals.

Active tag-group definitions use labeled `group.groups` entries. Each grouping
tag listed below currently occupies its own entry with a one-value `any_tags`
array; none of the active groups combine multiple tags yet.

## Proposed Goals

### Board Preparation

For users preparing for written boards, oral boards, initial certification, or
maintenance of certification.

Suggested filter:

- `board-preparation` OR `case-vignettes`

Group matching resources by:

- `case-vignettes`
- `practice-based-learning`
- `quick-reference`
- `physics`
- `radiation-biology`

Physics and radiation-biology resources intended for board study carry the
`board-preparation` tag explicitly. Broad textbooks no longer enter this goal
merely because they cover physics or radiation biology.

Current result: **23 of 79 resources**, including 8 books.

The goal page hides books and displays **15 unique resources**:
`case-vignettes` (4), `practice-based-learning` (1),
`quick-reference` (1), `physics` (4), `radiation-biology` (4), and `Other` (4).
Group counts overlap because a resource appears in every group whose tag it
carries.

Tags represented: `board-preparation` (20), `clinical-knowledge` (15),
`high-yield-overview` (8), `case-vignettes` (6), `physics` (5),
`practice-based-learning` (5), `radiation-biology` (5),
`resource-list` (5), `treatment-planning` (5),
`comprehensive-reference` (3), `contouring` (3), `anatomy` (2),
`professional-development` (2), `ai-tools` (1), `core-reference` (1),
`guidelines` (1), `introductory-education` (1), `literature-review` (1),
and `quick-reference` (1).

This accepted change includes case-vignette resources in the board-preparation
shortcut while keeping the existing board-focused physics and radiation biology
coverage.

Statistics does not currently represent enough of the collection to warrant
inclusion as a separate goal.

### Contouring & Treatment Planning

For users looking for contouring practice, atlases, dose constraints, planning
guidance, or technical delivery resources.

Suggested filter:

- `contouring` OR `treatment-planning` OR `constraints`
- `anatomy` (tx planning is the main reason we care about anatomy)

Group matching resources by:

- `contouring`
- `anatomy`
- `constraints`
- `proton-therapy`
- Other, for resources without one of those four grouping tags

Current result: **30 of 79 resources**.

With books hidden by default, the goal page initially displays
`contouring` (8), `anatomy` (5), `constraints` (2), `proton-therapy` (3), and
Other (1). Group counts overlap because resources carrying multiple grouping
tags appear under each applicable heading.

Tags represented: `treatment-planning` (26), `clinical-knowledge` (16),
`contouring` (13), `comprehensive-reference` (7), `quick-reference` (6),
`anatomy` (5), `core-reference` (5), `high-yield-overview` (5),
`board-preparation` (4), `constraints` (4), `physics` (4),
`radiation-biology` (4),
`practice-based-learning` (3), `professional-development` (3),
`proton-therapy` (3), `case-vignettes` (2), `introductory-education` (2),
`ai-tools` (1), `literature-review` (1), `on-call` (1), and
`resource-list` (1).

### Clinical Quick References

For users preparing for clinic, reviewing a new patient, checking a treatment
recommendation, answering on-call questions, or looking something up during a
tumor board.

Suggested filter:

- `clinical-knowledge`
- AND at least one of `on-call`, `quick-reference`, `guidelines`, or
  `clinical-decision-support-tools`

Group matching resources by:

- `quick-reference`
- `guidelines`
- `clinical-decision-support-tools`

Current result: **14 of 79 resources**.

Tags represented: `clinical-knowledge` (14), `quick-reference` (9),
`core-reference` (6), `guidelines` (4), `treatment-planning` (3),
`on-call` (3), `board-preparation` (2),
`clinical-decision-support-tools` (2), `constraints` (2), `ai-tools` (1),
`case-vignettes` (1), `comprehensive-reference` (1), `contouring` (1), and
`resource-list` (1).

With books hidden by default, the goal page initially displays
`quick-reference` (4), `guidelines` (4), and `clinical-decision-support-tools`
(2). Group counts overlap because resources carrying multiple grouping tags
appear under each applicable heading.

`clinical-knowledge` should not be used alone for this goal because it applies
to a large portion of the collection.

### Active Learning / Retrieval Practice

For users who want to learn actively through questions, mock cases, exercises,
or clinical scenarios.

Suggested filter:

- `case-vignettes` OR `practice-based-learning`

Current result: **12 of 79 resources**.

Tags represented: `clinical-knowledge` (12), `board-preparation` (7),
`practice-based-learning` (7), `case-vignettes` (6),
`high-yield-overview` (4), `treatment-planning` (3), `ai-tools` (1),
`comprehensive-reference` (1), `contouring` (1), `core-reference` (1), and
`quick-reference` (1).

This and Clinical Quick References replace the less intuitive Case Preparation
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
  `comprehensive-reference`, `high-yield-overview`, `quick-reference`, or
  `guidelines`

Group matching resources by:

- `introductory-education`
- `core-reference`
- `comprehensive-reference`
- `high-yield-overview`
- `quick-reference`
- `guidelines`

Current filter result: **40 of 79 resources**.

Tags represented: `clinical-knowledge` (40), `treatment-planning` (14),
`high-yield-overview` (12), `core-reference` (11),
`board-preparation` (10), `comprehensive-reference` (10),
`quick-reference` (9), `practice-based-learning` (5),
`introductory-education` (5), `contouring` (4), `physics` (4),
`professional-development` (4), `radiation-biology` (4),
`guidelines` (4), `case-vignettes` (3), `on-call` (3), `anatomy` (2),
`constraints` (2), `literature-review` (2), and `resource-list` (2).

The current global goal-page option hides books. The page therefore displays
**21 unique resources** for this goal: `introductory-education` (5),
`core-reference` (3), `comprehensive-reference` (3),
`high-yield-overview` (7), `quick-reference` (4), and `guidelines` (4). Group
counts overlap because a resource appears in every group whose tag it carries.

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
`contouring` (2), `literature-review` (2), `resource-list` (2), `physics`
(1), and `radiation-biology` (1).

The goal page groups these resources by audience: `residents` (6),
`researchers` (3), `attendings` (2), and `medical-students` (2).

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

## Current Decisions

- Use goal categories as filter shortcuts rather than tags.
- Replace Case Preparation with Clinical Quick References and Practice With Cases.
- Do not add `case-preparation`.
- Add `on-call` for resources materially focused on urgent clinical questions,
  emergencies, handoffs, or call preparation.
- Do not add `research-methods`.
- Do not add or emphasize `statistics`.
- Keep `literature-review` as the umbrella for evidence updates.
- Include `newsletters` in Staying Current and exclude `core-reference` and
  `resource-list`.
- Keep ARRO Webinars and ACRO Resident Webinars in Staying Current for now.
- Group Contouring & Treatment Planning by `contouring`, `anatomy`,
  `constraints`, and `proton-therapy`, with remaining matches under Other.
- Add Build Clinical Framework using `clinical-knowledge` plus at least one
  introductory, core, comprehensive, high-yield, quick-reference, or guidelines
  tag.
- Group Build Clinical Framework by those six learning-resource tags.
