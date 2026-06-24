# claude-switch

A minimal bash script to switch between multiple Claude Code accounts.

No dependencies beyond `bash`, `python3`, and `curl`.

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
```

**`status` output:**
```
active profile : pro
config account : you@gmail.com
  - pro         config:o  cred:o  5h: 15%  7d: 24%
  - teams       config:o  cred:o  5h:  0%  7d: 10%
```

`5h` = 5-hour rolling window usage, `7d` = 7-day usage.

## How it works

Claude Code stores account identity in two files:

| File | Contents |
|------|----------|
| `~/.claude.json` | Account info (email, org UUID) |
| `~/.claude/.credentials.json` | OAuth token |

`claude-switch` keeps a copy of both files per profile and swaps them as a pair. Before each switch, it writes the current live state back so refreshed tokens are never lost.

> **Note:** Claude Code must be fully quit before switching profiles.
