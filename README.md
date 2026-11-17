# MCFL Rake Analytics

A single-file browser app for the month-wise freight registers of rakes loaded from Mahanadi Coalfields (MCFL): customer rakes, POL / UL, POL & UL PMT, RSR movement, coalfield and commercial analytics.

**Open the app:** https://eternalethereal43.github.io/mcfl-rake-analytics/ (Chrome or Edge)

The online app reads every register in this repository's [`data/`](data) folder on its own. Nothing needs to be selected.

> **Note:** the registers in `data/` are public. Anyone can download them, including consignee GSTINs, invoice and e-payment IDs and freight amounts. Files removed later still remain in the git history.

## Adding a new month

1. Copy the new register (for example `MCFL_NOV26_assembled.xlsx`) into the `data` folder of this repository on your PC.
2. In GitHub Desktop, enter a summary such as "Add Nov 2026", click **Commit to main**, then **Push origin**.
3. Reload the app (or click **Rescan**). The new file is picked up straight away.

The app lists `data/` through GitHub's file listing, so no other file needs editing. `data/manifest.json` is only a fallback for when that listing is unavailable (GitHub allows 60 listings an hour per network). Add new file names to it if you want the fallback to stay complete.

## Using local files instead

The app can also read a folder on your own computer: **Change folder** (or **Choose files**) loads registers from your PC without uploading them. You can download `index.html` and open it offline too. Its **Load data from GitHub** button fetches the online registers.

## Expected input

FOIS freight-register exports (`.xlsx`), one row per RR, with a header row containing at least:
`STTN FROM`, `STTN TO`, `RR NO.`, `RR DATE`, `CNSG CODE`, `WGHT CHRG`, `WGHT ACTL`, `WGHT POL1`, `WGHT POL2`.
Freight columns used when present: `RATE(Q)`, `BSC FRGT`, `POL`, `TOTL FRGT`, `GST`, `DMRG LDNG`, `DMRG ULDG`, `TOTAL REBATE`, `TOTAL OTHR CHRG (EXCL GST)`, `GRAND TOTAL INCL DMRG`.
Rows repeated across files (same station, RR no., FNR and date) are counted once.

## Definitions

| Term | Meaning |
|---|---|
| Rake | An RR with CC wt (`WGHT CHRG`) of 2,000 MT or more. Smaller RRs are kept for commercial accounting only. The limit can be changed in settings. |
| POL wt | `WGHT POL1` + `WGHT POL2` |
| UL | CC wt − RR wt (`WGHT ACTL`) − POL wt, floored at 0 (the floor can be switched off) |
| POL PMT | POL charges (₹) ÷ RR wt: ₹ per MT of coal loaded |
| UL PMT | UL × freight rate `RATE(Q)` (₹) ÷ RR wt: freight paid on unused capacity, per MT loaded |
| RSR | Rail–sea–rail movement: rakes booked to PPAP, PEQP or CBSP (editable in settings) |
| Customer | Consignee code (`CNSG CODE`), optionally split by destination |

Group figures are tonnage-weighted (total ₹ ÷ total MT), not averages of per-rake ratios.

## Coalfield mapping

Siding codes are grouped into **Talcher**, **IB Valley** and **Sardega** using published siding lists. Three codes not found in those lists were placed by matching freight rates to shared destinations: MCGB → Sardega, MLGB → IB Valley, LMGT → Talcher. RRs showing `MCFL` as the station are left out of rake analytics but kept in commercial totals. Any new code triggers a banner and can be assigned under **Data & settings**. Changes are saved in the browser.

## Tabs

Overview · Rake register · Customer rakes · POL & UL · POL & UL PMT · RSR movement · IB Valley / Talcher / Sardega · Trend · Commercial · Data & settings. Every table exports to CSV.

## Third-party code

`index.html` embeds [SheetJS Community Edition](https://sheetjs.com) (Apache License 2.0) to read Excel files.
