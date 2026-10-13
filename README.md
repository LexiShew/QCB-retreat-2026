# Department Retreat site (GitHub Pages + Jekyll)

All talk/poster content is generated from `_data/abstracts.csv`, and the schedule from
`_data/schedule.csv`. To update the site, replace a CSV and push. You never edit the HTML for content.

## Setup (~15 min)

1. Create a new GitHub repo and upload everything in this folder, including the folders that start with `_`.
2. Replace `_data/abstracts.csv` with your real file (see "CSV format" below).
3. Edit `_config.yml`: title, dates, location, contact email, and **`baseurl`**.
   - Project site (`https://USERNAME.github.io/REPO/`): `baseurl: "/REPO"`
   - User site (`https://USERNAME.github.io/`): `baseurl: ""`
4. On GitHub, go to **Settings → Pages → Build and deployment**. Choose *Deploy from a branch*, then pick `main` and `/ (root)`.
5. Wait 1–2 minutes. Build errors appear under the repo's **Actions** tab.

## CSV format

### `_data/abstracts.csv`

| column        | required | notes |
|---------------|----------|-------|
| `id`          | recommended | Short unique code such as `T1` or `P12`. Used for links and the schedule. If blank, a link is generated from the title. |
| `type`        | yes | `Talk`, `Poster`, or anything else. Order is set by `type_order` in `_config.yml`. |
| `title`       | yes | |
| `authors`     | yes | |
| `affiliation` | no  | |
| `abstract`    | yes | Line breaks inside a cell are preserved. |

**Different column names?** Don't rename your spreadsheet headers. Instead, change the mapping under
`columns:` in `_config.yml`, for example `title: "Presentation Title"`. Header names must match exactly,
including capitalization and spaces.

### `_data/schedule.csv`

Columns: `day, time, event, location, speaker, talk_id, kind`

- Set `talk_id` to an abstract's `id` (e.g. `T1`). The row then shows that talk's title and authors and links to its abstract. `event` and `speaker` can be left blank for those rows.
- Set `kind` to `break` for meals and coffee to shade the row.
- Days appear in the order they first show up in the file.

## Exporting from Excel (common gotchas)

- **Use Google Sheets' "File → Download → CSV"**, or Excel's plain *CSV (Comma delimited)*.
  Excel's *CSV UTF-8* option adds an invisible byte-order mark to the first header. This can make the
  first column (often `id`) silently disappear. If accents or special characters look garbled,
  open the file in Google Sheets and re-download it from there.
- Delete empty rows at the bottom of the sheet. Otherwise they show up as blank abstracts.
- Merged cells don't survive export. Unmerge them first.

## Features

- Searchable abstracts page. The search box filters both the table of contents and the abstracts.
- Table of contents grouped by type, with each abstract linked by anchor (e.g. `/abstracts/#abs-p12`, handy for QR codes on posters).
- Printing the abstracts page (Ctrl/Cmd+P) gives a clean abstract book without the navigation.
- Light and dark mode. Colors: cardinal `#990000` and gold `#FFCC00`. Gold is used only for accents and on cardinal backgrounds, because gold text on white is unreadable.

## Optional: preview locally

```bash
gem install jekyll
jekyll serve   # then open http://localhost:4000
```
