---
---

# Changelogs

In the frontend, we keep a changelog in every project.

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

We write a changelog for every version, based on [Keep a Changelog]. Read what
is written there, it is short and to the point. Described here is only where we
deviate from it or complement it.

### Deviations, refinements, and additions

- Make the text understandable for the target audience. Mention only what is
  relevant for them. Remember who the reader is:
  - In apps and in the normal changelog of the launcher: Users. They see the
    changelog in the launcher.
  - In `Changelog-dev.md` of the launcher and shared: Developers.
  - Users are especially not aware of technical internals. E.g. they do not know
    what it means that a library is upgraded. Instead describe what (and if) the
    library upgrade leads to observable changes for them (e.g. instead of
    “Upgraded nrfjprog to 10.11.1” write “Added support for nRF53 devices”).
- Unlike on [Keep a Changelog]: Do not add a preamble (“All notable changes to
  this project …”) in user visible changelogs. It looks strange when viewed in
  the launcher. We may filter it out later, though, then we can add it.
- Unlike on [Keep a Changelog]: Do not add links to the headers (like
  [https://github.com/NordicSemiconductor/pc-nrfconnect-dtm/compare/v1.0.0...v1.1.0](https://github.com/NordicSemiconductor/pc-nrfconnect-dtm/compare/v1.0.0...v1.1.0)).
  We assume they are confusing for our users, when displayed in the launcher.
- End every entry with a full stop.
- Do not duplicate the section headings “Fixed”, “Added” and alike in the
  entries. So, do not write like this :

```md
### Added
- Added button to convert trace.
- Added choice for the trace format.

### Fixed
- Fixed crash on stopping trace.
- Fixed wrong file size.
```

Instead drop the repeated `Added` and `Fixed` and write like this:

```md
### Added
- Button to convert trace.
- Choice for the trace format.

### Fixed
- Crash on stopping trace.
- Wrong file size.
```

- When fixing bugs, it is usually more natural to describe the bug, sometimes
  the fix. Compare these and think what is easier to understand as a user:
  - “The app crashed when entering special symbols like “\*” in the search
    field.”
  - ”Protected app from special symbols like “\*” in the search field.”
- In shared we can have an additional section “Steps to upgrade”, so we
  developers remember what needs to be done when we upgrade another project to
  depend on this new version.

### Notable changes from before

- No “Version” in the heading anymore. “Version 0.9.1” → “0.9.1”. The context
  makes it clear, that this is a version number.
- Add a release date to every version.
- Standardised sections headings (e.g. “Updates” → “Updated”, “Bugfixes” or
  “Fixes” → “Fixed”, no more “Features”).

### When doing a release

- If the changelog is not up to date, then first amend it with missing entries.
- Add a heading with the new version number and release date below “Unreleased”.
- Use the recent changelog entry as GitHub release note.

[Keep a Changelog]: https://keepachangelog.com/en/1.0.0/
