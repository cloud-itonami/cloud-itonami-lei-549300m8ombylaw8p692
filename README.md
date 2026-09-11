# cloud-itonami-lei-549300m8ombylaw8p692

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Trenitalia S.p.A.**

This repository archives the publicly published Condizioni Generali di Trasporto
(Conditions of Transport) of **Trenitalia S.p.A.**, with source-url and retrieval-date
provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Trenitalia S.p.A.
- **LEI (ISO 17442)**: [549300M8OMBYLAW8P692](https://search.gleif.org/#/record/549300M8OMBYLAW8P692) (GLEIF-verified)
- **Jurisdiction**: IT
- **Website**: https://www.trenitalia.com
- **Ticker**: none (wholly owned subsidiary of Ferrovie dello Stato Italiane, not separately listed)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Conditions-of-Transport
  documents, each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `facts.edn` — 9 verified registry facts with per-fact provenance. **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO URLs were fetched and nine facts recorded — the LEI record
(legal name as GLEIF spells it, **`TRENITALIA S.P.A.`**, in upper case and tagged
`en` even though the name is Italian; entity **ACTIVE**, registration **ISSUED**,
`FULLY_CORROBORATED` / `CONFORMING`; entity status and registration status are
different fields and are recorded separately; legal and headquarters address both
`PIAZZA DELLA CROCE ROSSA 1, 00161, ROME, IT-RM, IT` as GLEIF spells them — the
same address GLEIF gives for the parent; entity creation date and initial LEI
registration date both `2013-09-28T03:03:00Z`, which is what GLEIF reports and was
not investigated; last updated `2026-04-22`, next renewal `2027-05-22`; S&P Global
id `5466838`; OpenCorporates id recorded as `nil` because the cited record carries
none), its ISIN mapping (**0** instrument identifiers — a count read from
`meta.pagination.total` of the cited page; a measured zero — GLEIF maps no
instrument identifier to this LEI, and whether any instrument exists outside
GLEIF's ISIN mapping was not investigated),
its managing LOU and LEI-issuer accreditation (London Stock Exchange LEI Limited,
GB, accredited 2017-11-06), registration authority `RA000407` (Infocamere, the
Italian Business Register — `registroimprese.it` — where the entity is entry
`05403151003`, its *codice fiscale* / VAT number), ISO 20275 legal form `P418`
(`Società Per Azioni`), and **both consolidation levels**: the direct parent and the
ultimate parent are the same entity, `FERROVIE DELLO STATO ITALIANE S.P.A.`
(LEI `549300J4SXC5ALCJM731`, IT, ACTIVE), each recorded as its own entity with
the relationship kind GLEIF reports (`IS_DIRECTLY_CONSOLIDATED_BY` /
`IS_ULTIMATELY_CONSOLIDATED_BY`). GLEIF stops at FS Italiane; that FS Italiane
is itself owned by the Italian Ministry of Economy and Finance is not something
this registry records, and it is not recorded here either. Nine of the eleven
URLs answered `200` when the file was written; the `direct-parent-reporting-
exception` and `ultimate-parent-reporting-exception` endpoints answered `404`
because GLEIF publishes the *parent* side of that pair for this entity — the
inverse of a widely-held listed group — which the checker treats as a fact rather
than a failure.

GLEIF records **0 direct children** for this LEI (`meta.pagination.total` of the
cited `direct-children` page). That is a measured zero about what GLEIF's
relationship data holds, not a claim that Trenitalia has no subsidiaries: a
company appears as a child only if it holds an LEI and reports the relationship.
Cross-checked out of band when this landed: a GLEIF search on the legal name
`trenitalia` returns exactly one record, this one — so no other Trenitalia-named
entity exists in the registry to be a child. That search is not cited in
`facts.edn` because the generator does not record it.

The cited LEI record carries no `otherNames`, no `otherAddresses`, no successor
entities, no event groups, no BIC, no `qcc` and no OpenCorporates id for this
entity, so nothing the record says is left out of `facts.edn`.

The check has three exit codes, not two: `0` when every cited URL answered and
every recorded value still matches, `1` when a citation broke or a value drifted
(each difference is named, with the recorded and live values side by side), and
`3` when the check could not be performed at all — `facts.edn` missing or empty,
or GLEIF unreachable at the transport level — because a check that could not run
must not look like a check that ran and found nothing. Before this landed, all
three were shown against the live API: unmodified → `0`; `:company/jurisdiction`
edited from `IT` to `FR` → `1`, naming `gleif-lei-record :company/jurisdiction`;
the `gleif-direct-parent` entity deleted → `1`, naming it as `ADDED`; `fetch`
made to fail with `ENOTFOUND` → `3`; `facts.edn` absent → `3`.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
