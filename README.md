# portkill

See what's listening on your ports, in a table, and kill it in one command.

```
$ portkill
PORT     PID      PROCESS                  USER
3000     52790    node                     malhar
5432     76954    postgres                 malhar
8765     55851    Python                   malhar

$ portkill -kill 3000
[INFO]  Killing PID 52790 (node) on port 3000
[OK]    Port 3000 freed.
```

## Install

Via Homebrew (personal tap):

```
brew tap justmalhar/tap
brew install portkill
```

Manual install (no Homebrew):

```
curl -o /usr/local/bin/portkill https://raw.githubusercontent.com/Justmalhar/portkill/main/portkill
chmod +x /usr/local/bin/portkill
```

## Usage

```
portkill                 # table of every listening TCP port
portkill 5432             # only show what's on port 5432
portkill -kill 3000       # kill whatever is on port 3000 (SIGTERM, escalates to SIGKILL if needed)
portkill -kill 3000 -f    # force-kill (SIGKILL) immediately
portkill -kill 3000 -n    # dry run — show what would be killed
```

Flags: `-k`/`-kill`/`--kill PORT`, `-f`/`--force`, `-n`/`--dry-run`, `-v`/`--verbose`, `-h`/`--help`.

## Requirements

macOS or Linux with `lsof` and `awk` (both ship by default on macOS).

## How it works

Reads `lsof -iTCP -sTCP:LISTEN` for the listing, and `lsof -ti tcp:<port>` to resolve PIDs for
killing. No daemons, no config file, no dependencies beyond what's already on your machine.
