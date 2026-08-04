# Project Instructions

These instructions apply to the entire repository. They are intended for both
automated agents and human contributors.

## Start Here

Before changing anything:

1. Read `README.md` for the current schemas, taxonomy, goal definitions, and
   deployment instructions.
2. Read `ARCHITECTURE.md` for runtime structure, data flow, state, and known
   limitations.
3. Run `git status --short` and inspect relevant diffs. The worktree may
   contain changes from another human or agent; preserve them and avoid broad
   rewrites.
4. Identify whether the request concerns resource data, goal YAML, interface
   behavior, deployment, or documentation. Do not expand the scope without a
   clear reason.

## Sources of Truth

- `index.html`, `script#embeddedSources`: resource catalog.
- `index.html`, `script#embeddedGoalFilters`: active goal options, filters,
  ordering, and grouping.
- `README.md`: supported schemas and active taxonomy.
- `2026-07-28-user-facing-goal-categories.md`: user-facing goal rationale and
  count snapshots.
- `2026-07-28-user-facing-goal-category-revision-plan.md`: historical decision
  and implementation record for the current goal model.
- `ARCHITECTURE.md`: application architecture and change-impact guidance.
- `.github/workflows/pages.yml`: production deployment.

The application has no separate `sources.yml` or `goal-filters.yml` file.
Their contents are embedded YAML inside `index.html`.

## Change Discipline

### Resource data

For catalog-only work, edit only the YAML inside
`script#embeddedSources`. Avoid reformatting unrelated HTML, CSS, JavaScript, or
other resource records.

- Keep `id` stable, unique, and lowercase kebab-case.
- Treat `priority` as optional editorial ordering; higher values sort first.
- Keep descriptions short, specific, and about scope or use.
- Use controlled tag, audience, and media-type values from `README.md`.
- Use no more than one reference-depth tag:
  `comprehensive-reference`, `core-reference`, or `high-yield-overview`.
- Use `url: null` when there is no suitable destination.
- Update `last_verified_at` only when the destination and basic metadata were
  actually checked.
- Preserve user-authored ordering unless the task explicitly changes it.

Tags must describe a substantial focus of the source, not every topic that
appears somewhere within it. This is especially important for webinar
archives, newsletters, thread collections, and resource lists. Follow their
links when classification depends on their contents. A broad collection should
not inherit a specialized tag merely because one item in the collection covers
that subject.

### Goal definitions

For goal-only work, edit the YAML inside `script#embeddedGoalFilters`.

Supported filter rules are:

- `all_tags`
- `any_tags`
- `none_tags`
- `any_audiences`

Every populated rule must pass. Do not invent unsupported fields.

Supported grouping values are `media_type`, `audience`, and `tags`. Tag
grouping uses:

```yaml
group:
  by: "tags"
  groups:
    - label: "Example Group"
      any_tags:
        - "example-tag"
```

Every tag group requires a nonempty `label` and `any_tags` array. Values within
`any_tags` use OR logic. The older `group.tags` shorthand remains supported,
but new and revised definitions should use `group.groups`, and a group object
must not populate both forms. `by_tags` is invalid. Tag grouping is
nonexclusive: a resource appears in every configured tag group it matches. A
match carrying none of the configured group tags appears under Other.

Goal names are navigation shortcuts, not resource tags. Do not create a new
resource tag solely to make a goal easier to express without first documenting
and agreeing to that taxonomy change.

### Interface code

The HTML, CSS, and JavaScript are intentionally colocated in `index.html`.
Make targeted changes and avoid whole-file formatting.

- Preserve escaping through `escapeHtml`.
- Preserve URL validation through `safeUrl`; rendered resource links must be
  HTTP or HTTPS.
- Keep external links protected with `rel="noopener noreferrer"`.
- Maintain keyboard, focus, responsive, reduced-motion, and print behavior.
- Do not change pinned CDN versions or SRI hashes casually. A dependency
  update requires matching version, URL, integrity hash, documentation, and
  browser validation.
- Do not add a build system or framework unless the project owner explicitly
  chooses that architectural change.

### Deployment

Production is GitHub Pages. `.github/workflows/pages.yml` deploys only
`index.html` and a generated `.nojekyll` marker. Project notes, the README, and
the archival PDF are not part of the deployed artifact.

Do not trigger a deployment, push, or commit unless the user explicitly asks.

## Mandatory Pre-Commit Documentation Audit

Before every commit:

1. Inspect the complete working and staged changes with `git status`,
   `git diff`, and `git diff --cached`.
2. Determine whether the changes require updates to `README.md`, `AGENTS.md`,
   or `ARCHITECTURE.md`.
3. Make every necessary documentation update before committing.
4. Re-read the three documents against the final diff and current behavior.
5. Commit only after this documentation audit is complete.

This check is mandatory even when the requested change appears small. Examples:

- Resource or taxonomy changes may alter goal counts and README examples.
- Goal filter or grouping changes require README and architecture review.
- Runtime, dependency, navigation, or deployment changes usually require
  `ARCHITECTURE.md`.
- Workflow or contributor-process changes usually require `AGENTS.md`.

Also update `2026-07-28-user-facing-goal-categories.md` whenever resource
metadata or goal logic changes its counts, represented tags, grouping, or
rationale.

## Validation

There is no package manager, build command, or automated test suite. Validation
must be proportional to the change.

At minimum:

1. Parse both embedded YAML blocks.
2. Confirm resource IDs and goal IDs are unique.
3. Recalculate affected goal counts.
4. Run `git diff --check`.
5. Inspect the final diff for accidental changes outside the requested scope.

A useful resource-data smoke check is:

```bash
ruby -ryaml -e 'h=File.read("index.html"); s=YAML.safe_load(h[/<script[^>]+id="embeddedSources"[^>]*>(.*?)<\/script>/m,1], aliases:true); g=YAML.safe_load(h[/<script[^>]+id="embeddedGoalFilters"[^>]*>(.*?)<\/script>/m,1], aliases:true); abort "bad resources" unless s.is_a?(Array); abort "bad goals" unless g.is_a?(Hash) && g["goals"].is_a?(Array); abort "duplicate resource ids" unless s.map{|x|x["id"]}.uniq.length==s.length; abort "duplicate goal ids" unless g["goals"].map{|x|x["id"]}.uniq.length==g["goals"].length; puts "#{s.length} resources, #{g["goals"].length} goals"'
```

For browser-facing changes, serve the repository with any static HTTP server
and test both routes:

- `index.html` or `#goals`: goal lists are the default view.
- `#library`: searchable and filterable library.

Manual checks should cover the affected behavior and, when relevant:

- CDN dependencies load without integrity errors.
- Goal filters, exclusions, ordering, groups, Other buckets, Show Books
  control, and metadata tooltips.
- Library search, priority sorting, include/exclude facets, tag-pill shortcuts,
  and pagination.
- Keyboard focus, narrow viewport layout, reduced motion, and external links.
- Error states for malformed YAML or unavailable CDN dependencies.

## Documentation Conventions

- Keep `README.md` user- and maintainer-oriented.
- Keep `ARCHITECTURE.md` focused on runtime structure, contracts, and tradeoffs.
- Keep this file focused on contributor behavior and guardrails.
- Prefer exact field names, function names, and file paths over vague prose.
- Distinguish raw goal-filter counts from initially displayed counts. Books are
  currently matched normally but hidden by default on the Goal lists page.
- Treat count snapshots as derived data. Recalculate them from the live YAML
  rather than copying older documentation.

## Current Intentional Conditions

At the time of this handoff:

- The catalog contains 79 resources and seven active goal filters.
- Books are hidden by default from goal lists but can be revealed.
- Resources may match multiple goals and multiple tag groups.
- ROECSG Podcast Search Engine intentionally matches no goal and remains
  available in the Library.
- Network access is required at runtime for the two pinned CDN dependencies and
  for external resource links.

These are current decisions, not permanent constraints. If they change, update
the implementation and all affected handoff documentation together.
