#### Wildcards
The shell provides special characters that allow you to specify groups of filenames. These special characters are called **wildcards** and they allow you to select groups of files based on patterns of characters. They can be used with any command that accepts filenames as arguments.

**Table of wildcards**

| wildcard        | matches                                       |
| --------------- | --------------------------------------------- |
| *               | any character                                 |
| ?               | any single character                          |
| [*characters*]  | any character that is a member of the set     |
| [!*characters*] | any character that is NOT a member of the set |
| [ [:*class*:]]  | any character that is a member of the class   |

**Classes**
- `alnum` - any letter or numeral
- `alpha` - any letter
- `digit` - any numeral
- `lower` - any lowercase letter
- `upper` - any uppercase letter

#### `mkdir`- Make directories
The command used to make a directory is `mkdir` which can take any number of directory names in the form `mkdir directory1 directory2`.

#### `cp`- Copy files & directories
The `cp` command can be used either to copy a single file / directory to another file / directory as in
```sh
cp item1 item2 
```
or copy multiple files into a directory
```sh
cp item... directory
```
**Options**
- `-a --archive` - copies the files and directories and all of their attributes
- `-i --interactive` - copies files and directories but prompts the user if it can overwrite a file
- `-r --recursive` - recursively copies files when copying a directory (required)
- `-u --update` - when copying files from one directory to another, copy only files that don't exist or are newer than the corresponding files in the destination directory

#### `mv`- Move and rename files
`mv` is used in much the same way as `cp`, and supports similar options. You can rename files or directories with
```sh
mv item1 item2
```
or you can move items to a directory with
```sh
mv item... directory
```

#### `rm`- Remove files and directories
You can remove files and directories with the `rm` command. To delete files, you can do 
```sh
rm item...
```
or to delete directories, you can do
```sh
rm -rf directory
```
Unlike other operating systems, Linux assumes you know what you're doing, and so there's no undelete command. Whatever you delete is final. What you *can* do is first list the files that will be deleted, and then replace the `ls` with `rm`.

#### `ln`- Create links
The `ln` command is used to create symbolic links. You can create a symbolic link with, where `item` is either a file or a directory
```sh
ln -s item link
```
Symbolic links work by creating a special type of file that contains a text pointer to the referenced file or directory. In that way, they work the same way as Windows shortcuts.

A symbolic link and the file / directory it's referencing is hardly indistinguishable. If you make changes to the symlink, you also change the referenced item. But when you delete the item, the link stays, but points to nothing.


