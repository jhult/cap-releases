# cap-releases

Unofficial, unsigned source builds of [Cap](https://github.com/CapSoftware/Cap), built daily by GitHub Actions. Downloads go to the GitHub Releases of this repo.

Per Cap's [commercial license page](https://cap.so/docs/commercial-license), the usage restriction applies to binaries distributed by Cap itself, not to builds made from source. This repo builds the app from each `cap-vX.Y.Z` release tag of upstream and publishes the resulting binaries for free commercial use.

## Binaries produced

For every upstream release tag, a release tagged `cap-vX.Y.Z` is published here containing:

| Platform | Artifacts |
|---|---|
| macOS (Apple Silicon) | `.dmg` — **unsigned** |
| Windows (x64) | NSIS `.exe` installer — **unsigned** |
| Linux (x86_64) | `.deb` package |
| All of the above | `cap` CLI binary (best effort) |

## How it works

`.github/workflows/release.yml` runs on a schedule and can be triggered manually:

1. checks CapSoftware/Cap's latest release tag
2. skips if that same tag was already built here
3. otherwise checks out the exact source tag and builds the desktop app and CLI on macOS, Windows, and Linux
4. publishes a `cap-vX.Y.Z` release containing all artifacts

## Install & run

- **macOS**: download the `.dmg`, drag `Cap.app` into Applications. First launch shows a Gatekeeper warning because the build is unsigned — right-click the app and choose **Open**, or run `xattr -dr com.apple.quarantine /Applications/Cap.app`.
- **Windows**: run the `.exe` installer. SmartScreen may warn because the build is unsigned — choose **More info** → **Run anyway**.
- **Linux**: `sudo apt install ./Cap_*.deb`.

## Triggering manually

```
gh workflow run release.yml --repo jhult/cap-releases
```

Build a specific upstream ref instead of the latest release:

```
gh workflow run release.yml --repo jhult/cap-releases -f upstream_tag=cap-v0.5.7
```

## License

The build pipeline in this repository is MIT licensed (see [LICENSE](LICENSE)). The binary artifacts it publishes are compiled from upstream source and are governed by their own licenses: Cap's [terms](https://cap.so/docs/commercial-license) as applied to non-distributed builds, FFmpeg under LGPLv2.1+/GPLv2+, and each dependency under its respective license. Verify upstream license files in the source bundle if you require redistribution terms.

## Notes

- These builds are unsigned; expect OS warnings.
- The auto-updater is disabled in these builds; re-download new releases to update.
- The app connects to Cap's hosted backend (`cap.so`) for features outside of local recording.