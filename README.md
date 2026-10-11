# harishell

A Unix shell built from scratch in C++, guided by the open-source build-your-own-shell guide and the Operating Systems course at Hochschule Darmstadt. It supports foreground and background process management, job control and Unix signal handling, and covers most of what you'd use in a regular terminal day to day.

- Runs real commands with arguments (`git`, `ls`, `pwd`, ...) via `fork` and `execvp`, plus the builtins `cd`, `exec` and `logout`
- Runs processes in the background with `&`; background processes ignore `SIGHUP` and keep running
- Stops the foreground process with `Ctrl+Z` and terminates it with `Ctrl+C`; `stop <pid>` and `cont <pid>` stop and resume any process by its PID
- Lists all background and stopped processes after every command
- Chains commands with `;`, `&&` and `||`, inverts an exit status with `!`, and groups commands in a subshell with `( ... )`
- Connects commands with pipes, e.g. `ls | grep cpp | wc -l`
- Supports I/O redirection with `<`, `>`, `>>`, `<>`, `<&` and `>&`, including explicit file descriptors like `2>errors.txt`, `2>&1` or `>&-`
- Treats quoted text (`"..."` or `'...'`) as a single argument and continues a command on the next line after a trailing `\`
- Offers line editing and command history through GNU readline
- Shows the current working path as the prompt, which turns red after a command fails
- Refuses `logout` while background processes are still running, and asks for confirmation otherwise

Each child process gets its own process group (`setpgid(0, 0)`), so signals like `Ctrl+C` reach only the foreground process and never the shell itself. The shell waits for foreground processes with `waitpid` and `WUNTRACED` to detect both stopped and finished processes, while a `SIGCHLD` handler reaps finished background processes so no zombies are left behind. `sigsetjmp` and `siglongjmp` bring the shell cleanly back to the prompt when `Ctrl+C` or `Ctrl+Z` is pressed without a running process.

The tokenizer splits input on spaces, so operators and parentheses need spaces around them: `( cd /tmp ; ls )` works

Antsa Haritiana Ramparany, computer science student at Hochschule Darmstadt (h_da), Germany

