# 🖥️ Terminal Basics

---

## 💻 Navigating the Command Line

The **command line** (terminal) lets you interact with your computer using text commands.

```
┌─────────────────────────────────────────────────────────────────┐
│  Terminal Window                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  user@computer:~$ _                                             │
│                 ↑                                               │
│                 Cursor waiting for your command                 │
│                                                                 │
│  ~ means "home directory"                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📋 Essential Commands

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print Working Directory (where am I?) | `pwd` → `/home/user` |
| `ls` | List files in current directory | `ls` |
| `ls -la` | List ALL files (including hidden) | `ls -la` |
| `cd <dir>` | Change Directory | `cd source` |
| `cd ..` | Go up one level | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd /` | Go to root of filesystem | `cd /` |
| `touch <file>` | Create empty file | `touch test.txt` |
| `rm <file>` | Remove (delete) file | `rm test.txt` |
| `cat <file>` | Display file contents | `cat main.cc` |
| `clear` | Clear terminal screen | `clear` |

---

## 🗂️ Navigation Example

```bash
$ pwd
/home/user

$ ls
bin  source

$ cd source          # Enter 'source' folder
$ pwd
/home/user/source

$ ls
hello_world.cc  test.txt

$ cd ..              # Go back up one level
$ pwd
/home/user

$ cd /               # Go to root
$ pwd
/
```

---

## 📍 Absolute vs Relative Paths

```
┌─────────────────────────────────────────────────────────────────┐
│                       FILE SYSTEM                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   /                         ← Root                              │
│   └── home/                                                     │
│       └── user/             ← Home (~)                          │
│           ├── bin/                                              │
│           └── source/       ← You are here                      │
│               └── main.cc                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

```bash
# ABSOLUTE PATH: From root (/)
$ cd /home/user/source

# RELATIVE PATH: From current location
$ cd ../bin          # Go up one level, then into bin
$ cd ./source        # Stay here, then into source (same as: cd source)
```

---

## 💡 Tips & Tricks

```bash
# Tab auto-complete: Type partial name, press Tab
$ cd sou<Tab>        # Completes to: cd source

# View hidden files (start with .)
$ ls -la
.bashrc  .profile  bin  source

# Create and view a file
$ touch notes.txt
$ echo "Hello" > notes.txt   # Write "Hello" to file
$ cat notes.txt              # Display: Hello
```

---

## 🔑 Quick Reference

| Action | Command |
|--------|---------|
| Where am I? | `pwd` |
| What's here? | `ls` |
| Go into folder | `cd folder_name` |
| Go up one level | `cd ..` |
| Go home | `cd ~` |
| Create file | `touch filename` |
| Delete file | `rm filename` |
| View file | `cat filename` |
| Clear screen | `clear` |

