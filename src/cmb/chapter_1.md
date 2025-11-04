# Linux Fundamentals

## The History of Linux

In 1880, the French government awarded the Volta Prize to Alexander Graham Bell. Instead of going to the Maldives (kidding—he had work to do), he went to America and opened Bell Labs.

This lab researched electronics and something revolutionary called the mathematical theory of communications. In the 1950s came the transistor revolution. Bell Labs scientists won 10 Nobel Prizes—not too shabby.

But around this time, Russia made the USA nervous by launching the first satellite, Sputnik, in 1957 (named after its "father" Sergei Korolev, though the satellite project itself was called Sputnik). This had nothing to do with operating systems—it was literally just a satellite beeping in space—but it scared America enough to kickstart the space race.

President Eisenhower responded by creating ARPA (Advanced Research Projects Agency) in 1958, and asked James Killian, MIT's president, to help develop computer technology. This led to Project MAC (Mathematics and Computation) at MIT.

Before Project MAC, using a computer meant bringing a stack of punch cards with your instructions, feeding them into the machine, and waiting. During this time, no one else could use the computer—it was one job at a time.

The big goal of Project MAC was to allow multiple programmers to use the same computer simultaneously, executing different instructions at the same time. This concept was called **time-sharing**.

MIT and Bell Labs cooperated and developed the first operating system to support time-sharing: CTSS (Compatible Time-Sharing System). They wanted to expand this to larger mainframe computers, so they partnered with General Electric (GE), who manufactured these machines. In 1964, they developed the first real OS with time-sharing support called **Multics**. It also introduced the terminal as a new type of input device.

In the late 1960s, GE and Bell Labs left the project. GE's computer department was bought by Honeywell, which continued the project with MIT and created a commercial version that sold for 25 years.

In 1969, Bell Labs engineers (Dennis Ritchie and Ken Thompson) developed a new OS based on Multics. In 1970, they introduced **Unics** (later called **Unix**—the name was a sarcastic play on "Multics," implying it was simpler).

The first two versions of Unix were written in assembly language, which was then translated by an assembler and linker into machine code. The big problem with assembly was that it was tightly coupled to specific processors, meaning you'd need to rewrite Unix for each processor architecture. So Dennis Ritchie decided to create a new programming language: **C**.

They rebuilt Unix using C. At this time, AT&T owned Bell Labs (now it's Nokia). AT&T declared that Unix was theirs and no one else could touch it—classic monopolization.

AT&T did make one merciful agreement: universities could use Unix for educational purposes. But after AT&T was broken up into smaller companies in 1984, even this stopped. Things got worse.

One person was watching all this and decided to take action: **Andrew S. Tanenbaum**. In 1987, he created a new Unix-inspired OS called **MINIX**. It was free for universities and designed to work on Intel chips. It had some issues—occasional crashes and overheating—but this was just the beginning. This was the first time someone made a Unix-like OS outside of AT&T.

The main difference between Unix and MINIX was that MINIX was built on a **microkernel architecture**. Unix had a larger monolithic kernel, but MINIX separated some modules—for example, device drivers were moved from kernel space to user space.

It's unclear if MINIX was truly open source, but people outside universities wanted access and wanted to contribute and modify it.

Around the same time MINIX was being developed, another person named **Richard Stallman** started the free software movement based on four freedoms: Freedom to run, Freedom to study, Freedom to modify, and Freedom to share. This led to the GPL license (GNU General Public License), which ensured that if you used something free, your product must also be free. They created the **GNU Project**, which produced many important tools like the GCC compiler, Bash shell, and more.

But there was one problem: the kernel—the beating heart of the operating system that talks to the hardware—was missing.

Let's leave the USA and cross the Atlantic Ocean. In Finland, a student named **Linus Torvalds** was stuck at home while his classmates vacationed in Baltim Egypt (kidding). He was frustrated with MINIX, had heard about GPL and GNU, and decided to make something new. "I know what I should do with my life," he thought. As a side hobby project in 1991, he started working on a new kernel (not based on MINIX) and sent an email to his classmates discussing it.

Linus announced Freax (maybe meant "free Unix") with a GPL license. After six months, he released another version and called it **Linux**. He improved the kernel and integrated many GNU Project tools. He uploaded the source code to the internet (though Git came much later—he initially used FTP). This mini-project became the most widely used OS on Earth.

The penguin mascot (Tux) came from multiple stories: Linus was supposedly bitten by a penguin at a zoo, and he also watched March of the Penguins and was inspired by how they cooperate and share to protect their eggs and each other. Cute and fitting.

...And that's the history intro.

---

## Linux Distributions

Okay... let's install Linux. Which Linux? Wait, really? There are multiple Linuxes?

Here's the deal: the open-source part is the kernel, but different developers take it and add their own packages, libraries, and maybe create a GUI. Others add their own tweaks and features. This leads to many different versions, which we call **distributions** (or **distros** for short).

Some examples: Red Hat, Slackware, Debian.

Even distros themselves can be modified with additional features, which creates a version of a version. For example, Debian led to Ubuntu—these are called **derivatives**.

How many distros and derivatives exist in the world? Many. How many exactly? I said many. Anyone with a computer can create one.

So what's the main difference between these distros, so I know which one is suitable for me? The main differences fall into two categories: **philosophical** and **technical**.

One of the biggest technical differences is **package management**—the system that lets you install software, including the type and format of software itself.

Another difference is **configuration files**—their locations differ from one distro to another.

We agreed that everything is free, right? Well, you may find some paid versions like **Red Hat Enterprise Linux**, which charges for features like an additional layer of security, professional support, and guaranteed upgrades. Fedora is also owned by Red Hat and acts as a testing ground (a "backdoor," if you will) for new features before they hit Red Hat Enterprise.

The philosophical part is linked to the functional part. If you're using Linux for research, there are distros with specialized software for that. Maybe you're into ethical hacking—Kali Linux is for you. If you're afraid of switching from another OS, you might like Linux Mint, which even has themes that make it look like Windows.

Okay, which one should I install now? Heh... There are a ton of options and you can install any of them, but my preference is **Ubuntu**.

Ubuntu is the most popular for development and data engineering. But remember—in all cases, you'll be using the terminal a lot. So install Ubuntu, maybe in dual boot, and keep Windows if possible so you don't regret it later and blame me.

---

## The Terminal

Yes, this is what matters for us. Every distro will come with a default terminal but you can install others if you want. Anyway, open the terminal from the apps or just click `Ctrl+Alt+T`.

Zoom in using `Ctrl+Shift++` or out using `Ctrl+-`

By default first thing you will see the prompt `name@host:path$` which your name @ the machine name then `~` then dollar sign colon then `$`. After `$` you can write your command.

You can change the colors and all preferences and save each for profile.

---

## Basic Commands

First, everything is **case sensitive**, so be careful.

### [1] echo

This command echoes whatever you write after it.

```bash
$ echo "Hello, terminal"
```

**Output:**
```
Hello, terminal
```

### [2] pwd

This prints the current directory.

```bash
$ pwd
```

**Output:**
```
/home/mahmoudxyz
```

### [3] cd

This is for changing the directory.

```bash
$ cd Desktop
```

The directory changed with no output, you can check this using `pwd`.

To go back to the main directory use:

```bash
$ cd ~
```

Or just:

```bash
$ cd
```

Note that this means we are back to `/home/mahmoudxyz`

To go back to the previous directory (in this case `/home`) even if you don't know the name, you can use:

```bash
$ cd ..
```

### [4] ls

This command outputs the current files and directories (folders).

First let's go to desktop again:

```bash
$ cd /home/mahmoudxyz/Desktop
```

Yes, you can go to a specific dir if you know its path. Note that in Linux we are using `/` not `\` like Windows.

Now let's see what files and directories are in my Desktop:

```bash
$ ls
```

**Output:**
```
file1  python  testdir
```

If you notice that in my case, my terminal supports colors. The blue ones are directories and the grey (maybe black) is the file.

But you may deal with some terminal that doesn't support colors, in this case you can use:

```bash
$ ls -F
```

**Output:**
```
file1  python/  testdir/
```

What ends with `/` like `python/` is a directory otherwise it's a file like `file1`.

You can see the hidden files using:

```bash
$ ls -a
```

**Output:**
```
.  ..  file1  python  testdir  .you-cant-see-me
```

We saw `.you-cant-see-me`, but we are not hackers that we saw something hidden, being hidden is more than organizing purpose than actually hiding something.

You can also list the files in the long format using:

```bash
$ ls -l
```

**Output:**
```
total 8
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz    0 Nov  2 10:48 file1
drwxrwxr-x 2 mahmoudxyz mahmoudxyz 4096 Oct 16 15:20 python
drwxrwxr-x 2 mahmoudxyz mahmoudxyz 4096 Nov  1 21:45 testdir
```

Let's take the file1 and analyze the output:

| Column | Meaning |
|--------|---------|
| `-rw-rw-r-- 1` | File type + permissions (more on this later) |
| `1` | Number of hard links (more on this later) |
| `mahmoudxyz` | Owner name |
| `mahmoudxyz` | Group name |
| `0` | File size (bytes) |
| `Nov  2 10:48` | Last modification date & time |
| `file1` | File or directory name |

We can also combine these flags/options:

```bash
$ ls -l -a -F
```

**Output:**
```
total 16
drwxr-xr-x  4 mahmoudxyz mahmoudxyz 4096 Nov  2 10:53 ./
drwxr-x--- 47 mahmoudxyz mahmoudxyz 4096 Nov  1 21:55 ../
-rw-rw-r--  1 mahmoudxyz mahmoudxyz    0 Nov  2 10:48 file1
drwxrwxr-x  2 mahmoudxyz mahmoudxyz 4096 Oct 16 15:20 python/
drwxrwxr-x  2 mahmoudxyz mahmoudxyz 4096 Nov  1 21:45 testdir/
-rw-rw-r--  1 mahmoudxyz mahmoudxyz    0 Nov  2 10:53 .you-cant-see-me
```

Or shortly:

```bash
$ ls -laF
```

The same output. The order of options is not important so `ls -lFa` will work as well.

### [5] clear

This cleans your terminal. You can also use shortcut `Ctrl+L`

### [6] mkdir

This makes a new directory.

```bash
$ mkdir new-dir
```

Then let's see the output:

```bash
$ ls -F
```

**Output:**
```
file1  new-dir/  python/  testdir/
```

### [7] rmdir

This will remove the directory.

```bash
$ rmdir new-dir
```

Then let's see the output:

```bash
$ ls -F
```

**Output:**
```
file1  python/  testdir/
```

### [8] touch

This command is for creating a new file inside a folder.

```bash
$ mkdir new-dir
$ cd new-dir
$ touch file1
$ ls -l
```

**Output:**
```
total 0
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:26 file1
```

You can also make more than one file with:

```bash
$ touch file2 file3
$ ls -l
```

**Output:**
```
total 0
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:26 file1
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file2
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file3
```

In fact `touch` was created for modifying the timestamp of the file so let's try again:

```bash
$ touch file1
$ ls -l
```

**Output:**
```
total 0
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:30 file1
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file2
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file3
```

What changed? The timestamp of file1. The `touch` is the easiest way to create a new file, it just changes the timestamp of the file and if it doesn't exist, it will create a new one.

### [9] rm

This will remove the file.

```bash
$ rm file1
$ ls -l
```

**Output:**
```
total 0
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file2
-rw-rw-r-- 1 mahmoudxyz mahmoudxyz 0 Nov  2 11:28 file3
```

### [10] echo & cat (revisited)

Yes again, but this time, it will be used to create a new file with some text inside it.

```bash
$ echo "Hello, World" > file1
```

To output this file we can use:

```bash
$ cat file1
```

**Output:**
```
Hello, World
```

**Notes:**
- If `file1` doesn't exist, it will create a new one.
- If it does exist → it will be overwritten.

To append text instead of overwrite use `>>`:

```bash
$ echo "Hello, Mah" >> file1
```

To output this file we can use:

```bash
$ cat file1
```

**Output:**
```
Hello, World
Hello, Mah
```

### [11] rm -r

Let's go back:

```bash
$ cd ..
```

And then let's try to remove the directory:

```bash
$ rmdir new-dir
```

**Output:**
```
rmdir: failed to remove 'new-dir': Directory not empty
```

In case the directory is not empty, we can use `rm` that we used for removing a file but this time with a flag `-r` which means recursively remove everything in the folder.

```bash
$ rm -r new-dir
```

### [12] cp

This command is for copying a file.

```bash
cp source destination
```

(you can also rename it while copying it)

For example, let's copy the hosts file:

```bash
$ cp /etc/hosts .
```

The dot `.` means the current directory. Meaning copy this file from this source to here. You can see the content of the file using `cat` as before.

### [13] man

`man` is the built-in manual for commands. It contains short descriptions for the command and its options and their functions. It is useful and can be replaced nowadays with online search or even AI.

Try:

```bash
$ man ls
```

And then try:

```bash
$ man cd
```

No manual entry for `cd`. I don't know why exactly, but it's probably because `cd` is built into the shell itself and not an external command or maybe programmer choice.

---

## Unix Philosophy

**Second System Syndrome:** If a software or system succeeds, any similar system that comes after it will likely fail. This is probably a psychological phenomenon—developers constantly compare themselves to the successful system, wanting to be like it but better. The fear of not matching that success often causes failure. Maybe you can succeed if you don't compare yourself to it.

Another thing: when developers started making software for Linux, everything was chaotic and random. This led to the creation of principles to govern development—a philosophy to follow. These principles ensure that when you develop something, you follow the same Unix mentality:

1. **Small is Beautiful** – Keep programs compact and focused; bloat is the enemy.
2. **Each Program Does One Thing Well** – Master one task instead of being mediocre at many.
3. **Prototype as Soon as Possible** – Build it, test it, break it, learn from it—fast iteration wins.
4. **Choose Portability Over Efficiency** – Code that runs everywhere beats code that's blazing fast on one system.
5. **Store Data in Flat Text Files** – Text is universal, readable, and easy to parse; proprietary formats lock you in.
6. **Use Software Leverage** – Don't reinvent the wheel; use existing tools and combine them creatively.
7. **Use Shell Scripts to Increase Leverage and Portability** – Automate tasks and glue programs together with simple scripts.
8. **Avoid Captive User Interfaces** – Don't trap users in rigid menus; let them pipe, redirect, and automate.
9. **Make Every Program a Filter** – Take input, transform it, produce output—programs should be composable building blocks.

These concepts all lead to one fundamental Unix principle: **everything is a file**. Devices, processes, sockets—treat them all as files for consistency and simplicity.

Not all people follow this now, but the important question is: is it important? I don't know. But still the question is: is it important for you as a data engineer or analyst who will deal with data and different distros and different computers which maybe will be remote? Yes, it is important and very important.

---

## Text Files

It's a bit strange that we are talking about editing text files in 2025. Really, does it matter?

Yes, it matters and it's a big topic in Linux because of what we discussed in the previous section.

There are a lot of editors on Linux like `vi`, `nano` and `emacs`. There is a famous debate between emacs and vim.

You can find `vi` in almost every distro. The shortcuts for it are many and hard to memorize if you are not dealing with it much, but you can use cheatsheets.

Simply put: `vi` is just two things, **insert mode** and **command mode**. The default when you open a file for the first time is the command mode. To start writing something you have to enter the insert mode by pressing `i`.

`nano` on the other hand is more simple and easier to use and edit files with.

Use any editor, probably `vi` or `nano` and start practicing on one.

---

## Terminal vs Shell

**Terminal ≠ Shell.** Let's clear this up.

The **shell** is the thing that actually interprets your commands. It's the engine doing the work. File manipulation, running programs, printing text. That's all the shell.

The **terminal** is just the program that opens a window so you can talk to the shell. It's the middleman, the GUI wrapper, the pretty face.

---

## Pipes, Filters and Redirection

### Standard Streams

Unix processes use I/O streams to read and write data.

Input stream sources include keyboards, terminals, devices, files, output from other processes, etc.

Unix processes have three standard streams:

- **STDIN (0)** – Standard Input (data coming in from keyboard, file, etc.)
- **STDOUT (1)** – Standard Output (normal output going to terminal, file, etc.)
- **STDERR (2)** – Standard Error (error messages going to terminal, file, etc.)

**Example:** Try running `cat` with no arguments—it waits for input from STDIN and echoes it to STDOUT.

- `Ctrl+D` – Stops the input stream and sends an EOF (End of File) signal to the process.
- `Ctrl+C` – Sends an INT (Interrupt) signal to the process (i.e., kills the process).

### Redirection

Redirection allows you to change the defaults for stdin, stdout, or stderr—sending them to different devices or files using their file descriptors.

#### File Descriptors

A file descriptor is a reference (or handle) used by the kernel to access a file. Every process gets its own file descriptor table.

#### Redirect stdin with `<`

Use the `<` operator to redirect standard input from a file:

```bash
$ wc < textfile
```

#### Using Heredocs with `<<`

Accepts input until a specified delimiter word is reached:

```bash
$ cat << EOF
# Type multiple lines here
# Press Enter, then type EOF to end
EOF
```

#### Using Herestrings with `<<<`

Pass a string directly as input:

```bash
$ cat <<< "Hello, Linux"
```

#### Redirect stdout using `>` and `>>`

Overwrite a file with `>` (or explicitly with `1>`):

```bash
$ who > file      # Redirect stdout to file (overwrite)
$ cat file        # View the file
```

Append to a file with `>>`:

```bash
$ whoami >> file  # Append stdout to file
$ cat file        # View the file
```

#### Redirect stderr using `2>` and `2>>`

Redirect error messages to a file:

```bash
$ ls /xyz 2> err  # /xyz doesn't exist, error goes to err file
$ cat err         # View the error
```

#### Combining stdout and stderr

Redirect both stdout and stderr to the same file:

```bash
# Method 1: Redirect stderr to err, then stdout to the same place
$ ls /etc /xyz 2> err 1>&2

# Method 2: Redirect stdout to err, then stderr to the same place
$ ls /etc /xyz 1> err 2>&1

# Method 3: Shorthand for redirecting both
$ ls /etc /xyz &> err

$ cat err  # View both output and errors
```

#### Ignoring Error Messages with `/dev/null`

The black hole of Unix—anything sent here disappears:

```bash
$ ls /xyz 2> /dev/null  # Suppress error messages
```

---

## User and Group Management

It is not complicated. The user here is like any other OS. An account with some permission and can do some operations.

There are three types of users in Linux:

### Super user
The administrator that can do anything in the world. It is called **root**.
- ID from 0 to 999

### System user
This represents software and not a real person. Some software may need some access and permissions to do some tasks and operations or maybe install something.
- ID from 0 to 999

### Normal user
This is us.
- ID >= 1000

Each user has its ID, shell, environmental vars and home dir.

---

## File Ownership and Permissions

*(Content to be added)*

---

## File Management

The root is like "C" in Windows.

- **Absolute path:** from root
- **Relative path:** relative to your current path

Commands:
- `ls -lhd`
- `ls -lr`
- `tree`
- `stat`

### File Structure

A file is three things:
1. **Filename**
2. **Data block:** the content in the memory or on the disk
3. **inode:** metadata of the file (command: `ls -i`)

**Q:** What if you deleted the original in both cases?

### Links

**Link:** shortcut in Windows

**Hard link:** another filename for the same inode and same data block (like a box with two labels on it, the two labels will not change the content of the file)
- Command: `ln original-file hardlink`
- Nothing happens to the hardlink file when original is deleted

**Soft link:** shortcut in Windows, note the inode is different here
- Command: `ln -s original-file softlink`
- Label exists but file content is deleted when original is deleted

---

## Understanding the Filesystem Hierarchy Standard

### Mounting

There's no link between the hierarchy of directories and their location on the disk.

For more details, see: [Linux Foundation FHS 3.0](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)