# liberusoftware/.github

Organisation defaults and the reusable CI workflows every Liberu package repository calls.

## Reusable workflows

| Workflow | What it proves |
| --- | --- |
| `package-tests.yml` | the package's own suite passes, optionally against a coverage floor |
| `package-install.yml` | the package is installable and its Composer metadata is honest |
| `package-compatibility.yml` | the package works at both ends of the version range its constraints claim |

A package repository owns three thin callers. `CI.md` asks for repeated setup to live in
one place; `MODULES.md` rule 20 asks each repository to own its workflows. Both hold — a
callable Actions workflow is not a Composer package.

```yaml
# .github/workflows/tests.yml in a package repository
name: Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:

jobs:
  tests:
    uses: liberusoftware/.github/.github/workflows/package-tests.yml@main
    with:
      coverage-threshold: 0
```

Pin `@main` only while the fleet is migrating. Once each repository sets a coverage floor,
pin a tag so a workflow change cannot turn 44 repositories red at once.
