# Assessment: SampleApps to .NET 11 (STS)

Target: .NET 11 (STS), confirmed by the user (`target-framework`). Scope: repository `abpiskunov/SampleApps`, no exclusions. This replaces the earlier .NET 10 assessment, which is stale.

## Coverage

- Assessment record: `assessment_01M8G93M4NR2P6SG81SM1J074M` (scan task `tsk_01M8G92QFTW29VGY4G81F2YG38`, status completed, repository done).
- Scanned commit `19edb36107c1e9e4d55fc4f47e1164236698e594`, equal to the base HEAD.
- Covered: 1 of 1 projects. Reused: none. Excluded: none.
- Limitation: the bulk analyzer reports partial inventory/findings coverage for the project. This is its normal claim, not a gap.

## Resolved projects

- `AspNetCore/AspNetCore.csproj`, reason: picked. No package or project references, so no dependency in either direction.

## Project facts

- `Microsoft.NET.Sdk.Web`, `net10.0` today; no PackageReference or ProjectReference; no tests.
- Solution: `AspNetCore.sln`.
- No `global.json`, `Dockerfile` or `Directory.Build.*` is tracked.
- Publish profile `AspNetCore/Properties/PublishProfiles/SimpleApp1234567 - Web Deploy.pubxml` contains no `net10.0` folder name or framework value, so no publish path needs editing.
- 14 workflows in `.github/workflows` (not 15) set `DOTNET_CORE_VERSION: 10.0.x` and use `actions/setup-dotnet@v1`.
- SDKs installed: 8.0.428, 10.0.203, 10.0.300, 10.0.302, 10.0.404 and 11.0.100-rc.1.26425.128. Only a prerelease .NET 11 SDK is available locally.

## Findings

| Rule | Severity | Project | Summary |
|---|---|---|---|
| Project.0002 | required | AspNetCore | Change TargetFramework net10.0 to net11.0 |
| Api.0003 (x2) | potential, trivial | AspNetCore | API behavior to confirm at build or run time |

No package, security or blocking findings were reported.

## Blockers

None.

## Risks

- CI prerelease handling: `actions/setup-dotnet@v1` with `11.0.x` may not resolve while .NET 11 is prerelease. The executor should either set `include-prerelease: true` (and consider upgrading the action past v1) or use a version string that resolves, and record the choice.
- Local validation must use SDK 11.0.100-rc.1.
- The Startup/UseEndpoints host should run unchanged but needs a build and a smoke run.

## Decisions raised

- CI prerelease handling for `setup-dotnet` (see Risks). It does not need a user interview.

## Proposed wave outline

1. Single wave: retarget `AspNetCore.csproj` to `net11.0`, update all 14 workflows to `11.0.x` with prerelease handling, then check README/docs.
2. Final validation: `dotnet build AspNetCore.sln -c Release` on the .NET 11 SDK, run the app, expect HTTP 200 at `/`, and `git grep -nE 'net10\.0|10\.0\.x'` returns nothing outside `.github/upgrades`.
