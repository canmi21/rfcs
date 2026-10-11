---
name: "The Site's Routes Are Kit's"
title: "The site's non-page routes are SvelteKit's, and every other worker's are hono's"
status: draft
supersedes: []
blocked-by: []
commits: []
references: []
---

## Question

`apps/site/api` is hono, as the platform's Workers are. `apps/site` is not, and it serves thirteen `+server.ts`
routes, measured on 2026-10-05, plus a handle that answers `<url>.md` before the router sees it:

| Route                                                           | Answers                                     |
| --------------------------------------------------------------- | ------------------------------------------- |
| `/atom.xml`                                                     | the assembled feed                          |
| `/sitemap.xml`                                                  | the assembled sitemap                       |
| `/llms.txt`, `/llms-full.txt`                                   | the assembled index, and every agent view   |
| `/sitemap.xsl`                                                  | the sitemap's stylesheet                    |
| `/site.webmanifest`                                             | the install manifest                        |
| `/favicon.ico`                                                  | the site's mark, followed for the browser   |
| `/.well-known/security.txt`                                     | the security contact                        |
| `/robots.txt`                                                   | a constant                                  |
| `/{key}.txt`                                                    | the IndexNow key                            |
| `/licenses.txt`, `/licenses/full.txt`, `/licenses/{...package}` | license text                                |
| `<url>.md`                                                      | an article's source, from `hooks.server.ts` |

The intent is that a page stays SvelteKit's and everything else becomes one hono app mounted
inside it, so that every non-HTML response this project serves is written the same way: one
router, one `failure` helper, one place a cache header is decided. Today the site answers those
questions in thirteen files and a handle, none of which share the helpers `apps/site/api`
already has.

**What has to be decided before it can be done.** Where the hono app is mounted -- a catch-all
`+server.ts` forwarding `event.request`, or `handle` in `hooks.server.ts` ahead of the router --
and the two differ in what they can reach. Hono would not have `event.fetch`, which is what makes
a same-origin subrequest work in SSR and what `$lib/published` takes as an argument, so that has
to be passed in rather than imported. `<url>.md` is the awkward one: it is a suffix on every
page's path rather than a route, so it is the case that decides whether the mount point can be a
route at all. And whether the license text routes survive the surface decision above is open, so
there is no reason to move them first.
