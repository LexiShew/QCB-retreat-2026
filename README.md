# QCB Retreat 2026

Website for the USC QCB department retreat (November 11, 2026, Los Angeles), built with Jekyll and served by GitHub Pages:

**https://lexishew.github.io/QCB-retreat-2026/**

The site has three pages: a home page, the day's schedule, and a searchable book of talk and poster abstracts. All of the content comes from CSV files in `_data/`. To update the site, edit or replace a CSV and push to `main`; GitHub rebuilds it in a minute or two. Build errors show up in the repo's **Actions** tab.

## Layout

```
_config.yml                 site title, dates, location, CSV column mapping, poster themes
_layouts/default.html       shared header, nav, banner, footer
index.html                  home page (welcome text is written inline here)
schedule.html               /schedule/: built from the two schedule CSVs
abstracts.html              /abstracts/: built from the two abstract CSVs
assets/css/style.css        all styling, including the print layout
assets/images/header.jpg    banner photo
assets/images/favicon.svg   tab icon
_data/
  talk_abstracts.csv        one row per talk
  poster_abstracts.csv      one row per poster
  abridged-schedule.csv     one-line-per-block overview
  detailed-schedule.csv     every talk with start/end time and speaker
```

## Updating the abstracts

`talk_abstracts.csv` and `poster_abstracts.csv` are exports of the submission form's response sheet, and keep its column headers. `_config.yml` maps those headers to fields under `talk_columns` and `poster_columns`:

| field           | default header                              | notes |
|-----------------|---------------------------------------------|-------|
| `title`         | `Talk/Poster Title`                         | Blank shows "Title to come". |
| `authors`       | `Author(s)`                                 | |
| `affiliation`   | `Please indicate your lab affiliation.`     | Optional. |
| `abstract`      | `Abstract`                                  | Line breaks inside a cell are kept. |
| `sort_name`     | `Last Name`                                 | Each list is sorted alphabetically by this column. Remove the line to keep the CSV's row order. |
| `section`       | `Section` (posters only)                    | Poster theme number. See below. |

If the form's questions are reworded, change the header text in `_config.yml` rather than renaming the CSV columns. Matching is exact, including capitalization, spaces and trailing punctuation.

The talk CSV must also keep its `First Name` and `Last Name` columns, because the schedule uses them to link speakers to their abstracts (see below).

### Poster themes

Posters are grouped by the number in their `Section` column, and each group is labelled from `poster_columns` in `_config.yml`:

```yaml
section_label: "Theme "
section_names:
  "1": "Population Genomics and Systems Biology"
  "2": "Structural Bioinformatics and Genome Organization"
  "3": "Genomic Algorithms and Computational Methods"
```

So a poster with `Section` = `2` is listed under "Theme 2: Structural Bioinformatics and Genome Organization". A blank or `0` puts it under "Unassigned". If the whole column is empty or missing, posters appear as a single list.

### Links to individual abstracts

Every abstract has a stable anchor built from its title, e.g. `/abstracts/#talk-single-molecule-ctcf-occupancy-...` or `/abstracts/#poster-glacier-a-multimodal-...`. Copy the link from the page's table of contents. If a title is edited, its link changes.

## Updating the schedule

**`abridged-schedule.csv`**: columns `Start, Activity, Type`.

**`detailed-schedule.csv`**: columns `Start, End, Activity, Speaker, Affiliation, Type`.

`Type` sets the row style:

| `Type`    | appearance |
|-----------|------------|
| `session` | Gold band. In the detailed table, it is a full-width heading with no time cell. |
| `break`   | Pale cardinal band (meals, coffee, check-in). |
| `talk`    | Plain row. |

In the detailed schedule, the speaker appears in bold after the activity, followed by their affiliation. If a `Speaker` value exactly matches `First Name` + space + `Last Name` in `talk_abstracts.csv`, the row links to that talk's abstract, and the abstract links back to the row. Names that differ in any way, such as a missing title or a middle initial, don't link. Use `Dr. …` only for speakers who don't have an abstract.

## Exporting CSVs without garbled characters

Characters like `µ`, `–` or accented names show up as `�` when a CSV isn't saved as UTF-8. This usually comes from Excel's default *CSV (Comma delimited)* format or from text pasted out of Word.

- Download from Google Sheets (**File → Download → Comma-separated values**). That export is always UTF-8.
- Avoid saving the CSV from Excel. If you must, use *CSV UTF-8*, then check that the first column still appears on the site. Excel adds a hidden marker to the first header that can break matching.
- To find bad bytes in a file, run:

  ```bash
  python3 -c "open('_data/poster_abstracts.csv', encoding='utf-8').read()"
  ```

  It prints nothing if the file is clean; otherwise it reports the byte position of the first bad character.
- Delete empty rows at the bottom of the sheet before exporting, or they show up as blank "Title to come" entries.

## Other edits

- **Title, date, location**: `title`, `dates`, `location` in `_config.yml`. These feed the header, home page and footer. Add `contact_email:` to show a mailto link in the footer.
- **Welcome text**: edit the paragraph at the bottom of `index.html`.
- **Banner photo**: replace `assets/images/header.jpg`, or change `header_image` in a page's front matter.
- **Colors**: CSS variables at the top of `assets/css/style.css` (cardinal `#990000`, gold `#ffcc00`). Gold is only used on cardinal or as an accent, because gold text on white is unreadable.
- **Repo name**: if the repo is renamed, update `baseurl` in `_config.yml` to match (`"/NEW-NAME"`), or every link and stylesheet will 404.

`columns`, `type_order` and `tagline` in `_config.yml` are left over from an earlier version and aren't used by any page.

## Printing an abstract book

Open the Talks & Posters page and print it (Ctrl/Cmd+P). The print stylesheet hides the navigation, search box and back-links, so it saves as a clean PDF.

## Previewing locally

```bash
gem install jekyll
jekyll serve        # http://localhost:4000/QCB-retreat-2026/
```

`_config.yml` is only read at startup, so restart `jekyll serve` after editing it.
