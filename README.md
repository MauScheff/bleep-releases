# Bleep

Connect agents to people on Bleep. The CLI and MCP provide communication; your agent supplies reasoning.

## Install on macOS

Apple Silicon, macOS 13 or later. Initial preview release:

```sh
brew install MauScheff/bleep/bleep
bleep --help
```

Update:

```sh
brew update
brew upgrade MauScheff/bleep/bleep
```

[Download the signed and notarized archive](https://github.com/MauScheff/bleep-releases/releases/tag/v0.1.0-r2) or use [the Homebrew tap](https://github.com/MauScheff/homebrew-bleep). The package includes CLI, MCP, native adapters, voice host and media helper. No Rust, Node, Python or Codex runtime is bundled or required for the Bleep communication tools. Codex remains a separately installed optional consumer.

## Connect an agent

The Phone starts a Conversation. The CLI exposes enrollment/status, deliberate listen/speak and MCP configuration generation. Use `bleep --help` for current connection arguments. Voice uses your OpenAI transcription and ElevenLabs speech credentials; keep them in private environment files or the process environment, outside the package. Existing paired installations can be reused.

MCP and host configurations use the stable absolute command `$(brew --prefix)/bin/bleep`. An older `~/.local/bin/bleep` can shadow the short command; check `command -v bleep`. Close an active host before upgrading and restart it after the update. Pairing, credentials and configuration survive package upgrade and uninstall.

This first release supports one host owner per installation and Phone-initiated Conversations. Guided setup and direct attachment to an existing Codex desktop work chat remain future work.

## Distribution

First-party binaries are free to use, including commercial use, and may be redistributed unchanged under the attached Bleep Binary License. Copyright Maurice Scheffmacher. Application, SDK, backend and adapter source remain private; installation scripts and metadata accompany the binaries. Third-party terms are unchanged.

This software uses FFmpeg under [LGPL 2.1 or later](https://ffmpeg.org/legal.html). [Exact corresponding FFmpeg source](https://github.com/MauScheff/bleep-releases/releases/download/v0.1.0-r2/ffmpeg-8.0.3.tar.xz), build instructions and notices are attached to the same release and included in the archive. Opus source and license are also included. FFmpeg is a separate executable and is not linked into the proprietary Bleep tools.

All six executables are Developer ID signed and Apple notarized. Standalone binaries use online Gatekeeper ticket lookup; offline stapling is not claimed. Download SHA256 and licensing files accompany the release.
