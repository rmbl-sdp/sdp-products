# RMBL SDP — Data Products

This repository serves two purposes for the **RMBL Spatial Data Platform**:

1. **Issue tracking** for SDP **datasets** — incorrect values, missing
   dates, metadata errors, acquisition gaps, documentation problems.
   File via the structured **Dataset issue** form.
2. **Source code** for derived SDP data products (planned). As products
   that have a reproducible processing pipeline are migrated here, their
   code will live under `products/<CatalogID>/`.

> Bugs and feature requests for the `rSDP` / `pySDP` client packages or
> the SDP Browser viewer belong in their respective repositories. This
> repo is for the **data**, not the clients.

## Filing a dataset issue

Click **New issue → Dataset issue** and fill out the structured form.
The most important field is the **CatalogID** (the 6-character SDP
code like `R3D009` or `BM012`). A validation bot will comment if the
ID isn't recognized.

You can find a dataset's CatalogID by:

- Browsing [sdpbrowser.org](https://sdpbrowser.org/)
- Calling `rSDP::sdp_get_catalog()` or `rSDP::sdp_browse()` from R
- Calling `pysdp.get_catalog()` from Python
- Inspecting the STAC catalog at
  <https://radiantearth.github.io/stac-browser/#/external/rmbl-sdp.s3.us-east-2.amazonaws.com/stac/v1/catalog.json>

You can also file an issue directly from R or Python for the dataset
you have open:

```r
rSDP::sdp_report_issue("R3D009")          # opens the prefilled form
```

## What happens after you file

1. **Auto-labels** are applied within a minute — `product:<CATID>`,
   `type:<...>`, `severity:<...>`, and `status:triage`.
2. A maintainer reviews and either confirms it
   (`status:investigating`), asks for more information
   (`status:needs-info`), declines to fix it (`status:wontfix`), or
   immediately resolves it.
3. The STAC catalog's collection-level `rmbl:open_issues_count` updates
   within ~30 seconds of any label or state change, so other users see
   the issue count on the affected dataset.
4. When the issue is fixed, the closing comment links the commit (or
   data update) that resolved it.

## Label scheme

| Group | Labels |
|---|---|
| Status | `status:triage`, `status:investigating`, `status:fixed`, `status:wontfix`, `status:needs-info` |
| Type | `type:incorrect-values`, `type:missing-dates`, `type:metadata-error`, `type:acquisition-gap`, `type:documentation`, `type:other` |
| Severity | `severity:critical`, `severity:high`, `severity:medium`, `severity:low` |
| Product | `product:<CATID>` — created on demand by the validation workflow |

## How known issues surface back to users

- `rSDP::sdp_known_issues("R3D009")` returns a data frame of open
  issues for a CatalogID (cached for an hour).
- `rSDP::sdp_get_catalog(with_issue_counts = TRUE)` adds an
  `OpenIssues` column to catalog results.
- `rSDP::sdp_browse(with_issue_counts = TRUE)` shows a `⚠ N issues`
  badge on affected cards.
- The static STAC catalog carries `rmbl:open_issues_count` and a
  `report-issue` rel link on each collection.

## Product source code (planned)

Where it makes sense to publish the processing pipeline for a derived
SDP product, code will land under `products/<CatalogID>/`. The repo's
issue tracker will then double as the bug tracker for that product's
code — issues filed against a product whose source lives here can be
fixed in the same place.

## Reporting issues without a GitHub account

Not yet — currently filing requires a GitHub account. A no-auth intake
form (`rmbl.org/sdp/report-issue/`) is on the roadmap.

## License

Issue metadata and any source code in this repo are published under
CC-BY-4.0 / MIT respectively (see `LICENSE` once code lands).
