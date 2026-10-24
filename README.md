# Pepper security advisories

The advisory database that `pepper package audit` reads, as RustSec's does for `cargo audit`. It holds one TOML file per advisory, under `advisories/`.

`pepper package audit` clones this repository into the Pepper home's download cache, and fetches it again once its copy is an hour old. It then checks every package a project's `pep.lock` records against it. To read a different database, such as a mirror or a private one, set `PEPPER_ADVISORY_DB` to its Git URL. See [`pep-package`'s documentation](https://github.com/pep-lang/pepper/blob/main/docs/src/tools/pep-package.md#audit).

## An advisory

Name the file after its id: `advisories/PEP-2026-0001.toml`.

```toml
id = "PEP-2026-0001"                      # required, PEP-<year>-<number>
package = "github:owner/name"             # required: the source pep.lock records
title = "One line saying what is wrong"
severity = "high"                          # low, medium, high or critical
url = "https://github.com/owner/name/security/advisories/..."
patched = [">=1.4.2"]                      # the versions that fix it
unaffected = ["<1.2.0"]                    # the versions it never affected
```

A version is affected unless a `patched` or `unaffected` entry holds it. Each entry is either a single version (`"1.2.3"`) or a range written the way dependencies write it (`">=1.2.3, <2"`, `"^1.4"`).

## Reporting an advisory

- **The package is your own:** fix it, release the fixed version, then open a pull request here with the advisory.
- **The package is someone else's:** report it to its maintainers privately first, for example through GitHub's private vulnerability reporting. Open the pull request once a fix has been released, or once the maintainers have had a reasonable time to respond.
- **Ids:** use the next free `PEP-<year>-<number>`.

A maintainer of this repository reviews each pull request before it merges. Once merged, every `pepper package audit` sees it within an hour.

A version that is broken rather than insecure needs no advisory. Its package can retract it with `[retract]` in its own `pep.toml` instead.

## License

The advisories are dedicated to the public domain under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/), so any tool may use them.
