# Sotto releases

Signed builds of Sotto, a macOS dictation app (hold a hotkey, speak, release; speech-to-text and
cleanup run on the Mac). The source is private; this repo holds only the DMGs (under Releases) and
`appcast.xml`, the feed installed copies check once a day.

## Install (macOS 15 or later, Apple silicon)

1. From Releases, download `Sotto-<version>.dmg`.
2. Open it, drag Sotto onto Applications, eject.
3. Open Sotto. It is self-signed, not notarized, so macOS blocks the first launch: dismiss the
   warning, then System Settings → Privacy & Security → **Open Anyway**.
4. Allow Microphone, then switch Sotto on under Accessibility. The 2.5 GB cleanup model downloads
   in the background.

**Without admin rights**, install into your own Applications folder instead:

```sh
xattr -d com.apple.quarantine ~/Downloads/Sotto-<version>.dmg
hdiutil attach ~/Downloads/Sotto-<version>.dmg
mkdir -p ~/Applications && cp -R /Volumes/Sotto/Sotto.app ~/Applications/
hdiutil detach /Volumes/Sotto
open ~/Applications/Sotto.app
```

## Updates

Sotto checks `appcast.xml` daily and offers new versions; **Check for Updates…** in its menu checks
now. Each DMG carries an EdDSA signature that the installed app verifies before installing. The
check is a plain request for `appcast.xml`: no query parameters, no system profile, standard headers
and a User-Agent of `Sotto/<version> Sparkle/<version>`.

`Sotto-Dev.cer` is the public half of the code-signing certificate, for provenance only.
