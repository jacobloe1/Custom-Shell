#  Custom Shell 

> *A Unix shell built from scratch in pure C — no shortcuts, no standard library string functions*

```
╔══════════════════════════════════════════════════════════════╗
║  [my_shell]> echo "Welcome to the terminal"                  ║
║  Welcome to the terminal                                     ║
║  [my_shell]> _                                               ║
╚══════════════════════════════════════════════════════════════╝
```

---

##  Skills & Knowledge Gained

Building this project transformed my understanding of how operating systems actually work

**C Programming**
- Pointer arithmetic became second nature — manipulating strings character by character
- Developed intuition for memory layout and manual allocation strategies
- Learned to think about null terminators, buffer sizes, and boundary conditions constantly

**Operating Systems Internals**
- Finally understood what happens when you type a command and press Enter
- Grasped the parent-child process relationship through hands-on `fork()` implementation
- Discovered how the kernel distinguishes between built-in shell commands and external executables
- Learned why PATH exists and how shells resolve command locations

**Problem Decomposition**
- Learned to break complex systems into modular, testable components
- Each file has a single responsibility — parser parses, executor executes
- Custom helper functions mirror libc, teaching what those functions actually do internally

---

##  Features

| Command | Description |
|---------|-------------|
| `cd <path>` | Navigate the filesystem |
| `pwd` | Where am I? |
| `echo [-n] <text>` | Displays text (with `$VAR` expansion!) |
| `env` | Looks at the environment |
| `setenv VAR=value` | Sets environment variable |
| `unsetenv VAR` | Removes environment variable |
| `which <cmd>` | Find executables hiding in your PATH |
| `.help` | Guide to commands |
| `exit` / `quit` | Leaves terminal |

**Plus:** Full external command execution via `fork()` + `execve()` — run `ls`, `cat`, `gcc`, anything in your PATH.

---

##  Architecture

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

##  Reimplemented from Scratch

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

##  Quick Start

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

##  File Manifest

| File | Purpose |
|------|---------|
| `shell.c` | Entry point, main loop, command dispatch |
| `shell_header.h` | Function prototypes & includes |
| `builtins.c` | Built-in command implementations |
| `executor.c` | Process creation & external commands |
| `input_parser.c` | Input tokenization |
| `helper.c` | Custom string library |

---

##  Learning Outcomes

Building this shell taught me:

- **Process control** — `fork()`, `execve()`, `waitpid()`
- **File system navigation** — `chdir()`, `getcwd()`, `access()`
- **Environment manipulation** — how shells modify `env`
- **Memory management** — manual `malloc`/`free` discipline
- **String handling** — what libc does under the hood
- **Unix philosophy** — small tools, composable design

---

##  Future Ideas

- [ ] Piping (`cmd1 | cmd2`)
- [ ] Redirection (`>`, `>>`, `<`)
- [ ] Background processes (`&`)
- [ ] Command history (↑↓ arrows)
- [ ] Tab completion
- [ ] Signal handling (`Ctrl+C`, `Ctrl+Z`)
- [ ] Scripting support

---

##  License

Do whatever you want with it!

---
>
