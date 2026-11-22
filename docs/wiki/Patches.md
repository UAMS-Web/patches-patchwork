Written against v1.0.0.

The patches are applied in the order they appear in `composer.json`, which is the order below. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it. Each patch is written against `antecedent/patchwork` `2.2.4`.

There are 2 patches.

## `patchwork-directory-wrapper.patch`

Retains delegated directory wrappers. It is temporary: the ticket that tracks removing it is `UAMS-Web/UAMSWP-Logo-List#82`.

| Item | Value |
| --- | --- |
| File | `patches/patchwork-directory-wrapper.patch` |
| Patched file | `src/CodeManipulation/Stream.php` in `antecedent/patchwork` |
| Applies with | `git apply -p1` |

It makes two changes to `Stream.php`:

- **A resource check in the return path.** The condition `$shouldReturnResource` becomes `$shouldReturnResource && is_object($resource)`, so the resource is returned only when it is an object.
- **A delegated `opendir`.** `dir_opendir` passes one extra argument, `true`, to `static::alternate('opendir', ...)`, so the directory wrapper it opens is retained by the stream.

## `patchwork-internal-callback-line-numbers.patch`

Keeps the line breaks Patchwork removes from calls to callback-taking internals. It is temporary: the upstream report is `antecedent/patchwork#230`, and the patch retires once a tagged Patchwork release carries the fix.

| Item | Value |
| --- | --- |
| File | `patches/patchwork-internal-callback-line-numbers.patch` |
| Patched file | `src/CodeManipulation/Actions/RedefinitionOfInternals.php` in `antecedent/patchwork` |
| Applies with | `git apply -p1`, after `patchwork-directory-wrapper.patch` |
| Retires when | a tagged `antecedent/patchwork` release carries the fix for `antecedent/patchwork#230` |

When any internal is listed in `redefinable-internals`, Patchwork also reroutes calls to the callback-taking internals (`array_filter`, `array_map`, `usort` and the rest of its default list). The rewrite of such a call removed everything between the function name and the first argument, so a call whose first argument starts on a new line lost that line break, and every later line of the loaded file reported a line number one lower. PCOV then credited executed lines to the line above, and a mutation run reported mutants uncovered that tests cover.

It changes `spliceNamedCallsWithin()` to replace the name token and the left parenthesis separately, so the whitespace and comments between them and the first argument stay. A redefined internal is still called for a call split over lines and for a call with no arguments.

## How the set is verified

Apply the patches in order to the `2.2.4` tag of `antecedent/patchwork` with `git apply -p1`. Applying either patch a second time must fail.

Related: [Versioning](Versioning.md), [Adding or retiring a patch](Adding-or-retiring-a-patch.md).
