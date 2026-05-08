---
---

# Changelog guidelines

We keep a changelog in every project.

## Example for a changelog

```md
## Unreleased

### Removed

- Preliminary instructions.

## 0.6.1 - 2021-06-30

### Fixed

- Crash on macOS when stopping trace.

## 0.6.0 - 2021-06-22

### Added

- Button in the side panel to convert trace.

### Changed

- PCAP-NG files have the file extension `.pcapng`.
```

## The rules

We write a changelog for every version, based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/). Read what
is written there, it is short and to the point. Described here is only where we
deviate from it or complement it.

### Deviations, refinements, and additions

The following custom rules apply:

- Make the text understandable for the target audience. Mention only what is
  relevant for them. Remember who the reader is:

  - In apps and in the normal changelog of the launcher, write for _users_. They see the
    changelog in the launcher. Users are not aware and do not care about technical internals.
    For example, they do not know what it means that a library is upgraded.
    Instead, describe what (and if) the library upgrade leads to observable changes for them.
    For example, instead of “Upgraded nrfjprog to 10.11.1”, write “Added support for nRF53 devices”.
  - In `Changelog.md` of shared and `Changelog.minor.md` of the launcher, write for _developers_.

- Unlike on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/): Do not add a preamble (“All notable changes to
  this project …”) in user-visible changelogs. It looks strange when viewed in
  the launcher.
- Unlike on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/): Do not add links to the headers (like
  [https://github.com/nordicsemi/pc-nrfconnect-dtm/compare/v1.0.0...v1.1.0](https://github.com/nordicsemi/pc-nrfconnect-dtm/compare/v1.0.0...v1.1.0)).
  They are confusing for our users when displayed in the launcher.
- End every entry with a full stop.
- Do not duplicate the section headings “Fixed”, “Added” and alike in the
  entries. So, do not write like this :

```md
### Added

- Added button to convert trace.
- Added choice for the trace format.

### Fixed

- Fixed an issue with crashing on stopping trace.
- Fixed an issue with wrong file size.
```

Instead, drop the repeated `Added` and `Fixed` and write like this:

```md
### Added

- Button to convert trace.
- Choice for the trace format.

### Fixed

- Issue with crashing on stopping trace.
- Issue with wrong file size.
```

- When fixing bugs, it is more natural to describe the bug, sometimes
  the fix. Compare the following:
  - “[Fixed an] Issue with the app crashing when entering special symbols like “\*” in the search
    field.”
  - ”The app is now protected from special symbols like “\*” in the search field.”
- In shared, we can have an additional section “Steps to upgrade”, so we
  developers remember what needs to be done when we upgrade another project to
  depend on this new version.

### Notable changes from before

- No “Version” in the heading anymore. “Version 0.9.1” → “0.9.1”. The context
  makes it clear, that this is a version number.
- Add a release date to every version.
- Standardised sections headings (e.g. “Updates” → “Updated”, “Bugfixes” or
  “Fixes” → “Fixed”, no more “Features”).

### When doing a release

- If the changelog is not up-to-date, first amend it with missing entries.
- Add a heading with the new version number and release date below “Unreleased”.
- Use the recent changelog entry as GitHub release note.
