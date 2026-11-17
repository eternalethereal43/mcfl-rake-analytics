# MCFL Rake Analytics

A single-file browser app for the month-wise freight registers of rakes loaded from Mahanadi Coalfields (MCFL). Point it at the folder of registers and it builds rake, POL / UL, PMT, RSR and commercial analytics.

**Open the app:** https://eternalethereal43.github.io/mcfl-rake-analytics/ (Chrome or Edge)

You can also download `index.html` and open it from your own computer. It works offline.

## Your data stays on your computer

The app has no server. Register files are read inside your browser and never uploaded, including when you open it from the GitHub Pages link. This repository holds only the app; `.gitignore` blocks `.xlsx`, `.xls` and `.csv` files so register data can't be committed by mistake.

## How to use

1. Open the app in Chrome or Edge.
2. Click **Select MCFL folder** and choose the folder that holds the monthly registers (for example `D:\Railways Freight Register\MCFL`). Sub-folders are read too.
3. Use the filter bar (period, coalfield, siding, customer, movement) to narrow every tab.
4. When a new month's register is added, click **Rescan**. Chrome and Edge remember the folder.

Other browsers can use **Choose files** to pick the registers each time.

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
