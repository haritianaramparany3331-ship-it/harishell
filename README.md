# harishell

harishell: A Unix shell written in C++ that supports foreground/background process management, job control and Unix signal handling.

---

## Changelog

- Added execution of real commands with arguments (`git`, `ls`, `pwd`, ...) via `fork` + `execvp`
- Added background execution with `&`; background processes ignore `SIGHUP`
- Added `Ctrl+Z` (SIGTSTP) to stop the foreground process
- Added automatic zombie cleanup via `SIGCHLD`
- Added each child getting its own process group (`setpgid(0, 0)`) so signals don't reach the shell itself
- Added `stop <pid>` to stop a process
- Added `logout`, refused while background processes are still running, with a y/n confirmation otherwise
- Added the `cd <path>` builtin
- Added `Ctrl+C` (SIGINT) to terminate the foreground process
- Changed foreground handling to use `waitpid` with `WUNTRACED`, so the shell detects both stopped and finished processes (`WIFEXITED` / `WIFSIGNALED`)
- Added `sigsetjmp` / `siglongjmp` so `Ctrl+C` / `Ctrl+Z` without a running process return cleanly to the prompt
- Added `Ctrl+D` (EOF) to exit the shell
- Added a list of background and stopped processes, printed after every command
- Added the current working path as the prompt, as in a standard shell
- Changed all shell output from German to English
- Added `cont <pid>` to resume a stopped process; a resumed foreground process is waited for again
- Added quoted arguments: `"..."` and `'...'` are passed as a single argument
- Added command chaining with `;`, `&&` and `||`
- Added `!` to invert the exit status of a command
- Added a red prompt after a command exits with a non-zero status
- Added subshell grouping with `( ... )`, e.g. `( cd /tmp ; ls ) && pwd` (parentheses must be separated by spaces)
- Added the `exec` builtin, which replaces the shell with the given command
- Changed the structure: command execution moved into `hilfsfunktionen.cpp` / `hilfsfunktionen.h`
- Added line continuation: a trailing `\` continues the command on the next line
- Added pipes with `|`, e.g. `ls | grep cpp | wc -l`
- Added I/O redirection: `<`, `>`, `>>`, `<>`, `<&`, `>&`
- Added explicit file descriptors for redirection, e.g. `2>errors.txt`, `2>&1`, and closing a descriptor with `>&-`
- Added GNU readline: line editing with arrow keys and command history

---

## Author and resources
Antsa Haritiana Ramparany, computer science student at Hochschule Darmstadt (h_da), Germany

