# NamiTalk Releases

This public repository hosts official release artifacts for the NamiTalk Windows desktop app.
Application source code and internal release operations are maintained separately.

## Supported platform

- Windows x64
- Per-machine NSIS installer

macOS, Linux, and Windows ARM builds are not currently published here.

## Download and install

1. Open the [Releases](https://github.com/TryHand-Co-Ltd/namitalk-releases/releases)
   page.
2. Select the latest stable tag matching `namitalk-app-v*`.
3. Download only `NamiTalk-<version>-windows-x64-setup.exe`.
4. Confirm the browser download URL belongs to
   `github.com/TryHand-Co-Ltd/namitalk-releases`.
5. Run the installer and follow the on-screen steps. Administrator approval may be required because
   the app is installed per machine.

Do not download NamiTalk installers from mirrors, file-sharing sites, chat attachments, or
unofficial links.

## Release assets

Each complete stable release contains:

| Asset                                               | Purpose                                                |
| --------------------------------------------------- | ------------------------------------------------------ |
| `NamiTalk-<version>-windows-x64-setup.exe`          | Windows installer                                      |
| `NamiTalk-<version>-windows-x64-setup.exe.blockmap` | Differential-update metadata used by the app           |
| `latest.yml`                                        | Version and integrity metadata used by the app updater |

Most users only need the `.exe`. Do not edit or manually install the blockmap or `latest.yml`.

Releases published by the current automation point to a release-specific bot-authored commit
containing `release-manifest.json`. The manifest records the public tag, version, generation time,
and the name, size, and SHA-256 digest of every required artifact. It contains no application
source code, private source revision, or release credential.

Historical releases published before this automation was introduced do not have a manifest
commit. Their GitHub-hosted asset digest remains the available integrity reference; their existing
tags and artifacts are intentionally left unchanged.

## Verify a download

GitHub records a SHA-256 digest for uploaded release assets. To calculate the digest of a downloaded
installer in PowerShell:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath '.\NamiTalk-<version>-windows-x64-setup.exe'
```

For a release created by the current automation, compare the result with both the digest in the
tagged `release-manifest.json` and the digest reported by GitHub for the corresponding release
asset. For a historical release without a manifest, compare it with the GitHub asset digest. Do
not run the file when the available values differ or when the download came from another domain.

You can also inspect its Windows signature status:

```powershell
Get-AuthenticodeSignature -LiteralPath '.\NamiTalk-<version>-windows-x64-setup.exe' |
  Select-Object Status, StatusMessage, SignerCertificate
```

Current releases may be unsigned and can therefore show an `Unknown Publisher`, SmartScreen, or
Defender warning. This repository and the GitHub release asset digest are the authoritative download
source until trusted code signing is introduced. An invalid or unexpected signature is not normal;
do not continue installing it.

## Updates

Packaged NamiTalk builds use this repository as their update feed. The app checks for updates after
launch and periodically while running. It does not require users to download `latest.yml` manually.

When an update is offered:

1. review the target version and release notes;
2. allow the app to download the update;
3. save or finish active work; and
4. approve restart and installation when prompted.

If an update fails, the currently installed app should remain available. Restart the app, confirm
internet access to GitHub, and try again later. You can always download the latest stable installer
from the Releases page.

## Uninstall

Open Windows **Settings → Apps → Installed apps**, locate **NamiTalk**, and choose **Uninstall**.
Follow the Windows prompts. Contact support before manually deleting application data when you need
to preserve local session history or diagnose an issue.

## Troubleshooting

### Windows blocks or warns about the installer

Verify the repository URL and SHA-256 digest first. Current unsigned builds can display a publisher
warning. If organizational policy blocks unsigned applications, contact your administrator or the
usual NamiTalk support channel rather than bypassing that policy.

### The installer asks for administrator approval

NamiTalk currently uses a per-machine installer, so Windows may require an administrator account.

### The app cannot find an update

Confirm GitHub is reachable, restart the app, and check the Releases page for the latest stable tag.
Draft and prerelease builds are not normal stable update targets.

## Support and security

For product issues, installation help, or feedback, contact **TRYHAND Co., Ltd** through your usual
support channel.

Never include passwords, access tokens, private keys, OAuth credentials, or other secrets in a
public issue, screenshot, or log attachment. Redact personal data and workspace content before
sharing diagnostics.
