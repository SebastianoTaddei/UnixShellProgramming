# Unix Shell Programming pt. 1

We will break down the basics of Unix Shell Programming into two sessions. In this session, we will cover the following topics:

- Creating and running shell scripts
- Variables
- Arithmetic operations
- Passing arguments to scripts

## 1 Creating and Running Shell Scripts

Let us start by creating a folder to hold our shell scripts. Open a terminal, navigate to the home directory, and create a new folder called `scripts`:

```bash
cd ~ && mkdir scripts
```

Now, navigate into the `scripts` folder:

```bash
cd scripts
```

Let us create our first shell script. Create a new file called `hello.sh` using `touch`:

```bash
touch hello.sh
```

Now, open the `hello.sh` file in your text editor and add the following lines:

```bash
#!/bin/bash
echo "Hello, Unix!"
```

Save the file and close the text editor. Now, let us make the script executable using `chmod`:

```bash
chmod u+x hello.sh
```

To run the script, use the following command:

```bash
./hello.sh
```

or simply:

```bash
bash hello.sh
```

You should see the output `Hello, Unix!` printed on the terminal.

NOTE: Contrary to the previous session, we used `chmod u+x` to make the script executable only by the current user. In general, it is a good practice to limit the execution permissions to fit the specific needs of the script.

You have successfully created and run your first shell script!

### 1.1 Adding Scripts to the PATH

Up to now, to run a script, we had to specify the path to the script. To avoid this, we can add the `scripts` folder to the `PATH` environment variable. This way, we can run the script from any directory without specifying the path. In general, I do not recommend this practice for every script you write, but it is useful for scripts you use frequently.

By executing the following command, we can add the `scripts` folder to the `PATH`:

```bash
export PATH=$PATH:$HOME/scripts
```

Now, we can run the `hello.sh` script from any directory:

```bash
hello.sh
```

NOTE: The `PATH` variable is reset every time you open a new terminal. To make the change permanent, add the `export PATH=$PATH:$HOME/scripts` command to your `.bashrc` file, which is a sort of configuration file for the `bash` shell that is executed every time you open a new terminal.
NOTE: `$HOME` is an environment variable that holds the path to the home directory of the current user. It should be equivalent to `~`.

## 2 Variables

In the previous section I threw around the term “variable”, but what is it? A variable is a container that holds a value. In shell scripting, variables are untyped, meaning they can hold any type of data. To create a variable, you simply assign a value to it. Here is an example:

```bash
variable="Hello, Unix!"
```

Now, the variable `variable` holds the string `Hello, Unix!`. To access the value of a variable, you simply prepend a `$` to the variable name. Here is an example:

```bash
echo $variable
```

and the output will be `Hello, Unix!`.

To better understand variables, let us create a new script called `variables.sh` and add the following lines:

```bash
name="Sebastiano"
age=26

echo "Hello, $name! You are $age years old."
```

This script creates two variables, `name` and `age`, and prints a message using the values of these variables. You can verify it by running the script, after having given it the execution permission, of course. These variables hold two different types of data: `name` holds a string, while `age` holds an integer. `bash` does not really care about the type of data you store in a variable, it mostly treats everything as a string, but some operations are type-sensitive (e.g., arithmetic operations).

## 3 Arithmetic Operations

Arithmetic operations are a fundamental part of programming. In shell scripting, you can perform arithmetic operations as well. Let us create a new script called `arithmetic.sh` and add the following lines:

```bash
#!/bin/bash
a=10
b=5
echo "a + b  = $((a + b))"
echo "a - b  = $((a - b))"
echo "a * b  = $((a * b))"
echo "a / b  = $((a / b))"
echo "a % b  = $((a % b))"
echo "a ** b = $((a ** b))"
```

running it will give you the results of the arithmetic operations. The syntax `$((...))` is used to evaluate the arithmetic expression inside the parentheses. You can use the following operators in shell scripting:

- `+` for addition
- `-` for subtraction
- `*` for multiplication
- `/` for division
- `%` for modulus
- `**` for exponentiation

## 4 Passing Arguments to Scripts

The easiest way to pass an argument to a shell script is by asking for user input using the `read` command. Let us create a new script called `arguments.sh` and add the following lines:

```bash
#!/bin/bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

Running this script will prompt you to enter your name, and it will greet you with the message `Hello, <your name>!`.

This method is useful when you want to interact with the user, but what if you want to pass arguments directly from the command line? You can access the arguments passed to the script using special variables. The arguments passed to the script are stored in the variables `$1`, `$2`, `$3`, and so on. Here is an example:

```bash
#!/bin/bash
echo "Hello, $1!"
```

If you can the script like this:

```bash
arguments.sh Sebastiano
```

it will print `Hello, Sebastiano!`.

You can pass more than one argument by separating them with spaces. The arguments are stored in the variables `$1`, `$2`, `$3`, and so on, depending on the position of the argument. There are ways to interact with these arguments, like shifting them to the left or right, we will cover them in the future.
