# 🐚 my_shell

> *A handcrafted Unix shell built from scratch in pure C — no shortcuts, no standard library string functions, just raw systems programming.*

```
╔══════════════════════════════════════════════════════════════╗
║  [my_shell]> echo "Welcome to the rabbit hole"               ║
║  Welcome to the rabbit hole                                  ║
║  [my_shell]> _                                               ║
╚══════════════════════════════════════════════════════════════╝
```

---

## 🧬 Philosophy

Why use `strcmp()` when you can write your own? This shell is an exercise in **understanding Unix from the ground up** — every string comparison, every memory allocation, every fork and exec is intentional and educational.

---

## ✨ Features

| Command | Description |
|---------|-------------|
| `cd <path>` | Navigate the filesystem |
| `pwd` | Where am I? |
| `echo [-n] <text>` | Speak your truth (with `$VAR` expansion!) |
| `env` | Peek at the environment |
| `setenv VAR=value` | Shape your world |
| `unsetenv VAR` | Let go of the past |
| `which <cmd>` | Find executables hiding in your PATH |
| `.help` | A friendly guide |
| `exit` / `quit` | Graceful departure |

**Plus:** Full external command execution via `fork()` + `execve()` — run `ls`, `cat`, `gcc`, anything in your PATH.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        shell.c                              │
│                    (main loop & dispatch)                   │
└─────────────────────┬───────────────────────────────────────┘
                      │
        ┌─────────────┼─────────────┐
        ▼             ▼             ▼
┌───────────┐  ┌────────────┐  ┌───────────┐
│ builtins.c│  │ executor.c │  │ helper.c  │
│           │  │            │  │           │
│ cd, pwd,  │  │ fork/exec  │  │ my_strcmp │
│ echo, env │  │ PATH search│  │ my_strdup │
│ setenv... │  │ child proc │  │ my_strtok │
└───────────┘  └────────────┘  └───────────┘
        ▲             ▲
        └──────┬──────┘
               │
      ┌────────┴────────┐
      │ input_parser.c  │
      │                 │
      │ tokenization    │
      │ whitespace skip │
      └─────────────────┘
```

---

## 🔧 Reimplemented from Scratch

No `<string.h>` dependency for core operations:

```c
my_strcmp()    // String comparison
my_strlen()    // String length  
my_strncmp()   // Bounded comparison
my_strdup()    // Duplicate with malloc
my_strcpy()    // String copy
my_strchr()    // Character search
my_strtok()    // Tokenizer (stateful)
my_strncpy()   // Bounded copy
my_getenv()    // Environment lookup
```

---

## 🚀 Quick Start

```bash
# Compile
gcc -Wall -Wextra -Werror -o my_shell shell.c builtins.c executor.c helper.c input_parser.c

# Run
./my_shell

# Play
[my_shell]> pwd
/home/you
[my_shell]> echo Hello $USER
Hello you
[my_shell]> which gcc
/usr/bin/gcc
[my_shell]> exit
```

---

## 📁 File Manifest

| File | Purpose |
|------|---------|
| `shell.c` | Entry point, main loop, command dispatch |
| `shell_header.h` | Function prototypes & includes |
| `builtins.c` | Built-in command implementations |
| `executor.c` | Process creation & external commands |
| `input_parser.c` | Input tokenization |
| `helper.c` | Custom string library |

---

## 🎯 Learning Outcomes

Building this shell teaches:

- **Process control** — `fork()`, `execve()`, `waitpid()`
- **File system navigation** — `chdir()`, `getcwd()`, `access()`
- **Environment manipulation** — how shells modify `env`
- **Memory management** — manual `malloc`/`free` discipline
- **String handling** — what libc does under the hood
- **Unix philosophy** — small tools, composable design

---

## 🔮 Future Ideas

- [ ] Piping (`cmd1 | cmd2`)
- [ ] Redirection (`>`, `>>`, `<`)
- [ ] Background processes (`&`)
- [ ] Command history (↑↓ arrows)
- [ ] Tab completion
- [ ] Signal handling (`Ctrl+C`, `Ctrl+Z`)
- [ ] Scripting support

---

## 📜 License

Do whatever you want with it. Learn from it. Break it. Rebuild it better.

---

<p align="center">
  <i>Built with curiosity and caffeine ☕</i>
</p>
