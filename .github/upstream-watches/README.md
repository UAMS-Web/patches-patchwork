<!--
  Synced from UAMS-Web/uams-claude-skills: shared/.claude/skills/patch-package/templates/upstream-watches-README.md
  Edit it there, not here; the next sync overwrites this copy.
  Delivered to this repository through the profile(s): patches.
-->
# Upstream watches

<!-- cspell:ignore Packagist -->

One file per upstream issue or pull request this repository is waiting on. The `watch-upstream` workflow (`.github/workflows/watch-upstream.yml`) reads every `*.json` file here once a day and comments on the row's tracking issue when something moves. This README is also what keeps the directory tracked while it holds no rows, which is a valid state: the workflow then has nothing to do.

## Row format

Name the file `<tracker>-<owner>-<repo>-<number>.json`, for example `412-laravel-framework-58123.json` for tracker `412` watching `laravel/framework#58123` (`/` in the repository becomes `-`). The loader rejects a file whose name does not match its contents, so the name is always unique and says what it watches.

```json
{ "tracker": 412, "repo": "laravel/framework", "kind": "pr", "number": 58123, "composer": "laravel/framework", "released": true }
```

| Key | Required | Meaning |
| --- | --- | --- |
| `tracker` | yes | Issue number in this repository that receives the comments. Closing it turns the watch off. |
| `repo` | yes | The upstream repository, `owner/name`. |
| `kind` | yes | `issue` or `pr`. |
| `number` | yes | The upstream issue or pull request number. |
| `composer` | no | A `vendor/package` name. Adds a signal for a Packagist release newer than the version in `composer.lock`. |
| `released` | no | `true` on a `pr` row only. Adds a signal for the first release on the pull request's base-branch line that contains the merge commit. |

Unknown keys, a missing key and a wrong type fail the run with the file named, so a malformed row is never silently skipped.

## Adding and retiring a row

- **Add** a row in the same pull request that adds the local patch it exists for, so the patch and its removal trigger land together.
- **Retire** a row in the pull request that drops the patch: delete the file and close the tracking issue. Closing the tracker alone stops the comments; deleting the file stops the workflow reading it.
- Two rows under one tracker share an off switch, so give threads separate trackers whenever either can outlive the other.

## Notes on current rows

Three rows, each under the removal ticket that waits on the thread:

| Row | Tracker | Watches | Why it earns its place |
| --- | --- | --- | --- |
| `1-antecedent-patchwork-234.json` | `1` | `antecedent/patchwork#234` (issue) | The report that the stream wrapper's directory open drops the delegated wrapper; the removal ticket for `patches/patchwork-directory-wrapper.patch` is blocked by it. |
| `1-antecedent-patchwork-235.json` | `1` | `antecedent/patchwork#235` (pull request, `released: true`) | The upstream fix for #234. Merging is not releasing, so `released: true` also reports the first release on its base-branch line that contains the merge. |
| `16-antecedent-patchwork-230.json` | `16` | `antecedent/patchwork#230` (issue) | The report that Patchwork shortens a file when it reroutes a callback-taking internal; the removal ticket for `patches/patchwork-internal-callback-line-numbers.patch` is blocked by it. |

No row carries `composer`: the pin in `composer.lock` is already behind a published release, so the Packagist signal would report at once and say nothing about the fix. The upstream pull request for `antecedent/patchwork#230` is `antecedent/patchwork#233`; it has no row because tracker `16` already receives that thread's reports and a second row under one tracker shares its off switch.
