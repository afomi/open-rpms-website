---
layout: post
title: "Building the RPMS Open-Source Community"
date: 2026-05-28
author: "Ryan Wold"
excerpt: "OSEHRA is gone. The gap it left is real. Here is what Lakeraven, CivicActions, Civic Studio, and anyone else who cares about open-source health IT can do about it — starting now."
---

The [previous post](/blog/what-we-lost-when-osehra-closed/) described what was lost when OSEHRA closed in 2020.
This post is about what to do about it.

Not a proposal to re-create OSEHRA.
Not a manifesto.
A practical accounting of who is doing what, what gaps remain, and where contributors can plug in.

## Who is doing the work

Three organizations are actively building open-source RPMS infrastructure today.
None of them set out to fill OSEHRA's role.
Each started solving a specific problem and ended up holding a piece of the puzzle.

### CivicActions — the container pipeline

CivicActions maintains [docker-vista](https://github.com/CivicActions/docker-vista), the build system that turns FOIA-RPMS source code into a runnable IRIS container image.
They also maintain a fork of the [FOIA-RPMS](https://github.com/CivicActions/FOIA-RPMS) source repository.

This is foundational work.
Without docker-vista, there is no reproducible way to stand up an RPMS instance from source.
The last complete database file from IHS was March 2021.
docker-vista is now the de facto build pipeline for anyone deploying RPMS on modern infrastructure.

**What CivicActions provides:**
- A Docker build system that imports FOIA-RPMS routines and globals into IRIS
- Container images published to GitHub Container Registry (`ghcr.io/civicactions/rpms-iris`)
- Support for Linux amd64 and arm64
- Pre-configured brokers (XWB on 9100, CIA on 9200, BMX on 9101)
- Seed test data and users for development

**What CivicActions could use help with:**
- IRIS version pinning decisions (the current image runs 2026.1; BMW targets 2022.1)
- Automated testing of KIDS patch imports
- Documentation of the 10-step build process for new contributors
- CI/CD for building and publishing images on each FOIA release

### Lakeraven — tribal cloud infrastructure

Lakeraven operates cloud-hosted RPMS infrastructure for tribal health organizations.
They are doing something that IHS documentation does not cover: running RPMS in production on cloud infrastructure, on Linux, with infrastructure-as-code.

This matters because the default IHS deployment model assumes on-premise AIX or Windows servers managed by IHS Area IT staff.
Tribal organizations that want to self-host — in the cloud, on their own terms — have no IHS-supported path.
Lakeraven is building that path.

**What Lakeraven provides:**
- Production cloud hosting for tribal RPMS instances
- Proof that RPMS runs on AWS with Terraform-managed infrastructure
- Operational knowledge about running BPRM and VueCentric EHR in the cloud

### Civic Studio — documentation, integration, and security research

[Civic Studio](https://civic.studio) maintains this site (OpenRPMS) and has been working on the integration layer — getting the IHS application stack (BMW, BPRM, VueCentric) running on Linux IRIS, documenting the gaps, and reporting what we find.

**What Civic Studio provides:**
- [OpenRPMS](/) — open-source documentation of the RPMS domain model, architecture, workflows, and security patterns
- BMW extraction methodology — how to convert the Windows-only IRIS.DAT binary into portable XML class definitions
- IRIS compatibility workarounds — documented fixes for IRIS 2026.1 regressions
- Security research — responsible disclosure of vulnerabilities found during integration work
- Letters and proposals to IHS advocating for open-source distribution practices

**What Civic Studio could use help with:**
- Peer review of the BMW extraction process
- Testing BPRM on additional IRIS versions
- Contributing workflow documentation and clinical domain expertise
- Security auditing of RPMS components

### WorldVistA — the institutional anchor

[WorldVistA](https://worldvista.org/) is the 501(c)(3) nonprofit that inherited OSEHRA's VistA repositories.
They maintain the canonical open-source VistA distribution and host RPMS FOIA releases.
They hold annual conferences and maintain the community of practice.

WorldVistA's role is different from the other three.
They are the closest thing the ecosystem has to a governing body — not in the sense of authority, but in the sense of continuity.
When someone asks "where is open-source VistA?", the answer is WorldVistA.

## What the gaps are

Listing who does what makes the gaps visible:

### 1. No cross-platform BMW build

BMW ships as a binary IRIS.DAT for Windows and AIX.
We have a working extraction process (export as XML, import to Linux IRIS), but it is not automated, not tested in CI, and not published as a reproducible pipeline.

**What's needed:** A documented, scripted process that takes the IHS BMW distribution and produces a platform-neutral artifact (XML classes or a Linux IRIS.DAT).
Ideally integrated into the docker-vista build.

### 2. No IRIS version compatibility matrix

IHS certifies against one IRIS version.
InterSystems releases new versions annually.
Nobody publishes a compatibility matrix.
Each organization discovers version issues independently.

**What's needed:** A shared, community-maintained compatibility table: which RPMS packages work on which IRIS versions, with known issues and workarounds.
This site could host it.

### 3. No automated patch testing

IHS releases KIDS patches via FTP.
There is no automated way to test whether a patch applies cleanly to a given RPMS instance, whether it introduces regressions, or whether it is compatible with a specific IRIS version.

**What's needed:** A CI pipeline that applies patches to a docker-vista container and runs basic smoke tests.
Even simple tests — "does the system start, can a user log in, do the brokers respond" — would catch breakage before it reaches production.

### 4. No shared security review process

We found a hardcoded encryption key in BPRM by examining a publicly downloadable DLL.
There is no shared process for security review of RPMS components.
Each organization discovers issues independently, reports them independently, and waits independently.

**What's needed:** A lightweight coordinated disclosure process.
When someone finds an issue, there should be a known channel — beyond just the HHS VDP — where the RPMS community can be notified without publishing exploit details.

### 5. No contributor pathway for IHS

IHS maintains RPMS actively.
They ship patches, release new versions, and respond to bug reports.
But there is no mechanism for the community to contribute back — no pull request process, no issue tracker, no public roadmap.

This is the hardest gap to close because it requires IHS to change how it operates.
The [letter requesting GitHub publication](/implementation-notes/) is part of that effort, as is the [build process proposal](https://github.com/CivicActions/FOIA-RPMS).
But progress on this front depends on IHS leadership.

## What you can do

If you are a **tribal health IT team**: you do not need to wait for any of this to come together.
The docker-vista container, the BMW extraction process, and the IRIS compatibility workarounds are documented and available now.
If you are deploying RPMS, your experience — what worked, what broke, what was confusing — is the most valuable contribution the community needs.

If you are a **systems integrator or consultant** working with IHS or tribal health: the documentation on this site is open source.
If you find errors, gaps, or better approaches, contribute them.
File an issue or submit a pull request.

If you are a **security researcher**: RPMS components are available via FOIA.
The attack surface is non-trivial — MUMPS globals, RPC brokers, .NET middleware, Delphi thick clients.
Responsible disclosure through HHS VDP is the established channel.
Coordinated community notification is the gap.

If you are at **IHS**: we know you are maintaining this system with limited resources for a critical mission.
This is not adversarial.
Everything documented here — the extraction methodology, the compatibility workarounds, the security disclosures — is offered in the spirit of helping RPMS serve patients better.
The single most impactful thing IHS could do is publish RPMS artifacts on GitHub.
The community would do the rest.

If you are at **InterSystems**: the `%qarTune` auto-stats bug in IRIS 2026.1 affects 22,023 BMW classes.
The workaround (disabling AdaptiveMode) degrades SQL performance for the entire namespace.
A fix would benefit every RPMS deployment on current IRIS.

## The shape of the thing

What we are describing is not a new OSEHRA.
OSEHRA was a top-down creation of the VA — well-funded, institutionally backed, formally governed.
That model is not available to us.

What is available is something more like the way most open-source infrastructure actually gets built: a loose federation of organizations that each maintain their piece, share documentation, coordinate on compatibility, and keep the lights on because their users need the system to work.

CivicActions maintains the build pipeline.
Lakeraven operates cloud infrastructure.
Civic Studio documents the domain and integration layer.
WorldVistA holds the institutional continuity.
Tribal health IT teams run the system in production and report what they find.

That is not a governance structure.
It is a contributor graph.
It may be enough.

The work is: make each piece visible, make it easy to contribute, and keep showing up.

---

*Ryan Wold is the founder of [Civic Studio](https://civic.studio) and a contributor to the OpenRPMS project.
Reach out at wold@afomi.com if you want to contribute.*
