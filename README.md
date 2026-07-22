# PCS Scrubber

Process raw CSV contact files, strip prefixes, assign channels, deduplicate contact IDs, and output a clean 3-column CSV (`channel`, `connect_to_agent_date`, `contact_id`).

## Supported input formats

- **E2E** — has a `Contact ID` column (bare UUID) plus `Channel` and `Initiation timestamp`.
- **Pre-scrubbed** — already has `channel` + `contact_id` columns with clean UUIDs.
- **RVOC (prefixed)** — `contact_id` carries a channel prefix (`IN_CALL-`, `OUT_CALL-`, `CHAT-`).
- **RVOC (bare UUID)** — `contact_id` is a bare UUID and the channel comes from a
  separate `contact_channel` (or `channel`) column. Date is read from `contact_day`,
  `contact_day_pst`, `contact_create_time`, or `connect_to_agent_date`.

Rows whose channel is not VOICE or CHAT (e.g. EMAIL) are skipped and reported.

## Output defaults

When an input file is selected, the output directory defaults to the input file's
folder and the filename is pre-filled by taking the input name and replacing a
trailing `-Raw`/`-raw` with `-PCS` (e.g. `RVOC-520-Raw.csv` → `RVOC-520-PCS`). Both
the directory and filename remain editable.

## Build

The Windows `.exe` is built automatically via GitHub Actions on push to `main`. Download the artifact from the Actions tab.

To build manually on Windows:
```
pip install -r requirements.txt
pyinstaller --onefile --windowed --name "PCS Scrubber" --icon icon.ico --hidden-import tkinterdnd2 --hidden-import PIL --collect-all tkinterdnd2 --add-data "icon.png;." --add-data "icon.ico;." --version-file version_info.txt pcs_scrubber.py
```
