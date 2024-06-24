# Unix Shell Programming: Theory

We will break down the basics of Unix Shell Programming into the following topics:

- Creating and running shell scripts
- Variables
- Arithmetic operations
- Passing arguments to scripts
- Conditional statements
- Loops
- Arrays
- Functions

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

You have successfully created and ran your first shell script!

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
#!/bin/bash

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

If you run the script like this:

```bash
arguments.sh Sebastiano
```

it will print `Hello, Sebastiano!`.

You can pass more than one argument by separating them with spaces. The arguments are stored in the variables `$1`, `$2`, `$3`, and so on, depending on the position of the argument. There are ways to interact with these arguments, like shifting them to the left or right, I suggest you to look more into it if you are interested.

## 5 Arrays

Arrays are a collection of elements that are stored in a single variable. In shell scripting, you can create an array by assigning a list of values to a variable. The general syntax to create an array is as follows:

```bash
array=(value1 value2 value3)
```

We can access the elements of an array using the index of the element. The index of an array starts at 0. Let us create a new script called `arrays.sh` and add the following lines:

```bash
#!/bin/bash

elements=(1 2 3 4 5)
echo "First element: ${elements[0]}"
echo "Second element: ${elements[1]}"
```

Running this script will print the first and second elements of the array. To add an element to an array, you can use the `+=` operator. Let us modify the `arrays.sh` script to add a new element:

```bash
#!/bin/bash

elements=(1 2 3 4 5)
elements+=(6)
echo "First element: ${elements[0]}"
echo "Second element: ${elements[1]}"
echo "Last element: ${elements[-1]}"
```

You can print all the elements of an array by using the `*` operator. Let us modify the `arrays.sh` script to print all the elements:

```bash
#!/bin/bash

elements=(1 2 3 4 5)
elements+=(6)
echo "All elements: ${elements[*]}"
```

Running this script will print all the elements of the array, plus the new element we appended to the end. To delete an element from an array, you can use the `unset` command. Let us modify the `arrays.sh` script to delete an element:

```bash
#!/bin/bash

elements=(1 2 3 4 5)
elements+=(6)
echo "All elements: ${elements[*]}"
unset elements[0]
echo "All elements: ${elements[*]}"
```

Running this script will first print the original array, and then the same array without the element we deleted from it. If you call `unset` directly on the array without specifying an index it will delete the entire array.

Lastly, arrays in bash are not typed, meaning they can store heterogeneous data types. You can store strings, integers, and even other arrays in a single array. Let us create a new script called `arrays_heterogeneous.sh` and add the following lines:

```bash
#!/bin/bash

heterogeneous_elements=(1 "two" 3 "four" 5)
echo "Heterogeneous array: ${heterogeneous_elements[*]}"
```

Running this script will print the heterogeneous array. You can access the elements of the array using the index of the element, as usual.

## 6 Conditional Statements

Conditional statements are a construct that allows you to execute a block of code based on a condition. The idea is the same as in any other programming language. In `bash`, we have two types of conditional statements: `if` and `case`. Let us start with the `if` statement. The general syntax of an `if` statement is as follows:

```bash
if [ condition ]; then
    # code block
fi
```

The `condition` is a test that returns a boolean value. The `then` keyword is used to indicate the beginning of the code block. The `fi` keyword is used to indicate the end of the code block. Let us create a new script called `conditional.sh` and add the following lines:

```bash
#!/bin/bash

if [ 1 -eq 1 ]; then
    echo "1 is equal to 1"
fi
```

Running this script will print the message `1 is equal to 1`. The `-eq` operator is used to compare two values for equality. Beware that you **must** have spaces around the brackets and the operator, otherwise you will get an error. We can extend the `if` statement to include an `else` block:

```bash
if [ condition ]; then
    # code block
else
    # code block
fi
```

The `else` block is executed when the condition is false. Let us modify the `conditional.sh` script to include an `else` block:

```bash
#!/bin/bash

if [ 1 -eq 0 ]; then
    echo "1 is equal to 0"
else
    echo "1 is not equal to 0"
fi
```

Running this script will print the message `1 is not equal to 0`. We are not limited to a single condition, we can also use `elif` to add more conditions:

```bash
if [ condition ]; then
    # code block
elif [ condition ]; then
    # code block
elif [ condition ]; then
    # code block
else
    # code block
fi
```

The `elif` block is executed if the previous condition is false and the current condition is true. Let us modify the `conditional.sh` script to include an `elif` block:

```bash
#!/bin/bash

if [ 1 -eq 0 ]; then
    echo "1 is equal to 0"
elif [ 1 -eq 1 ]; then
    echo "1 is equal to 1"
else
    echo "1 is not equal to 0 or 1"
fi
```

Running this script will print the message `1 is equal to 1`. It goes without saying that you can nest conditional statements to create more complex logic.

Aside from the `if` statement, we have the `case` statement, which is used to match a value against a list of patterns. The general syntax of a `case` statement is as follows:

```bash
case "variable" in
    "pattern1")
        # code block
        ;;
    "pattern2")
        # code block
        ;;
    "pattern3")
        # code block
        ;;
    *)
        # code block
        ;;
esac
```

The `variable` is matched against the patterns, and the code block corresponding to the first matching pattern is executed. The `*` pattern is used as a default case when no pattern matches. The `case` statement is useful when you have multiple pattern matching conditions as it allows you to check each pattern in an efficient and easier to read way than `if-elif-else` statements. Let us create a new script called `case.sh` and add the following lines:

```bash
#!/bin/bash

case "1" in
    "0")
        echo "1 is equal to 0"
        ;;
    "1")
        echo "1 is equal to 1"
        ;;
    *)
        echo "1 is not equal to 0 or 1"
        ;;
esac
```

Running this script will print the message `1 is equal to 1`. You can use the `case` statement to match strings as well as integers and regular expressions.

Several test conditions exist in `bash`, such as:

- `-eq` for equality
- `-ne` for inequality
- `-lt` for less than
- `-le` for less than or equal to
- `-gt` for greater than
- `-ge` for greater than or equal to
- and many more

Fortunately, you do not have to memorize all of them, you can use the `man test` command to see the full list of test conditions.

## 7 Loops

Loops that allows you to execute a block of code multiple times. In `bash`, we have three types of loops: `for`, `while`, and `until`. Let us start with the `for` loop. We have two types of `for` loops in `bash`: the C-style and the list/range-style (similar to `Python`). The general syntax of a C-style `for` loop is as follows:

```bash
for (( initialization; condition; increment )); do
    # code block
done
```

For example, let us create a new script called `for_c.sh` and add the following lines:

```bash
#!/bin/bash

for ((i=0; i<5; i++)); do
    echo "i = $i"
done
```

Running this script will print the numbers from 0 to 4. The initialization, condition, and increment are similar to the C-style `for` loop. The general syntax of a list/range-style `for` loop is as follows:

```bash
for variable in list; do
    # code block
done
```

where `list` is a list or range of values. Let us create a new script called `for_list.sh` and add the following lines:

```bash
#!/bin/bash

for i in {0..4}; do
    echo "i = $i"
done
```

Running this script will print the numbers from 0 to 4. The `{0..4}` syntax is used to create a range of values. You can also use a list of values separated by spaces. The `for` loop will iterate over each value in the list.

The `while` loop is used to execute a block of code as long as a condition is true. The general syntax of a `while` loop is as follows:

```bash
while [ condition ]; do
    # code block
done
```

where `condition` is a test that returns a boolean value. Let us create a new script called `while.sh` and add the following lines:

```bash
#!/bin/bash

i=0
while [ $i -lt 5 ]; do
    echo "i = $i"
    (( i++ ))
done
```

Running this script will print the numbers from 0 to 4. The `-lt` operator is used to compare two values for less than. The `(( i++ ))` syntax is used to increment the value of `i` by 1.

The `until` loop is used to execute a block of code as long as a condition is false. The general syntax of an `until` loop is as follows:

```bash
until [ condition ]; do
    # code block
done
```

where `condition` is a test that returns a boolean value. Let us create a new script called `until.sh` and add the following lines:

```bash
#!/bin/bash

i=0
until [ $i -ge 5 ]; do
    echo "i = $i"
    (( i++ ))
done
```

Running this script will print the numbers from 0 to 4. The `-ge` operator is used to compare two values for greater than or equal to.

Whilst the `for` loop is useful when you know the number of iterations beforehand, the `while` and `until` loops are useful when you do not know the number of iterations beforehand. Be careful when using `while` and `until` loops, as they can lead to infinite loops if the condition is never met.

## 8 Functions

As in any other programming language, functions allow you to write reusable code. In `bash`, you can also define functions, but they are a bit different from other programming languages. The general syntax of a function is as follows:

```bash
function_name () {
    # code block
}
```

or alternatively:

```bash
function function_name {
    # code block
}
```

To call a function, you just need to use the function name. Keep in mind that the function must be defined before you call it. Let us create a new script called `functions.sh` and add the following lines:

```bash
#!/bin/bash

hello_world () {
    echo "Hello, World!"
}

hello_world
```

Running this script will print the message `Hello, World!`. You can pass arguments to a function just like you would pass arguments to a script. The arguments passed to the function are stored in the variables `$1`, `$2`, `$3`, and so on. Let us modify the `functions.sh` script to pass an argument

```bash
#!/bin/bash

hello () {
    echo "Hello, $1!"
}

hello "Unix"
```

Running this script will print the message `Hello, Unix!`. Note that even though the syntax is similar to passing arguments to a script, the arguments passed to a function are stored in different variables local to the function. Therefore, let us modify the `functions.sh` script to print the value of the argument

```bash
#!/bin/bash

hello () {
    echo "Hello, $1!"
}

hello "Unix"
echo "Argument: $1"
```

Running this script with a single argument like `./functions.sh Linux` will print the message `Hello, Unix!` and the value of the argument, `Linux`.

By default, `bash` functions return the exit status of the last command executed in the function. You can also explicitly return a value from a function using the `return` keyword. Let us modify the `functions.sh` script to return a value

```bash
#!/bin/bash

hello () {
    echo "Hello, $1!"
    return 0
}

hello "Unix"
echo "Exit status: $?"
```

When you run this script, you will see the message `Hello, Unix!` and the exit status `0`. The value returned by the function can be accessed using the `$?` variable.

Functions in `bash` can define local variables using the `local` keyword and access all global variables. Let us modify the `functions.sh` script to define a local variable

```bash
#!/bin/bash

name1="Unix"
name2="Linux"

hello () {
    local name2="Shell"
    echo "Local variable: $name2"
    echo "Global variable: $name1"
}

hello
echo "The global variable did not change: $name2"
```

By running this script we can see that even though the global variable `name2` is defined, the local variable `name2` takes precedence. The output will be `Local variable: Shell`, `Global variable: Unix`, and `The global variable did not change: Linux`.

By this point I think you can see a pattern with functions in `bash`: they behave pretty much exactly like scripts. In fact, you could call other scripts within another script and the syntax and behaviour would be basically the same.

Lastly, functions in `bash` support recursion. Recursive functions are functions that call themselves. Let us create a new script called `recursion.sh` and add the following lines:

```bash
#!/bin/bash

factorial () {
    if [ $1 -eq 0 ]; then
        echo 1
    else
        echo $(( $1 * $(factorial $(( $1 - 1 ))) ))
    fi
}

factorial 5
```

This function calculates the factorial of a number using recursion. Running this script will print the value of `5!`, which is `120`. Recursion is a powerful concept in programming, but it can lead to infinite loops if not used correctly, always make sure your recursive function has a base case.

## 9 Conclusion

After looking at all these simple examples, I believe that if you have any prior programming experience with languages like `C/C++`, `Python`, `Rust`, etc., you might be wondering "The syntax of `bash` looks a little clunky, the idea of solving classical problems (*e.g.,* network programming, scientific computing) looks horrible, and it does not seem as powerful as other languages, why should I learn it?". The answer to that is simple: `bash` is not meant to be like other programming languages, it does not try to solve those problems. `bash` is meant to be the glue between other processes, interacting with your system and automating tasks. You can use other languages to solve this problems, `Python` has a first-party library to call processes on your system, but you will find that it feels unnatural. That is why, after this theoretical part of Unix Shell Programming, we will move to the practical part, where we will see how to use `bash` to do what it does best: interacting with your system.

## 10 References

- [Start Learning Bash](https://linuxhandbook.com/bash/)
