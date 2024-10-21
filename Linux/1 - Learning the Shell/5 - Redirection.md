I/O redirection is the process of redirecting input and output of commands to and from files.

#### Standard Input, Output, and Error
Many commands / programs produce output of some kind. These consist of 2 types:
- **program results** - the data that the program is designed to produce
- **status and error messages** - tells us how the program is getting a long

In keeping with the Unix theme of everything being a file, programs send their results to a special file called *standard output (stdout)* and their status messages to another file called *standard error (stderr)*. By default, both files are linked to the screen and not saved to disk.

In addition, many programs take input from a file called *standard input (stdin)* which is, by default, attached to the keyboard.

I/O redirection allows us to change where output goes and where input comes from. That is, redirection allows us to change the fact that output goes to the screen and input comes from the keyboard.

#### Redirecting Standard Output
To redirect standard output to another file instead of the screen, we use the `>` redirection operator followed by the name of the file. Errors will still appear in the screen since you're only redirecting standard output, and not standard error. Meanwhile, to append the result of standard output to a file instead of overwriting it, you can use the `>>` operator.

#### Redirecting Standard Error
To redirect standard error, you must refer to its *file descriptor*. A program can produce output on any number of file streams, but the ones mentioned and their respective file descriptors that the shell uses are:
- 0 - standard input
- 1 - standard output
- 2 - standard error

And so to redirect standard error, you can do:
```sh
ls -l /bin/usr 2> ls-error.txt
```
##### Redirecting stderr and stdout to 1 file
To redirect4 both standard error and standard output into one file, you can use the `&>` redirection operator.
```sh
ls -l /usr/bin /bin/usr &> ls-output-and-errors.txt
```
This operator is available for newer version of Bash, for older ones, you can do
```sh
ls -l /usr/bin /bin/usr > ls-output-and-errors.txt 2>&1
```

#### Disposing of unwanted output
If you find yourself wanting to throw away the output / error of a command, you can redirect it to a special file `/dev/null`. This file is called *bit bucket*, which accepts input and does nothing with it.
```sh
ls -l /bin/usr 2> /dev/null
```

#### Redirecting Standard Input
##### `cat`- Concatenate files
The `cat` command is used to read one or more files and copy them to standard output without paging
```sh
cat hello-world.txt
```
The reason why it's called cat is because it can also concatenate multiple files together
```sh
cat movie.mpeg.0* > movie.mpeg
```
Furthermore, the reason why `cat` is linked to stdin is because when you type `cat` alone, it waits for input to be entered via stdin, and by default, redirects it to stdout. This makes it useful to create short text files
```sh
cat
cat > lazy_dog.txt
```

A useful option for cat is `-n --number` which outputs the line numbers as well.





