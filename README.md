<!--
  Copy this file into the NEW public repo Zuse-Technologies/winget-pkgs as its
  README.md. It documents the universal package-metadata repo.
-->
# Zuse-Technologies packages (WinGet)

Public package-distribution metadata for Zuse Technologies products. This repo
holds **manifests only** — the actual installers are served from the public host
`https://pkgs.truzt.lk/release/...` (S3). It's the Windows/WinGet counterpart to
the Homebrew tap (`Zuse-Technologies/homebrew-tap`).

It is **multi-product**: every product is a `Publisher.Package` under the standard
WinGet layout, so new products just add their own folder.

## Layout

```
manifests/<lowercased-first-letter>/<Publisher>/<Package>/<Version>/
  <Publisher>.<Package>.yaml               # version manifest
  <Publisher>.<Package>.installer.yaml      # InstallerUrl -> pkgs.truzt.lk, sha256
  <Publisher>.<Package>.locale.en-US.yaml   # metadata
```

Example (Truzt):
```
manifests/z/ZuseTechnologies/Truzt/2.8.2/ZuseTechnologies.Truzt.*.yaml
```

## Install

**Local manifest (no extra setup):**
```powershell
git clone https://github.com/Zuse-Technologies/winget-pkgs
winget install --manifest winget-pkgs\manifests\z\ZuseTechnologies\Truzt\<VERSION>
```

**`winget install <Id>` via a custom source:** stand up a WinGet REST source backed
by these manifests (see <https://andrewstaylor.com/2023/11/09/creating-a-custom-winget-repo/>),
then:
```powershell
winget source add --name zuse --arg <rest-source-url> --type Microsoft.Rest
winget install ZuseTechnologies.Truzt
```

## How it's updated

Each product's release pipeline generates and pushes its manifest here
automatically (Truzt: `scripts/gen-winget-manifest.sh` run from the release
workflow). Do not hand-edit generated manifests.

## Notes

- Installers are **unsigned** → Windows SmartScreen shows "More info → Run anyway".
- Binaries/versions live on S3 (`pkgs.truzt.lk`), not in this repo.
