# Sotto releases

Signed builds of Sotto, a macOS dictation app (hold a hotkey, speak, release; speech-to-text and
cleanup run on the Mac). This repo holds only the DMGs (under Releases) and `appcast.xml`, the
feed installed copies check once a day.

## Source

Sotto's engine is open source: https://github.com/sidbhargava1/sotto-engine (Apache-2.0). It is
the whole dictation pipeline as a Swift package: microphone capture, on-device speech-to-text
(Parakeet through FluidAudio, or Apple's SpeechAnalyzer), local cleanup with Qwen3-4B through
llama.cpp, and text injection at the caret. Build it with Xcode 26 on macOS 15 or later:
`git clone` it and run `swift build` and `swift test`, or add it to your own package with
`.package(url: "https://github.com/sidbhargava1/sotto-engine", .upToNextMinor(from: "0.1.0"))`. The app around
it (menu bar, onboarding, history and Insights) stays private.

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
