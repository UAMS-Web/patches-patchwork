Written against v1.0.0.

- [A patch does not apply](#a-patch-does-not-apply)
- [The patches are not picked up from the package](#the-patches-are-not-picked-up-from-the-package)
- [A non-default vendor directory](#a-non-default-vendor-directory)

## A patch does not apply

**Symptom:** the install reports that a patch could not be applied to `antecedent/patchwork`.

**Cause:** every patch is cut against exactly `antecedent/patchwork` `2.2.4`. A different release of that package changes the lines the patch expects.

**Fix:** pin the consumer to the package's own pin by letting this package's `require` resolve it, and do not override it in the consumer. To test a patch by hand, apply it to the `2.2.4` tag of `antecedent/patchwork` with `git apply -p1`.

## The patches are not picked up from the package

**Symptom:** the install succeeds and `vendor/antecedent/patchwork` carries no patch from this package.

**Cause:** the patch plugin applies a dependency's patches only from `packages` in `composer.lock`. A package listed only in `require-dev` of a project that installs without dev packages, or one the lock file does not record, is not read.

**Fix:** require the package the way [Using the patches](Using-the-patches.md) describes, then re-lock.

## A non-default vendor directory

**Symptom:** a patch path is reported missing.

**Cause:** patch paths start `vendor/uams-web/patches-patchwork/patches/`, which assumes the default `vendor-dir`.

**Fix:** none in this package. A consumer with another `vendor-dir` is not covered.
