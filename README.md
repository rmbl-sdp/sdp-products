# RMBL SDP — Dataset Issue Tracker

This repository is **not for code bugs**. It exists solely to track
issues with the **data products** in the RMBL Spatial Data Platform
(SDP) — incorrect values, missing dates, metadata errors, acquisition
gaps, documentation problems, and so on. Code issues with the `rSDP` /
`pySDP` packages or the SDP Browser belong in their respective
repositories.

## Filing an issue

Click **New issue → Dataset issue**, then fill out the structured form.
The most important field is the **CatalogID** (the 6-character SDP code
like `R3D009` or `BM012`). A validation bot will comment if the ID
isn't recognized.

You can find a dataset's CatalogID by:

- Browsing [sdpbrowser.org](https://sdpbrowser.org/)
- Calling `rSDP::sdp_get_catalog()` or `rSDP::sdp_browse()` from R
- Calling `pysdp.get_catalog()` from Python
- Inspecting the STAC catalog at
  <https://radiantearth.github.io/stac-browser/#/external/rmbl-sdp.s3.us-east-2.amazonaws.com/stac/v1/catalog.json>

You can also file an issue directly for the dataset you have open in
rSDP with:

```r
rSDP::sdp_report_issue("R3D009")          # opens the prefilled form
```

## What happens after you file

1. **Auto-labels** are applied within a minute — `product:<CATID>`,
   `type:<...>`, `severity:<...>`, and `status:triage`.
2. A maintainer reviews the issue and either confirms it
   (`status:investigating`), asks for more information
   (`status:needs-info`), declines to fix it (`status:wontfix`), or
   immediately resolves it.
3. The STAC catalog's collection-level
   `rmbl:open_issues_count` updates within ~30 seconds of any label
   or state change, so other users see the issue count on the affected
   dataset.
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
- `rSDP::sdp_browse()` shows a `⚠ N issues` badge on affected cards.
- The static STAC catalog carries an `rmbl:open_issues_count` field
  and a `report-issue` rel link on each collection.

## Reporting issues without a GitHub account

Not yet — currently filing requires a GitHub account. A no-auth intake
form (`rmbl.org/sdp/report-issue/`) is on the roadmap.

## License

Issue metadata is published under CC-BY-4.0 like the rest of the SDP.
