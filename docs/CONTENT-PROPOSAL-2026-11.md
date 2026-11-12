# Content proposal: openrpms.org, November 2026

What this is: a review of https://openrpms.org as a visitor meets it, and a proposal for making its people-first approach clear and its two releases usable.
It covers who visits and what each needs first, what works today, what is unclear or missing page by page, a "how to use it" path for each artifact, a proposed information architecture, copy changes, and a ranked list of what to do first.
The pull request that carries this file makes the top changes, so they can be seen rendered.

Reviewed 2026-11-11 against this repository at `fab4499`, the live site, https://ux.openrpms.org, the rpms-ops documentation (`docs/DELIVERABLES.md`, `README.md`, `docs/ENGINES.md`) and its releases up to `bcer-9.0-20261111-7d0e827-ydb` and `bcer-9.0-20261109-d3adcff-iris`, and the site kit runbook.

## Who visits, and what each needs first

The four visitors in the brief hold up, with one addition.

| Visitor | Their first question | What they need to do first | Does the site let them today? |
|---|---|---|---|
| **Tribal and IHS site IT staff**, evaluating or installing | "Can I run this, on what, and what do I give up?" | Pick an engine, see what each includes, run one, and know it worked | Partly. /install/ is good, but nothing links to it from the home page or the menu, and it has no path for a site that already runs RPMS |
| **Clinical and administrative staff**, deciding whether it fits their work | "Does it do my job the way my clinic does it?" | Sign in to the demo as their role and walk one task | Partly. /demo/ is clear and kind, but the home page never mentions it, so they have to guess at the URL |
| **Developers and integrators**, building on the RPCs | "What can I call, and how do I make a first call?" | Run an instance and make one call from code | Yes, if they find /install/: its hello world is complete. The RPC list on https://ux.openrpms.org is not linked from here |
| **IHS and partners**, interested in building RPMS in the open together | "Who is doing this, how is it built, and how do we take part?" | Understand what the build does, see its evidence, and find a way to engage | No. There is no page about the open build, how a release is checked, or how to take part |
| **Learners and researchers** (public health students, program staff, funders) | "What is RPMS, and how is it put together?" | Read the reference pages | Yes. The domain model, architecture, workflows and catalog serve them well |

## What works today

- **The people-first framing is right, and /design/ says it well.**
  People, needs, processes, screens is a clear ladder, and the example user story is concrete.
- **/install/ has been rehearsed by a newcomer** (https://github.com/afomi/open-rpms-website/pull/7) and trimmed as the build improved (https://github.com/afomi/open-rpms-website/pull/8, https://github.com/afomi/open-rpms-website/pull/10).
  It takes a reader from nothing to a sign-on and a first call from code.
- **/demo/ is a model of a terms page:** plain, specific, respectful, and it asks for the feedback the project needs.
- **The voice is consistent:** short sentences, no hype, and honest about limits.
- **The reference pages** (domain model, architecture, workflows, security, catalog) give the site depth that a learner can trust.

## What is unclear or missing, page by page

### Home (`index.html`)

- **The promise is about knowledge only.**
  It says the site "distills patterns into open, reusable knowledge", which was true before the releases existed.
  It does not say that you can try RPMS, run it, or build on it today.
- **No way in to the demo or the install.**
  Neither /demo/ nor /install/ is linked from the home page or the menu.
  The two most useful things on the site are reachable only by typing the URL.
- **"Why this exists" closes on what the project is free of**: "free of the MUMPS runtime, the IRIS license, and the 1990s UX".
  It reads as against things, and it now contradicts the site, which offers RPMS on IRIS and runs RPMS on M.
  The aligned version says what the project carries forward.
- **Two footers.**
  The page closes its own footer and then the layout adds the site footer.

### Navigation (`_includes/nav.html`)

- Six items, all reference: Design, Domain Model, Architecture, Workflows, Security, Applications.
  Nothing for trying, installing, or taking part.
- The mobile menu lists two pages the desktop menu does not (EHR Site Tracking, EHR Site Survey), so the two menus disagree.

### Install (`install.html`)

- **Two paths, where there are three.**
  It covers YottaDB and IRIS for a newcomer.
  A site that already runs RPMS, which is how most sites will meet this work, has no path.
  A KIDS patch is how RPMS sites take changes, and every YottaDB release carries one.
- **No "what's included" by engine.**
  The YottaDB line has no BPRM or BMW (the web registration and scheduling tier) and no BUSA audit reviewer report.
  A site choosing an engine needs that before it pulls 6 GB.
- **No word on certification.**
  IRIS is the engine IHS certifies RPMS on.
  Site IT staff will ask first.
- **No "you know it worked when" for each path,** and no "where to get help".
- **The release lookup reads only the first 30 releases.**
  YottaDB releases come out daily, so the newest `-iris` release will drop off the first page of the GitHub API within weeks.
  Asking for `per_page=100` fixes it.
- **The release lookup needs the repository to be public.**
  Today `api.github.com/repos/lakeraven/rpms-ops/releases` answers 404 to a visitor, and the image `ghcr.io/lakeraven/rpms-ydb` needs a login.
  The page is written for the public repositories, which is the intent; until they are public, every command on it fails for an outside reader.

### Demo (`demo.html`)

- Strong as it is.
- Two small gaps: it lists five personas while /install/ lists seven (the registration supervisor is missing, and the billing technician is terminal only, which is worth saying), and it does not point to https://ux.openrpms.org, where each role's journeys are drawn.

### Design (`design.html`)

- The page is titled "People first" and the menu calls it "Design".
  One name for both would help; "People" is the one a clinic reader recognizes.

### Architecture, security, domain model, workflows, catalog

- Good reference material, but nothing on them says how a release of RPMS is built today: from IHS's FOIA release, gated, with evidence on every release.
  That story belongs on a page for IHS and partners (below), linked from architecture.

### Blog (`blog.html`, `_posts/`)

- The two posts are the template's own ("Getting Started With Your Own Website", author "Your Name").
  They are not linked from the menu, but they are published at /blog/ and in the feed and sitemap.
  Remove them, or replace them with a first real post.

### https://ux.openrpms.org

- It opens on a status snapshot from an older build ("YDB stack 0928") and a dense workflow table, and it has no link back to https://openrpms.org.
- For a clinic reader, a short "start here: pick your role" above the table, and a link home, would make it the natural next step from /design/.
- Those changes belong in the rpms-ux repository, not here.

## How to use each artifact

Each path below says what you get, what you need, the first step, how you know it worked, and where to get help.

### 1. RPMS on YottaDB: the container image

**For:** site IT staff evaluating, developers, and anyone who wants RPMS running in minutes with no license.

- **What you get:** one container image, `ghcr.io/lakeraven/rpms-ydb:<tag>`, with RPMS at the current IHS certified patch level, the demo clinic (staff personas and synthetic patients), TaskMan, the CIA broker on 9100 (VueCentric and rpms-rpc), the BMX broker on 9101 (iCare and .NET clients) and the terminal over SSH.
  Every release carries its evidence: what it contains, the client calls it answered, and each persona's first steps.
- **What you need:** Docker, about 20 GB of disk, `curl` and `jq`.
  It runs on x86-64, or on Apple Silicon under emulation.
- **First step:** look up the newest `-ydb` release and `docker run` it (the commands are on /install/).
- **You know it worked when:** `docker logs` shows `TaskMan live`, and `ssh -p 2222 rpms@127.0.0.1` reaches `ACCESS CODE:` and signs on as PROFILE,MD to `Select IHS Core <TEST ACCOUNT> Option:`.
- **Where to get help:** the release's `MANIFEST.md`; issues on https://github.com/lakeraven/rpms-ops/issues.
- **What it does not include:** BPRM and the BMW SQL tier, and the BUSA audit reviewer report, which run on IRIS only.
  It is not a certified deployment.
- **For a site's own VMware:** the site kit (in the deploy repository) installs the same image on a site's own machines with a share, workstations and checks.
  It is sent to a site, not published, so the site names it and asks for it.

### 2. RPMS on IRIS for Health: the databases, with your own license

**For:** site IT staff who run IRIS today or need the engine IHS certifies, and anyone who needs BPRM.

- **What you get:** the RPMS database, `IRIS.DAT`, with the IHS patch ladder already installed, and the BMW database that BPRM reads, each with a sha256 sidecar, plus the same evidence as a YottaDB release.
- **What you need:** an InterSystems IRIS for Health license and an image it covers (from the InterSystems registry, which needs a login), Docker, about 45 GB of disk, `curl`, `jq`, `git`.
  To evaluate without a license, IRIS for Health Community Edition runs the RPMS database alone.
- **First step:** download the two databases from the newest `-iris` release, check their sums, and run `bin/standup.sh` from rpms-ops.
- **You know it worked when:** the script prints `BUILD-LEVEL: PASS` and then `RPMS up`, and `ssh -p 12222 rpms@127.0.0.1` signs on as PROFILE,MD.
- **Where to get help:** the release's `MANIFEST.md`; issues on https://github.com/lakeraven/rpms-ops/issues; InterSystems for license and engine questions.

### 3. A KIDS patch, for a site that already runs RPMS

**For:** a site's RPMS system administrator.
A KIDS patch is how RPMS sites take changes, so this is the path most existing sites will look for first.

- **What you get today:** every YottaDB release carries `LKR*1.0*<n>`, the next patch of LAKERAVEN (namespace `LKR`), with an install note.
  Patch 1 carries every routine fix; each later patch carries what changed since the one before.
  A release can also carry stand-alone KIDS builds, such as `LKR BMX LISTENER 1.0`, the BMX listener iCare uses.
- **Which sites it is for:** **YottaDB sites only.**
  Several of the routines are YottaDB rewrites of stock routines, and on IRIS they would replace working code.
  A site on IRIS, as IHS sites are, keeps taking IHS's own patches.
  Our IRIS releases carry their fixes inside the database, not as a KIDS build.
- **What you need:** a YottaDB RPMS, programmer access to the Kernel Installation & Distribution System menu, a backup (a snapshot of the database, because KIDS has no uninstall), and the LKR patches in order from patch 1.
- **First step:** download the LKR patches and their install notes, read the note, then in KIDS: Load a Distribution, Verify Checksums, Compare Transport Global to Current System, Backup a Transport Global, Install Package(s).
- **You know it worked when:** KIDS reports `Install Completed`, and `W $$PATCH^XPDUTL("LKR*1.0*<n>")` answers `1`.
- **Where to get help:** the install note on each patch; issues on https://github.com/lakeraven/rpms-ops/issues.
- **Said plainly:** the LKR patches are proven on our own image, installed into an older release; they have not yet been proven on a YottaDB RPMS other than ours.

### 4. The brokers, for developers (rpms-rpc and lakeraven-ehr)

- **What you get:** https://github.com/lakeraven/rpms-rpc, a Ruby client for the CIA, XWB and BMX brokers, and https://github.com/lakeraven/lakeraven-ehr, a web tier built on it.
- **What you need:** a running release (path 1 or 2), Ruby 4.0 or later, Bundler, a C compiler.
- **First step and how you know it worked:** the hello world on /install/ prints the first five patients of the demo clinic.
- **Where to get help:** issues on each repository.
  The RPCs each release registers are listed in its `rpcs.json`, and browsable on https://ux.openrpms.org.

### 5. The demo, for clinical and administrative staff

- **What you get:** https://demo.openrpms.org, a browser sign-in to a running release with synthetic patients.
- **What you need:** a browser, and to read the terms on /demo/.
- **First step:** sign in as your role and try one task from "Things to try".
- **You know it worked when:** you finish the task, or you find where it differs from how your clinic works, which is the feedback the project wants.
- **Where to get help, and to give feedback:** issues on https://github.com/afomi/open-rpms-website/issues.

## What's included, by engine

| | YottaDB release | IRIS release |
|---|---|---|
| License | none (YottaDB is open source) | your own InterSystems IRIS for Health license |
| What ships | a container image | `IRIS.DAT` and `BMW-IRIS.DAT`, mounted by `bin/standup.sh` |
| RPMS packages and patch level | the same | the same |
| Demo clinic, personas, synthetic patients | yes | yes |
| Terminal (SSH sign-on) | yes, port 2222 | yes, port 12222 |
| CIA broker (VueCentric, rpms-rpc) | yes, port 9100 | yes, port 9200 inside the container |
| XWB broker | no | yes, port 9100 inside the container |
| BMX broker (iCare, .NET) | yes, port 9101 | yes, inside the container; the standup does not publish it |
| BPRM and BMW (web registration and scheduling) | no | yes |
| BUSA audit reviewer report | no | yes |
| KIDS patch for an existing site | `LKR*1.0*<n>`, YottaDB sites only | none yet |
| Certification | not certified; we intend to pursue it | IRIS is the engine IHS certifies RPMS on. Our release reproduces a certified patch level and is not itself a certified deployment |
| Evidence on every release | yes | yes |

## Proposed information architecture

The menu leads with what a visitor can do, then with the reference.

| Menu | Page | For |
|---|---|---|
| **Try it** | /demo/ | clinical and administrative staff |
| **Install** | /install/: pick a path (YottaDB, IRIS, KIDS into existing RPMS), what's included, hello world | site IT staff, developers |
| **People** | /design/ (title "People first") | everyone, clinic staff first |
| **Workflows** | /workflows/ | clinic staff, learners |
| **Reference** | /reference/, a new index: domain model, architecture, security, application catalog, licensing, the IHS site survey and tracking record | learners, developers, site IT staff |
| **Build with us** | /together/, new: how a release is built and checked, what is public, how to take part | IHS and partners, contributors |

The home page becomes the hub: one promise, then "Start here" by visitor, then the engines at a glance, then what's here.
The footer keeps Licensing and the IHS RPMS link, and gains the repositories.

## Copy changes

- **Home headline.**
  From "Open-source business processes for healthcare." to "RPMS, built in the open, starting with the people who use it."
- **Home lede.**
  Say the three things a visitor can do today: try it in a browser, run it on YottaDB with no license or on IRIS with your own, and build on its RPCs.
  Keep the sentence about the work people do every day, which is the heart of the site.
- **"Why this exists", last paragraph.**
  From "free of the MUMPS runtime, the IRIS license, and the 1990s UX" to what the project carries forward: the operational knowledge, a reproducible build of each IHS release, and a way for clinics, IHS and developers to look at the same thing.
- **Install lede.**
  Lead with the choice: three ways in, and the table that says which one fits.
- **Certification, on /install/ and /together/.**
  "IRIS is the engine IHS certifies RPMS on.
  The YottaDB line runs the same certified patch level and is not certified today; we intend to pursue it."
- **Menu label.**
  "Design" becomes "People", to match the page's own title.
- **Demo.**
  Add the registration supervisor to the persona table, say the billing technician works in the terminal only, and point to the journeys on https://ux.openrpms.org.

## What to do first

1. **Put Try it and Install in the menu and on the home page** (in this pull request).
   The two most useful pages are invisible today.
2. **Rewrite the home promise and add "Start here" by visitor** (in this pull request).
   It turns a reference site into a site people can act on.
3. **Give /install/ its three paths, each ending in "you know it worked when" and "where to get help", with the what's-included table and the certification statement** (in this pull request).
4. **Make the repositories and the `rpms-ydb` image public** (rpms-ops, not this repository).
   Until then, every command on /install/ fails for an outside reader.
5. **Add /together/ for IHS and partners, and /reference/ to hold the reference pages** (in this pull request, as first drafts).
   Then remove the template blog posts, align the demo's personas with /install/, and add a "start here" and a link home on https://ux.openrpms.org.

## Commands on /install/, and how each was checked

- **Release lookup (paths 1 and 2):** the `jq` filters were run on 2026-11-11 against the releases list fetched with an authenticated `gh api`, and answered `bcer-9.0-20261111-7d0e827-ydb` and `bcer-9.0-20261109-d3adcff-iris`.
  `per_page=100` is new on this page.
  The anonymous `curl` answers 404 until rpms-ops is public, so the exact command as published is untested.
- **`docker run`, the sign-on, the IRIS download and `bin/standup.sh`:** unchanged from the page that a newcomer rehearsed (https://github.com/afomi/open-rpms-website/pull/7); not rerun for this review.
- **The LKR download (path 3):** the `jq` filter was run on the same releases list and found all 13 patches, `LKR*1.0*1` through `LKR*1.0*13`, and the `sort` lists them in install order.
  Patch 1 was downloaded with `gh release download` and is a KIDS distribution of `LKR*1.0*1`.
  The anonymous download, the KIDS steps and the `$$PATCH^XPDUTL` check were not run here; the KIDS steps are the ones each patch's install note gives.
- **The hello world:** unchanged; not rerun for this review.
- **The site:** `bundle exec jekyll build` succeeds, and every internal link and anchor on the changed pages resolves in the built site.

## Questions for the maintainer

1. **An IRIS KIDS path.**
   IHS sites run RPMS on IRIS, and today we offer them no KIDS build: our IRIS fixes ship inside the database.
   Should the release produce an `LKR` patch for IRIS sites, carrying the shared (both-engine) fixes, so an existing site can take them the usual way?
2. **Going public.**
   When do `lakeraven/rpms-ops` and the `rpms-ydb` package go public?
   The install page is written as if they are; should it say "coming soon" until then?
3. **The site kit.**
   Should it be named on the public site, with a way for a site to ask for it, or kept to direct conversations?
4. **Help channel.**
   Is rpms-ops issues the right place to send site IT staff, or should this repository's issues take everything, with triage behind it?
5. **Who stands behind it.**
   The site credits "OpenRPMS contributors".
   IHS and partners will want to know who to talk to.
   Is a contact address, or a named organization on /together/, wanted?
6. **The blog.**
   Remove the template posts, or keep /blog/ for release notes and updates?
