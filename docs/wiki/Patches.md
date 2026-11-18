Written against v1.0.0.

The patches are applied in the order they appear in `composer.json`, which is the order below. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it. Each patch is written against `antecedent/patchwork` `2.2.4`.

There is 1 patch.

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

## How the set is verified

Apply the patches in order to the `2.2.4` tag of `antecedent/patchwork` with `git apply -p1`. Applying the first patch a second time must fail.

Related: [Versioning](Versioning.md), [Adding or retiring a patch](Adding-or-retiring-a-patch.md).
