---
layout: post
title: "What We Lost When OSEHRA Closed"
date: 2026-05-28
author: "Ryan Wold"
excerpt: "In February 2020, the Open Source Electronic Health Record Alliance shut its doors. Six years later, the gap it left is wider than ever — and the people who need open-source health IT the most are the ones paying the price."
---

On February 14, 2020, Dr. Seong K. Mun announced that OSEHRA — the Open Source Electronic Health Record Alliance — would cease operations at noon that day.
A nonprofit founded by the VA in 2011 to steward open-source VistA, gone in a sentence.

Six years later, the consequences are clear.
Not in the VA system, where billions have been spent on a commercial replacement.
In the places OSEHRA was never primarily about: the tribal health facilities, small hospitals, and community clinics that depended on the open-source ecosystem it anchored.

## What OSEHRA was

OSEHRA was the institutional home for open-source VistA.
The VA created it in 2011 under Secretary Eric Shinseki.
Its mission was to build a vendor-neutral community around the VA's electronic health record system — VistA — which the VA had developed over decades and released as open-source software.

At its peak, OSEHRA had over 2,700 registered members across 160+ organizations.
It was ANSI-accredited.
It maintained the canonical open-source VistA distribution.
It hosted an annual summit, published a technical journal, and provided the governance infrastructure that let government agencies, private companies, and independent developers collaborate on the same codebase.

For RPMS — the Indian Health Service's fork of VistA — OSEHRA was also the closest thing to an institutional ally.
IHS joined as a community member and contributed RPMS artifacts to the OSEHRA Technical Journal.
The VistA community's tooling, documentation patterns, and testing infrastructure all benefited RPMS by extension.

## What killed it

In 2017, the VA signed a $10 billion no-bid contract with Cerner Corporation to replace VistA entirely.
The contract later expanded to $16 billion.
Oracle acquired Cerner in 2022 for nearly $30 billion.

The logic was straightforward: VistA was old, maintaining it was expensive, and the VA wanted a commercial off-the-shelf EHR that would interoperate with the Department of Defense's MHS GENESIS system.

The effect on OSEHRA was immediate and fatal.
The VA — OSEHRA's creator and anchor institution — was now committed to replacing the software OSEHRA existed to steward.
Without the VA's institutional backing, OSEHRA could not sustain itself.

By the 2019 OSEHRA Open Source Summit, the community was already in triage mode — debating whether to pivot, contract, or simply document what existed before the lights went out.
Five months later, the lights went out.

## What happened to the assets

When OSEHRA closed, its assets were distributed:

- **VistA repositories** went to WorldVistA, a 501(c)(3) nonprofit that continues to maintain them on GitHub.
- **Non-VistA repositories** (EHMP, VA Knowledge Artifacts, Synthea-based projects) went to Logica Health.
- **1.5 terabytes of archival content** — Tech Journal issues, event recordings, VM images, Docker snapshots — went to Logica's AWS S3 Glacier storage.

WorldVistA continues active work.
They held a virtual conference in February 2026.
They maintain the VistA and RPMS FOIA repositories.
They are doing essential, largely volunteer-driven work.

But WorldVistA is not OSEHRA.
OSEHRA had institutional standing, ANSI accreditation, and a governance model that gave the ecosystem legitimacy with government procurement officers and health IT decision-makers.
WorldVistA has heart and code.
What it lacks is the institutional weight to shift how agencies distribute and maintain their software.

## What the VA got instead

The Oracle Health EHR modernization program — the initiative that made OSEHRA expendable — has deployed to 6 of 171 VA medical centers.
The lifecycle cost has grown to approximately $37 billion.
According to GAO, 58% of users reported it increased safety risks.
Fewer than 1 in 5 clinicians said it enabled high-quality care.
Patient notes disappeared.
Prescription dosages were incorrect.
A former VA EHR modernization director was charged with accepting contractor cash and gifts.

The VA paused most deployments in 2023 due to patient safety concerns.
Rollouts resumed tentatively in 2026, starting with four sites in Michigan.

Meanwhile, VistA continues to run at the other 165 VA medical centers.
It runs because it works.

## What RPMS lost

For RPMS, the loss is specific and compounding.

RPMS was never the center of OSEHRA's mission — that was VistA.
But OSEHRA provided an ecosystem: community norms around open-source health IT, a shared understanding that government-funded software should be inspectable and modifiable, and a venue where IHS technical staff could engage with the broader VistA community.

Without that ecosystem:

**IHS stopped releasing complete database files.**
In March 2026, IHS declined WorldVistA's request for updated CACHE.DAT files, citing proprietary content.
The last complete FOIA release with a loadable database was March 2021.
New deployments now require either applying five years of patches manually or using the community-maintained CivicActions docker-vista build pipeline.

**BMW remains a binary-only distribution.**
The SQL mapping layer that BPRM and other .NET applications depend on ships as a platform-specific IRIS.DAT file — Windows and AIX only.
No Linux build. No source. No schema export.
When a tribal health organization needs to run on Linux in the cloud, they have to reverse-engineer the binary.
We know this because we did it.

**Version coupling has no escape hatch.**
BMW 2025.3 requires IRIS 2022.1.3.
IHS has not released BMW for any IRIS version since.
The gap between IHS's release cadence and InterSystems' runtime versions grows wider each year, and without source access, organizations cannot bridge it themselves.

**Security issues accumulate in the dark.**
We found a hardcoded AES encryption key in BPRM that affects every installation worldwide.
It was sitting in a publicly downloadable DLL.
We reported it through HHS Vulnerability Disclosure.
With open source, this would have been found and fixed years ago.
Without it, the key is still there.

## The pattern

This is not a story about one nonprofit shutting down.
It is a story about what happens when the institutional home for open-source government software disappears, and no replacement emerges.

The software keeps running — VistA at 165 VA hospitals, RPMS at 400+ tribal and federal health facilities.
The code is still technically public domain.
But the infrastructure for maintaining, distributing, and improving it as a community erodes.
Distribution channels narrow.
Binary artifacts replace source distributions.
Version compatibility becomes someone else's problem — except there is no one else.

The people who feel this most acutely are not the VA, which has $37 billion allocated for its commercial replacement.
They are the tribal health organizations operating on thin IT budgets, serving populations with some of the worst health outcomes in the country, trying to keep a system running that the federal government built for them and then walked away from maintaining in the open.

## What this means now

OSEHRA is not coming back.
The VA is not going to re-create it.
The question is whether the people and organizations still working with VistA and RPMS can build something that fills the gap — not a replica of OSEHRA, but something suited to the actual situation in 2026.

The situation is: RPMS is actively maintained by IHS.
New patches ship regularly.
BPRM got a major release in November 2025.
The software is alive.
What is missing is the open-source infrastructure around it — the version control, the cross-platform builds, the community governance, the shared understanding that this software belongs to the public and should be maintainable by the public.

That is the work ahead.

---

*Ryan Wold is the founder of [Civic Studio](https://civic.studio) and a contributor to the OpenRPMS project.
This post reflects independent analysis, not the position of any government agency.*
