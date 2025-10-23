# Git Howl (Git How Late???)

Because sometimes we need to see just how bad our work-life balance is.

```text
Usage: git-howl [OPTIONS]

Find commits made at unsociable hours (because your git log shouldn't judge you).

OPTIONS:
    -s, --start HOUR        Start of unsociable hours (0-23, default: 0)
    -e, --end HOUR          End of unsociable hours (0-23, default: 6)
    -w, --weekends          Include weekends as unsociable
    -W, --weekends-all-day  Count ALL hours on weekends as unsociable
    -S, --stats             Show statistics summary
    -f, --full              Show full commit details (not just dates)
    -n, --no-color          Disable colored output
    -a, --author PATTERN    Filter by author (grep pattern)
    --since DATE            Only commits after date (e.g., "2 weeks ago")
    --until DATE            Only commits before date
    -h, --help              Show this help message
    -v, --version           Show version

EXAMPLES:
    git-howl                          # Default: 00:00-06:00 weekdays
    git-howl -s 22 -e 7               # 22:00-07:00 (late night to early morning)
    git-howl -w                       # Include weekends midnight-6am
    git-howl -W                       # All weekend hours
    git-howl -s 0 -e 9 -w -S          # 00:00-09:00 + weekends, with stats
    git-howl -a "John Doe" --since "1 month ago"
    git-howl -s 23 -e 2 -f            # Show full commits for 23:00-02:00
```

This started out as a cheap shell trick:

```bash
#!/bin/bash
git log --pretty=fuller | grep '^CommitDate:.*0[0123456]:.*:'
```

By the magic of AI (Claude Sonnet 4.5) it's now grown legs.

## Configurable Hours:

- **-s/--start HOUR and -e/--end HOUR** to set your unsociable hours range
- Handles wrap-around ranges (e.g., 22:00 to 02:00)

## Weekend Support:

- **-w/--weekends** includes weekend hours (using the same time range)
- **-W/--weekends-all-day** counts ALL hours on weekends as unsociable

## Better Output:

- **-f/--full** shows complete commit details instead of just one line
- Colorized output by default (disable with -n/--no-color)

## Filtering:

- **-a/--author** PATTERN to filter by author
- **--since and --until** for date ranges (e.g., "2 weeks ago", "2024-01-01")

## Statistics

Need some ammo for that awkward conversation with your manager? Get some `git
howl` stats in your daily reports!

- **-S/--stats** displays summary statistics with percentage and a cheeky
  judgment of your work-life balance

## Other Improvements:

- Proper argument parsing with getopts
- Validates hour inputs
- Correctly handles date parsing and weekend detection
- Exit status indicates whether unsociable commits were found
- Help text with examples

## Installation

Just copy or symlink `git-howl` to somewhere in `$PATH` (`~/bin` or
`~/.local/bin`) and make it executable (`chmod +x`). Deps are minimal:

- git (obvs.)
- date
- awk

Then run it as:

```bash
$ git howl
```

If you want `man git howl` or `git howl --help` to work then just:

```bash
$ mkdir -p ~/.local/share/man/man1
$ cp man/man1/git-howl.1 ~/.local/share/man/man1
```

## Examples

```bash
# Default behavior (00:00-06:00 on weekdays)
$ git-howl

# Late night coding (22:00-07:00)
$ git-howl -s 22 -e 7

# Include all weekend hours + stats
$ git-howl -W -S

# Check last month's late-night commits
$ git-howl -s 23 -e 6 --since "1 month ago" -S

# See who's burning the midnight oil
$ git-howl -s 0 -e 5 -f --since "1 week ago"
```

Output:

```bash
$ git howl -W -S                           
167848e Author:     Bryn M. Reeves <bmr@redhat.com> 2025-10-22 00:54:50 +0100
c0f15df Author:     Bryn M. Reeves <bmr@redhat.com> 2025-10-22 00:54:50 +0100
333a67e Author:     Bryn M. Reeves <bmr@redhat.com> 2025-10-22 00:54:50 +0100
da66e68 Author:     Bryn M. Reeves <bmr@redhat.com> 2025-09-24 04:21:45 +0100
...
78da2fc Author:     Bryn M. Reeves <bmr@redhat.com> 2024-06-08 14:20:01 +0100
6719a27 Author:     Bryn M. Reeves <bmr@redhat.com> 2024-06-08 14:20:01 +0100

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Statistics
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total commits:      740
Unsociable commits: 110
Percentage:         14.9%

🌙 Burning the midnight oil occasionally...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## I found a bug!

Great! Report it [here](https://github.com/bmr-cymru/git-howl/issues/).

## I did a thing!

Even better! Propose it [here](https://github.com/bmr-cymru/git-howl/pulls/new).

