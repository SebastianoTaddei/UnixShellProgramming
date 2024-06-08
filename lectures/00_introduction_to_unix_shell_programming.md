# Introduction to Unix Shell Programming

Unix Shell Programming is a powerful tool that allows you to automate tasks, manage files, and interact with the operating system. This seminar will introduce you to the basics of Unix Shell Programming and provide you with the knowledge and skills to start writing your own shell scripts. To show the power of shell programming, throughout this seminar we will try to only use the Unix shell and a keyboard to fully interact with the system. It will not be easy at first, but hopefully it will provide you with a new perspective on how to interact with your computer.

Note: It goes without saying that you can follow this seminar as you see fit. You can use a graphical interface, a mouse, or any other tool you feel comfortable with. The goal is to learn and have fun!

Before we dive into the details, let's first understand what Unix is and why it is important.

## Introduction to Unix

### Historical Context

The history of Unix, a powerful and enduring operating system, began in the late 1960s and early 1970s. Developed at AT&T's Bell Labs by Ken Thompson, Dennis Ritchie, and others, Unix was initially a reaction to the complexities and limitations of existing systems. Its origins lie in the Multics project, an ambitious attempt to create a versatile, multi-user operating system. When Multics proved overly complex and ultimately unsuccessful, Thompson and Ritchie sought to create a simpler, more efficient system. This led to the birth of Unix, a name that itself is a pun on Multics, suggesting something more streamlined and practical.

### Core Philosophy

Unix's design philosophy is founded on simplicity, modularity, and reusability. These principles are encapsulated in a set of core ideas that have guided Unix development from the beginning. Some of the key principles include:

1. **Everything is a File:** In Unix, most entities, including hardware devices and inter-process communication channels, are represented as files. This abstraction simplifies the interaction between different parts of the system and provides a consistent interface.

2. **Small, Single-purpose Programs:** Unix encourages the creation of small, modular programs that perform specific tasks. These programs can be combined in flexible ways (*e.g.,* using pipes) to accomplish complex tasks, adhering to the philosophy of "do one thing and do it well."

3. **Use of Plain Text for Data Storage:** Unix systems prefer plain text for data storage, configuration, and communication. This choice enhances portability and simplicity, allowing users and programs to easily read and manipulate files.

4. **Build Software to Work Together:** Unix systems are designed so that programs can easily interoperate. This is achieved through mechanisms like pipes and redirection, which allow the output of one program to be used as the input for another.

### Technical Innovations

Unix introduced several groundbreaking technical innovations that have had a lasting impact on the field of computing:

- **Hierarchical File System:** Unix implemented a hierarchical file system structure, organizing files in a tree-like directory structure. This organization makes file management intuitive and scalable.

- **Process Control:** Unix introduced sophisticated process control mechanisms, allowing users to manage multiple processes simultaneously. Features like multitasking, background processing, and process signaling were revolutionary at the time.

- **Shell and Scripting:** The Unix shell, a command-line interpreter, allows users to interact with the system through textual commands. Shell scripting capabilities enable the automation of tasks and the chaining of commands, significantly enhancing productivity.

- **Portable Code:** Unix was one of the first operating systems to be written in the C programming language, developed by Dennis Ritchie. This decision made Unix highly portable, enabling it to be adapted to various hardware platforms with relative ease.

### Unix Variants and Influence

Over the decades, Unix has spawned a multitude of variants and inspired numerous operating systems. Some of the most notable Unix variants include: BSD, Linux, Solaris, and macOS.

Today, Unix and its derivatives play a crucial role in the world of computing. Unix-like systems are prevalent in servers, workstations, and embedded systems. They form the backbone of the internet, powering web servers, database servers, and various networking infrastructure. The principles and practices established by Unix continue to influence modern software development, from cloud computing to mobile applications. In addition, Unix is also used to develop the Operating Systems for mobile devices, like iOS.

The Unix philosophy of simplicity, modularity, and reusability has left an indelible mark on the software engineering field. Concepts like version control systems (e.g., Git), containerization (e.g., Docker), and configuration management (e.g., Ansible) all draw inspiration from Unix principles.

### Conclusion

Unix's journey from a small, experimental operating system to a cornerstone of modern computing is a testament to its robust design and enduring relevance. By emphasizing simplicity, modularity, and portability, Unix has created a legacy that transcends decades, shaping the evolution of operating systems and software development practices. As we look to the future, Unix's influence remains pervasive, guiding new generations of technology and innovation.

### References

- [A Quick Introduction to Unix](https://en.wikibooks.org/wiki/A_Quick_Introduction_to_Unix#:~:text=Unix%20is%20an%20operating%20system%20designed%20for%20use%20on%20any%20kind%20of%20computer%20or%20computing%20device.)
- [Unix](https://en.wikipedia.org/wiki/Unix)
- [Unix philosophy](https://en.wikipedia.org/wiki/Unix_philosophy)

## Basic Unix Commands

Without further ado, let's dive into the Unix command line and explore some basic commands that will be essential for our shell programming journey.

### Preliminary Steps

All we will need for this seminar is a Unix-like operating system (Linux, macOS, WSL on Windows) and a terminal emulator. We will use `bash` as our shell, but the concepts we discuss are applicable to other shells as well. In addition, I will use `vim` as my text editor, but feel free to use any text editor you are comfortable with.

Verify that you have everything set up by opening a terminal and running the following command:

```bash
echo "Hello, Unix!"
```

If you see the output `Hello, Unix!`, you are ready to go!

### Changing Directories

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

### Listing Directory Contents

Up to now we changed directories assuming we knew where we wanted to go. But what if we want to explore the contents of a directory before moving into it? We can use the `ls` command to list the contents of a directory. For example, to list the contents of the root directory, we can run:

```bash
ls /
```

this will show you all the files and directories in the root directory. You can also list the contents of the current directory by running `ls` without any arguments. Again, there are several options that you can use with `ls` to customize the output, we will explore them as we go along. The `ls` command stands for "list."

### Creating and Removing Directories

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

NOTE: Be careful when using the `rm` command, as it can permanently delete files and directories without moving them to the trash. Always double-check the arguments before running the command.

### More Options Available, but where!?

Let us take a stop for one second and think about the commands we have seen up to now. We have seen `pwd`, `cd`, `ls`, `mkdir`, `rmdir`, and `rm`. These are just a few of the commands available in Unix systems. I keep telling you that there are several options available for each command, but where can you find them? The answer is simple: the manual pages or directly asking the command for help. For example, to see the manual page for the `ls` command, you can run:

```bash
man ls
```

or to see the available options for the `ls` command, you can run:

```bash
ls --help
```

more often than not, the `--help` option is available for most commands and provides a quick overview of the available options.

NOTE: The `--help` options is usually available, but it often is under diffrene names, like `-h`, `-H`, `-?`, etc. You will have to experiment a bit to find the correct option for each command.

### Working with Files
