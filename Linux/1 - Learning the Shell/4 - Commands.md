A **command** can be 1 of 4 things:
- **an executable program** - commands can be programs that are compiled binaries, such as programs written in C/C++
- **built-in commands** - command that are built-in to the shell itself e.g. `cd`
- **a shell function** - small shell scripts incorporated into the environment
- **an alias** - alias for other already-existing commands

#### Identifying commands
##### `type`
To identify which of the 4 types of forms a command can take, you can use the `type` command
```sh
type <command>
```
##### `which`
Whereas `type` tells you what kind of command is a specific command, `which` tells you the location of the executable of the command. This only works for executable programs, and not built-in commands / aliases
```sh
which <command>
```

#### Creating aliases
To create an alias, you can do:
```sh
alias name='command'
```
To see the list of aliases in the shell:
```sh
alias
```

