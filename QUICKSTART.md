# Bleep quickstart

Initial preview for Apple Silicon, macOS 13 or later. Bleep provides communication; your agent provides reasoning. The teacher is a reference consumer, not a built-in assistant.

## Install and update

```sh
brew install bleep-talk/bleep/bleep
BLEEP="$(brew --prefix)/bin/bleep"
"$BLEEP" --help
```

Update with `brew update` and `brew upgrade bleep-talk/bleep/bleep`. Close the current host first, then restart it. Keep pairing, provider files and host configuration outside the Cellar. An older local installation can shadow `bleep`; use the stable absolute command above.

## Link an agent

Replace `@YOUR_HANDLE` with your Bleep handle. Use a distinct agent name and state directory for a new agent. Reuse the exact existing directory to retain an existing link.

```sh
STATE="$HOME/.local/state/bleep/my-agent"
mkdir -p "$(dirname "$STATE")"
"$BLEEP" enroll https://api.bleep.talk @YOUR_HANDLE my-agent "$STATE"
```

Approve the printed short code in the Phone's Agents screen. Then claim the approved installation:

```sh
"$BLEEP" continue https://api.bleep.talk @YOUR_HANDLE my-agent "$STATE"
"$BLEEP" status https://api.bleep.talk @YOUR_HANDLE my-agent "$STATE"
```

`pending` does not mean approved. `enrolled` confirms the local credential claim. `authenticated` confirms the status proof at that time; it does not grant voice permission. `cancel` cancels an unclaimed enrollment. Revoke an enrolled agent on the Phone. Never copy installation state between owners or API origins.

## Supply connection and provider configuration

This preview needs an explicit trusted public bridge configuration; guided relay discovery/setup is not shipped. Reuse the public configuration from an existing accepted installation or obtain it from the Bleep deployment operator. It contains the relay identifier, relay IP:port, TLS server name, issuer and public verification key-set object. It must contain no private keys or tokens, and no old test fields `replyPcm` or `replyDelayMs`. The public file is not a provider environment file.

Supply your OpenAI transcription and ElevenLabs speech credentials through separate owner-only private environment files, each at most 16 KiB. Each file contains its matching `OPENAI_API_KEY` or `ELEVENLABS_API_KEY` assignment. Keep the files private (`chmod 600`) and outside package directories. No credentials belong in command arguments, public configuration or logs. Use an ElevenLabs voice ID that your account can access.

## One CLI Conversation

Replace each absolute path and `YOUR_VOICE_ID`:

```sh
"$BLEEP" conversation https://api.bleep.talk @YOUR_HANDLE my-agent "$STATE" \
  /absolute/public-config.json bundled YOUR_VOICE_ID \
  --openai-env-file /absolute/openai.env \
  --elevenlabs-env-file /absolute/elevenlabs.env \
  --mp3-bitrate 128
```

Connect to the agent on the Phone. In the terminal, type `listen`, then send a short Phone message. After the transcript appears, type `speak Hello from Bleep.` to reply. Type `quit` for graceful shutdown. This terminal consumer performs no reasoning. Media transmission receipts do not prove that the person heard the audio.

## MCP configuration

Generate a credential-free Codex configuration fragment with the same connection arguments:

```sh
"$BLEEP" mcp-config codex https://api.bleep.talk @YOUR_HANDLE my-agent "$STATE" \
  /absolute/public-config.json bundled YOUR_VOICE_ID \
  --openai-env-file /absolute/openai.env \
  --elevenlabs-env-file /absolute/elevenlabs.env \
  --mp3-bitrate 128 > /absolute/bleep-mcp.toml
```

Add the generated fragment using your host's MCP settings, preserving unrelated entries. `mcp-config json` emits the equivalent generic launch description. Restart the MCP server after registration. The configured server exposes listen/speak/end/status tools over the same SDK operations. Only one CLI/MCP/background host can own an installation at a time. Discovery alone does not acquire custody. Bare `bleep mcp` exposes metadata but has no voice connection.

Deliberate MCP listen is supported. Automatic delivery into an existing Codex desktop work chat is not shipped. The native `host`, `voice-host`, `codex-adapter` and `codex-transport` commands are included for explicit host integration; these do not create an agent model or teacher.

## Reference teacher

The Pi teacher is a separately configured reference client that consumes the same communication interface. It owns its prompt, memory, model and cloud wake lifecycle. It is not included in this package. Custom agents can own those choices independently. Bleep's optional `voice-instructions` command supplies adapter-specific speech guidance; the caller decides how to use it.

## Distribution

First-party binaries are free to use, including commercial use, and can be redistributed unchanged under the release's Bleep Binary License. Application/SDK/backend/adapter source remains private. Third-party rights are unchanged. The signed/notarized download includes notices and exact corresponding FFmpeg/Opus sources.
