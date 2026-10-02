# claude-switch

A minimal bash script to switch between multiple Claude Code accounts.

No dependencies beyond `bash` and `python3`.

## Install

**Option A — system-wide:**
```bash
sudo cp claude-switch /usr/local/bin/claude-switch
sudo chmod 755 /usr/local/bin/claude-switch
```

**Option B — user bin:**
```bash
mkdir -p ~/bin
cp claude-switch ~/bin/claude-switch
chmod 755 ~/bin/claude-switch
# Add ~/bin to PATH if not already there:
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## Setup

Do this once per account. Profile names are arbitrary — use whatever you like.

```bash
claude-switch save account_name
```

ex)

```bash
# Log in to account A inside Claude Code (/login), then:
claude-switch save pro

# Log in to account B inside Claude Code (/login), then:
claude-switch save teams
```

## Usage

```bash
claude-switch account_name          # switch to saved account
claude-switch status                # show all profiles and usage %
claude-switch delete account_name   # remove a saved profile
claude-switch -h                    # show help
```

**`status` output:**
```
active profile : pro
config account : you@gmail.com
  * pro        cred:o  5h: 15%  7d: 24%
  - teams      cred:o  5h:  0%  7d: 10%
```

`5h` = 5-hour rolling window usage, `7d` = 7-day usage. `*` marks the active profile.

## How it works

Claude Code stores account identity in two files:

| File | Contents |
|------|----------|
| `~/.claude.json` | Account info (email, org UUID) |
| `~/.claude/.credentials.json` | OAuth token |

Everything else in `~/.claude.json` (projects, trusted folders, allowed tools, MCP settings) and all of `~/.claude/` (sessions, settings, skills) is shared across accounts.

`claude-switch` keeps each profile's `oauthAccount` and credentials, and on switch replaces only those, so you continue in the exact same environment with a different account. Before each switch, the live account is written back to the profile that owns it (matched by account UUID), so refreshed tokens are never lost. If the live account isn't saved in any profile, the switch is refused until you `save` it.

> **Note:** Claude Code must be fully quit before switching profiles.
