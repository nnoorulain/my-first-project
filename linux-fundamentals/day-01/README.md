# Day 01 — Linux Fundamentals
Today was my first proper hands-on Linux practice for cybersecurity. I am using Kali Linux inside a virtual machine, and my main goal for today was to get comfortable working in the terminal.

Instead of trying to learn lots of security tools immediately, I started with basic Linux commands because I will need them throughout cybersecurity.

## What I learned

### `pwd`

I started with `pwd`, which means **Print Working Directory**.

It tells me where I currently am in the Linux filesystem.

For example:

```bash
pwd
```

I got:

```text
/home/kali
```

This means I was inside the home directory of the `kali` user.

---

### `ls`

I used `ls` to see the files and directories in my current location.

```bash
ls
```

I also came across:

```bash
ls -l
ls -a
```

`ls -l` gives a more detailed listing, while `ls -a` also shows hidden files.

I will explore these options more deeply as I continue learning Linux.

---

### `cd`

I used `cd` to move between directories.

Some examples I practised were:

```bash
cd /
cd ~
cd ..
```

I learned that:

- `/` is the root of the Linux filesystem.
- `~` represents my home directory.
- `..` represents the parent directory.

One mistake I made was typing:

```bash
cd~
```

instead of:

```bash
cd ~
```

The first command did not work because the shell treated `cd~` as the name of a command. This helped me understand that spaces in Linux commands matter.

---

### `mkdir`

`mkdir` means **Make Directory**.

I used it to create a directory for my cybersecurity practice:

```bash
mkdir cybersecurity
```

I also learned that giving something a `.txt` extension does not automatically make it a text file.

For example:

```bash
mkdir report.txt
```

would create a directory called `report.txt`, not a text file.

---

### `touch`

I used `touch` to create empty files.

For example:

```bash
touch test.txt
```

After that, I used:

```bash
ls
```

to confirm that the file existed.

I learned that `touch` is mainly used to update file timestamps, but when the specified file does not exist, it can also create an empty file.

---

### `rm`

I learned that `rm` is used to remove files.

For example:

```bash
rm test.txt
```

I also learned about:

```bash
rm -i test.txt
```

The `-i` option asks for confirmation before removing the file, which is useful while I am still learning.

I need to be careful with `rm` because files removed from the terminal are not normally sent to the desktop Trash.

---

### `cp`

I used `cp` to copy files.

For example:

```bash
cp notes.txt notes-copy.txt
```

This creates a copy while keeping the original file.

A simple way I remember the syntax is:

```text
cp source destination
```

---

### `mv`

I learned that `mv` can move files, but it can also be used to rename them.

For example:

```bash
mv notes-copy.txt linux-notes.txt
```

This changes the name from `notes-copy.txt` to `linux-notes.txt`.

---

### `cat`

I used `cat` to display the contents of a text file directly in the terminal.

For example:

```bash
cat notes.txt
```

This was useful for checking whether the text I wrote into a file was actually there.

---

### `echo`

I used `echo` to output text.

For example:

```bash
echo "Learning Linux"
```

prints:

```text
Learning Linux
```

I then learned how to redirect that output into files.

---

## `>` vs `>>`

This was one of the most useful things I learned today.

I used:

```bash
echo "My Linux Notes" > notes.txt
```

The `>` writes the output into the file. If the file already contains something, its previous contents are replaced.

Then I used:

```bash
echo "Today I learned Linux commands." >> notes.txt
```

The `>>` adds the new text to the end of the file without deleting what was already there.

So the way I remember it is:

```text
>   overwrite
>>  append
```

This is something I need to be careful with because accidentally using `>` on an important file could overwrite its existing contents.

---

## A problem I ran into

The most interesting part of today's practice was actually an error.

I tried:

```bash
mkdir cybersecurity
```

but Kali returned:

```text
mkdir: cannot create directory 'cybersecurity': Read-only file system
```

At first I thought I was using `mkdir` incorrectly.

To test whether the problem was only with `mkdir`, I tried creating a file:

```bash
touch test.txt
```

That also failed with a read-only filesystem error.

This showed me that the command itself was not the problem.

---

## Finding the problem

I used:

```bash
findmnt -T /home/kali
```

and found that the filesystem had:

```text
ro
```

I learned that:

```text
ro = read-only
rw = read-write
```

Because the filesystem was mounted as read-only, I could look at existing files, but I could not create or modify files and directories.

I also used:

```bash
lsblk -f
```

and learned that my Kali VM has a virtual disk, with `/dev/sda1` containing the EXT4 filesystem being used for `/`.

I checked disk space using:

```bash
df -h /home/kali
```

There was plenty of free space, so a full disk was not causing the problem.

I also looked at kernel messages with:

```bash
sudo dmesg | tail -30
```

and later filtered them using:

```bash
sudo dmesg | grep -Ei 'ext4|sda|error|I/O|read-only'
```

I do not know all of these commands properly yet, but I understand why I used them during troubleshooting.

After restarting the Kali virtual machine, I checked again:

```bash
findmnt -T /
```

This time I saw:

```text
rw,relatime,errors=remount-ro
```

The important part was `rw`, meaning the filesystem was now read-write.

I did not manually change `ro` to `rw`. After restarting the VM, Linux mounted the filesystem normally as read-write during the new boot.

After that, I could successfully run:

```bash
mkdir cybersecurity
cd cybersecurity
pwd
touch test.txt
ls
```

and continue practising.

---

## Mistakes I made

I made a few mistakes today, but they helped me understand how Linux behaves.

I typed:

```bash
cd~
```

instead of:

```bash
cd ~
```

I also misspelled `cybersecurity` while trying to enter the directory.

Another thing I learned was the difference between creating a directory and creating a file:

```bash
mkdir report.txt
```

creates a directory, while:

```bash
touch report.txt
```

can create an empty file.

And:

```bash
echo "Security Report" > report.txt
```

can create the file and write text into it at the same time.

---

## Commands I practised today

| Command | What I learned |
|---|---|
| `pwd` | Shows my current working directory |
| `ls` | Lists directory contents |
| `cd` | Changes my current directory |
| `mkdir` | Creates a directory |
| `touch` | Can create an empty file |
| `rm` | Removes files |
| `cp` | Copies files |
| `mv` | Moves or renames files |
| `cat` | Displays file contents |
| `echo` | Outputs text |
| `>` | Redirects output and overwrites |
| `>>` | Redirects output and appends |

During troubleshooting, I also encountered `findmnt`, `lsblk`, `df`, `dmesg`, `grep`, filesystem mounting, EXT4, `/dev/sda1`, and pipes (`|`). These are not commands I consider learned yet, but today gave me my first exposure to them.

---

## What I learned from Day 01

The biggest lesson for me was not just learning commands. I learned that when a command fails, I should read the error message and try to understand the actual cause.

When `mkdir` failed, repeatedly typing the command would not have fixed anything because `mkdir` was not the problem. The filesystem was read-only.

My basic troubleshooting process was:

```text
Command fails
    ↓
Read the error
    ↓
Understand what the error is saying
    ↓
Test the problem
    ↓
Investigate the cause
    ↓
Fix or remove the cause
    ↓
Verify
    ↓
Try the original task again
```

This was my first day of Linux fundamentals. I still need practice with these commands, but I now understand the basic workflow of navigating directories, creating and managing files, and using the terminal instead of relying entirely on the GUI.

## Progress

- [x] Kali Linux set up in a virtual machine
- [x] Basic terminal navigation
- [x] Creating files and directories
- [x] Copying, moving and removing files
- [x] Reading file contents
- [x] Basic output redirection
- [x] First Linux troubleshooting experience
- [ ] Linux filesystem structure and paths
- [ ] Permissions
- [ ] Users and groups
- [ ] Processes and services
- [ ] Bash scripting

### Next: Day 02 — Linux Filesystem Structure and Paths
