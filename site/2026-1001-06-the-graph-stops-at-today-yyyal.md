---
name: The Graph Stops at Today
title: "The structured graph stops at what today's data says"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

Each of these waits on data or a page that does not exist yet; the graph grows by the same rules
when it does. See web's `spec/architecture/entities.md`.

- **The home page as a `CollectionPage`**, its articles as `hasPart`, once the home page is decided
  to be the list it is.
- **What an article is about**: `about` and `mentions` naming Wikidata entities rather than strings,
  which needs a record mapping each tag to its item; the author's `knowsAbout` follows from the same
  record.
- **A project as two entities**: the running service as `WebApplication` and the code as
  `SoftwareSourceCode` with `codeRepository` and `programmingLanguage`, once a page shows projects.
  Something that fits no narrower type -- hardware, an experiment -- is a `CreativeWork`.
- **The status page's checks as `Service` nodes** in an `ItemList`, each identified by an anchor on
  its row, which the page does not have yet; `about` from the page, never `status` on the service.
- **`TechArticle.proficiencyLevel` and `dependencies`**, when the author states them in an
  article's frontmatter.
