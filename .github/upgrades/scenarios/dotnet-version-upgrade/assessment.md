# Assessment: SampleApps to .NET 10 (LTS)

Target: .NET 10 (LTS), confirmed by the user (`target-framework`). Scope: repository `abpiskunov/SampleApps`, no exclusions.

## Coverage

- Assessment record: `assessment_01M8DTTKJCJQT5DYDM10XN6G21` (scan task `tsk_01M8DTSY171RZRZKXKF9M23J14`, status completed, repository done).
- Scanned commit `94cca7cb83e5f055b199c09f4739ddc405b07405`, equal to the current HEAD, so nothing is stale.
- Covered: 1 of 1 projects. Reused: none. Excluded: none.
- Limitation: the bulk analyzer reports partial inventory/findings coverage for the project. This is its normal claim, not a gap.

## Resolved projects

- `AspNetCore/AspNetCore.csproj`, reason: picked. It has no package or project references, so there is no dependency in either direction.

## Project facts

- `Microsoft.NET.Sdk.Web`, `net5.0`, `Exe`; no PackageReference or ProjectReference.
- No `global.json` and no test projects.
- SDKs on the machine: 8.0.428, 10.0.203, 10.0.300, 10.0.302, 10.0.404, 11.0.100-rc.1. A .NET 10 SDK is available.

## Findings

| Rule | Severity | Project | Summary |
|---|---|---|---|
| Project.0002 | required | AspNetCore | Change TargetFramework net5.0 to net10.0 |

No API, package, source-compatibility or security findings were reported.

## Blockers

None.

## Risks

- The Startup/UseEndpoints Razor Pages host is expected to run unchanged on .NET 10, but needs a build and a smoke run to confirm.
- 14 workflows in `.github/workflows` set `DOTNET_CORE_VERSION: 5.0.x` and need `10.0.x`.
- `README.md` should be checked for stated framework or SDK requirements.

## Decisions raised

None open.

## Proposed wave outline

1. Single wave: retarget `AspNetCore.csproj` to `net10.0`, update all 14 workflow files to `10.0.x`, update README/docs, then build and smoke-run (home page returns HTTP 200).
2. Final validation: build the repository with a .NET 10 SDK and run the app.
