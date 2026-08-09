# 🎭 SNS Auto-Host — Shift and Seek Mod for Among Us

> The only fully automated Shift and Seek lobby host for Among Us. Drop it in, forget it, and let it run.

![Among Us](https://img.shields.io/badge/Among%20Us-v17.4i-red?style=flat-square)
![BepInEx](https://img.shields.io/badge/BepInEx-6.0.0--be-blueviolet?style=flat-square)
![Reactor](https://img.shields.io/badge/Reactor-2.5.1-blue?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-itch.io%20%7C%20PC-green?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

---

## What is Shift and Seek?

Shift and Seek (SNS) is one of the most popular custom game modes in Among Us. The rules are simple but intense:

- 🔀 **Impostors must shapeshift** into their target before killing them
- 📡 **Communications sabotage only** — no reactor, O2, lights, or doors
- 🚫 **No body reports** — bodies stay on the ground
- 🚫 **No emergency meetings** — the button does nothing
- 🚪 **No venting** — impostors are locked out of vents
- ✅ **Crewmates win by finishing all tasks**
- ☠️ **Miss your kill?** The mod kills YOU instantly

Running SNS manually is a nightmare. This mod automates everything.

---

## Features

### 🤖 Full Auto-Host
- Locks all game settings to SNS configuration the moment the lobby opens
- Automatically restarts the lobby 8 seconds after each game ends
- No host interaction needed — fully hands-free

### ⚙️ Enforced Game Settings
| Setting | Value |
|---------|-------|
| Emergency Meetings | 0 |
| Discussion Time | 15s |
| Voting Time | 15s |
| Kill Cooldown | 15s |
| Shapeshift Duration | 30s |
| Shapeshift Cooldown | 15s |
| Shapeshift Evidence | ON |
| Tasks | 1 Common / 1 Long / 1 Short |
| Engineer Vent Duration | 15s |
| Engineer Vent Cooldown | 5s |

### 🎭 Role Configuration
| Role | Status |
|------|--------|
| Engineer | ✅ 100% chance |
| Shapeshifter | ✅ 100% chance |
| Scientist | ❌ Disabled |
| Noisemaker | ❌ Disabled |
| Tracker | ❌ Disabled |
| Guardian Angel | ❌ Disabled |
| Detective | ❌ Disabled |
| Phantom | ❌ Disabled |
| Viper | ❌ Disabled |

### 🛡️ Rule Enforcement (automatic)
- **No venting** — vent button blocked for impostors at every level
- **No body reports** — report RPC killed entirely
- **No emergency meetings** — button does nothing
- **Comms-only sabotage** — lights, doors, reactor, O2 all blocked
- **Miss kill detection** — impostor shapeshifts but doesn't kill in time → instant death
- **Revert without kill** — impostor reverts form without killing → instant death

### 🚨 Community Moderation
- Auto-checks player names on join (racist/NSFW/slur names = instant kick)
- Chat monitoring with a **warn system**: first offense = warning, second = kick
- Host chat commands: `/kick`, `/ban`, `/warn`, `/warns`, `/rules`
- Session-based ban list (persists across lobby restarts, resets on mod restart)

### 💬 Player Experience
- Welcome message + full rules sent to every player who joins
- Game start reminder broadcast to all players
- Kill miss announcements in lobby chat
- Works with **vanilla Among Us clients** — players don't need the mod installed

---

## Requirements

- Among Us **v17.4i (build 7044)** — itch.io version
- [BepInEx 6.0.0-be (x86)](https://builds.bepinex.dev/projects/bepinex_be) — must be x86, not x64
- [Reactor 2.5.1](https://github.com/NuclearPowered/Reactor/releases)
- .NET SDK 6+ (to build from source)

---

## Installation

### 1. Install BepInEx (x86)
Download the `BepInEx-Unity.IL2CPP-win-x86` build and extract it into your Among Us folder. Launch the game once to initialize, then close it.

### 2. Install Reactor
Drop `Reactor.dll` into `Among Us/BepInEx/plugins/`.

Then open `BepInEx/config/gg.reactor.api.cfg` and set:
```
Modded handshake = false
```
This is required for itch.io — without it Reactor crashes looking for Steam.

### 3. Install SNS Mod
Drop `SNSMod.dll` into `Among Us/BepInEx/plugins/`.

Launch Among Us. Check `BepInEx/LogOutput.log` for:
```
[Info :SNS Auto-Host] SNS Auto-Host Mod loaded! Version 1.0.0
```

---

## Building from Source

```bash
# Clone the repo
git clone https://github.com/randomgyattappers/sns-discord-bot.git
cd sns-bot/reactor-mod/SNSMod

# Set your Among Us path (PowerShell)
[System.Environment]::SetEnvironmentVariable("AmongUs", "C:\path\to\Among Us", "User")

# Build
dotnet build

# Output: bin/Debug/net6.0/SNSMod.dll
```

---

## Host Commands

Type these in the Among Us lobby chat as host:

| Command | Effect |
|---------|--------|
| `/kick <name>` | Kicks a player from the lobby |
| `/ban <name>` | Bans a player for this session |
| `/warn <name>` | Issues a warning to a player |
| `/warns <name>` | Checks a player's warning count |
| `/rules` | Prints SNS rules in chat |

---

## How Miss Kill Works

if the impostor KILLS someone they didnt ss into. THEY DIE.

The kill window timer resets cleanly between games.

---

## File Structure

```
SNSMod/
├── Plugin.cs                          # Entry point
├── SessionBanList.cs                  # Session ban/kick list
├── RuleChecker.cs                     # Name/chat content scanner
├── WarnSystem.cs                      # Warn tracking system
└── Patches/
    ├── RoleAndSettingsManager.cs      # Forces SNS settings + roles
    ├── VentPatches.cs                 # Blocks impostor venting
    ├── MeetingPatches.cs              # Blocks reports + meetings
    ├── SabotagePatches.cs             # Comms-only sabotage
    ├── MissKillPatches.cs             # Miss kill detection + execution
    ├── PlayerJoinPatches.cs           # Name check + chat moderation
    ├── WelcomeMessagePatches.cs       # Welcome + rules messages
    └── AutoRestartPatches.cs          # Auto lobby restart
```

---

## FAQ

**Do players need to install this mod?**
No. Only the host needs it. Vanilla Among Us clients join and play normally.

**Does this work on Steam?**
This build targets the itch.io version. Steam support may work but is untested.

**Can I change the word filter?**
Yes — open `RuleChecker.cs` and edit the word lists at the top of the file.

**Can I change the miss kill timer?**
Yes — open `MissKillPatches.cs` and change `KillWindowSeconds`.

---

## License

MIT — do whatever you want with it, just don't claim you made it from scratch.

---

*Built with [Reactor](https://github.com/NuclearPowered/Reactor) — the Among Us modding framework.*
