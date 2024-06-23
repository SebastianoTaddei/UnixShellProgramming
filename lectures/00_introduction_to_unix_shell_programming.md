# Introduction to Unix Shell Programming

Unix Shell Programming is a powerful tool that allows you to automate tasks, manage files, and interact with the operating system. This seminar will introduce you to the basics of Unix Shell Programming and provide you with the knowledge and skills to start writing your own shell scripts. To show the power of shell programming, throughout this seminar we will try to only use the terminal and a keyboard to fully interact with the system. It will not be easy at first, but hopefully it will provide you with a new perspective on how to interact with your computer.

Note: It goes without saying that you can follow this seminar as you see fit. You can use a graphical interface, a mouse, or any other tool you feel comfortable with. The goal is to learn and have fun!

Before we dive into the details, let's first understand what Unix is and why it is important.

## 1 Introduction to Unix

### 1.1 Historical Context

The history of Unix, a powerful and enduring operating system, began in the late 1960s and early 1970s. Developed at AT&T's Bell Labs by Ken Thompson, Dennis Ritchie, and others, Unix was initially a reaction to the complexities and limitations of existing systems. Its origins lie in the Multics project, an ambitious attempt to create a versatile, multi-user operating system. When Multics proved overly complex and ultimately unsuccessful, Thompson and Ritchie sought to create a simpler, more efficient system. This led to the birth of Unix, a name that itself is a pun on Multics, suggesting something more streamlined and practical.

### 1.2 Core Philosophy

Unix's design philosophy is founded on simplicity, modularity, and reusability. These principles are encapsulated in a set of core ideas that have guided Unix development from the beginning. Some of the key principles include:

1. **Everything is a File:** In Unix, most entities, including hardware devices and inter-process communication channels, are represented as files. This abstraction simplifies the interaction between different parts of the system and provides a consistent interface.

2. **Small, Single-purpose Programs:** Unix encourages the creation of small, modular programs that perform specific tasks. These programs can be combined in flexible ways (*e.g.,* using pipes) to accomplish complex tasks, adhering to the philosophy of "do one thing and do it well."

3. **Use of Plain Text for Data Storage:** Unix systems prefer plain text for data storage, configuration, and communication. This choice enhances portability and simplicity, allowing users and programs to easily read and manipulate files.

4. **Build Software to Work Together:** Unix systems are designed so that programs can easily interoperate. This is achieved through mechanisms like pipes and redirection, which allow the output of one program to be used as the input for another.

### 1.3 Technical Innovations

Unix introduced several groundbreaking technical innovations that have had a lasting impact on the field of computing:

- **Hierarchical File System:** Unix implemented a hierarchical file system structure, organizing files in a tree-like directory structure. This organization makes file management intuitive and scalable.

- **Process Control:** Unix introduced sophisticated process control mechanisms, allowing users to manage multiple processes simultaneously. Features like multitasking, background processing, and process signaling were revolutionary at the time.

- **Shell and Scripting:** The Unix shell, a command-line interpreter, allows users to interact with the system through textual commands. Shell scripting capabilities enable the automation of tasks and the chaining of commands, significantly enhancing productivity.

- **Portable Code:** Unix was one of the first operating systems to be written in the C programming language, developed by Dennis Ritchie. This decision made Unix highly portable, enabling it to be adapted to various hardware platforms with relative ease.

### 1.4 Unix Variants and Influence

Over the decades, Unix has spawned a multitude of variants and inspired numerous operating systems. Some of the most notable Unix variants include: BSD, Linux, Solaris, and macOS.

Today, Unix and its derivatives play a crucial role in the world of computing. Unix-like systems are prevalent in servers, workstations, and embedded systems. They form the backbone of the internet, powering web servers, database servers, and various networking infrastructure. The principles and practices established by Unix continue to influence modern software development, from cloud computing to mobile applications.

NOTE: Unix is also used in a variety of other devices, in fact it is used to develop the Operating Systems for mobile devices, like iOS.

The Unix philosophy of simplicity, modularity, and reusability has left an indelible mark on the software engineering field. Concepts like version control systems (*e.g.,* Git), containerization (*e.g.,* Docker), and configuration management (*e.g.,* Ansible) all draw inspiration from Unix principles.

### 1.5 Conclusion

Unix's journey from a small, experimental operating system to a cornerstone of modern computing is a testament to its robust design and enduring relevance. By emphasizing simplicity, modularity, and portability, Unix has created a legacy that transcends decades, shaping the evolution of operating systems and software development practices. As we look to the future, Unix's influence remains pervasive, guiding new generations of technology and innovation.

### 1.6 References

- [A Quick Introduction to Unix](https://en.wikibooks.org/wiki/A_Quick_Introduction_to_Unix#:~:text=Unix%20is%20an%20operating%20system%20designed%20for%20use%20on%20any%20kind%20of%20computer%20or%20computing%20device.)
- [Unix](https://en.wikipedia.org/wiki/Unix)
- [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)

## 2 Basic Unix Commands

Without further ado, let's dive into the Unix command line and explore some basic commands that will be essential for our shell programming journey.

### 2.1 Preliminary Steps

All we will need for this seminar is a Unix-like operating system (Linux, macOS, WSL on Windows) and a terminal emulator. We will use `bash` as our shell, but the concepts we discuss are applicable to other shells as well. In addition, I will use `vim` as my text editor, but feel free to use any text editor you are comfortable with.

Verify that you have everything set up by opening a terminal and running the following command:

```bash
echo "Hello, Unix!"
```

If you see the output `Hello, Unix!`, you are ready to go!

### 2.2 Changing Directories

First, let us see what directory we are currently in. Run the following command:

```bash
pwd
```

If you are on a fresh Ubuntu 22.04 installation, like me, you will see an output similar to:

```bash
/home/username
```

where `username` is your username. This is your home directory, the one that the system usually defaults to when you open a terminal. We already can understand a lot from this output:

- The command `pwd` stands for "print working directory."
- The `/` character is used to separate directories in Unix systems.
- The `home` directory is where user directories are usually stored.
- The first `/` in the output represents the root directory of the system. All paths start from this directory.

Now that we now where we are, let us change directories. If we run:

```bash
cd /
```

we will move to the root directory of the system. You can verify this by running `pwd` again. To move back to the home directory, we can run:

```bash
cd /home/username
```

What we did was use the command `cd` (short for "change directory") followed by the path we wanted to move to. We moved using an absolute path, but we can also move using a relative path. For example, to move to the root directory from the home directory, we can run:

```bash
cd ../../
```

this command moves two directories up from the current directory. You can verify this by running `pwd`. `cd` has several shortcuts that you can use to navigate quickly, we will discuss them as we go along. For the rest of the seminar, we will use the home directory as our working directory. You can always move back to the home directory by running `cd` without any arguments.

NOTE: By now you probably have a lot of old text in your terminal. You can clear it by running `clear` or via a key combination (on macOS, it is `Cmd + K`, on Linux it is `Ctrl + L`).

NOTE: On most terminals, you can use the `Tab` key to autocomplete commands and paths. This is a very useful feature that will save you a lot of typing.

### 2.3 Listing Directory Contents

Up to now we changed directories assuming we knew where we wanted to go. But what if we want to explore the contents of a directory before moving into it? We can use the `ls` command to list the contents of a directory. For example, to list the contents of the root directory, we can run:

```bash
ls /
```

this will show you all the files and directories in the root directory. You can also list the contents of the current directory by running `ls` without any arguments. Again, there are several options that you can use with `ls` to customize the output, we will explore them as we go along. The `ls` command stands for "list."

NOTE: Another cool command to show the contents of a directory is `tree`, which should be available on most Linux installations (otherwise you can always download it via your package manager). `tree` shows the contents of a directory in a tree-like format, making it easier to visualize the directory structure. Beware that by default it will show you all folders and files, so running it on the root directory may be a bit overwhelming. In this cases, we will see how you can show just enough output of any command.

### 2.4 Creating and Removing Directories

To create a new directory, you can use the `mkdir` command followed by the name of the directory you want to create. For example, to create a directory named `test`, you can run:

```bash
mkdir test
```

this will create a new directory named `test` in the current directory. You can verify this by running `ls`. I think you can already start to guess the meaning of the commands we are using, `mkdir` stands for "make directory".

Of course we can navigate to the directory we just created by running:

```bash
cd test
```

If you want to remove the directory, you can use the `rmdir` command followed by the name of the directory. For example, to remove the `test` directory, you can run:

```bash
rmdir test
```

Although we can use the `rmdir` command to remove directories, more often than not we will use the `rm` command to remove files and directories. To perform the same operation as above using `rm`, you can run:

```bash
rm -d test
```

Several options can be used with `rm` and `mkdir` to customize their behavior, we will explore them as we go along.

NOTE: Be careful when using the `rm` command, as it will permanently delete files and directories without moving them to the trash. Always double-check the arguments before running the command.

### 2.5 More Options Available, but where!?

Let us take a stop for one second and think about the commands we have seen up to now. We have seen `pwd`, `cd`, `ls`, `mkdir`, `rmdir`, and `rm`. These are just a few of the commands available in Unix systems. I keep telling you that there are several options available for each command, but where can you find them? The answer is simple: the manual pages or directly asking the command for help. For example, to see the manual page for the `ls` command, you can run:

```bash
man ls
```

or to see the available options for the `ls` command, you can run:

```bash
ls --help
```

more often than not, the `--help` option is available for most commands and provides a quick overview of the available options.

Linux built-in commands, like `cd` often do not have `man` pages, but using `cd --help` or `help cd` will provide you with the available options.

NOTE: The `--help` options is usually available, but it often is under different names, like `-h`, `-H`, `-?`, etc. You will have to experiment a bit to find the correct option for each command.

### 2.6 Working with Files

Since we will spend most of this seminar working inside files, let us see how we can create, read, and write to files using the command line. First and foremost, navigate to the home directory and create an empty directory names `test_files`:

```bash
cd ~ && mkdir test_files
```

Before proceeding, let me explain a couple of tricks we used in the command above. The `~` character is a shortcut for the home directory, so `cd ~` moves you to the home directory. The `&&` operator is used to chain commands, so `cd ~ && mkdir test_files` moves you to the home directory and then creates a new directory named `test_files`. Note that `&&` only chains commands if the previous command was successful, if the previous command fails, all subsequent commands are not executed.

Now that we have a directory to work with, let us navigate to it and create a new file named `test.txt`:

```bash
cd test_files && touch test.txt
```

To create empty files (with any extension), we can use the `touch` command followed by the name of the file. The `touch` command is used to create empty files and update the access and modification times of existing files. You can verify that the file was created by running `ls`.

We now have a file, but it is empty. To write to the file, we can use the `echo` command followed by the text we want to write. For example, to write the text "Hello, Unix!" to the `test.txt` file, you can run:

```bash
echo "Hello, Unix!" > test.txt
```

The `echo` command is used to print text to the standard output, and the `>` character is used to redirect the output to a file. You can verify that the text was written to the file by running `cat test.txt`. The `cat` command is used to concatenate and display the contents of a file, but it can also be used to create new files and immediately write to them. For example, to create a new file named `test2.txt` and write the text "Hello, Unix!" to it, you can run:

```bash
cat > test2.txt
Hello, Unix!
```

NOTE: Do not worry to much about `>` for now, we will discuss it, with other redirection operators, in detail later on.

Using `echo` and `cat` is pretty cool and useful in some situations, but what if we want to edit a file that already has content? For this, we can use a command-line text editor, like `vim`. To open the `test.txt` file in `vim`, you can run:

```bash
vim test.txt
```

This will open the file in `vim`, a powerful and versatile text editor. Learning `vim` will not be covered in the seminar, but I encourage you to explore it on your own. If you want to use it during this seminar, the basic commands you need to know are:

- `i`: Enter insert mode (to start writing text).
- `Esc`: Exit insert mode (to stop writing text).
- `:w`: Save the file.
- `:q`: Quit `vim`.
- `h`, `j`, `k`, `l`: Move the cursor left, down, up, and right, respectively when not in insert mode.

If instead you do not feel comfortable using `vim`, you can use any other text editor you are comfortable with, like `nano` or even `VSCode`.

Aside from using `vim`, we now have opened a file in a text editor and as such we can edit it as we see fit. Once you are done editing the file, you can save and exit the editor. You can verify that the changes were saved by running `cat test.txt`.

Let us wrap up on file management by copying and moving files. To copy a file, you can use the `cp` command followed by the name of the file and the destination. For example, to copy the `test.txt` file to a new file named `test_copy.txt`, you can run:

```bash
cp test.txt test_copy.txt
```

Like `cp`, the `mv` command is used to move files and directories. For example, to move the `test_copy.txt` file to a new directory named `test_files_copy`, you can run:

```bash
mkdir test_files_copy && mv test_copy.txt test_files_copy
```

NOTE: more often than not, built-in commands create folders and files if they do not exist, so take a little time to experiment with the commands and see what happens.

Lastly, to remove files, you can use the `rm` command followed by the name of the file. For example, to remove the `test_copy.txt` file, you can run:

```bash
rm test_copy.txt
```

The `cp` and `mv` commands can also be used with directories, but you will have to use the `-r` option to copy or move the directories and their contents recursively.

### 2.7 Redirection and Pipes

Let us conclude this brief introduction to Unix commands by discussing redirection and pipes. Redirection and pipes are powerful features of Unix systems that allow you to control the flow of data between commands and files. Redirection allows you to change the input and output sources of commands, while pipes allow you to chain commands together, passing the output of one command as the input to another.

#### 2.7.1 Redirection

The most common redirection operators are:

- `>`: Redirects the output of a command to a file, overwriting the file if it already exists.
- `>>`: Redirects the output of a command to a file, appending the output to the end of the file.
- `<`: Redirects the input of a command from a file.

For example, to redirect the output of the `ls` command to a file named `directory_contents.txt`, you can run:

```bash
ls > directory_contents.txt
```

This will create a new file named `directory_contents.txt` and write the output of the `ls` command to it. If you want to append the output of the `ls` command to an existing file, you can use the `>>` operator:

```bash
ls >> directory_contents.txt
```

If you want to redirect the input of a command from a file, you can use the `<` operator. For example, to count the number of lines in a file named `test.txt`, you can run:

```bash
wc -l < test.txt
```

There are several options that you can use to customize the behaviour of these operators, like redirect only the `stdout` and not the `stderr`.

#### 2.7.2 Pipes

Pipes are one of the most powerful features of Unix systems. They allow you to chain commands together, passing the output of one command as the input to another. The pipe operator `|` is used to connect commands. For example, to list the contents of the root directory and count the number of lines in the output, you can run:

```bash
ls / | wc -l
```

or to count the lines of the previous file we created, you can run:

```bash
cat test.txt | wc -l
```

Although it seems like a trivial example, pipes are incredibly powerful and can be used to create complex command pipelines that perform sophisticated data processing and manipulation. Although we will use them throughout the seminar, we will not go into much detail about them, but I encourage you to explore them on your own.

NOTE: We can solve the issue of too much output we had before using pipes! For example, you can run `tree | less` on the root directory to see the output of the `tree` command page by page, as if it was seen in a text editor.

### 2.8 Conclusion

Although we could spend the entire seminar talking about Linux commands, we will stop here now. As we progress through this seminar, we will discuss more commands, but we will not go into much detail about them. I encourage you to discover them on your own, experiment with them, and see how you can use them to automate tasks and interact with the system.

### 2.9 References

- [Getting Started With Linux Terminal](https://itsfoss.com/linux-terminal-basics/)
- [Explained: Input, Output and Error Redirection in Linux](https://linuxhandbook.com/redirection-linux/?ref=itsfoss.com#the-input-redirection)

## 3 Shell Scripting Basics

Let us conclude this first section of the seminar by introducing the very basics of shell scripting: creating and running a shell script that prints "Hello, Unix!" to the standard output. To create a new shell script, you can use any text editor you are comfortable with. For example, to create a new shell script named `hello_unix.sh` using `vim`, you can run:

```bash
vim hello_unix.sh
```

Inside the editor, you can write the following script:

```bash
#!/bin/bash
echo "Hello, Unix!"
```

Then, save and exit the editor. To make the script executable, you can use the `chmod` command followed by the `+x` option and the name of the script. For example, to make the `hello_unix.sh` script executable, you can run:

```bash
chmod +x hello_unix.sh
```

To run the script, you can use the `./` operator followed by the name of the script. For example, to run the `hello_unix.sh` script, you can run:

```bash
./hello_unix.sh
```

Alternatively, you can run the script using the `bash` command. For example, to run the `hello_unix.sh` script using `bash`, you can run:

```bash
bash hello_unix.sh
```

Okay, that was a lot of information, let us break it down:

- The script extension `.sh` is not required, but it is a common convention to use it for shell scripts. You may often see scripts without an extension, but it is recommended to use one for clarity.
- The first line of the script `#!/bin/bash` is called a shebang. It tells the system which interpreter to use to run the script. In this case, we are using `bash`. Basically, if we call the script using `bash hello_unix.sh`, we are specifying the interpreter to use, but if we make the script executable and call it using `./hello_unix.sh`, the system will use the interpreter specified in the shebang, or the default interpreter if no shebang is present.
- The `echo` command is used to print text to the standard output.
- The `chmod` command is used to change the permissions of a file. The `+x` option makes the file executable. We need to make the script executable if we want to run it directly using `./hello_unix.sh`. If the script is not executable, we can still run it using `bash hello_unix.sh`.
- The `./` operator is used to run the script directly from the current directory. We are simply specifying the path to the script we want to run (`.` is the current directory). Note that we must specify the path to the script, even if it is in the current directory.
- The `bash` command is used to run the script using the `bash` interpreter. We are specifying the interpreter to use and the path to the script we want to run.

NOTE: The `chmod` command can be used with different options to change the permissions of a file. We will see some options later on.
NOTE: The `bash` command actually calls the bash interpreter, you can call it without any arguments to start an interactive shell session (like the one you are using now if your default shell is `bash`).

## 4 Conclusion

Let us recap what we have learned in this first section of the seminar:

- We learned about the history and core philosophy of Unix, as well as some of its technical innovations.
- We explored some basic Unix commands for file and directory management.
- We discussed redirection and pipes, powerful features of Unix systems.
- We introduced the basics of shell scripting and created a simple shell script.

Next, we will dive deep into shell scripting, exploring variables, control structures, and functions. [Click here to continue to the next section](./01_unix_shell_programming_theory.md).
