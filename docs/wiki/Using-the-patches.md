Written against v1.0.0.

This page takes a consumer repository from nothing to a Composer install that applies the patches in this package. At the end, `vendor/antecedent/patchwork` carries every patch listed on [Patches](Patches.md).

The package is read from GitHub with a `vcs` repository, not from Packagist, and the install commands are in the [README](https://github.com/UAMS-Web/patches-patchwork#install) rather than copied here. The steps below say where each one goes.

1. **List the repository in the root `composer.json`.** Composer reads `repositories` only from the root project, so a package that another repository installs still needs its entry in the root of the project being installed.
2. **Require the package.** An application requires it in `require`, because the patch plugin applies a dependency's patches only from `packages` in `composer.lock`. A package that another repository installs requires it with `--dev` and lists the patch files in its own root `patches.json`.
3. **Install.** The package pins `antecedent/patchwork` to exactly `2.2.4`, so the install resolves to that release.
4. **Check that the patches applied.** Each patch is a file under `vendor/uams-web/patches-patchwork/patches/`. The patched lines are named on [Patches](Patches.md), so reading them in `vendor/antecedent/patchwork/src/CodeManipulation/Stream.php` confirms the result.

**Note:** patch paths start `vendor/uams-web/patches-patchwork/patches/`, so a consumer with a non-default `vendor-dir` is not covered.

The full procedure for moving an existing consumer onto the package, including `composer patches-relock` and `composer patches-repatch`, is the `upstream-patch-lifecycle` skill's *Moving a consumer to the package*, delivered to each consumer repository.

Reference: [Patches](Patches.md), [Versioning](Versioning.md). If an install fails, see [Troubleshooting](Troubleshooting.md).
