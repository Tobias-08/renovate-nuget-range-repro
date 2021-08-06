# renovate-nuget-range-repro

- minimal repro for current incomplete nuget range/floating support in [renovate](https://docs.renovatebot.com/)
- nuget background
  - nuget supports a "prefer most stable version of a given major.minor.patch (including prerelease)" behavior ([nuget spec](https://github.com/NuGet/Home/wiki/Support-pre-release-packages-with-floating-versions#common-scenarios))
  - example:
    - given a PackageReference with `Version="3.4.142-*"`
    - results in `3.4.142` being preferred if this stable version exists
    - results in (e. g.) `3.4.142-alpha` being preferred if no stable and instead this prerelease version exists
- renovate
  - current behavior
    - `-*`-PackageReferences are replaced with a concrete version, e. g. `3.4.142-alpha`
  - expected behavior
    - renovate leaves the `-*` untouched and just cares about the major.minor.patch-part, at least if this is configured (via `"rangeStrategy": "replace"` or another appropriate config option)
    - example:
      - given a PackageReference with `Version="3.4.142-*"`
      - renovate creates a PR with `Version="3.4.231-*"`
