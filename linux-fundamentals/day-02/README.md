linux-fundamentals/day-02/README.md
# Day 02 — Linux Filesystem & Paths

Today I continued practising Linux fundamentals in Kali Linux. My main focus was understanding the filesystem, navigating with paths, and using what I learned to build a small security workspace.

## What I Learned

### Linux Filesystem

I learned that `/` is the root of the Linux filesystem and that everything exists underneath it.

Some directories I explored:

- `/home` — user home directories
- `/etc` — system and application configuration
- `/var` — changing system data
- `/var/log` — system and application logs
- `/tmp` — temporary files
- `/root` — home directory of the root user

I also learned that `/` and `/root` are different. `/` is the top of the filesystem, while `/root` is the root user's home directory.

### Paths

I learned the difference between absolute and relative paths.

An absolute path starts from `/`:

```bash
cd /var/log
```

A relative path starts from my current location:

```bash
cd reports
```

I also practised:

```text
~   → current user's home directory
.   → current directory
..  → parent directory
```

For example:

```bash
cd ..
```

moves one directory up.

### `ls` Options

I practised:

```bash
ls
ls -l
ls -a
ls -lh
```

`ls -l` gives a detailed listing, `ls -a` shows hidden files, and `ls -lh` shows a detailed listing with human-readable file sizes.

I also learned that files beginning with `.` are normally hidden from a regular `ls`.

## Mini Project — Security Lab

I created a small workspace:

```text
security-lab/
├── investigations/
├── notes/
├── reports/
└── scripts/
```

I used it to practise navigating between directories, creating files, writing text with `echo`, reading files with `cat`, and working with hidden files.

I also started an investigation-style exercise:

```text
investigations/
└── case-001/
    ├── case-notes.txt
    └── .case-status
```

This was a Linux filesystem exercise rather than a real investigation.

## Mistakes I Learned From

I tried entering directories from the wrong location and learned why relative paths depend on my current directory.

I also accidentally used `touch` when I needed `mkdir`, which helped reinforce the difference between creating files and directories.

At one point I typed a filename directly into the terminal and got `command not found` because the shell tried to execute it.

I also encountered `dquote>` after forgetting to close a quotation mark while using `echo`. I learned that `Ctrl + C` can cancel an unfinished command.

## Progress

- [x] Linux filesystem basics
- [x] Absolute and relative paths
- [x] `~`, `.`, and `..`
- [x] `ls -l`, `ls -a`, and `ls -lh`
- [x] Hidden files
- [x] Security lab mini project
- [x] Navigation practice

### Next: Day 03 — Linux Permissions & Ownership
