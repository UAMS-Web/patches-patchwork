# uams-web/patches-patchwork

Patches that UAMS-Web repositories apply to [`antecedent/patchwork`](https://github.com/antecedent/patchwork) with [`cweagans/composer-patches`](https://github.com/cweagans/composer-patches) 2.x. One package replaces the copies each repository carried, so a patch is changed once and retires everywhere in one release.

This package pins `antecedent/patchwork` to exactly `2.2.4`. Every patch is written against that release.

## Install

An application requires it in `require` (the plugin applies a dependency's patches only from `packages` in `composer.lock`):

```bash
composer require --no-plugins uams-web/patches-patchwork:^1.0
```

A package that another repository installs requires it with `--dev` and lists the patch files in its own root `patches.json`. The full procedure, including `composer patches-relock` and `composer patches-repatch`, is the `upstream-patch-lifecycle` skill's *Moving a consumer to the package*.

Patch paths are relative to the consumer's root and start `vendor/uams-web/patches-patchwork/patches/`, so a consumer with a non-default `vendor-dir` is not covered.

## Patches

Applied in this order, which is the order in `composer.json`. A later patch is cut against the output of the earlier ones, so none is reordered without re-cutting those after it.

- **`patchwork-directory-wrapper.patch`**: Retain delegated directory wrappers (temporary; see UAMS-Web/UAMSWP-Logo-List#82)

Each description names the ticket that tracks the upstream fix, where there is one.

## Versioning

| Change | Release |
| --- | --- |
| A patch's content changes, same upstream pin | patch |
| A patch is added or retired, or the pin moves within the upstream major | minor |
| The pin moves to a new upstream major | major |

## Verifying the set

Apply the 1 patch in order to the `2.2.4` tag of `antecedent/patchwork` with `git apply -p1`. Applying the first patch a second time must fail.

## License

MIT, see [`LICENSE`](LICENSE). The patches modify `antecedent/patchwork`, which is also MIT licensed.
